---
tags:
  - Changelog
---

# Changelog

Registro de todas as mudanças significativas do projeto EnerSync, organizadas por fase.

---

## [Fase 25] — Internacionalização (i18n)

**179 testes** | Março 2026

### Adicionado
- Backend: `LocaleMiddleware` para `Accept-Language`
- Backend: função `t()` em `lib/i18n.py` para tradução de mensagens
- Backend: header `Content-Language` na response
- Frontend: `react-intl` com `IntlProvider` customizado
- Frontend: 2 locale files — `pt-BR.json` e `en-US.json` (70+ chaves)
- Frontend: sidebar com labels traduzidos via `intl.formatMessage()`
- Frontend: seletor de idioma no header (Globe icon + Select)
- Frontend: `localStorage` para persistir preferência do usuário
- 6 testes novos (pt-BR, en-US, fallback, Content-Language header)

---

## [Fase 24] — Ações em Lote (Bulk)

**173 testes** | Março 2026

### Adicionado
- `POST /faturamento/bulk` — marcar pago, cancelar em lote
- `POST /notificacoes/bulk/lida` — marcar lidas em lote
- `BulkActionRequest` / `BulkActionResult` schemas
- Frontend: DataTable com checkbox selection
- Frontend: `BulkActionBar` flutuante com contagem + ações
- 6 testes novos

---

## [Fase 23] — Logs e Observabilidade

**167 testes** | Fevereiro 2026

### Adicionado
- structlog com output JSON (prod) e console (dev)
- `RequestIDMiddleware` — UUID por request, header `X-Request-ID`
- `AccessLogMiddleware` — log de method/path/status/duration_ms
- Sentry SDK — inicialização condicional via `SENTRY_DSN`
- Health check melhorado: `db_status`, `version`, `uptime_seconds`
- 3 config fields: `sentry_dsn`, `log_level`, `log_format`

---

## [Fase 22] — Onboarding Multi-tenant

**164 testes** | Fevereiro 2026

### Adicionado
- `POST /auth/register` — endpoint público para registro
- Cria Tenant + User(ADMIN) + seed permissions em transação única
- Validação de slug (regex), email único, password min 8 chars
- Retorna JWT imediatamente após registro
- Frontend: página `/registro` com link login ↔ registro
- 7 testes novos

---

## [Fase 21] — Relatórios PDF

**157 testes** | Fevereiro 2026

### Adicionado
- fpdf2 para geração de PDF (pure Python)
- `GET /faturamento/{id}/pdf` — PDF da fatura com header da empresa
- `GET /relatorios/financeiro/export/pdf` — relatório financeiro em PDF
- Frontend: botões de download PDF em faturamento e relatórios
- 4 testes novos

---

## [Fase 20] — Envio de Email

**153 testes** | Fevereiro 2026

### Adicionado
- aiosmtplib para envio assíncrono
- Jinja2 templates: `base.html`, `credito_expirando.html`, `fatura_atrasada.html`, `contrato_vencendo.html`
- Config SMTP por tenant (cada tenant com suas credenciais)
- Integração com notificações (email automático se habilitado)
- `POST /settings/test-email` para testar configuração
- 5 testes novos

---

## [Fase 19] — Tarefas Agendadas

**148 testes** | Janeiro 2026

### Adicionado
- 3 tasks: `expire_credits`, `mark_overdue_invoices`, `generate_all_notifications`
- `GET /tasks` e `POST /tasks/run/{task_name}` (ADMIN only)
- 2 permissões: `tasks.view`, `tasks.execute`
- Frontend: página `/tarefas` com cards por task
- Migration 0009
- 8 testes novos

---

## [Fase 18] — Configurações do Tenant

**140 testes** | Janeiro 2026

### Adicionado
- 13 colunas de configuração no Tenant (company, SMTP, default_service_fee)
- `GET/PATCH /settings` + `POST /settings/test-email`
- `TenantSettingsRead` mascara `smtp_password` (expõe `smtp_password_set: bool`)
- 2 permissões: `settings.view`, `settings.update`
- Frontend: página `/configuracoes` com 3 tabs
- Migration 0008
- 6 testes novos

---

## [Fase 17] — Dashboard com Gráficos

**134 testes** | Janeiro 2026

### Adicionado
- `GET /dashboard/charts?months=12` com 3 datasets
- Recharts: AreaChart (consumo), BarChart (faturamento), PieChart (créditos)
- Gráficos responsivos em grid 2 colunas
- 3 testes novos

---

## [Fase 16] — Exportação CSV/Excel

**131 testes** | Janeiro 2026

### Adicionado
- `GET /faturas/export/{csv|xlsx}` com filtros aplicados
- `GET /relatorios/financeiro/export/{csv|xlsx}`
- `services/export.py` com helpers genéricos
- Frontend: botões CSV/Excel em faturas e relatórios
- 6 testes novos

---

## [Fase 15] — Filtros Avançados

**125 testes** | Janeiro 2026

### Adicionado
- Filter params opcionais em faturas, contratos, créditos, faturamento
- Frontend: filter bars com Select/Input acima do DataTable
- Reset de página ao mudar filtro
- 12 testes novos

---

## [Fase 14] — Importação CSV/Excel

**113 testes** | Dezembro 2025

