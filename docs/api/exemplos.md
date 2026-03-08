---
tags:
  - Backend
  - API
---

# Backend — Exemplos de Request/Response

Exemplos práticos para cada grupo de endpoints da API EnerSync.

Todos os endpoints autenticados requerem o header:

```
Authorization: Bearer <access_token>
```

---

## Autenticação

### Login

```bash
curl -X POST /api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "admin@empresa.com",
    "password": "senha123456"
  }'
```

??? success "Response 200"

    ```json
    {
      "access_token": "eyJhbGciOiJIUzI1NiIs...",
      "refresh_token": "eyJhbGciOiJIUzI1NiIs...",
      "token_type": "bearer"
    }
    ```

### Registro de Novo Tenant

```bash
curl -X POST /api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "tenant_name": "Solar Energy LTDA",
    "tenant_slug": "solar-energy",
    "admin_full_name": "João Silva",
    "admin_email": "joao@solarenergy.com.br",
    "admin_password": "senhaSegura123"
  }'
```

??? success "Response 201"

    ```json
    {
      "access_token": "eyJhbGciOiJIUzI1NiIs...",
      "refresh_token": "eyJhbGciOiJIUzI1NiIs...",
      "token_type": "bearer"
    }
    ```

??? failure "Response 422 — Slug já existe"

    ```json
    {
      "detail": "Tenant com slug 'solar-energy' já existe"
    }
    ```

### Refresh Token

```bash
curl -X POST /api/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{
    "refresh_token": "eyJhbGciOiJIUzI1NiIs..."
  }'
```

### Dados do Usuário Logado

```bash
curl -X GET /api/auth/me \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "email": "admin@empresa.com",
      "full_name": "João Silva",
      "role": "ADMIN",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

---

## Distribuidoras

### Criar

```bash
curl -X POST /api/distribuidoras \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "CEMIG Distribuição S.A.",
    "code": "CEMIG-D",
    "state": "MG"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
      "name": "CEMIG Distribuição S.A.",
      "code": "CEMIG-D",
      "state": "MG",
      "is_active": true,
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

### Listar (paginado)

```bash
curl -X GET "/api/distribuidoras?skip=0&limit=20&search=cemig" \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "items": [
        {
          "id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
          "name": "CEMIG Distribuição S.A.",
          "code": "CEMIG-D",
          "state": "MG",
          "is_active": true,
          "created_at": "2026-01-15T10:30:00Z"
        }
      ],
      "total": 1,
      "page": 1,
      "page_size": 20
    }
    ```

---

## Usinas

### Criar

```bash
curl -X POST /api/usinas \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Solar Fazenda Boa Vista",
    "usina_type": "solar",
    "installed_capacity_kwp": 500.00,
    "uc_number": "UC-001-USINA",
    "address": "Fazenda Boa Vista, km 12",
    "city": "Uberlândia",
    "state": "MG",
    "distribuidora_id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
    "status": "active",
    "commissioning_date": "2025-06-01"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "u1s2i3n4-a567-890a-bcde-f12345678901",
      "name": "Solar Fazenda Boa Vista",
      "usina_type": "solar",
      "installed_capacity_kwp": "500.00",
      "uc_number": "UC-001-USINA",
      "address": "Fazenda Boa Vista, km 12",
      "city": "Uberlândia",
      "state": "MG",
      "distribuidora_id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
      "status": "active",
      "commissioning_date": "2025-06-01",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

### Atualizar

```bash
curl -X PATCH /api/usinas/u1s2i3n4-a567-890a-bcde-f12345678901 \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "installed_capacity_kwp": 750.00,
    "status": "active"
  }'
