---
tags:
  - Backend
  - Padrões
---

# Backend — Padrões de Código

## Models (SQLAlchemy 2.0)

- Herdam de `TenantMixin, Base` (tenant-scoped) ou apenas `Base` (shared)
- `TenantMixin` fornece: `tenant_id` (FK), `created_at`, `updated_at`
- PK: `UUID(as_uuid=True)`, default `uuid.uuid4`
- Enums: `enum.StrEnum` (nunca `str, enum.Enum` — ruff UP042)
- Soft-delete: campo `is_active: bool = True`
- Todos os enums centralizados em `models/enums.py`

```python
class Usina(TenantMixin, Base):
    __tablename__ = "usinas"

    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    name = Column(String, nullable=False)
    type = Column(SQLEnum(UsinaType), nullable=False)
    status = Column(SQLEnum(UsinaStatus), default=UsinaStatus.active)
    is_active = Column(Boolean, default=True)
```

## Schemas (Pydantic v2)

- Padrão: `Read` (from_attributes=True), `Create` (campos obrigatórios), `Update` (todos opcionais)
- Decimais como `Decimal` com `Field(max_digits=..., decimal_places=...)`
- UUIDs como `uuid.UUID`
- `PaginatedResponse[T]` genérico com items, total, page, page_size

```python
class PaginatedResponse[T](BaseModel):
    items: list[T]
    total: int
    page: int
    page_size: int
```

## Services

- Funções async standalone: `async def create_xxx(session, tenant_id, data)`
- Recebem `session: AsyncSession` + `tenant_id: UUID` (quando tenant-scoped)
- **Sem classe, sem state** — funções puras
- Retornam modelos SQLAlchemy, não schemas
- Regras de negócio ficam aqui
- `list_xxx()` retorna `tuple[list[Model], int]` (items + total count)

```python
async def list_usinas(
    session: AsyncSession,
    tenant_id: UUID,
    skip: int = 0,
    limit: int = 20,
    search: str | None = None,
) -> tuple[list[Usina], int]:
    ...
```

## Routers

- `APIRouter(prefix="/xxx", tags=["Xxx"])`
- Usam `DB` e `CurrentUser` (Annotated aliases)
- `tenant_id` extraído de `current_user.tenant_id`
- List endpoints: `response_model=PaginatedResponse[XxxRead]`
- Erros de negócio: `ValueError` no service → catch no router → `HTTPException(422)`

```python
@router.get("/", response_model=PaginatedResponse[UsinaRead])
async def list_usinas(
    db: DB,
    current_user: CurrentUser,
    skip: int = Query(default=0, ge=0),
    limit: int = Query(default=20, ge=1, le=100),
    search: str | None = None,
):
    items, total = await usina_svc.list_usinas(db, current_user.tenant_id, skip, limit, search)
    return PaginatedResponse(items=items, total=total, page=skip // limit + 1, page_size=limit)
```

## Dependencies

```python
DB = Annotated[AsyncSession, Depends(get_db)]
CurrentUser = Annotated[User, Depends(get_current_user)]
```

- `get_current_user` faz `SET LOCAL` RLS via f-string (asyncpg não suporta bind params em SET)
- `require_permission("codename")` — dependency para RBAC

## Lint (Ruff)

- Target: `py312`, line-length: `100`
- Rules: `E, F, W, I, N, UP, B, A, SIM, TCH`

```bash
uv run ruff check src/ tests/ && uv run ruff format --check src/ tests/
```