### Adicionado
- `POST /faturas/import/preview` e `POST /faturas/import/confirm`
- Suporte CSV (delimitador `;` ou `,`) e XLSX
- Formatos BR: `DD/MM/YYYY`, `MM/YYYY`, vírgula decimal
- Aliases de colunas em português
- Limite 500 linhas por arquivo
- Frontend: dialog 2 etapas (upload → preview → confirmar)
- Audit log com batch UUID
- 10 testes novos

---

## [Fase 13] — Testes E2E com Playwright

**103 + 25 testes** | Dezembro 2025

### Adicionado
- 7 specs Playwright, 25 testes E2E
- `seed_e2e.py` para setup de dados
- Fixtures com `RUN_ID` para unicidade entre runs
- CI: job E2E com PostgreSQL service

### Corrigido
- Login JSON vs form-urlencoded
- TanStack Router redirect race condition (`router.invalidate()`)
- Auth guard loading state
- Empty string → null para campos opcionais

---

## [Fase 12] — Paginação e Filtros

**103 testes** | Dezembro 2025

### Adicionado
- `PaginatedResponse[T]` genérico em todos endpoints de listagem
- Helper `paginate()` reutilizável
- Busca ILIKE em 5 entidades
- Frontend: paginação server-side em todas páginas

---

## [Fase 11] — Deploy (Docker + CI/CD)

**103 testes** | Novembro 2025

### Adicionado
- Nginx reverse proxy (porta 80)
- `Dockerfile` multi-stage para web (Node → Nginx)
- `docker-compose.prod.yml` com health checks
- `entrypoint.sh` com alembic auto-migrate
- CI: 4 jobs paralelos (lint-api, test-api, lint-web, build-web)

---

## [Fase 10] — Frontend Web

**103 testes** | Novembro 2025

### Adicionado
- React 19 + Vite 7 + TypeScript + TanStack Router/Query/Table
- shadcn/ui + Tailwind CSS v4 (dark mode)
- 13 páginas: login, dashboard, 7 CRUDs, otimizador, faturamento, relatórios, notificações, audit
- Axios com JWT interceptors (token attach + 401 refresh queue)
- 13 hooks TanStack Query

---

## [Fase 9] — Gestão de Usuários

**103 testes** | Novembro 2025

### Adicionado
- CRUD `/users` completo (tenant-scoped)
- Regras: email único, proteção contra auto-desativação
- 4 permissões novas: `users.view/create/update/delete`
- Migration 0007
- 13 testes novos

---

## [Fase 8] — Permissões Granulares (RBAC)

**90 testes** | Outubro 2025

### Adicionado
- 36 codenames iniciais em `permissions` + `role_permissions`
- 3 roles: ADMIN (36), MANAGER (29), VIEWER (10)
- Cache em memória para permission checks
- `require_permission()` em todos 46 endpoints
- Migration 0006
- 10 testes novos

---

## [Fase 7] — Dashboard / Indicadores

**80 testes** | Outubro 2025

### Adicionado
- `GET /dashboard` com 6 categorias de KPIs
- Queries eficientes com `CASE WHEN + SUM/COUNT`
- 7 testes novos

---

## [Fase 6] — Sistema de Notificações

**73 testes** | Outubro 2025

### Adicionado
- 3 tipos: `credito_expirando`, `fatura_atrasada`, `contrato_vencendo`
- Prioridades: critical/high/medium/low
- Deduplicação via UniqueConstraint
- Endpoints: gerar, listar, resumo, marcar lida
- Migration 0005
- 10 testes novos

---

## [Fase 5] — Audit Trail

**63 testes** | Setembro 2025

### Adicionado
- `AuditLog` em todas entidades tenant-scoped
- 6 ações: created, updated, deleted, executed, generated, cancelled
- Endpoints: `GET /audit-log` com filtros e histórico
- Migration 0004
- 8 testes novos

---

## [Fase 4] — Módulo de Faturamento

**55 testes** | Setembro 2025

### Adicionado
- `FaturaInterna`: cobrança = % da economia gerada
- `POST /faturamento/gerar` e `/gerar-periodo`
- `POST /relatorios/financeiro` (resumo por UC)
- Migration 0003
- 8 testes novos

---

## [Fase 3] — Agente Otimizador

**47 testes** | Agosto 2025

### Adicionado
- Algoritmo FIFO global para alocação de créditos
- Cálculo de tarifa média e economia estimada
- `POST /otimizador/simular` e `/executar`
- 9 testes novos

---

## [Fase 2] — Modelos de Domínio GD

**38 testes** | Agosto 2025

### Adicionado
- 6 entidades: Distribuidora, Usina, UC, Contrato, Fatura, Crédito
- 8 enums centralizados
- CRUD completo (schemas, services, routers)
- Regras: quota ≤ 100%, expiração 60 meses
- Migration 0002 com RLS policies
- 31 testes novos

---

## [Fase 1] — Fundação

**7 testes** | Julho 2025

### Adicionado
- Auth JWT (login, refresh, me) com bcrypt
- Multi-tenancy RLS (`tenant_id` + `SET LOCAL`)
- Models base: Tenant, User
- CI/CD: GitHub Actions (lint + test)
- Docker + docker-compose
- Migration 0001
