---
description: >-
  Description of endpoints used to construct DAO Transactions on the DeSo
  blockchain
---

# DeSo Tokens Transactions API

<mark style="color:red;">Note: "DAO Coins" are now referred to as "</mark><mark style="color:red;">**DeSo Tokens**</mark><mark style="color:red;">" in all public-facing documentation, but the code and API have not yet been updated to reflect this change.</mark>\


## Create DeSo Token (DAO Coin)

<mark style="color:green;">`POST`</mark> `/api/v0/dao-coin`

Create a DeSo Token transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

A `mint` operation creates new DeSo Tokens

A `burn` operation destroys DeSo Tokens

An `update_transfer_restriction_status` operation updates the restrictions on transferring a DeSo Tokens

A `disable_minting` operation prevents minting of any future DeSo Tokens

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/transaction.go#L2271).

Example usages in frontend:\
&#x20; \- Make request to [DeSo Tokens](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1739)\
&#x20; \- Use DeSo Tokens to [mint DeSo Tokens](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L214)\
&#x20; \- Use DeSo Tokens to [burn DeSo Tokens](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L333)\
&#x20; \- Use DeSo Tokens to [update transfer restriction status](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L292)\
&#x20; \- Use DeSo Tokens to [disable minting](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L257)

#### Request Body

| Name                                                                    | Type               | Description                                                                                                                                                                                                                                                                                                                  |
| ----------------------------------------------------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UpdaterPublicKeyBase58Check<mark style="color:red;">\*</mark>           | String             | Public key performing the DeSo Token operation                                                                                                                                                                                                                                                                               |
| ProfilePublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark> | String             | Public key  or username of the creator of the Token on whose DeSo Token Updater is operating                                                                                                                                                                                                                                 |
| OperationType<mark style="color:red;">\*</mark>                         | String             | Type of DeSo Token operation being perform. Must be `mint`, `burn`, `update_transfer_restriction_status`, or `disable_minting`                                                                                                                                                                                               |
| CoinsToMintNanos                                                        | String             | <p>Hex string representing the number of DeSo Tokens in this operation.</p><p></p><p><strong>Required if OperationType is <code>mint</code></strong></p>                                                                                                                                                                     |
| CoinsToBurnNanos                                                        | String             | <p>Hex string representing the number of DeSo Tokens burned in this operation.</p><p></p><p><strong>Required if OperationType is <code>burn</code></strong></p>                                                                                                                                                              |
| TransferRestrictionStatus                                               | String             | <p>String representing the new transfer restriction status. Valid values are <code>unrestricted</code>, <code>profile_owner_only</code>, <code>dao_members_only</code>, <code>permanently_unrestricted</code></p><p></p><p><strong>Required if OperationType is <code>update_transfer_restriction_status</code></strong></p> |
| MinFeeRateNanosPerKB<mark style="color:red;">\*</mark>                  | uint64             | Rate per KB                                                                                                                                                                                                                                                                                                                  |
| TransactionFees                                                         | TransactionFees\[] | <p>Array of</p><p><a data-mention href="./#transactionfee">#transactionfee</a></p><p>objects that define additional outputs that need to be added to this transaction</p>                                                                                                                                                    |

{% tabs %}
{% tab title="200: OK Successfully constructed a DeSo Token transaction" %}
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
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}

## Transfer DeSo Token (DAO Coin)

<mark style="color:green;">`POST`</mark> `/api/v0/transfer-dao-coin`

Create a transfer DeSo Token transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

Transfer DeSo Token coin transactions sends DeSo Token owned by the sender to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/transaction.go#L2448).

Example usages in frontend:\
&#x20; \- Make request to [Transfer DeSo Token](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1761)\
&#x20; \- Use TransferDAOCoin to [construct, sign, and submit a DeSo Token transfer transaction](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/transfer-dao-coin-modal/transfer-dao-coin-modal.component.ts#L67).

#### Request Body

