---
description: >-
  Description of endpoints used to construct Derived Key Transactions on the
  DeSo blockchain
---

# Derived Keys Transaction API

{% swagger method="post" path="" baseUrl="/api/v0/authorize-derived-key" summary="Authorize Derived Key" %}
{% swagger-description %}
Create an authorize derived key transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Authorize derived key transactions allows another public key to submit transaction on behalf of the owner public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/954490fee2319072a9d6af710e3a3dd21bfc4f2d/routes/transaction.go#L2646).
{% endswagger-description %}

{% swagger-parameter in="body" required="true" type="String" name="OwnerPublicKeyBase58Check" %}
Public key of the derived key owner
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="String" name="DerivedPublicKeyBase58Check" %}
The derived public key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="DerivedKeySignature" type="Boolean" %}
Set to true if you intend to sign this transaction with the derived public key instead of the owner public key. This will add the DerivedPublicKey to ExtraData
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="uint64" name="ExpirationBlock" %}
Height of block after which this derived key will no longer work.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="String" name="AccessSignature" %}
The signature of hash(derived key + expiration block) made by the owner.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="Boolean" name="DeleteKey" %}
The intended operation on the derived key. If true, this derived key will no longer be valid.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="uint64" name="MinFeeRateNanosPerKB" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" type="TransactionFee[]" name="TransactionFees" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction 
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionSpendingLimitHex" type="String" required="true" %}
Hex string representing a TransactionSpendingLimit struct that will be merged with the TransactionSpendingLimitTracker for this derived key. We require this be sent as a hex in order toguarantee that the AccessHash computed from this value is consistent with what the user is requesting.

The TransactionSpendingLimit is an object defining the permissions of this derived key. Derived key will be restricted to certain transaction types and can only spend up to the specified amount of DESO. See the section on [#transactionspendinglimitresponse](../../blockchain-data/basics/data-types.md#transactionspendinglimitresponse "mention")for more details.     &#x20;
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Memo" type="String" %}
Memo to describe the purpose of this derived key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AppName" type="String" %}
App requested this derived key. This is used if a memo is not provided so a user can remember which app has access with this derived key.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[string]string" %}
extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a derived key
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed Authorize Derived Key transaction" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
