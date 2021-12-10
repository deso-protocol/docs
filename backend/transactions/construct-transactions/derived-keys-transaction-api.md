---
description: >-
  Description of endpoints used to construct Derived Key Transactions on the
  DeSo blockchain
---

# Derived Keys Transaction API

## Authorize Derived Key

```
POST /api/v0/authorize-derived-key
```

Create an authorize derived key transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Authorize derived key transactions allows another public key to submit transaction on behalf of the owner public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L2210).

**Parameters**

| Name                        | Type               | Description                                                                                               | Required | Restrictions |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| OwnerPublicKeyBase58Check   | string             | Public key of the derived key owner                                                                       | y        |              |
| DerivedPublicKeyBase58Check | string             | The derived public key                                                                                    | y        |              |
| ExpirationBlock             | uint64             | Height of block after which this derived key will no longer work.                                         | y        |              |
| AccessSignature             | string             | The signature of hash(derived key + expiration block) made by the owner.                                  | y        |              |
| DeleteKey                   | bool               | The intended operation on the derived key. If true, this derived key will no longer be valid.             | y        |              |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y        |              |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |              |

**Response**

```json5
{
  "TotalInputNanos": 49999779,
  "ChangeAmountNanos": 49999558,
  "FeeNanos": 221,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 49999558
      }
    ],
    "TxnMeta": {
      "DerivedPublicKey": [1,2,3, ...], // Bytes of the Derived public key 
      "ExpirationBlock": 91273 // Block height at which this derived key expires
      "AccessSignature": [23, 34, 45, ...], // Bytes access signature. This is the signed hash of (derivedPublicKey + expirationBlock) made with the ownerPublicKey. Signature is in DER format.
      "OperationType": 1, // 1 means this transaction makes the derived public key valid. 0 means this transaction makes the derived public key invalid.
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 22
  },
  "TransactionHex": "0161b49620c72975d8397836c6b28981a0257d846e28ee74b296264bf1e2109036000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45c6ddeb17152167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
