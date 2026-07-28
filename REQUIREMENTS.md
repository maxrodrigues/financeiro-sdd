# Requirements Document: App de Gestão Financeira Pessoal / Familiar

## 1. Visão Geral do Produto
O aplicativo é uma solução **WebApp** voltada para o controle financeiro pessoal e familiar, operando sob o modelo **local-first com sincronização**. O objetivo principal é proporcionar ao usuário visibilidade total sobre suas receitas, despesas, saldos e cartões de crédito, permitindo a organização necessária para a quitação de dívidas e a criação de patrimônio para investimentos.

---

## 2. Objetivos Principais (Problem & Impact)
* **Prevenção do Endividamento:** Garantir que o usuário saiba com precisão o valor acumulado de dívidas, faturas a vencer e compromissos futuros.
* **Planejamento de Investimentos:** Ajudar no rastreamento de sobras financeiras mensais para transição de um perfil endividado/equilibrado para investidor.
* **Privacidade e Acessibilidade:** Priorizar a posse local dos dados com sincronização ágil e segura em múltiplos dispositivos da família.

---

## 3. Escopo do MVP (Minimum Viable Product)

### 3.1. Módulo: Gestão de Contas e Saldos
* **[REQ-CNT-01]** O sistema deve permitir o cadastro de múltiplas contas (ex.: Conta Corrente, Conta Poupança, Carteira Física, Conta Investimento).
* **[REQ-CNT-02]** O sistema deve exibir o saldo individual de cada conta e calcular o **Saldo Total Consolidado**.
* **[REQ-CNT-03]** O sistema deve permitir a transferência entre contas internas sem impactar o relatório de receitas/despesas globais.

### 3.2. Módulo: Lançamentos e Categorização
* **[REQ-LAN-01]** O sistema deve permitir o registro manual de lançamentos do tipo **Receita** e **Despesa**.
* **[REQ-LAN-02]** O sistema deve permitir associar lançamentos a uma **Conta**, **Data**, **Valor**, **Status** (Pendente/Pago) e **Categoria**.
* **[REQ-LAN-03]** O sistema deve permitir o gerenciamento de **Categorias** e **Subcategorias** customizáveis pelo usuário (com padrões pré-definidos para iniciantes).
* **[REQ-LAN-04]** O sistema deve permitir a criação de lançamentos recorrentes ou fixos (ex.: Salário, Aluguel).

### 3.3. Módulo: Cartões de Crédito
* **[REQ-CRT-01]** O sistema deve permitir o cadastro de cartões de crédito definindo **Limite Total**, **Dia de Fechamento** e **Dia de Vencimento**.
* **[REQ-CRT-02]** O sistema deve permitir o registro de compras parceladas, calculando e distribuindo automaticamente as parcelas pelas faturas dos meses subsequentes.
* **[REQ-CRT-03]** O sistema deve calcular o limite disponível do cartão em tempo real com base nos lançamentos futuros não pagos.
* **[REQ-CRT-04]** O sistema deve disponibilizar o registro do **Pagamento da Fatura**, abatendo o valor da conta selecionada e liberando o limite correspondente.

---

## 4. Requisitos Não-Funcionais (RNF)

* **[RNF-ARCH-01] Arquitetura Local-First:** Os dados devem ser gravados prioritariamente no armazenamento local do navegador (IndexDB/SQLite Wasm ou similar) para permitir funcionamento offline fluído.
* **[RNF-ARCH-02] Sincronização:** Os dados locais devem ser sincronizados com a nuvem/servidor de forma transparente quando houver conexão disponível, garantindo a consistência entre dispositivos familiares.
* **[RNF-PERF-01] Desempenho:** A interface WebApp deve responder de forma instantânea a novos lançamentos (tempo de resposta < 100ms para gravações locais).
* **[RNF-SEC-01] Segurança e Privacidade:** Dados financeiros locais e transmitidos devem ser criptografados.

---

## 5. Fora do Escopo Inicial (Futuro / Pós-MVP)
* Importação automática de extratos via Open Finance / APIs Bancárias.
* Leitura de comprovantes via OCR / Câmera.
* Módulo dedicado de Mapeamento/Gráficos Avançados de Investimentos.
* Gestão avançada de permissões e relatórios por membro da família (no MVP, o controle familiar compartilha o mesmo espaço/workspace).

---

## 6. Premissas e Dependências (Assumptions)
* O usuário final possui um navegador moderno compatível com armazenamento local seguro (IndexedDB/WebSockets/Service Workers).
* A sincronização entre dispositivos assumirá um modelo de resolução de conflitos simples (ex.: *Last-Write-Wins* ou sincronização append-only de eventos no MVP).
