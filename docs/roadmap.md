# Roadmap

Todas as **25 fases** do EnerSync foram concluídas com sucesso.

## Resumo

- :test_tube: **179 testes** backend passando
- :shield: **44 permissões** RBAC (3 roles)
- :rocket: **25 fases** implementadas
- :package: Stack completa: Auth, RBAC, CRUD, Otimizador, Faturamento, Audit, Notificações, Dashboard, Import/Export, Filtros, PDF, Email, Cron, Onboarding, Observabilidade, Bulk, i18n

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
