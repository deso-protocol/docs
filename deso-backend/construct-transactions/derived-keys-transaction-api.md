---
description: >-
  Description of endpoints used to construct Derived Key Transactions on the
  DeSo blockchain
---

# Derived Keys Transaction API

## Authorize Derived Key

<mark style="color:green;">`POST`</mark> `/api/v0/authorize-derived-key`

Create an authorize derived key transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Authorize derived key transactions allows another public key to submit transaction on behalf of the owner public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/954490fee2319072a9d6af710e3a3dd21bfc4f2d/routes/transaction.go#L2646).

#### Request Body

| Name                                                          | Type               | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| OwnerPublicKeyBase58Check<mark style="color:red;">\*</mark>   | String             | Public key of the derived key owner                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| DerivedPublicKeyBase58Check<mark style="color:red;">\*</mark> | String             | The derived public key                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ExpirationBlock<mark style="color:red;">\*</mark>             | uint64             | Height of block after which this derived key will no longer work.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| AccessSignature<mark style="color:red;">\*</mark>             | String             | The signature of hash(derived key + expiration block) made by the owner.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| DeleteKey<mark style="color:red;">\*</mark>                   | Boolean            | The intended operation on the derived key. If true, this derived key will no longer be valid.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| MinFeeRateNanosPerKB<mark style="color:red;">\*</mark>        | uint64             | Rate per KB                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| TransactionFees                                               | TransactionFee\[]  | <p>Array of</p><p><a data-mention href="./#transactionfee">#transactionfee</a></p><p>objects that define additional outputs that need to be added to this transaction</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| TransactionSpendingLimitHex<mark style="color:red;">\*</mark> | String             | <p>Hex string representing a TransactionSpendingLimit struct that will be merged with the TransactionSpendingLimitTracker for this derived key. We require this be sent as a hex in order toguarantee that the AccessHash computed from this value is consistent with what the user is requesting.</p><p>The TransactionSpendingLimit is an object defining the permissions of this derived key. Derived key will be restricted to certain transaction types and can only spend up to the specified amount of DESO. See the section on <a data-mention href="../api/#transactionspendinglimitresponse">#transactionspendinglimitresponse</a> for more details.      </p> |
| Memo                                                          | String             | Memo to describe the purpose of this derived key                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| AppName                                                       | String             | App requested this derived key. This is used if a memo is not provided so a user can remember which app has access with this derived key.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ExtraData                                                     | map\[string]string | extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a derived key                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| DerivedKeySignature                                           | Boolean            | Set to true if you intend to sign this transaction with the derived public key instead of the owner public key. This will add the DerivedPublicKey to ExtraData                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

{% tabs %}
{% tab title="200: OK Successfully constructed Authorize Derived Key transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
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
    "ExtraData": { Any additional keys in the ExtraData field of the request body will be included here as well.
      "DerivedKeyMemo": "Nzg3ODc5Nzk3YTdh", // bytes of the memo for this derived key
      "TransactionSpendingLimit": "gNDbw/QCAgIKFgEAAAA=", // bytes of the transaction spending limits for this derived key
    },
    "Signature": null,
    "TxnTypeJSON": 22
  },
  "TransactionHex": "0161b49620c72975d8397836c6b28981a0257d846e28ee74b296264bf1e2109036000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45c6ddeb17152167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}
