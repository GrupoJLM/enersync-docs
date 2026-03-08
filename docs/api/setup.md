---
tags:
  - Backend
  - Setup
---

# Backend — Setup

## Pré-requisitos

- Python 3.12+
- [uv](https://docs.astral.sh/uv/) (gerenciador de pacotes)
- Docker (para PostgreSQL)

## Instalação

```bash
cd enersync-api

# Instalar dependências
uv sync --all-extras

# Subir PostgreSQL
docker compose up -d postgres

# Rodar migrations
uv run alembic upgrade head

# Iniciar servidor dev
uv run uvicorn energy_saas.main:app --reload
```

!!! tip "uv no PATH"
    Se `uv` não for encontrado, adicione ao PATH:
    ```bash
    export PATH="$HOME/.local/bin:$PATH"
    ```

## Testes

```bash
# Subir banco de testes (porta 5434)
docker run -d --name energy_test_db \
  -e POSTGRES_USER=energy \
  -e POSTGRES_PASSWORD=energy123 \
  -e POSTGRES_DB=energy_saas_test \
  -p 5434:5432 \
  postgres:16-alpine

# Rodar testes
DATABASE_URL="postgresql+asyncpg://energy:energy123@localhost:5434/energy_saas_test" \
  uv run pytest -v
```

## Lint

```bash
uv run ruff check src/ tests/
uv run ruff format --check src/ tests/
```

## Docker

=== "Desenvolvimento"

    ```bash
    docker compose up -d
    ```

=== "Produção"

    ```bash
    docker compose -f docker-compose.prod.yml --env-file .env.prod up -d --build
    ```

## Variáveis de Ambiente

| Variável | Default | Descrição |
|----------|---------|-----------|
| `DATABASE_URL` | — | Connection string PostgreSQL (asyncpg) |
| `SECRET_KEY` | — | Chave para assinatura JWT |
| `SENTRY_DSN` | — | DSN do Sentry (opcional) |
| `LOG_LEVEL` | `INFO` | Nível de log (DEBUG, INFO, WARNING, ERROR) |
| `LOG_FORMAT` | `json` | Formato do log (`json` ou `console`) |

Veja `.env.example` para o template completo.
