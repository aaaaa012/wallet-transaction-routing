# Transaction Routing Logic Requirement

**Module:** Routing Engine  
**Document Owner:** Business Analyst  

---

## 1. Overview
The Routing Engine is responsible for determining the optimal path for a wallet transaction. Unlike a static direct integration, this system supports multiple downstream providers ("Module A" and "Module B"). The engine makes real-time decisions based on configuration to ensure higher availability.

---

## 2. Core Routing Rules

### 2.1 Route Selection
**Rule**: Every Wallet Provider (e.g., Khalti, IME Pay) must be mapped to **exactly one** active Route Module in the configuration database.

- **IF** Wallet ID is mapped to `Route A` → Send to **Module A**.
- **IF** Wallet ID is mapped to `Route B` → Send to **Module B**.
- **IF** Wallet ID is NOT mapped → **Fail Fast** (Status: `STUCK/ERROR`).

### 2.2 Source Bank Check
**Rule**: A Route Module cannot process a transaction without a funded Source Bank.

- **Check**: Before calling the module's API, the system must query `GetActiveSourceBank(RouteID)`.
- **Logic**:
  - If `ActiveSourceBank` is NULL/Empty → **Block Transaction**.
  - Reason: We cannot debit funds if there is no bank linked.

---

## 3. Transaction Lifecycle States

| State | Description | Retryable? |
|:---|:---|:---:|
| `INITIATED` | Request received from Partner. | N/A |
| `ROUTING_CHECK` | System is looking up the route config. | No |
| `VALIDATING` | Checking Account Validity at Destination. | No |
| `PROCESSING` | Debit sent to Bank, waiting for Wallet Credit. | Yes |
| `PAID` | Success (Debit 000 / Credit 000). | No |
| `STUCK` | Configuration Logic Failure (No Route/No Bank). | Manual Fix |
| `ERROR` | Technical Failure (Debit 999). | Depends |

---

## 4. Edge Cases

### 4.1 Dual Routing Conflict
**Scenario**: A wallet is historically configured for *both* Route A and Route B in legacy data.
**Resolution**: The system must respect the **Status** flag. Only the route marked `ACTIVE` for that wallet ID will be used. If both are active, default to **Route A** (Priority).

### 4.2 Orphaned Wallet
**Scenario**: A new wallet is added to the system but not assigned a route.
**Resolution**: Transaction status becomes `STUCK`. The Operations team receives a "Missing Route Config" alert.
