---
tags:
  - Arquitetura
  - Backend
  - Frontend
  - DevOps
---

# Arquitetura — Visão Geral

## Diagrama de Componentes

```mermaid
graph TB
    subgraph Cliente
        WEB[React SPA<br/>Porta 3000]
        MOB[Flutter App]
    end

    subgraph Servidor
        NGX[Nginx<br/>Reverse Proxy<br/>Porta 80]
        API[FastAPI<br/>Porta 8000]
        SCHED[APScheduler<br/>Cron Jobs]
    end

    subgraph Dados
        PG[(PostgreSQL 16<br/>RLS Multi-tenant)]
    end

    subgraph Externo
        SMTP[SMTP Server<br/>Email por tenant]
        SENTRY[Sentry<br/>Error Tracking]
    end

    WEB --> NGX
    MOB --> NGX
    NGX -->|/api/*| API
    NGX -->|/*| WEB
    API --> PG
    API --> SMTP
    API --> SENTRY
    SCHED --> PG
```

## Multi-tenancy com RLS

O isolamento de dados é feito no nível do banco via **Row-Level Security** do PostgreSQL:

```sql
CREATE POLICY tenant_isolation ON usinas
    USING (tenant_id = current_setting('app.current_tenant')::uuid);
```

A cada request, o middleware de tenant:

1. Extrai o `tenant_id` do JWT
2. Executa `SET LOCAL app.current_tenant = '{tenant_id}'`
3. Todas as queries ficam automaticamente filtradas

!!! warning "asyncpg e SET LOCAL"
    asyncpg **não** suporta bind params em comandos SET. A solução é validar o UUID antes e usar f-string.

## Autenticação

```mermaid
sequenceDiagram
    participant C as Cliente
    participant A as API
    participant DB as PostgreSQL

    C->>A: POST /auth/login {email, password}
    A->>DB: Buscar user por email
    A->>A: bcrypt.checkpw(password, hash)
    A-->>C: {access_token, refresh_token}
    C->>A: GET /api/* (Bearer token)
    A->>A: Decode JWT → user_id, tenant_id
    A->>DB: SET LOCAL app.current_tenant
    A->>DB: Query com RLS
    A-->>C: Response
```

- **JWT**: python-jose com algoritmo HS256
- **Hash**: bcrypt direto (sem passlib)
- **Refresh**: Token de longa duração para renovar o access token
- **RBAC**: 44 permissões distribuídas em 3 roles (ADMIN, MANAGER, VIEWER)

## Observabilidade

| Componente | Tecnologia | Descrição |
|------------|------------|-----------|
| **Logs** | structlog | JSON em prod, console em dev |
| **Request ID** | Middleware | UUID por request, header `X-Request-ID` |
| **Access Log** | Middleware | method, path, status, duration_ms |
| **Error Tracking** | Sentry SDK | Opcional, via `SENTRY_DSN` |
| **Health Check** | `/health` | DB status, version, uptime |
| **Audit Trail** | AuditLog model | Quem + o quê + quando (todas entidades) |
