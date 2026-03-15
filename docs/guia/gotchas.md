---
tags:
  - Backend
  - Frontend
  - DevOps
  - Gotchas
---

# Gotchas e Armadilhas

Lições aprendidas durante o desenvolvimento do EnerSync.

## Backend

### asyncpg + SET LOCAL

asyncpg **não** suporta bind params (`$1`) em comandos SET.

```python
# ❌ Não funciona
await db.execute(text("SET LOCAL app.current_tenant = :tid"), {"tid": tenant_id})

# ✅ Validar UUID primeiro, depois f-string
await db.execute(text(f"SET LOCAL app.current_tenant = '{tenant_id}'"))
```

### passlib + bcrypt 5.x

passlib não funciona com bcrypt >= 5.0 (erro `_stub_requires_backend`).

```python
# ❌ passlib
from passlib.context import CryptContext

# ✅ bcrypt direto
import bcrypt
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt()).decode()
```

### pytest-asyncio + event loop

- Engine criado em escopo de módulo causa `Future attached to a different loop`
- **Solução**: criar engine dentro de fixture por-teste (function scope)
- Sessões compartilhadas entre fixture e app causam `another operation in progress`
- **Solução**: factory separada no dependency override

### Dados residuais no test DB

Se teste falha no meio, tabelas ficam com dados.

**Solução**: `drop_all` ANTES de `create_all` no setup_db fixture.

### Pacote não importável

`pyproject.toml` precisa de `[build-system]` com hatchling para `uv sync` instalar o pacote.

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/energy_saas"]
```

### Middleware ordering

Middlewares são executados em ordem **LIFO** (último adicionado = primeiro executado).

```python
# Ordem de app.add_middleware():
app.add_middleware(LocaleMiddleware)        # 4º adicionado → 4º executado
app.add_middleware(AccessLogMiddleware)     # 3º adicionado → 3º executado
app.add_middleware(RequestIDMiddleware)     # 2º adicionado → 2º executado
app.add_middleware(TenantMiddleware)        # 1º adicionado → 1º executado

# Execução real: Tenant → RequestID → AccessLog → Locale
```

### Rota bulk ANTES de path params

```python
# ❌ "bulk" é capturado como {fatura_id}
router.get("/{fatura_id}")
router.post("/bulk")

# ✅ Rotas específicas primeiro
router.post("/bulk")
router.get("/{fatura_id}")
```

### response_model=None para retornos binários

FastAPI não aceita `Union[StreamingResponse, Response]` como response_model.

```python
# ✅ Endpoints que retornam bytes/stream
@router.get("/export/{fmt}", response_model=None)
```

---

## Frontend

### Node.js + Vite 7

Vite 7 requer Node `^20.19.0 || >=22.12.0`. **Node 18 não funciona**.

```bash
nvm install 22
nvm use 22  # Sempre antes de npm commands
```

### shadcn/ui + Tailwind v4

- Precisa ter `@import "tailwindcss"` no globals.css ANTES do `npx shadcn@latest init`
- Precisa de `baseUrl` + `paths` no tsconfig.json

### TanStack Router

- `routeTree.gen.ts` é gerado automaticamente pelo plugin Vite
- Novas rotas: reiniciar dev server ou rodar `npx @tanstack/router-cli generate`

### shadcn Select + valor "todos"

shadcn Select não aceita string vazia. Para filtros "Todos":

```typescript
// ✅ Usar valor "all"
<SelectItem value="all">Todos</SelectItem>

// No handler:
const filter = value !== 'all' ? value : undefined
```

### z.coerce.number() com TypeScript strict

`z.coerce.number()` infere tipo `unknown` no input.

```typescript
// ❌ Erro TS com zodResolver
z.coerce.number()

// ✅ Usar z.number() + valueAsNumber
z.number()
// No register:
form.register("field", { valueAsNumber: true })
```

### TanStack Router redirect race condition

`navigate()` após login não funciona se React state não flushou.

```typescript
// ❌ Falha
await login(email, password)
navigate({ to: '/' })

// ✅ Invalidar router antes
await login(email, password)
await router.invalidate()
navigate({ to: '/' })
```

### Empty strings → 422

Inputs HTML retornam `""` para campos vazios. Pydantic rejeita `""` para dates.

```typescript
// ✅ Limpar no onSubmit
const cleanData = { ...data, date_field: data.date_field || null }
```

### Alembic + asyncpg: sa.Enum vs postgresql.ENUM

`sa.Enum(create_type=False)` **não funciona** com asyncpg — o evento `_on_table_create` ainda dispara `CREATE TYPE`. Usar `postgresql.ENUM(create_type=False)`.

```python
# ❌ Ainda cria o TYPE com asyncpg
sa.Enum("a", "b", name="meu_enum", create_type=False)

# ✅ Correto
from sqlalchemy.dialects.postgresql import ENUM as pgENUM
pgENUM("a", "b", name="meu_enum", create_type=False)
```

Além disso, `sa.Enum.create(checkfirst=True)` não é confiável com asyncpg. Para criação idempotente de enums em migrations Alembic, usar bloco raw SQL:

```python
op.execute("""
DO $$
BEGIN
    IF NOT EXISTS (SELECT 1 FROM pg_type WHERE typname = 'meu_enum') THEN
        CREATE TYPE meu_enum AS ENUM ('a', 'b');
    END IF;
END$$;
""")
```

### SQLAlchemy Enum valores vs nomes

`SQLEnum(StrEnum)` usa **nomes** (ADMIN) por default. Para enviar valores (admin), usar `values_callable`.

```python
# ❌ Envia ADMIN, MANAGER, VIEWER
Column(SQLEnum(UserRole))

