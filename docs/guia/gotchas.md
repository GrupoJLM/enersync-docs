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
