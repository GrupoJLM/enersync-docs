---
tags:
  - Frontend
  - Padrões
---

# Frontend — Padrões de Código

## Hooks (TanStack Query)

- 1 arquivo por entidade em `src/hooks/use-xxx.ts`
- Padrão: `useXxx(page, pageSize, search)` → `useQuery` com `PaginatedResponse<T>`
- QueryKey: `['xxx', page, pageSize, search]` para listas
- `placeholderData: keepPreviousData` para transições suaves
- Todas mutations fazem `invalidateQueries` no `onSuccess`

```typescript
export function useUsinas(page = 1, pageSize = 20, search = '') {
  const skip = (page - 1) * pageSize
  return useQuery({
    queryKey: ['usinas', page, pageSize, search],
    queryFn: () => api.get('/usinas', { params: { skip, limit: pageSize, search } }),
    placeholderData: keepPreviousData,
  })
}
```

## Páginas CRUD

Cada página segue o padrão:

1. `useState` para `page`, `search`, `dialogOpen`, `deleteOpen`, `editing`
2. `useForm` com `zodResolver` para validação
3. `DataTable` com paginação server-side
4. `Dialog` para create/edit
5. `ConfirmDialog` para exclusão
6. `toast` (sonner) para feedback

## Componentes Reutilizáveis

| Componente | Descrição |
|------------|-----------|
| `DataTable<T>` | Genérico, recebe columns + data + paginação + seleção |
| `ConfirmDialog` | Confirmação de exclusão |
| `PageHeader` | Título + descrição + slot para botões |
| `BulkActionBar` | Barra flutuante com ações em lote |

## API Client

`src/lib/api.ts` — Axios com interceptors:

- **Request**: attach Bearer token de localStorage
- **Response**: 401 → refresh queue pattern (evita múltiplos refreshes simultâneos)

## Autenticação

`src/lib/auth.tsx` — AuthProvider:

- Login envia JSON com `{ email, password }`
- Tokens em localStorage (`access_token`, `refresh_token`)
- Route guard em `_authenticated.tsx` com `beforeLoad → redirect /login`

## Types

`src/lib/types.ts` espelha **todos** os schemas Python:

- Enums como union types (`'solar' | 'wind' | ...`)
- Label maps como `Record<EnumType, string>` para traduzir na UI

## Bulk Actions

- DataTable aceita `selectable`, `selectedIds`, `onSelectionChange`
- `BulkActionBar` mostra contagem + botões + limpar seleção
- Pattern: `useState` com `Set<string>` para IDs selecionados

## i18n (react-intl)

- `IntlProvider` no topo do app (antes de QueryClient e Auth)
- Locale persistido em `localStorage`
- Traduções em `lib/i18n/pt-BR.json` e `lib/i18n/en-US.json`
- Uso: `intl.formatMessage({ id: 'chave' })`
- Seletor de idioma no header (Globe icon)
