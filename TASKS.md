```markdown
# Task Implementation Roadmap (`TASKS.md`)
**Projeto:** App de Gestão Financeira Pessoal / Familiar  
**Status:** Em Execução  

---

## Fase 1: Setup da Infraestrutura e Frontend Base
- [ ] **TASK-1.1:** Inicializar projeto Vue 3 + TypeScript + Vite com Tailwind CSS e Shadcn Vue.
- [ ] **TASK-1.2:** Configurar o Dexie.js (IndexedDB) no Frontend com os schemas das entidades (`Account`, `Category`, `Transaction`, `CreditCard`, `CreditCardInvoice`) e tabela de `sync_queue`.
- [ ] **TASK-1.3:** Configurar utilitário de geração de ULID (`ulid`) e integrar aos métodos de inserção do Dexie.
- [ ] **TASK-1.4:** Implementar script/seeder de inicialização de categorias padrão no primeiro acesso do usuário.

## Fase 2: Stores Pinia & Módulos Core (Frontend Local-First)
- [ ] **TASK-2.1 (Contas):** Criar a Store `useAccountStore` (CRUD local no Dexie + cálculo do saldo total acumulado).
- [ ] **TASK-2.2 (Categorias):** Criar a Store `useCategoryStore` (CRUD de categorias e subcategorias).
- [ ] **TASK-2.3 (Lançamentos):** Criar a Store `useTransactionStore` para registro de Receitas, Despesas e Transferências.
- [ ] **TASK-2.4 (Cartões):** Criar a Store `useCreditCardStore` com lógica de fechamento de fatura, limite disponível e geração automática de parcelas atreladas às faturas.

## Fase 3: Interface do Usuário (UI / Views)
- [ ] **TASK-3.1:** Construir a Dashboard principal (Exibição de saldo consolidado, atalhos rápidos e resumo das últimas transações).
- [ ] **TASK-3.2:** Construir a tela de Gestão de Contas (Listagem, criação e edição de saldos).
- [ ] **TASK-3.3:** Construir o formulário/modal de Novo Lançamento (com suporte a parcelamento, categorias e seleção de conta/cartão).
- [ ] **TASK-3.4:** Construir a visão de Cartões de Crédito e Faturas (Visualização do limite, detalhamento da fatura aberta/fechada e ação de pagamento da fatura).

## Fase 4: Backend Laravel & Sincronização
- [ ] **TASK-4.1:** Inicializar projeto Laravel com PostgreSQL e criar Migrations com colunas ULID (`CHAR(26)`).
- [ ] **TASK-4.2:** Criar os Models e Endpoints REST de Sincronização em Batch (`/api/v1/sync`).
- [ ] **TASK-4.3:** Criar o mecanismo de resolução de conflitos no Laravel usando a estratégia *Last-Write-Wins (LWW)*.
- [ ] **TASK-4.4:** Criar o Worker/Service de Sincronização no Frontend Vue para drenar a `sync_queue` local quando a conexão com a internet estiver online.