```

---

## Unidades Consumidoras

### Criar

```bash
curl -X POST /api/ucs \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "uc_number": "3004567890",
    "holder_name": "Supermercado Mineirão LTDA",
    "address": "Av. Brasil, 1500",
    "city": "Belo Horizonte",
    "state": "MG",
    "connection_type": "trifasico",
    "tariff_group": "B3",
    "distribuidora_id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
    "status": "active"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "uc123456-7890-abcd-ef12-345678901234",
      "uc_number": "3004567890",
      "holder_name": "Supermercado Mineirão LTDA",
      "address": "Av. Brasil, 1500",
      "city": "Belo Horizonte",
      "state": "MG",
      "connection_type": "trifasico",
      "tariff_group": "B3",
      "distribuidora_id": "d1e2f3a4-5678-90ab-cdef-123456789abc",
      "status": "active",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

---

## Contratos

### Criar

```bash
curl -X POST /api/contratos \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901",
    "uc_id": "uc123456-7890-abcd-ef12-345678901234",
    "quota_percentage": 35.00,
    "service_fee_percentage": 15.00,
    "start_date": "2026-01-01",
    "end_date": "2028-12-31",
    "status": "active"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "ct123456-7890-abcd-ef12-345678901234",
      "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901",
      "uc_id": "uc123456-7890-abcd-ef12-345678901234",
      "quota_percentage": "35.00",
      "service_fee_percentage": "15.00",
      "start_date": "2026-01-01",
      "end_date": "2028-12-31",
      "status": "active",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

??? failure "Response 422 — Quota excede 100%"

    ```json
    {
      "detail": "Soma das quotas da usina excede 100% (atual: 80.00%, solicitado: 35.00%)"
    }
    ```

### Listar com Filtros

```bash
curl -X GET "/api/contratos?skip=0&limit=20&usina_id=<uuid>&status=active" \
  -H "Authorization: Bearer <token>"
```

---

## Faturas Distribuidora

### Criar

```bash
curl -X POST /api/faturas \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "uc_id": "uc123456-7890-abcd-ef12-345678901234",
    "reference_month": "2026-01-01",
    "consumption_kwh": 1500.00,
    "injected_kwh": 0,
    "credits_received_kwh": 800.00,
    "credits_compensated_kwh": 750.00,
    "credits_balance_kwh": 50.00,
    "bill_amount_brl": 450.75,
    "due_date": "2026-02-15",
    "status": "pending"
  }'
```

### Importar CSV/XLSX — Preview

```bash
curl -X POST /api/faturas/import/preview \
  -H "Authorization: Bearer <token>" \
  -F "file=@faturas_janeiro.csv"
```

??? success "Response 200"

    ```json
    {
      "valid_rows": [
        {
          "row_number": 1,
          "uc_number": "3004567890",
          "uc_id": "uc123456-7890-abcd-ef12-345678901234",
          "reference_month": "01/2026",
          "consumption_kwh": "1500.00",
          "bill_amount_brl": "450.75",
          "due_date": "15/02/2026",
          "status": "ok"
        }
      ],
      "error_rows": [
        {
          "row_number": 2,
          "uc_number": "9999999999",
          "uc_id": null,
          "reference_month": "01/2026",
          "consumption_kwh": "800.00",
          "bill_amount_brl": "250.30",
          "due_date": "15/02/2026",
          "status": "UC não encontrada"
        }
      ],
      "total": 2
    }
    ```

### Importar CSV/XLSX — Confirmar

```bash
curl -X POST /api/faturas/import/confirm \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "rows": [
      {
        "uc_id": "uc123456-7890-abcd-ef12-345678901234",
        "reference_month": "2026-01-01",
        "consumption_kwh": 1500.00,
        "bill_amount_brl": 450.75,
        "due_date": "2026-02-15"
      }
    ]
  }'
```

??? success "Response 200"

    ```json
    {
      "created": 1,
      "skipped": 0
    }
    ```

### Exportar

```bash
# CSV
curl -X GET "/api/faturas/export/csv?uc_id=<uuid>&status=pending" \
  -H "Authorization: Bearer <token>" -O -J

# XLSX
curl -X GET "/api/faturas/export/xlsx" \
  -H "Authorization: Bearer <token>" -O -J
