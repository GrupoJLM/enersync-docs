# Roadmap

**40 fases** do EnerSync foram concluídas (+ 1 pendente: microserviço externo).

## Resumo

- :test_tube: **276 testes** backend passando
- :shield: **44 permissões** RBAC (3 roles)
- :rocket: **40 fases** implementadas (26 pendente — escopo externo)
- :package: Stack completa: Auth, RBAC, CRUD, Otimizador, Faturamento, Audit, Notificações, Dashboard, Import/Export, Filtros, PDF, Email, Cron, Onboarding, Observabilidade, Bulk, i18n, Portal, Simulador, Integrações, Geração, Aprovações, Push, Marketplace, Compliance, Webhooks, Rate Limiting, Mobile Flutter, Assinatura/Billing, UI/UX Redesign, Qualidade/Segurança

---

## Fases

### Fase 1: Fundação :white_check_mark:
Auth JWT (login, refresh, me) com bcrypt. Multi-tenancy RLS. Models base: Tenant, User. CI/CD: GitHub Actions. Docker + docker-compose. **7 testes**.

### Fase 2: Modelos de Domínio GD :white_check_mark:
6 entidades (Distribuidora, Usina, UC, Contrato, Fatura, Crédito). 8 enums. CRUD completo. Regras: quota ≤ 100%, expiração 60 meses. **38 testes**.

### Fase 3: Agente Otimizador :white_check_mark:
Algoritmo FIFO global para alocação de créditos. Calculo de tarifa média e economia. Endpoints simular + executar. **47 testes**.

### Fase 4: Módulo de Faturamento :white_check_mark:
FaturaInterna: cobrança por % da economia. Relatório financeiro por UC. **55 testes**.

### Fase 5: Audit Trail :white_check_mark:
AuditLog em todas entidades tenant-scoped. 6 ações: created, updated, deleted, executed, generated, cancelled. **63 testes**.

### Fase 6: Sistema de Notificações :white_check_mark:
3 tipos: crédito expirando, fatura atrasada, contrato vencendo. Deduplicação via UniqueConstraint. **73 testes**.

### Fase 7: Dashboard / Indicadores :white_check_mark:
6 categorias de KPIs. Queries com CASE WHEN + SUM/COUNT. **80 testes**.

### Fase 8: Permissões Granulares (RBAC) :white_check_mark:
36 codenames iniciais. 3 roles: ADMIN, MANAGER, VIEWER. Cache em memória. **90 testes**.

### Fase 9: Gestão de Usuários :white_check_mark:
CRUD completo. Email único global. Proteção contra auto-desativação. **103 testes**.

### Fase 10: Frontend Web :white_check_mark:
React 19 + Vite 7 + TanStack. 13 páginas. DataTable, ConfirmDialog, PageHeader. **103 testes backend**.

### Fase 11: Deploy (Docker + CI/CD) :white_check_mark:
Nginx reverse proxy. Multi-stage Docker. CI com 4 jobs paralelos. **103 testes**.

### Fase 12: Paginação e Filtros :white_check_mark:
PaginatedResponse genérico. Busca ILIKE em 5 entidades. Server-side pagination. **103 testes**.

### Fase 13: Testes E2E com Playwright :white_check_mark:
7 specs, 25 testes E2E. Auth setup, seed, API fixtures. **103 + 25 testes**.

### Fase 14: Importação CSV/Excel :white_check_mark:
Preview + confirm. CSV auto-detect, XLSX. Formatos BR. Limite 500 linhas. **113 testes**.

### Fase 15: Filtros Avançados :white_check_mark:
4 services com filtros opcionais. Filter bar no frontend. **125 testes**.

### Fase 16: Exportação CSV/Excel :white_check_mark:
Faturas e relatórios. Botões CSV/Excel no frontend. **131 testes**.

### Fase 17: Dashboard com Gráficos :white_check_mark:
Recharts: AreaChart, BarChart, PieChart. 3 datasets do backend. **134 testes**.

### Fase 18: Configurações do Tenant :white_check_mark:
13 campos de configuração. SMTP mascarado. 3 seções no frontend. **140 testes**.

### Fase 19: Tarefas Agendadas :white_check_mark:
3 tasks automáticas. Endpoint manual POST /tasks/run/{name}. **148 testes**.

### Fase 20: Envio de Email :white_check_mark:
aiosmtplib + Jinja2 templates. SMTP do tenant. Integrado com notificações. **153 testes**.

### Fase 21: Relatórios PDF :white_check_mark:
fpdf2 para geração de PDF. Fatura e relatório financeiro. **157 testes**.

### Fase 22: Onboarding Multi-tenant :white_check_mark:
POST /auth/register público. Cria tenant + admin + permissions. JWT imediato. **164 testes**.

### Fase 23: Logs e Observabilidade :white_check_mark:
structlog JSON/console. RequestID + AccessLog middlewares. Sentry opcional. **167 testes**.

### Fase 24: Ações em Lote :white_check_mark:
Bulk update faturamento + notificações. DataTable com checkbox selection. **173 testes**.

