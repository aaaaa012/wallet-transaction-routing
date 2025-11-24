# wallet-transaction-routing

##Overview

This project demonstrates the **end-to-end routing and processing of wallet transactions** (e.g., IME Pay, Prabhu Pay) through multiple route modules. The system supports multiple routes with pre-configured source banks. Transactions are routed and processed based on the wallet’s configuration and routing setup.

## Routing and Processing Logic
1. Transaction Creation
   - Send Partner Initiates a transaction.
2. Routing Decision
   - Wallet routed to Route A -> processed via Route A Module
   - Wallet routed to Route B -> Processed via Route B Module
   - Wallet not routed anywhere -> transaction does not move forward ->stuck
3. Route Module Processing
    - Check Configured Source Bank
        - No active bank -> error /stuck
    - Account Validation
        - Failed -> Stuck
        - Passed -> Proceed
    - Send Transaction
        - Success -> Paid
        - Failure -> Processing
4. Transaction Status
    - Can be checked after the API Processing
    - Status: (Success:debit 000 /credit 000/) (Failure: debit 999 / credit 999)
  