```

---

## Créditos de Energia

### Criar

```bash
curl -X POST /api/creditos \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901",
    "amount_kwh": 1200.00,
    "reference_month": "2026-01-01",
    "status": "available"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "cr123456-7890-abcd-ef12-345678901234",
      "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901",
      "uc_id": null,
      "amount_kwh": "1200.00",
      "reference_month": "2026-01-01",
      "expiration_date": "2031-01-01",
      "status": "available",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

!!! info "Expiração automática"
    O campo `expiration_date` é calculado automaticamente: `reference_month + 60 meses`.

---

## Otimizador

### Simular Alocação

```bash
curl -X POST /api/otimizador/simular \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901"
  }'
```

??? success "Response 200"

    ```json
    {
      "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901",
      "usina_name": "Solar Fazenda Boa Vista",
      "reference_date": "2026-03-08T14:00:00Z",
      "total_credits_kwh": "3500.00",
      "total_allocated_kwh": "2800.00",
      "total_unallocated_kwh": "700.00",
      "credits_expiring_30d": 2,
      "credits_expiring_60d": 5,
      "estimated_total_savings_brl": "2100.00",
      "resumo_por_uc": [
        {
          "uc_id": "uc123456-7890-abcd-ef12-345678901234",
          "uc_number": "3004567890",
          "holder_name": "Supermercado Mineirão LTDA",
          "quota_percentage": "35.00",
          "consumption_kwh": "1500.00",
          "credits_allocated_kwh": "1200.00",
          "estimated_savings_brl": "900.00",
          "tarifa_media_brl": "0.75"
        },
        {
          "uc_id": "uc789012-3456-abcd-ef12-345678901234",
          "uc_number": "3009876543",
          "holder_name": "Padaria Central LTDA",
          "quota_percentage": "25.00",
          "consumption_kwh": "800.00",
          "credits_allocated_kwh": "800.00",
          "estimated_savings_brl": "560.00",
          "tarifa_media_brl": "0.70"
        }
      ],
      "items": [
        {
          "credito_id": "cr123456-7890-abcd-ef12-345678901234",
          "uc_id": "uc123456-7890-abcd-ef12-345678901234",
          "amount_kwh": "1200.00",
          "expiration_date": "2026-04-01"
        }
      ]
    }
    ```

### Executar Alocação

```bash
curl -X POST /api/otimizador/executar \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "usina_id": "u1s2i3n4-a567-890a-bcde-f12345678901"
  }'
```

!!! info "Mesma response que simular"
    A diferença é que `executar` **persiste** as alocações no banco (atualiza status dos créditos para `allocated` e vincula `uc_id`).

---

## Faturamento

### Gerar Fatura Individual

```bash
curl -X POST /api/faturamento/gerar \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "contrato_id": "ct123456-7890-abcd-ef12-345678901234",
    "fatura_distribuidora_id": "ft123456-7890-abcd-ef12-345678901234"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "fi123456-7890-abcd-ef12-345678901234",
      "contrato_id": "ct123456-7890-abcd-ef12-345678901234",
      "fatura_distribuidora_id": "ft123456-7890-abcd-ef12-345678901234",
      "reference_month": "2026-01-01",
      "economia_kwh": "750.00",
      "economia_brl": "562.50",
      "tarifa_media_brl": "0.75",
      "percentual_cobranca": "15.00",
      "valor_cobranca_brl": "84.38",
      "due_date": "2026-03-02",
      "payment_date": null,
      "status": "pending",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

### Gerar por Período (Bulk)

```bash
curl -X POST /api/faturamento/gerar-periodo \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "reference_month": "2026-01-01"
  }'
```

### Ações em Lote

```bash
curl -X POST /api/faturamento/bulk \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "ids": [
      "fi123456-7890-abcd-ef12-345678901234",
      "fi789012-3456-abcd-ef12-345678901234"
    ],
    "action": "mark_paid"
  }'
```

??? success "Response 200"

    ```json
    {
      "affected": 2,
      "errors": []
    }
    ```

### Download PDF

```bash
curl -X GET /api/faturamento/fi123456-7890-abcd-ef12-345678901234/pdf \
  -H "Authorization: Bearer <token>" -O -J
