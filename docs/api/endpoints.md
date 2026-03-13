---
tags:
  - Backend
  - API
---

# Backend — Endpoints

## Visão Geral

A API possui **25+ routers** com **80+ endpoints**, todos sob o prefixo `/api`.

Documentação interativa disponível em `/docs` (Swagger) e `/redoc`.

## Autenticação

| Método | Endpoint | Descrição | Auth |
|--------|----------|-----------|------|
| POST | `/auth/login` | Login (retorna JWT) | Público |
| POST | `/auth/refresh` | Refresh token | Público |
| POST | `/auth/register` | Registro de novo tenant | Público |
| GET | `/auth/me` | Dados do usuário logado | JWT |

## Health Check

| Método | Endpoint | Descrição | Auth |
|--------|----------|-----------|------|
| GET | `/health` | Status da API + DB | Público |

## CRUD — Entidades GD

### Distribuidoras

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/distribuidoras` | `distribuidoras.view` |
| POST | `/distribuidoras` | `distribuidoras.create` |
| GET | `/distribuidoras/{id}` | `distribuidoras.view` |
| PATCH | `/distribuidoras/{id}` | `distribuidoras.update` |
| DELETE | `/distribuidoras/{id}` | `distribuidoras.delete` |

### Usinas

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/usinas` | `usinas.view` |
| POST | `/usinas` | `usinas.create` |
| GET | `/usinas/{id}` | `usinas.view` |
| PATCH | `/usinas/{id}` | `usinas.update` |
| DELETE | `/usinas/{id}` | `usinas.delete` |

### Unidades Consumidoras

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/ucs` | `ucs.view` |
| POST | `/ucs` | `ucs.create` |
| GET | `/ucs/{id}` | `ucs.view` |
| PATCH | `/ucs/{id}` | `ucs.update` |
| DELETE | `/ucs/{id}` | `ucs.delete` |

### Contratos

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/contratos` | `contratos.view` |
| POST | `/contratos` | `contratos.create` |
| GET | `/contratos/{id}` | `contratos.view` |
| PATCH | `/contratos/{id}` | `contratos.update` |
| DELETE | `/contratos/{id}` | `contratos.delete` |

### Faturas Distribuidora

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/faturas` | `faturas.view` |
| POST | `/faturas` | `faturas.create` |
| GET | `/faturas/{id}` | `faturas.view` |
| PATCH | `/faturas/{id}` | `faturas.update` |
| DELETE | `/faturas/{id}` | `faturas.delete` |
| POST | `/faturas/import/preview` | `faturas.create` |
| POST | `/faturas/import/confirm` | `faturas.create` |
| GET | `/faturas/export/csv` | `faturas.view` |
| GET | `/faturas/export/xlsx` | `faturas.view` |

### Créditos de Energia

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/creditos` | `creditos.view` |
| POST | `/creditos` | `creditos.create` |
| GET | `/creditos/{id}` | `creditos.view` |
| PATCH | `/creditos/{id}` | `creditos.update` |
| DELETE | `/creditos/{id}` | `creditos.delete` |

## Otimizador

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| POST | `/otimizador/simular` | `otimizador.simulate` |
| POST | `/otimizador/executar` | `otimizador.execute` |

## Faturamento

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/faturamento` | `faturamento.view` |
| POST | `/faturamento/gerar` | `faturamento.create` |
| POST | `/faturamento/gerar-periodo` | `faturamento.create` |
| POST | `/faturamento/bulk` | `faturamento.update` |
| GET | `/faturamento/{id}` | `faturamento.view` |
| PATCH | `/faturamento/{id}` | `faturamento.update` |
| DELETE | `/faturamento/{id}` | `faturamento.delete` |
| GET | `/faturamento/{id}/pdf` | `faturamento.view` |

## Relatórios

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| POST | `/relatorios/financeiro` | `relatorios.view` |
| GET | `/relatorios/financeiro/export/csv` | `relatorios.view` |
| GET | `/relatorios/financeiro/export/xlsx` | `relatorios.view` |
| GET | `/relatorios/financeiro/export/pdf` | `relatorios.view` |

## Dashboard

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/dashboard` | `dashboard.view` |
| GET | `/dashboard/charts` | `dashboard.view` |

