# Backend — Testes

## Visão Geral

O backend possui **179 testes** distribuídos em 23 arquivos, cobrindo todas as funcionalidades.

## Executar

```bash
DATABASE_URL="postgresql+asyncpg://energy:energy123@localhost:5434/energy_saas_test" \
  uv run pytest -v
```

## Distribuição

| Arquivo | Testes | Cobertura |
|---------|--------|-----------|
| `test_health.py` | 1 | Health check (DB, version, uptime) |
| `test_auth.py` | 6 | Login, refresh, me |
| `test_distribuidora.py` | 7 | CRUD distribuidoras |
| `test_usina.py` | 5 | CRUD usinas |
| `test_uc.py` | 5 | CRUD UCs |
| `test_contrato.py` | 9 | CRUD + quota > 100% + filtros |
| `test_fatura.py` | 7 | CRUD + filtros |
| `test_credito.py` | 7 | CRUD + filtros |
| `test_otimizador.py` | 9 | FIFO, quota, consumo, savings, erros, idempotência |
| `test_faturamento.py` | 11 | Gerar, período, duplicata, pagamento, cancelar, relatório |
| `test_audit.py` | 8 | Create/update/delete audit, filtros, histórico |
| `test_notificacoes.py` | 10 | 3 tipos, duplicata, prioridade, lida, filtros |
| `test_dashboard.py` | 10 | 7 KPIs + 3 charts |
| `test_export.py` | 6 | CSV/XLSX faturas + relatório |
| `test_import_faturas.py` | 10 | CSV/XLSX preview, confirm, duplicata |
| `test_permissions.py` | 10 | Viewer/manager denied, RBAC |
| `test_users.py` | 13 | CRUD, duplicata, self-protect |
| `test_settings.py` | 6 | Get, update, SMTP masked |
| `test_tasks.py` | 8 | Expire, overdue, notifications, run |
| `test_email.py` | 5 | SMTP mock, template, erro |
| `test_pdf.py` | 4 | Fatura PDF, relatório PDF |
| `test_onboarding.py` | 7 | Registro, validações, isolamento |
| `test_observability.py` | 3 | Health, request ID |
| `test_bulk.py` | 6 | Mark paid, cancel, partial |
| `test_i18n.py` | 6 | pt-BR, en-US, fallback |

## Fixtures (`conftest.py`)

```
setup_db → engine → db_session
                   → tenant → user → auth_token → auth_client
                            → viewer_user → viewer_client
                            → manager_user → manager_client
                            → distribuidora → usina → uc → contrato
                   → _seed_permissions (autouse)
```

## Padrões de Teste

- **pytest-asyncio** com `mode=auto` (sem decorador `@pytest.mark.asyncio`)
- Engine criado por fixture (function scope), não module-level
- `drop_all` ANTES de `create_all` no setup_db
- Sessões separadas: fixture usa uma, dependency override usa outra factory
- Test DB: container Docker na porta **5434**