```

---

## Relatórios

### Resumo Financeiro

```bash
curl -X POST /api/relatorios/financeiro \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "periodo_inicio": "2026-01-01",
    "periodo_fim": "2026-03-31"
  }'
```

??? success "Response 200"

    ```json
    {
      "periodo_inicio": "2026-01-01",
      "periodo_fim": "2026-03-31",
      "total_economia_brl": "4500.00",
      "total_faturado_brl": "675.00",
      "total_pago_brl": "450.00",
      "total_em_atraso_brl": "225.00",
      "qtd_faturas": 9,
      "resumo_por_uc": [
        {
          "uc_id": "uc123456-7890-abcd-ef12-345678901234",
          "uc_number": "3004567890",
          "holder_name": "Supermercado Mineirão LTDA",
          "total_economia_brl": "2700.00",
          "total_faturado_brl": "405.00",
          "total_pago_brl": "270.00",
          "total_em_atraso_brl": "135.00",
          "qtd_faturas": 5
        }
      ]
    }
    ```

### Exportar Relatório

```bash
# CSV, XLSX ou PDF
curl -X GET "/api/relatorios/financeiro/export/pdf?periodo_inicio=2026-01-01&periodo_fim=2026-03-31" \
  -H "Authorization: Bearer <token>" -O -J
```

---

## Dashboard

### KPIs

```bash
curl -X GET /api/dashboard \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "creditos": {
        "total_disponivel_kwh": "5000.00",
        "total_alocado_kwh": "3200.00",
        "total_usado_kwh": "1800.00",
        "total_expirado_kwh": "200.00",
        "expirando_30d_count": 3,
        "expirando_30d_kwh": "450.00",
        "expirando_60d_count": 7,
        "expirando_60d_kwh": "1100.00"
      },
      "faturamento": {
        "total_economia_brl": "12500.00",
        "total_faturado_brl": "1875.00",
        "total_pago_brl": "1500.00",
        "total_em_atraso_brl": "375.00",
        "qtd_pendentes": 5,
        "qtd_pagas": 20,
        "qtd_atrasadas": 3,
        "qtd_canceladas": 1
      },
      "usinas": {
        "total_ativas": 4,
        "capacidade_instalada_kwp": "2500.00"
      },
      "ucs": {
        "total_ativas": 12
      },
      "contratos": {
        "total_ativos": 15,
        "total_suspensos": 2,
        "vencendo_30d": 1,
        "vencendo_60d": 3
      },
      "notificacoes": {
        "total_nao_lidas": 8,
        "por_tipo": {
          "credito_expirando": 3,
          "fatura_atrasada": 2,
          "contrato_vencendo": 3
        }
      }
    }
    ```

### Gráficos

```bash
curl -X GET "/api/dashboard/charts?months=12" \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "consumo_mensal": [
        { "month": "2025-04", "value1": "12500.00", "value2": "0" },
        { "month": "2025-05", "value1": "13200.00", "value2": "0" }
      ],
      "faturamento_mensal": [
        { "month": "2025-04", "value1": "562.50", "value2": "450.00" },
        { "month": "2025-05", "value1": "600.00", "value2": "600.00" }
      ],
      "creditos_distribuicao": [
        { "status": "available", "kwh": "5000.00" },
        { "status": "allocated", "kwh": "3200.00" },
        { "status": "used", "kwh": "1800.00" },
        { "status": "expired", "kwh": "200.00" }
      ]
    }
    ```

---

## Usuários

### Criar

```bash
curl -X POST /api/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "maria@empresa.com",
    "password": "senhaSegura456",
    "full_name": "Maria Santos",
    "role": "MANAGER"
  }'
```

??? success "Response 201"

    ```json
    {
      "id": "us123456-7890-abcd-ef12-345678901234",
      "email": "maria@empresa.com",
      "full_name": "Maria Santos",
      "role": "MANAGER",
      "is_active": true,
      "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
      "created_at": "2026-01-15T10:30:00Z"
    }
    ```

---

## Notificações

### Gerar

```bash
curl -X POST /api/notificacoes/gerar \
  -H "Authorization: Bearer <token>"
