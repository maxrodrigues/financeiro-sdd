# Technical & Functional Specification Document (`SPEC.md`)
**Projeto:** App de Gestão Financeira Pessoal / Familiar  
**Status:** Aprovado  
**Dependência:** `REQUIREMENTS.md` v1.0  

---

## 1. Arquitetura do Sistema & Stack Tecnológica

### 1.1. Pilha Tecnológica (Tech Stack)
* **Frontend Web:** Vue.js 3 (Composition API com TypeScript) + Vite.
* **Gerenciamento de Estado:** Pinia.js.
* **Estilização / UI:** Tailwind CSS + Shadcn Vue.
* **Persistência Local (Browser):** IndexedDB encapsulado via **Dexie.js**.
* **Identificador Único (PKs):** **ULID** (Universally Unique Lexicographically Sortable Identifier) gerado no cliente.
* **Backend (API & Sincronização):** PHP 8.3+ com **Laravel**.
* **Banco de Dados Central (Nuvem):** PostgreSQL (colunas de ID do tipo `CHAR(26)` ou `VARCHAR(26)`).
* **Comunicação Cliente-Servidor:** REST API / JSON com suporte a rotas de sincronização em lote.

### 1.2. Estratégia Local-First & Sincronização
1. Toda operação (Criar/Editar/Deletar) grava diretamente no **Dexie.js** local no navegador do usuário usando ULIDs ordenáveis no tempo e atualiza a UI via **Pinia**.
2. Cada alteração adiciona uma entrada na fila de mutações pendentes (`sync_queue`).
3. Quando a conexão com a internet estiver ativa, um worker envia a `sync_queue` para o **Laravel**, que persiste no **PostgreSQL**.
4. Conflitos de alteração no mesmo registro são resolvidos via *Last-Write-Wins (LWW)* baseado no atributo `updatedAt` (ISO 8601 UTC).

---

## 2. Modelagem de Dados (Data Schemas em TypeScript)

```typescript
// Tipo primitivo para identificadores do sistema
type ULID = string; // Ex: "01J2K3M4N5P6Q7R8S9T0U1V2W3"

// 2.1. Entidade: Account (Conta)
interface Account {
  id: ULID;
  name: string;
  type: 'CHECKING' | 'SAVINGS' | 'INVESTMENT' | 'CASH';
  initialBalance: number; // Em centavos (ex: R$ 10,50 -> 1050)
  color?: string;
  createdAt: string;
  updatedAt: string;
  deletedAt?: string | null;
}

// 2.2. Entidade: Category (Categoria)
interface Category {
  id: ULID;
  parentId?: ULID | null;
  name: string;
  type: 'INCOME' | 'EXPENSE';
  icon?: string;
  color?: string;
  createdAt: string;
  updatedAt: string;
  deletedAt?: string | null;
}

// 2.3. Entidade: Transaction (Lançamento / Transação)
interface Transaction {
  id: ULID;
  type: 'INCOME' | 'EXPENSE' | 'TRANSFER';
  amount: number; // Em centavos
  date: string; // YYYY-MM-DD
  description: string;
  status: 'PENDING' | 'PAID';
  accountId: ULID;
  destinationAccountId?: ULID | null;
  categoryId?: ULID | null;
  creditCardInvoiceId?: ULID | null;
  installmentIndex?: number | null;
  totalInstallments?: number | null;
  parentTransactionId?: ULID | null;
  createdAt: string;
  updatedAt: string;
  deletedAt?: string | null;
}

// 2.4. Entidade: CreditCard (Cartão de Crédito)
interface CreditCard {
  id: ULID;
  name: string;
  creditLimit: number; // Em centavos
  closingDay: number;
  dueDay: number;
  defaultAccountId: ULID;
  createdAt: string;
  updatedAt: string;
  deletedAt?: string | null;
}

// 2.5. Entidade: CreditCardInvoice (Fatura do Cartão)
interface CreditCardInvoice {
  id: ULID;
  creditCardId: ULID;
  month: number; // 1 - 12
  year: number; // YYYY
  status: 'OPEN' | 'CLOSED' | 'PAID';
  paymentTransactionId?: ULID | null;
  createdAt: string;
  updatedAt: string;
}
