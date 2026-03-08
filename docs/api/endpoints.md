# Backend — Endpoints

## Visão Geral

A API possui **17 routers** com **46+ endpoints**, todos sob o prefixo `/api`.

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
