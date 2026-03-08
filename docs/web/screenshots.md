---
tags:
  - Frontend
---

# Frontend — Screenshots

!!! info "Screenshots pendentes"
    Para capturar os screenshots, execute o frontend localmente e tire prints de cada página.
    Salve os arquivos em `docs/assets/screenshots/` e atualize os caminhos abaixo.

    ```bash
    cd enersync-web && nvm use 22 && npm run dev
    # Acesse http://localhost:3000
    ```

## Páginas

### Login
<!-- ![Login](../assets/screenshots/login.png) -->
*Página de login com email e senha. Link para registro de novo tenant.*

### Dashboard
<!-- ![Dashboard](../assets/screenshots/dashboard.png) -->
*6 KPI cards (créditos, faturamento, usinas, UCs, contratos, notificações) + 3 gráficos Recharts (consumo mensal, faturamento mensal, distribuição de créditos).*

### Usinas
<!-- ![Usinas](../assets/screenshots/usinas.png) -->
*DataTable com paginação server-side. Dialog para criar/editar com formulário validado (Zod). Campos: nome, tipo, capacidade, UC, distribuidora, status.*

### Unidades Consumidoras
<!-- ![UCs](../assets/screenshots/ucs.png) -->
*DataTable com busca por número UC e titular. Campos: UC number, titular, endereço, tipo de conexão, grupo tarifário, distribuidora.*

### Distribuidoras
<!-- ![Distribuidoras](../assets/screenshots/distribuidoras.png) -->
*CRUD de distribuidoras (dados compartilhados). Campos: nome, código ANEEL, UF.*

### Contratos
<!-- ![Contratos](../assets/screenshots/contratos.png) -->
*DataTable com filtros (usina, UC, status). Campos: usina, UC, quota %, service fee %, data início/fim, status. Validação de quota ≤ 100%.*

### Faturas
<!-- ![Faturas](../assets/screenshots/faturas.png) -->
*DataTable com filtros (UC, status, mês de/até). Botões de importação CSV/XLSX e exportação CSV/XLSX. Dialog de importação em 2 etapas (upload → preview → confirmar).*

### Créditos
<!-- ![Créditos](../assets/screenshots/creditos.png) -->
*DataTable com filtros (usina, status, mês de/até). Badge de status colorido (available=verde, allocated=azul, used=cinza, expired=vermelho).*

### Otimizador
<!-- ![Otimizador](../assets/screenshots/otimizador.png) -->
*Seletor de usina + botões Simular/Executar. Resultado: tabela de UCs com créditos alocados, economia estimada. Alertas de créditos expirando.*

### Faturamento
<!-- ![Faturamento](../assets/screenshots/faturamento.png) -->
*DataTable com checkbox selection + BulkActionBar. Filtros (status, mês de/até). Ações: gerar individual, gerar por período, download PDF, bulk mark paid/cancel.*

### Relatórios
<!-- ![Relatórios](../assets/screenshots/relatorios.png) -->
*Formulário de período (início/fim) + botão gerar. Tabela de resumo por UC. Botões de exportação: CSV, XLSX, PDF.*

### Notificações
<!-- ![Notificações](../assets/screenshots/notificacoes.png) -->
*Cards de notificação com badges de prioridade. Checkbox selection + BulkActionBar para marcar lidas em lote. Filtros por tipo e status de leitura.*

### Audit Log
<!-- ![Audit](../assets/screenshots/audit.png) -->
*Tabela de eventos com filtros (entidade, ação, período). Mostra quem fez o quê e quando.*

### Configurações
<!-- ![Configurações](../assets/screenshots/configuracoes.png) -->
*3 tabs: Empresa (nome, CNPJ, endereço, telefone, email), Faturamento (service fee padrão), Email (SMTP host/port/user/password, teste de envio).*

### Tarefas
<!-- ![Tarefas](../assets/screenshots/tarefas.png) -->
*3 cards: Expirar créditos, Marcar faturas atrasadas, Gerar notificações. Botão executar em cada card (ADMIN only).*

### Usuários
<!-- ![Usuários](../assets/screenshots/usuarios.png) -->
*DataTable com busca por nome/email. Dialog para criar/editar com seleção de role (ADMIN, MANAGER, VIEWER). Soft-delete.*

### Registro
<!-- ![Registro](../assets/screenshots/registro.png) -->
*Formulário de onboarding: nome da empresa, slug, nome do admin, email, senha. Link para login.*

---

## Como Adicionar Screenshots

1. Capture as telas com resolução **1920×1080** (ou o tamanho que preferir)
2. Salve em `docs/assets/screenshots/` com os nomes acima
3. Descomente as linhas `![...](...)` neste arquivo
4. Commit e push — o deploy é automático

```bash
mkdir -p docs/assets/screenshots
# Capture os prints e salve aqui
```