```

### Listar

```bash
curl -X GET "/api/notificacoes?skip=0&limit=20&is_read=false&notification_type=credito_expirando" \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "items": [
        {
          "id": "nt123456-7890-abcd-ef12-345678901234",
          "tenant_id": "f1e2d3c4-b5a6-7890-fedc-ba0987654321",
          "notification_type": "credito_expirando",
          "priority": "high",
          "title": "Crédito expirando em 30 dias",
          "message": "O crédito de 1200 kWh da usina Solar Fazenda Boa Vista expira em 15/04/2026",
          "entity_type": "credito",
          "entity_id": "cr123456-7890-abcd-ef12-345678901234",
          "is_read": false,
          "read_at": null,
          "created_at": "2026-03-08T10:00:00Z"
        }
      ],
      "total": 1,
      "page": 1,
      "page_size": 20
    }
    ```

### Resumo

```bash
curl -X GET /api/notificacoes/resumo \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "total_nao_lidas": 8,
      "por_tipo": {
        "credito_expirando": 3,
        "fatura_atrasada": 2,
        "contrato_vencendo": 3
      }
    }
    ```

### Marcar como Lida (Bulk)

```bash
curl -X POST /api/notificacoes/bulk/lida \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "ids": [
      "nt123456-7890-abcd-ef12-345678901234",
      "nt789012-3456-abcd-ef12-345678901234"
    ]
  }'
```

---

## Configurações do Tenant

### Obter

```bash
curl -X GET /api/settings \
  -H "Authorization: Bearer <token>"
```

??? success "Response 200"

    ```json
    {
      "company_name": "Solar Energy LTDA",
      "company_cnpj": "12.345.678/0001-90",
      "company_address": "Av. Afonso Pena, 1000, Belo Horizonte/MG",
      "company_phone": "(31) 3333-4444",
      "company_email": "contato@solarenergy.com.br",
      "company_logo_url": null,
      "default_service_fee": "15.00",
      "notification_email_enabled": true,
      "smtp_host": "smtp.gmail.com",
      "smtp_port": 587,
      "smtp_user": "noreply@solarenergy.com.br",
      "smtp_password_set": true,
      "smtp_from_email": "noreply@solarenergy.com.br"
    }
    ```

!!! warning "SMTP password"
    O campo `smtp_password` **nunca** é retornado na response. Em vez disso, `smtp_password_set: true/false` indica se uma senha está configurada.

### Atualizar

```bash
curl -X PATCH /api/settings \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{
    "company_name": "Solar Energy LTDA - Atualizado",
    "default_service_fee": 18.50,
    "smtp_password": "novasenha123"
  }'
```

### Testar Email

```bash
curl -X POST /api/settings/test-email \
  -H "Authorization: Bearer <token>"
```

---

## Tarefas Agendadas

### Listar

```bash
curl -X GET /api/tasks \
  -H "Authorization: Bearer <token>"
```

### Executar

```bash
curl -X POST /api/tasks/run/expire_credits \
  -H "Authorization: Bearer <token>"
```

Tarefas disponíveis: `expire_credits`, `mark_overdue_invoices`, `generate_all_notifications`

---

## Health Check

```bash
curl -X GET /api/health
```

??? success "Response 200"

    ```json
    {
      "status": "healthy",
      "version": "1.0.0",
      "uptime_seconds": 86400,
      "db_status": "connected"
    }
    ```

---

## Erros Comuns

| Status | Significado | Exemplo |
|--------|-------------|---------|
| `401` | Token inválido ou expirado | `{"detail": "Could not validate credentials"}` |
| `403` | Sem permissão | `{"detail": "Permission denied: usinas.delete"}` |
| `404` | Recurso não encontrado | `{"detail": "Usina not found"}` |
| `422` | Erro de validação/negócio | `{"detail": "Soma das quotas excede 100%"}` |
