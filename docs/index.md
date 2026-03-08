# EnerSync

**Plataforma SaaS para Gestão de Geração Distribuída (GD) de Energia**

Desenvolvido pelo **Grupo JLM**, o EnerSync é uma solução completa para gestão de créditos de energia, otimização de alocação e faturamento de serviços de geração distribuída no mercado brasileiro.

---

## Visão Geral

O EnerSync permite que empresas de gestão de GD:

- **Cadastrem** usinas, unidades consumidoras, contratos e distribuidoras
- **Importem** faturas de distribuidoras (CSV/Excel)
- **Otimizem** a alocação de créditos de energia com algoritmo FIFO
- **Faturem** seus clientes com base na economia gerada
- **Monitorem** indicadores via dashboard com gráficos
- **Notifiquem** sobre créditos expirando, faturas atrasadas e contratos vencendo
- **Exportem** relatórios em CSV, Excel e PDF
- **Auditem** todas as operações com trail completo

---

## Stack Tecnológica

| Camada | Tecnologias |
|--------|-------------|
| **Backend** | Python 3.12 · FastAPI · SQLAlchemy 2.0 (async) · Alembic |
| **Frontend** | React 19 · Vite 7 · TypeScript · TanStack Router/Query/Table · shadcn/ui · Tailwind CSS v4 |
| **Mobile** | Flutter 3.38 · Dart 3.10 · Riverpod · GoRouter · Dio |
| **Banco** | PostgreSQL 16 com Row-Level Security (multi-tenancy) |
| **Infra** | Docker · docker-compose · GitHub Actions CI |
| **Observabilidade** | structlog (JSON) · Sentry SDK · Request ID |

---

## Repositórios

| Repositório | Descrição |
|-------------|-----------|
| [enersync-api](https://github.com/GrupoJLM/enersync-api) | Backend FastAPI — 179 testes, 25 fases completas |
| [enersync-web](https://github.com/GrupoJLM/enersync-web) | Frontend React 19 — 16 páginas, i18n, bulk actions |
| [enersync-mobile](https://github.com/GrupoJLM/enersync-mobile) | App Flutter — Login + Dashboard |

---

## Números

- :test_tube: **179 testes** backend passando
- :shield: **44 permissões** RBAC (3 roles)
- :rocket: **25 fases** de desenvolvimento concluídas
- :earth_americas: **i18n** pt-BR e en-US
- :page_facing_up: **16 páginas** no frontend web

---

## Quick Start

=== "Backend"

    ```bash
    cd enersync-api
    uv sync --all-extras
    docker compose up -d postgres
    uv run alembic upgrade head
    uv run uvicorn energy_saas.main:app --reload
    ```

=== "Frontend"

    ```bash
    cd enersync-web
    nvm use 22
    npm install
    npm run dev
    ```

=== "Mobile"

    ```bash
    cd enersync-mobile
    flutter pub get
    dart run build_runner build --delete-conflicting-outputs
    flutter run
    ```
