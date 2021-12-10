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

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L2210).
{% endswagger-description %}

{% swagger-parameter in="body" required="true" type="String" name="OwnerPublicKeyBase58Check" %}
Public key of the derived key owner
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="String" name="DerivedPublicKeyBase58Check" %}
The derived public key
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
    "ExtraData": null,
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