| Name                                                                     | Type               | Description                                                                                                                                                               |
| ------------------------------------------------------------------------ | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SenderPublicKeyBase58Check<mark style="color:red;">\*</mark>             | String             | Public key of the user sending DeSo Token                                                                                                                                 |
| ProfilePublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark>  | String             | Public key of the creator whose DeSo Token will be sent in this transaction                                                                                               |
| ReceiverPublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark> | String             | Public key of the recipient                                                                                                                                               |
| DAOCoinToTransferNanos<mark style="color:red;">\*</mark>                 | String             | Hex string representing the amount of DeSo Tokens to transfer in this transaction                                                                                         |
| MinFeeRateNanosPerKB<mark style="color:red;">\*</mark>                   | uint64             | Rate per KB                                                                                                                                                               |
| TransactionFees                                                          | TransactionFees\[] | <p>Array of</p><p><a data-mention href="./#transactionfee">#transactionfee</a></p><p>objects that define additional outputs that need to be added to this transaction</p> |

{% tabs %}
{% tab title="200: OK Successfully constructed a DeSo Token transfer transaction" %}
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
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

## Create DeSo Token (DAO Coin) Limit Order

<mark style="color:green;">`POST`</mark> `/api/v0/create-doa-coin-limit-order`

Create a new limit order to trade DeSo Tokens. The transaction needs to be signed and submitted through `api/v0/submit-transaction` before the order can be placed on the book or the coins are traded.

DeSo Tokens can be traded on an on-chain order book exchange. There are two types of markets where DeSo Tokens can be traded on the exchange: 1) markets where a DeSo Token is traded for $DESO, and 2) markets where a DeSo Token is traded for another DeSo Token.

This endpoint allows the creation of limit orders for either type of market.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/transaction.go#L2582)

#### Request Body

| Name                                                                                  | Type               | Description                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TransactorPublicKeyBase58Check<mark style="color:red;">\*</mark>                      | String             | Public key of the user creating the limit order                                                                                                                                                                                                                                                                                                                                                             |
| BuyingDAOCoinCreatorPublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark>  | String             | <p>Public key or username of the creator of a profile, whose DeSo Token is being bought.</p><p></p><p>If the order is selling a DeSo Token for $DESO, then this parameter needs to be an empty string.</p>                                                                                                                                                                                                  |
| SellingDAOCoinCreatorPublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark> | String             | <p>Public key or username of the creator of a profile whose DeSo Token is being sold. </p><p></p><p>If the order is buying a DeSo Token with $DESO, then this parameter needs to be an empty string.</p>                                                                                                                                                                                                    |
| ExchangeRateCoinsToSellPerCoinToBuy<mark style="color:red;">\*</mark>                 | float64            | The desired exchange rate for the coin being sold relative to the coin being bought. The real exchange rate used to fill this order will be equal to or better than the exchange rate provided here, always in the favor of the transactor.                                                                                                                                                                 |
| MinFeeRateNanosPerKB<mark style="color:red;">\*</mark>                                | uint64             | Rate per KB                                                                                                                                                                                                                                                                                                                                                                                                 |
| TransactionFees                                                                       | TransactionFees\[] | <p>Array of</p><p><a data-mention href="./#transactionfee">#transactionfee</a></p><p>objects that define additional outputs that need to be added to this transaction</p>                                                                                                                                                                                                                                   |
| QuantityToFill<mark style="color:red;">\*</mark>                                      | float64            | <p>The desired quantity of coins to buy or sell. This is denominated in number of coins (not nanos) and can have fractional values.</p><p></p><p>For example, if you wanted to fill an order of 1 DESO, you would specify 1 here, not 10^9.</p>                                                                                                                                                             |
| OperationType<mark style="color:red;">\*</mark>                                       | string             | <p>Supports values "BID" or "ASK".</p><p></p><p>"BID" signifies that this order wants to buy <code>QuantityToFill</code> total coins of the <code>BuyingDAOCoinCreatorPublicKeyBase58CheckOrUsername</code> coin.</p><p></p><p>"ASK" signifies that this limit order wants to sell <code>QuantityToFill</code> total coins of the <code>BuyingDAOCoinCreatorPublicKeyBase58CheckOrUsername</code> coin.</p> |

