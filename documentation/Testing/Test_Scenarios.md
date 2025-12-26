# Routing System Test Scenarios

The following test cases validate the routing logic, API connectivity, and status reporting mechanisms.

| Module | Test Case | Scenario Description | Endpoint | Method | Expected Status |
|:---|:---|:---|:---|:---|:---:|
| **Onboarding** | T001 | **Valid Transfer**: Source Bank is set, Account details are valid. | `/V0/Transfer` | POST | `0` (Success) |
| **Discovery** | T002 | **List Agents**: Fetch all available wallet providers. | `/V0/GetWalletList` | POST | `0` (Success) |
| **Discovery** | T003 | **List Stress Test**: High volume fetch for wallet list. | `/V0/GetWalletList` | POST | `0` (Success) |
| **Validation** | T004 | **Account Valid**: Check existence of a registered mobile number. | `/V0/Validation` | POST | `0` (Success) |
| **Validation** | T005 | **Account Invalid**: Check unregistered mobile number. | `/V0/Validation` | POST | `1` (Failure) |
| **Execution** | T006 | **Send Transaction**: Commit a fund transfer to a valid wallet. | `/V0/Transfer` | POST | `0` (Success) |
| **Monitoring** | T007 | **Status Check**: Verify final status of a transaction ID. | `/V0/CheckTransactionStatus` | POST | `0` (Success) |

## Response Code Legend
- `0`: **Success** / Approved
- `1`: **Failure** / Rejected / Validation Error
- `999`: **System Error** (Not in table, but standard)