### Fase 25: Internacionalização (i18n) :white_check_mark:
react-intl frontend + t() backend. pt-BR/en-US. Seletor de idioma. **179 testes**.

### Fase 26: Microserviço de Cobrança :hourglass:
**PENDENTE** — Repositórios separados (cobranca-service + cobranca-web). PIX/Boleto com Efi + Santander PJ.

### Fase 27: Performance Indexes :white_check_mark:
Indexes compostos em faturas, creditos, contratos. 8 composite indexes para queries comuns. **194 testes**.

### Fase 28: Health Check :white_check_mark:
Endpoint GET /api/health com verificação de DB. Retorna status, version, uptime. **194 testes**.

### Fase 29: Compliance / LGPD :white_check_mark:
Audit log para LGPD. Exportação de dados pessoais. Anonimização sob demanda. **200 testes**.

### Fase 30: Marketplace :white_check_mark:
Equipamentos solares, categorias, favoritos. GET /marketplace/offers + POST /marketplace/simulate. **206 testes**.

### Fase 31: Sistema de Notificações In-App :white_check_mark:
Notificações in-app com read/unread. Bulk mark as read. **211 testes**.

### Fase 32: Relatórios Avançados :white_check_mark:
Resumo financeiro por UC. Filtros por período. Exportação PDF. **216 testes**.

### Fase 33: Tarefas Agendadas :white_check_mark:
Task registry. Endpoint run. Task history. **218 testes**.

### Fase 34: Sistema de Email :white_check_mark:
aiosmtplib + Jinja2 templates. SMTP por tenant. Emails de fatura e boas-vindas. **225 testes**.

### Fase 35: App Mobile Flutter :white_check_mark:
Flutter app (login + dashboard). Riverpod, GoRouter, Dio. **225 testes**.

### Fase 35.1: Melhorias de Robustez :white_check_mark:
Rate limiting (slowapi). Webhook system (HMAC-SHA256). Performance indexes. Monthly report task. Frontend Vitest. Webhook management UI. **242 testes**.

### Fase 36: Assinatura e Billing :white_check_mark:
Integração Asaas (sandbox). Planos mensal/anual. Checkout PIX/cartão/boleto. Webhook de pagamento. Portal do assinante. **254 testes**.

### Fase 36.1: Billing Anual :white_check_mark:
Toggle mensal/anual no frontend e mobile. Cálculo de desconto anual. **254 testes**.

### Fase 36.2: Asaas Webhook :white_check_mark:
Webhook de pagamento Asaas com validação de token. ECS task def rev 4 com secrets. **276 testes**.

### Fase 37: Geração :white_check_mark:
Módulo de geração de energia com leituras e histórico. Página `/geracao` no frontend. **276 testes**.

### Fase 38: Aprovações :white_check_mark:
Workflow de aprovação multi-nível. Página `/aprovacoes` no frontend. **276 testes**.

### Fase 39: UI/UX Redesign :white_check_mark:
Sidebar colapsável com ícones. Header com breadcrumbs (useMatches). StatCards reutilizáveis. Guided tour (react-joyride). Avatar com iniciais. Tema oklch. Empty states com ícone Inbox. .npmrc para peer deps.

### Fase 40: Qualidade, UX e Segurança :white_check_mark:
ErrorBoundary (class component) nas rotas autenticadas. Dark mode toggle (next-themes + Sun/Moon). Debounce 300ms no search do DataTable. Code splitting recharts (manualChunks: 505KB→110KB+409KB). Fix Array.isArray guard no configuracoes (s.map crash). Security headers no nginx.conf (docker local).

### Backlog (Março 2026)

Itens de melhoria executados após Fase 40:

| Item | Status | Detalhes |
|------|--------|----------|
| CI API fix | ✅ | Corrigidos 3 bugs (test_list_tasks, MissingGreenlet webhook, ruff B008), pip-audit ecdsa ignorada |
| CloudFront Security Headers | ✅ | Response Headers Policy com OWASP headers (HSTS, X-Frame, CSP) |
| E2E Coverage | ✅ | 12→19 specs, 58→82 tests (configuracoes, faturamento, usuarios, notificacoes, relatorios) |
| Dependency Scanning | ✅ | npm audit + pip-audit no CI (ambos repos) |
| Multi-idioma | ✅ | Frontend: 76→350+ keys pt-BR/en-US. Backend: 11→33 keys com format strings |
| Mobile Expandir | ✅ | Dashboard charts (fl_chart), detail screens (contratos/creditos/faturas), push token service |
| Dashboard Personalizável | ✅ | @dnd-kit sortable KPIs, localStorage persistence, reset layout |
| Push Notifications (FCM) | ✅ | firebase-admin backend, lazy init, token cleanup, graceful no-config |
| Push Nativo Mobile | ⏳ | Requer google-services.json + firebase_messaging Flutter |
| Web Push | ⏳ | Requer VAPID keys + Service Worker |
| Monitoring (Web Vitals) | ⏳ | Depende de OpenTelemetry infra |