## Usuários

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/users` | `users.view` |
| POST | `/users` | `users.create` |
| GET | `/users/{id}` | `users.view` |
| PATCH | `/users/{id}` | `users.update` |
| DELETE | `/users/{id}` | `users.delete` |

## Notificações

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| POST | `/notificacoes/gerar` | `notificacoes.create` |
| GET | `/notificacoes` | `notificacoes.view` |
| GET | `/notificacoes/resumo` | `notificacoes.view` |
| PATCH | `/notificacoes/{id}/lida` | `notificacoes.update` |
| POST | `/notificacoes/marcar-todas` | `notificacoes.update` |
| POST | `/notificacoes/bulk/lida` | `notificacoes.update` |

## Audit Log

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/audit-log` | `audit.view` |
| GET | `/audit-log/{type}/{id}` | `audit.view` |

## Configurações

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/settings` | `settings.view` |
| PATCH | `/settings` | `settings.update` |
| POST | `/settings/test-email` | `settings.update` |

## Tarefas

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/tasks` | `tasks.view` |
| POST | `/tasks/run/{task_name}` | `tasks.execute` |

## Portal do Cliente

Acesso read-only para titulares de UC via magic link (JWT portal).

| Método | Endpoint | Descrição | Auth |
|--------|----------|-----------|------|
| POST | `/portal/auth/exchange` | Trocar token por JWT portal | Token |
| GET | `/portal/me` | Dados da UC logada | JWT Portal |
| GET | `/portal/dashboard` | Dashboard da UC | JWT Portal |
| GET | `/portal/faturas` | Faturas da distribuidora | JWT Portal |
| GET | `/portal/faturas-internas` | Faturas internas (cobrança) | JWT Portal |
| GET | `/portal/faturas-internas/{id}/pdf` | PDF da fatura interna | JWT Portal |
| GET | `/portal/creditos` | Créditos de energia | JWT Portal |
| GET | `/portal/contrato` | Contrato ativo | JWT Portal |

### Admin — Tokens de Portal

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| POST | `/ucs/{uc_id}/portal-token` | `ucs.update` |
| DELETE | `/ucs/{uc_id}/portal-token` | `ucs.update` |

## Simulador

| Método | Endpoint | Descrição | Auth |
|--------|----------|-----------|------|
| POST | `/simulador/simular` | Simular economia GD (12 meses) | Público |

## Integrações

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/integracoes` | `settings.view` |

## Geração (Monitoramento)

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/geracao` | `geracao.view` |
| POST | `/geracao` | `geracao.create` |
| GET | `/geracao/{id}` | `geracao.view` |
| PATCH | `/geracao/{id}` | `geracao.update` |
| DELETE | `/geracao/{id}` | `geracao.delete` |

## Aprovações (Workflow)

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/aprovacoes` | `aprovacoes.view` |
| POST | `/aprovacoes` | `aprovacoes.create` |
| GET | `/aprovacoes/{id}` | `aprovacoes.view` |
| PATCH | `/aprovacoes/{id}` | `aprovacoes.update` |
| DELETE | `/aprovacoes/{id}` | `aprovacoes.delete` |
| POST | `/aprovacoes/{id}/review` | `aprovacoes.update` |

## Push Notifications

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| POST | `/push/tokens` | JWT (qualquer) |
| GET | `/push/tokens` | JWT (qualquer) |
| DELETE | `/push/tokens/{token}` | JWT (qualquer) |
| POST | `/push/send` | `notificacoes.manage` |

## Marketplace

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/marketplace/offers` | `marketplace.view` |
| POST | `/marketplace/simulate` | `marketplace.view` |

## Compliance ANEEL

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/compliance/check` | `compliance.view` |
| GET | `/compliance/quotas` | `compliance.view` |
| GET | `/compliance/credits-expiration` | `compliance.view` |

## Webhooks

| Método | Endpoint | Permissão |
|--------|----------|-----------|
| GET | `/webhooks` | `settings.view` |
| POST | `/webhooks` | `settings.update` |
| GET | `/webhooks/{id}` | `settings.view` |
| PATCH | `/webhooks/{id}` | `settings.update` |
| DELETE | `/webhooks/{id}` | `settings.update` |
