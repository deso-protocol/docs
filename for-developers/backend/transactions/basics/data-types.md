---
description: Description of common data types that appear in the Transaction endpoints
---

# Data Types

## TransactionFee

`TransactionFees` are additional transaction outputs.  These additional outputs are a way for both node operators (who can specify additional fees for all transactions of a certain type on their node) and app developers (who can specify additional fees when make a request to construct a transaction).

```json5
{
  "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user who will receive the additional output,
  "ProfileEntryResponse": <ProfileEntryResponse>, // This is only provided when TranssactionFees are retrieved through admin endpoints when managing node-leve transaction fees
  "AmountNanos": 10000, // The amount of DeSo in nanos this user should receive
}
```

For reference, `TransactionFee` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/admin\_fees.go#L16).
