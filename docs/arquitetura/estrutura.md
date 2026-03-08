# Estrutura de Arquivos

## Backend (`enersync-api/`)

```
energy-saas/
├── .github/workflows/ci.yml
├── infra/docker/
│   ├── docker-compose.yml              # DEV: postgres + api (hot-reload)
│   ├── docker-compose.prod.yml         # PROD: postgres + api + web (health checks)
│   ├── .env.example
│   └── .env.prod.example
├── apps/api/
│   ├── pyproject.toml                  # uv + hatchling
│   ├── ruff.toml                       # py312, line-length=100
│   ├── Dockerfile                      # python:3.12-slim + uv
│   ├── entrypoint.sh                   # alembic upgrade + uvicorn
│   ├── alembic/
│   │   └── versions/                   # 9 migrations (0001-0009)
│   └── src/energy_saas/
│       ├── config.py                   # Settings (pydantic-settings)
│       ├── database.py                 # AsyncEngine + sessionmaker
│       ├── dependencies.py             # get_db, get_current_user, require_permission
│       ├── main.py                     # App + CORS + 4 middlewares + 17 routers
│       ├── lib/i18n.py                 # t() para tradução backend
│       ├── middleware/
│       │   ├── tenant.py               # JWT → tenant_id → SET LOCAL
│       │   ├── request_id.py           # UUID por request
│       │   ├── access_log.py           # Logging de acesso
│       │   └── locale.py              # Accept-Language → Content-Language
│       ├── templates/email/            # 4 templates Jinja2
│       ├── models/                     # 14 modelos SQLAlchemy
│       ├── schemas/                    # Pydantic v2 (Read/Create/Update)
│       ├── services/                   # 20+ services (funções async)
│       └── routers/                    # 17 routers FastAPI
└── tests/                             # 179 testes (23 arquivos)
```

## Frontend (`enersync-web/`)

```
apps/web/
├── package.json                        # React 19, Vite 7, TanStack
├── vite.config.ts                      # proxy /api→:8000, TanStack Router plugin
├── Dockerfile                          # node:22 build → nginx serve
├── nginx.conf                          # gzip + proxy + SPA fallback
└── src/
    ├── main.tsx                         # IntlProvider + QueryClient + Auth + Router
    ├── globals.css                      # Tailwind v4 + shadcn theme
    ├── lib/
    │   ├── api.ts                       # Axios + JWT interceptors
    │   ├── auth.tsx                     # AuthProvider + useAuth()
    │   ├── types.ts                     # TS types espelhando schemas Python
    │   ├── utils.ts                     # cn, formatCurrency, formatDate, etc.
    │   └── i18n/                        # react-intl (pt-BR/en-US)
    ├── hooks/                           # 16 hooks TanStack Query
    ├── components/
    │   ├── ui/                          # 12 shadcn components
    │   ├── layout/                      # app-layout, sidebar, header
    │   ├── data-table.tsx               # Genérico com paginação + seleção
    │   ├── confirm-dialog.tsx
    │   ├── page-header.tsx
    │   └── bulk-action-bar.tsx          # Barra flutuante para ações em lote
    └── routes/
        ├── __root.tsx
        ├── login.tsx
        ├── registro.tsx                 # Onboarding
        ├── _authenticated.tsx           # Auth guard
        └── _authenticated/              # 16 páginas protegidas
```

## Mobile (`enersync-mobile/`)

```
enersync-mobile/
├── pubspec.yaml                         # Flutter 3.38, Riverpod, GoRouter, Dio
├── lib/
│   ├── main.dart
│   ├── core/
│   │   ├── config/                      # API config, theme
│   │   ├── network/                     # Dio client + JWT interceptor
│   │   └── storage/                     # Flutter Secure Storage
│   ├── features/
│   │   ├── auth/                        # Login + providers
│   │   └── dashboard/                   # Dashboard screen
│   └── routes/                          # GoRouter config
└── test/
```
