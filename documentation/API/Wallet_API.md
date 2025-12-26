# Wallet Routing API Reference

**Version:** 0.9 (Beta)  
**Base URL:** `https://api.routing-engine.internal/V0`

---

## 1. Get Wallet List
Retreives a list of supported wallet providers and their current status.

**Endpoint:** `POST /GetWalletList`  

### Response
```json
{
  "code": 0,
  "message": "Success",
  "wallets": [
    { "id": "IME_PAY", "name": "IME Pay", "status": "ACTIVE" },
    { "id": "KHALTI", "name": "Khalti", "status": "ACTIVE" },
    { "id": "PRABHU", "name": "Prabhu Pay", "status": "MAINTENANCE" }
  ]
}
```

---

## 2. Validation
Verifies if a mobile number is registered with the specified wallet provider.

**Endpoint:** `POST /Validation`

### Request
```json
{
  "walletId": "IME_PAY",
  "mobileNumber": "9800000000"
}
```

### Response (Success)
```json
{
  "code": 0,
  "message": "Valid Account",
  "accountName": "John Doe"
}
```

### Response (Failure)
```json
{
  "code": 1,
  "message": "Account Not Found"
}
```

---

## 3. Transfer Funds
Initiates a transaction. The system will automatically route this to Route A or Route B based on config.

**Endpoint:** `POST /Transfer`

### Request
```json
{
  "walletId": "IME_PAY",
  "mobileNumber": "9800000000",
  "amount": 500,
  "refId": "PARTNER_REF_123"
}
```

### Response
```json
{
  "code": 0,
  "message": "Transaction Processed",
  "txnId": "SYS_10001",
  "routeUsed": "ROUTE_A"
}
```

---

## 4. Check Status
Poll for the final status of a transaction.

**Endpoint:** `POST /CheckTransactionStatus`

### Request
```json
{
  "txnId": "SYS_10001"
}
```

### Response
```json
{
  "code": 0,
  "status": "PAID",
  "details": "Credit Successful"
}
```