{% tabs %}
{% tab title="200: OK Successfully constructed a DeSo Token coin limit order transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
   "ChangeAmountNanos":199979890,
   "FeeNanos":336,
   "SpendAmountNanos":0,
   "TotalInputNanos":199980226,
   "Transaction":{
      "ExtraData":null,
      "PublicKey":"Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
      "Signature":null,
      "TxInputs":[
         {
            "Index":0,
            "TxID":[...]
         }
      ],
      "TxOutputs":[
         {
            "AmountNanos":199979890,
            "PublicKey":"Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S"
         }
      ],
      "TxnMeta":{
         "BidderInputs":null,
         "BuyingDAOCoinCreatorPublicKey":[...] // binary encoding of the coin being bought,
         "CancelOrderID":null,
         "FeeNanos":336,
         "OperationType":2, // integer value representing operation type; ASK = 1, BID = 2
         "QuantityToFillInBaseUnits":"0x12a05f200", // hex encoding of the quantity to filled
         "ScaledExchangeRateCoinsToSellPerCoinToBuy":"0xe1b1e5f90f944d6e1c9e66c000000000", // hex encoding of the exchange rate
         "SellingDAOCoinCreatorPublicKey":[... ] // binary encoding of the coin being sold
      },
      "TxnTypeJSON":26
   },
   "TransactionHex":"0133f5d223856b61216c68433257f9ed851c5e8f15c325d29ac38661b999484adb000102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52f2e6ad5f1a8b012102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52210000000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000e1b1e5f90f944d6e1c9e66c00000000020000000000000000000000000000000000000000000000000000000012a05f200020000d0022102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000",
   "TxnHashHex":"8e89a8edab106eb81046fac36de8119c8ce325490ba2549c84e8d77e113572f2"
}

```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    "error": "..." // error message
}
```
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "error": "..." // error message
}
```
{% endtab %}
{% endtabs %}

## Cancel DeSo Token (DAO Coin) Limit Order

<mark style="color:green;">`POST`</mark> `/api/v0/cancel-dao-coin-limit-order`

Cancel an open limit order to trade DeSo Token. The transaction needs to be signed and submitted through `api/v0/submit-transaction` before the order is cancelled.&#x20;

This endpoint allows a transactor to cancel a limit order they had previously created. The request will only succeed if the order is still open, and has not been completely filled or previously cancelled.

See [#gets-all-open-limit-orders-created-by-a-transactor](../api/dao-endpoints.md#gets-all-open-limit-orders-created-by-a-transactor "mention") for how to retrieve all open orders created by a transactor.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/transaction.go#L2729)

#### Request Body

| Name                                                             | Type               | Description                                                                                                                                                                    |
| ---------------------------------------------------------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| TransactorPublicKeyBase58Check<mark style="color:red;">\*</mark> | String             | Public key of the user who created the the limit order being cancelled                                                                                                         |
| MinFeeRateNanosPerKB<mark style="color:red;">\*</mark>           | uint64             | Rate per KB                                                                                                                                                                    |
| TransactionFees                                                  | TransactionFees\[] | <p>Array of</p><p><a data-mention href="./#transactionfee">#transactionfee</a></p><p>objects that define additional outputs that need to be added to this transaction</p>      |
| CancelOrderID<mark style="color:red;">\*</mark>                  | string             | Unique order identifier for the original limit order to cancel. OrderID is also equivalent to the base64 transaction hash hex of the transaction that created the limit order. |

{% tabs %}
{% tab title="200: OK Successfully constructed a transaction to cancel an open DAO coin limit order" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
   "SpendAmountNanos":0,
   "TotalInputNanos":199978801,
   "ChangeAmountNanos":199978564,
   "FeeNanos":237,
   "Transaction":{
      "TxInputs":[
         {
            "TxID":[...],
            "Index":0
         }
      ],
      "TxOutputs":[
         {
            "PublicKey":"Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
            "AmountNanos":199978564
         }
      ],
      "TxnMeta":{
         "BuyingDAOCoinCreatorPublicKey":null,
         "SellingDAOCoinCreatorPublicKey":null,
         "ScaledExchangeRateCoinsToSellPerCoinToBuy":null,
         "QuantityToFillInBaseUnits":null,
         "OperationType":0,
         "CancelOrderID":[...], // binary encoding for the order id for the limit order being cancelled
         "BidderInputs":null,
         "FeeNanos":237
      },
      "PublicKey":"Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
      "ExtraData":null,
      "Signature":null,
      "TxnTypeJSON":26
   },
   "TransactionHex":"012f6fcc9bf20ae44b0fa6e399bedeafbc2080d6e8c8e89a58db5b28730b3ad4f4000102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52c4dcad5f1a2900000000002033f5d223856b61216c68433257f9ed851c5e8f15c325d29ac38661b999484adb00ed012102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000",
   "TxnHashHex":"b3bec41bc59852277808459efbdc1509c08347f9e381459e886e14235a661ae8"
}

```
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    "error": "..." // error message
}
```
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "error": "..." // error message
}
```
{% endtab %}
{% endtabs %}
