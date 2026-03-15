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

## Dark Mode (next-themes)

- `ThemeProvider` envolve toda a app em `main.tsx` (`attribute="class"`, `defaultTheme="light"`)
- Toggle no header com ícones Sun/Moon (transição CSS com rotate/scale)
- `useTheme()` para acessar/alterar tema
- Sonner (toasts) já integrado com next-themes

## ErrorBoundary

- Class component React em `error-boundary.tsx`
- Envolve `<Outlet />` no `_authenticated.tsx`
- Fallback styled com ícone AlertTriangle + botão "Tentar novamente"
- Captura erros de render sem derrubar toda a página

## StatCards

- Componente reutilizável para KPIs em `stat-cards.tsx`
- Aceita array de `{ title, value, icon, trend? }`
- Grid responsivo (2-4 colunas)

## DataTable — DebouncedInput

- Input com debounce de 300ms para busca server-side
- Usa `useRef` para timer e callback (evita re-renders desnecessários)
- `onChangeRef.current` atualizado via `useEffect` (requisito React Compiler)

## Sidebar

- Colapsável com estado persistido
- Ícones Lucide em cada item
- Tooltip com label quando colapsada

## Header Breadcrumbs

- `useMatches()` do TanStack Router para extrair rota atual
- Map de segmento → label em `ROUTE_LABELS`
- Último item sem link (rota atual), anteriores clicáveis

## Guided Tour

- `react-joyride` para onboarding interativo
- Steps definidos em `lib/tour.ts`
- Estado gerenciado em `hooks/use-tour.ts`
- `.npmrc` com `legacy-peer-deps=true` por incompatibilidade React 19

## Code Splitting

- Recharts extraído via `manualChunks` no `vite.config.ts`
- Dashboard chunk: 505KB → 110KB (recharts em chunk separado de 409KB)
- Lazy routes via TanStack Router (`*.lazy.tsx`)

## Dashboard Drag & Drop (@dnd-kit)

KPI cards são reordenáveis via `@dnd-kit/sortable`. A ordem é persistida em `localStorage` via `useDashboardLayout` hook. Cada card tem um `GripVertical` drag handle. Botão "Resetar layout" restaura a ordem padrão.

## i18n Completo (react-intl)

350+ chaves de tradução em `src/lib/i18n/pt-BR.json` e `en-US.json`. Organização por domínio:
- `common.*` — botões, labels genéricos
- `dashboard.*` — KPIs, gráficos, saudações
- `{entidade}.*` — CRUD completo (title, description, create, edit, delete, errors, toasts)
- `auth.*`, `settings.*`, `tasks.*` — funcionalidades transversais

Sidebar e header já usam `useIntl()`. Páginas podem migrar incrementalmente.
