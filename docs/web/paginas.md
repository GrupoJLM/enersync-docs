# Frontend — Páginas

## Rotas

O EnerSync Web utiliza **TanStack Router** com file-based routing.

### Públicas

| Rota | Arquivo | Descrição |
|------|---------|-----------|
| `/login` | `login.tsx` | Login com email/senha |
| `/registro` | `registro.tsx` | Registro de novo tenant (onboarding) |

### Protegidas (requerem autenticação)

| Rota | Arquivo | Descrição |
|------|---------|-----------|
| `/` | `index.tsx` | Dashboard — 6 KPI cards + 3 gráficos Recharts |
| `/usinas` | `usinas.tsx` | CRUD de usinas |
| `/ucs` | `ucs.tsx` | CRUD de unidades consumidoras |
| `/distribuidoras` | `distribuidoras.tsx` | CRUD de distribuidoras |
| `/contratos` | `contratos.tsx` | CRUD de contratos (filtros: usina, UC, status) |
| `/faturas` | `faturas.tsx` | CRUD + importação CSV/XLSX + exportação |
| `/creditos` | `creditos.tsx` | CRUD de créditos (filtros: usina, status, mês) |
| `/usuarios` | `usuarios.tsx` | CRUD de usuários (role management) |
| `/otimizador` | `otimizador.tsx` | Simular + executar alocação de créditos |
| `/faturamento` | `faturamento.tsx` | CRUD + gerar período + PDF + bulk actions |
| `/relatorios` | `relatorios.tsx` | Resumo financeiro + export CSV/XLSX/PDF |
| `/notificacoes` | `notificacoes.tsx` | Lista + marcar lida + filtros + bulk actions |
| `/audit` | `audit.tsx` | Tabela de audit log com filtros |
| `/configuracoes` | `configuracoes.tsx` | 3 tabs: Empresa, Faturamento, Email |
| `/tarefas` | `tarefas.tsx` | 3 cards de tarefas automatizáveis |

## Dashboard

O dashboard (`/`) exibe:

**KPI Cards** (6):

- Créditos por status + próximos de expirar
- Faturamento: economia gerada, pagamentos, atrasos
- Contagem de usinas, UCs, contratos ativos
- Notificações não lidas

**Gráficos** (3):

- **AreaChart**: Consumo mensal (últimos 12 meses)
- **BarChart**: Faturamento mensal
- **PieChart**: Distribuição de créditos por status

## Importação de Faturas

Fluxo em 2 etapas na página `/faturas`:

1. **Upload + Análise**: Upload de CSV/XLSX → preview com validação
2. **Confirmação**: Tabela com status ok/erro por linha → confirmar import

Formatos aceitos: CSV (delimitador `;` ou `,`), Excel (.xlsx). Suporta datas BR (`DD/MM/YYYY`, `MM/YYYY`) e vírgula como separador decimal.

## Bulk Actions

Disponíveis em:

- **Faturamento**: marcar como pago, cancelar (checkbox na DataTable)
- **Notificações**: marcar como lida (checkbox nos cards)

Componente `BulkActionBar` aparece fixo no bottom quando há itens selecionados.
