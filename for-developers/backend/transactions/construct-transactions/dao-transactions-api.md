---
description: >-
  Description of endpoints used to construct DAO Transactions on the DeSo
  blockchain
---

# DAO Transactions API

{% swagger method="post" path="" baseUrl="/api/v0/dao-coin" summary="DAO Coin" %}
{% swagger-description %}
Create a DAO coin transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

A `mint` operation creates new DAO coins

A `burn` operation destroys DAO coins

An `update_transfer_restriction_status` operation updates the restrictions on transferring a DAO coin

A `disable_minting` operation prevents minting of any future DAO coins

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/transaction.go#L2271).

Example usages in frontend:\
&#x20; \- Make request to [DAO Coin](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1739)\
&#x20; \- Use DAO Coin to [mint DAO coins](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L214)\
&#x20; \- Use DAO Coin to [burn DAO coins](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L333)\
&#x20; \- Use DAO Coin to [update transfer restriction status](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L292)\
&#x20; \- Use DAO Coin to [disable minting](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L257)
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="UpdaterPublicKeyBase58Check" type="String" %}
Public key performing the DAO Coin operation
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="ProfilePublicKeyBase58CheckOrUsername" type="String" %}
Public key  or username of the creator of the DAO on whose DAO coins Updater is operating
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="OperationType" type="String" %}
Type of DAO coin operation being perform. Must be 

`mint`

, 

`burn`

, 

`update_transfer_restriction_status`

, or 

`disable_minting`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="CoinsToMintNanos" type="String" %}
Hex string representing the number of DAO coins minted in this operation.



**Required if OperationType is `mint`**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="CoinsToBurnNanos" type="String" %}
Hex string representing the number of DAO coins burned in this operation.



**Required if OperationType is `burn`**
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransferRestrictionStatus" %}
String representing the new transfer restriction status. Valid values are `unrestricted`, `profile_owner_only`, `dao_members_only`, `permanently_unrestricted`



**Required if OperationType is `update_transfer_restriction_status`**
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="uint64" name="MinFeeRateNanosPerKB" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" type="TransactionFees[]" name="TransactionFees" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction 
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a DAO Coin transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    "TotalInputNanos": 799994621,
    "ChangeAmountNanos": 799994391,
    "FeeNanos": 230,
    "Transaction": {
      "TxInputs": [
        {
          "TxID": [... ],
          "Index": 0
        }
      ],
      "TxOutputs": [
        {
          "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
          "AmountNanos": 799994391
        }
      ],
      "TxnMeta": {
        "ProfilePublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S", // Public key of the DAO coin creator
        "OperationType": 0, // Integer representing operation type. 0 is mint, 1 is burn, 2 is update transfer restriction status, 3 is disable minting
        "CoinsToMintNanos": "0x3b9aca00", // Hex string representing the number of nanos be minted in this transaction. Only set if operation type is 0.
        "CoinsToBurnNanos": "0x0", // Hex string representing the number of nanos being burned in this transaction. Only set if operation type is 1.
        "TransferRestrictionStatus": 0 // Integer representing the transfer restriction status being set in this transaction. Only set if operation type is 2.is 
      },
      "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
      "ExtraData": null,
      "Signature": null,
      "TxnTypeJSON": 24
    },
    "TransactionHex": "01c64eebdaf08d50f8bea79e6f2004b6b7b826fb3245f044271c6c8b6708e76a68000102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce5297e4bbfd02182a2102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce5200043b9aca0000002102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000",
    "TxnHashHex": "88595b14baf461bdc04020bd705c8e7f08d9bacef88cbaedefcae6328852292e"
}
```
{% endtab %}

{% tab title=" Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/transfer-dao-coin" summary="Transfer DAO Coin" %}
{% swagger-description %}
Create a transfer DAO coin transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

Transfer DAO coin transactions sends DAO coins owned by the sender to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/transaction.go#L2448).

Example usages in frontend:\
&#x20; \- Make request to [Transfer DAO Coin](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1761)\
&#x20; \- Use TransferDAOCoin to [construct, sign, and submit a DAO coin transfer transaction](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/transfer-dao-coin-modal/transfer-dao-coin-modal.component.ts#L67).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderPublicKeyBase58Check" type="String" required="true" %}
Public key of the user sending DAO coins
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ProfilePublicKeyBase58CheckOrUsername" required="true" type="String" %}
Public key of the creator whose DAO coin will be sent in this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReceiverPublicKeyBase58CheckOrUsername" required="true" type="String" %}
Public key of the recipient
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="DAOCoinToTransferNanos" type="String" %}
Hex string representing the amount of DAO coins to transfer in this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" type="TransactionFees[]" name="TransactionFees" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a DAO coin transfer transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    "SpendAmountNanos": 0,
    "TotalInputNanos": 799994391,
    "ChangeAmountNanos": 799994130,
    "FeeNanos": 261,
    "Transaction": {
      "TxInputs": [
        {
          "TxID": [... ],
          "Index": 0
        }
      ],
      "TxOutputs": [
        {
          "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
          "AmountNanos": 799994130
        }
      ],
      "TxnMeta": {
        "ProfilePublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S", // Public key of the DAO coin creator whose DAO coin you wish to transfer
        "DAOCoinToTransferNanos": "0x3b9aca00", // Hex string representing the number of DAO coins being transferred
        "ReceiverPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF" // Public key of recipient
      },
      "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
      "ExtraData": null,
      "Signature": null,
      "TxnTypeJSON": 25
    },
    "TransactionHex": "015b9860bf3343449c19ff8227c0d932e4f996a4710ea0b6de268411137ed8e7cf000102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce5292e2bbfd0219492102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52043b9aca002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000",
    "TxnHashHex": "7be4ac172d789b25657a12e109ec5502301483af27f0a394fff030622819a2f3"
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
