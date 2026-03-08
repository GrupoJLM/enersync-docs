---
tags:
  - Frontend
  - Setup
---

# Frontend — Setup

## Pré-requisitos

- Node.js 22+ (via nvm)
- npm

!!! warning "Node.js 22 obrigatório"
    Vite 7 requer Node `^20.19.0 || >=22.12.0`. Node 18 **não** funciona.

## Instalação

```bash
cd enersync-web

# Usar Node 22
nvm use 22

# Instalar dependências
npm install

# Iniciar dev server (porta 3000, proxy /api → localhost:8000)
npm run dev
```

## Build

```bash
npm run build
```

## Lint

```bash
npm run lint
```

## Docker

```bash
# Build (multi-stage: Node 22 build → Nginx serve)
docker build -t enersync-web .

# Run
docker run -p 3000:80 enersync-web
```

## Variáveis de Ambiente

| Variável | Default | Descrição |
|----------|---------|-----------|
| `VITE_API_TARGET` | `http://localhost:8000` | URL da API para proxy em dev |