# ✅ Envia admin, manager, viewer
Column(SQLEnum(UserRole, values_callable=lambda e: [x.value for x in e]))
```

### Performance indexes em queries com tenant_id

Indexes compostos devem incluir `tenant_id` como primeira coluna para funcionar com RLS.

```python
# ✅ Index composto eficiente com RLS
Index("ix_faturas_tenant_ref", "tenant_id", "reference_month")
Index("ix_creditos_tenant_exp", "tenant_id", "expiration_date")
```

### Rate limiting com slowapi + proxy

`get_remote_address` pega o IP do cliente. Atrás de proxy (Nginx, API Gateway), usar `X-Forwarded-For`.

```python
# ✅ Atrás de proxy
from slowapi.util import get_remote_address
limiter = Limiter(key_func=get_remote_address)
# Certificar que proxy envia X-Forwarded-For
```

### Webhook HMAC signature com encoding

O body deve ser serializado para bytes **antes** de calcular o HMAC. Usar `json.dumps()` com `sort_keys=True` para garantir determinismo.

```python
# ✅ Payload determinístico
payload_bytes = json.dumps(payload, sort_keys=True, default=str).encode()
signature = hmac.new(secret.encode(), payload_bytes, hashlib.sha256).hexdigest()
```

### Flutter Secure Storage no Android

`flutter_secure_storage` requer `minSdkVersion 18+` no Android. Verificar `android/app/build.gradle`.

### useRef sem argumento inicial no CI

TypeScript strict no CI (diferente do local) exige argumento inicial em `useRef`. Sempre passar `undefined`.

```typescript
// ❌ Falha no CI
const ref = useRef<ReturnType<typeof setTimeout>>()

// ✅ Funciona em todos os ambientes
const ref = useRef<ReturnType<typeof setTimeout> | undefined>(undefined)
```

### React Compiler rejeita ref.current no render

O React Compiler trata `ref.current = value` no corpo do componente como "acesso a ref durante render". Mover para `useEffect`.

```typescript
// ❌ React Compiler error
const callbackRef = useRef(callback)
callbackRef.current = callback  // no render body

// ✅ Dentro de useEffect
useEffect(() => {
  callbackRef.current = callback
}, [callback])
```

### Array.isArray guard para dados de API

Dados de API podem retornar formato inesperado (string, objeto). Usar `Array.isArray()` antes de `.map()`.

```typescript
// ❌ Crash se webhooks não for array
!webhooks?.length

// ✅ Guard defensivo
!Array.isArray(webhooks) || !webhooks.length
```

### react-joyride peer dependency

react-joyride espera React 18. Com React 19, usar `.npmrc` com `legacy-peer-deps=true`.

### oklch colors e Tailwind v4

Variáveis CSS do tema usam `oklch()`. Cores como `hsl()` do shadcn default não funcionam — converter para oklch.

### useMatches breadcrumbs

`useMatches()` do TanStack Router retorna todas as rotas matched. Filtrar `pathname !== '/'` e `pathname !== '/_authenticated'` para breadcrumbs limpos.

### Security headers — CloudFront vs nginx

O frontend em produção usa CloudFront + S3 (estático). O `nginx.conf` no Dockerfile só é usado em Docker local. Para produção, configurar security headers via **CloudFront Response Headers Policy**.

### Sentry DSN deve ser validado antes de init

Sentry SDK falha silenciosamente com DSN inválido. Validar que começa com `https://` antes de `sentry_sdk.init()`.

```python
# ✅ Validar antes
if settings.sentry_dsn and settings.sentry_dsn.startswith("https://"):
    sentry_sdk.init(dsn=settings.sentry_dsn)
```

---

## Deploy

### Nginx proxy_pass trailing slash

```nginx
# ❌ /api/auth/login → /api/auth/login no backend
proxy_pass http://api:8000;

# ✅ /api/auth/login → /auth/login no backend
proxy_pass http://api:8000/;
```

### Docker health check para Python slim

Python slim **não** tem curl/wget.

```yaml
# ✅ Usar urllib do Python
healthcheck:
  test: python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"
```

### UniqueConstraint em testes bulk

Criar múltiplas faturas no mesmo teste com mesma UC requer `reference_months` diferentes.

```python
# ✅ Contador global para evitar constraint violations
_month_counter = 0
def next_month():
    global _month_counter
    _month_counter += 1
    return f"2024-{_month_counter:02d}"
```

### SQLAlchemy MissingGreenlet após db.commit()

Após `await db.commit()`, a sessão expira os objetos. Acessar atributos lazy (como `updated_at`) causa `MissingGreenlet`. Solução: `await db.refresh(obj)` antes de retornar o objeto na response.

### pip-audit ecdsa CVE-2024-23342

`ecdsa` é dependência transitiva de `python-jose`. O CVE é sobre timing side-channel no ECDSA puro Python. Como usamos `python-jose[cryptography]` (backend OpenSSL), não somos afetados. Ignorar com `--ignore-vuln CVE-2024-23342` no CI.

### ruff B008 com Query() e tipos Enum

`ruff` flageia `Query(default=None)` como B008 quando o tipo do parâmetro é um Enum (ex: `WebhookDeliveryStatus | None`). Outros tipos como `str | None` e `int` não são flaggeados. Usar `# noqa: B008` (padrão FastAPI).

### Firebase Admin sem credenciais

`firebase-admin` precisa de credenciais para enviar push. Se `FIREBASE_CREDENTIALS_JSON` e `GOOGLE_APPLICATION_CREDENTIALS` não estiverem configurados, o serviço deve logar warning e retornar sem crash. Usar lazy initialization.
