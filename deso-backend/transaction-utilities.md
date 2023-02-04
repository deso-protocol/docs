# 9⃣ Transactions: API

## Introduction

Transactions are the building material of every blockchain.

Transactions allow users to submit data to the blockchain which allows user to perform actions such as transferring DeSo, creating posts and profiles, and minting NFTs.

Transactions have three steps in their lifecycle

1. **Construct:** The first step for a developer is to interact with the DeSo Backend API through a transaction construction endpoint to get an unsigned user transaction.\
   &#x20;\
   [social-transactions-api.md](construct-transactions/social-transactions-api.md "mention"), [nft-transactions-api.md](construct-transactions/nft-transactions-api.md "mention"), [financial-transactions-api.md](construct-transactions/financial-transactions-api.md "mention"), and [derived-keys-transaction-api.md](construct-transactions/derived-keys-transaction-api.md "mention") explain the endpoints that will get you an unsigned transaction.
2. **Sign:** The developer will then take the output `TransactionHex` from the construct step's response, which encodes the user transaction, and signs it using the DeSo Identity.\
   \
   You can read about signing transactions in the[#sign](../deso-identity/iframe-api/endpoints.md#sign "mention") section of the [endpoints.md](../deso-identity/iframe-api/endpoints.md "mention") documentation.\

3. **Broadcast:** The signed transaction will be sent through the `/api/v0/submit-transaction` by the developer so that it can be added to the blockchain ledger.\
   \
   The [#submit-a-transaction](transaction-utilities.md#submit-a-transaction "mention") submit explains how this endpoint works.

You can read more about Transactions in this section of the [Identity documentation. ](../deso-identity/identity/concepts.md#transactions)

{% swagger method="post" path="" baseUrl="/api/v0/submit-transaction" summary="Submit a transaction" expanded="false" %}
{% swagger-description %}
Submit a signed transaction to DeSo blockchain.&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L85).

Example usages in frontend:\
&#x20; \- Make request to [Submit Transaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L685)\
&#x20; \- Use Submit Transaction [in conjunction with signing transaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L483)
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="TransactionHex" type="String" %}
Hex of transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
{% tabs %}
{% tab title="Sample Response" %}
```json
{
    "Transaction": {
      "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // public key of the transactor
      "TxnTypeJSON": 5, // Integer representing transaction type
      "ExtraData": { // Arbitrary key value map providing metadata about the transaction
        "key": "value"
      },
      "Signature": {
        "R": 981237981749831749879848321, // R attribute of signature
        "S": 843174832748124, // S attribute of signature
      },
      "TxInputs": [{
        "Index": 0, // Index within transaction where the unspent output occurs
        "TxID": [1, 2, 3, ...] // 32 byte transaction id where unspent output occurs 
      }],
      "TxOutputs": [{ 
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // Public key receiving the output
        "AmountNanos": 912739 // Amount of DeSo in the output
      }],
      "TxnMeta": { // Transaction metadata. more details explaining TxnMeta for each transaction type coming soon. 
      ...
      }
    },
    "TxnHashHex": "0f40a5fc7eb991cea55ebece1ec21ee5fb1c4bba537bb76643ee1d31f617bb56",
    "PostEntryResponse": <PostEntryResponse>, // If transaction is a Submit post transaction, include the PostEntryResponse that was created as a result of the transaction.
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name              | Type                                                                                           | Description                                                                                                                                                          |
| ----------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Transaction       | Transaction object                                                                             | transaction object that was broadcast to the DeSo blockchain                                                                                                         |
| PublicKey         | string                                                                                         | <p>Attribute of transaction</p><p></p><p>Public key of transactor</p>                                                                                                |
| TxnTypeJSON       | integer                                                                                        | <p>Attribute of transaction</p><p></p><p>Number representing transaction type</p>                                                                                    |
| ExtraData         | map\[string]string                                                                             | <p>Attribute of transaction</p><p></p><p>Arbitrary key value map providing metadata about the transaction</p>                                                        |
| Signature         | { R: integer, S: integer }                                                                     | <p>Attribute of transaction</p><p></p><p>Signature of transactoin</p>                                                                                                |
| TxInputs          | <p>Array of transaction iputs</p><p></p><p>[{ Index: integer, TxId: integer[] }]</p>           | <p>Attribute of transaction </p><p></p><p>Each element represents an input (DeSo being used) in the transaction. </p>                                                |
| TxOutputs         | <p>Array of transaction outputs</p><p></p><p>[{ PublicKey: string, AmountNanos: integer }]</p> | <p>Attribute of transaction</p><p></p><p>Each element represents an output (DeSo being received) in the transaction</p>                                              |
| TxnMeta           | Transaction Metadata object                                                                    | <p>Attribute of transaction</p><p></p><p>Transaction Metadata descriptions coming soon. Each transaction type has its own transaction metadata object structure.</p> |
| TxnHashHex        | String                                                                                         | Hex of transaction hash broadcasted to the DeSo blockchain                                                                                                           |
| PostEntryResponse | ``[`PostEntryResponse`](broken-reference)                                                      | If a transaction is a submit post transaction, the `PostEntryResponse` that was created by the transaction is included                                               |
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Unable to broadcast  transaction to network" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-txn" summary="Get Transaction" expanded="false" %}
{% swagger-description %}
Check if transaction is currently in mempool. This is particularly useful if you need to wait for a transaction to be broadcasted before submitting a subsequent transaction.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L34).

Example usages in frontend:\
&#x20; \- Make request to [Get Txn](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L591)\
&#x20; \- Use Get Txn to [see if a transaction has been broadcast to the network](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/app.component.ts#L268)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactionHashHex" type="Strng" required="true" %}
Hex of Transaction hash that we want to check made it to the mempool
{% endswagger-parameter %}

{% swagger-response status="200: OK" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    "TxnFound": true
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
<table><thead><tr><th>Name</th><th>Type</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>TxnFound</td><td>boolean</td><td>If true, the transaction is currently in the mempool</td><td></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/append-extra-data" summary="Append Extra Data" expanded="false" %}
{% swagger-description %}
Append custom ExtraData for a given transaction hex. This endpoint is typically used when signing with a derived key.

Note: If you will be using this endpoint, you will need to increase MinFeeRateNanosPerKB to 1500 when using a transaction construction endpoint.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L2314)
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="TransactionHex" type="String" %}
The hex of the transaction on which extra data will be appended.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[string]string" required="true" %}
Arbitrary key value map that will be merged with any extra data decoded from TransactionHex. Keys from this map will overwrite keys that were decoded from TransactionHex
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Hex of transaction with extra data appended" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TransactionHex": "sometransactionhex"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name           | Type   | Description                                 |
| -------------- | ------ | ------------------------------------------- |
| TransactionHex | string | hex of transaction with extra data appended |
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

{% swagger method="post" path="" baseUrl="/api/v0/get-transaction-spending" summary="Get Transaction Spending" expanded="false" %}
{% swagger-description %}
Calculates the total transaction spending by subtracting transaction output to sender from transaction inputs. This allows a convenient way to display to users how much they will spend if they submit a given transaction to the network.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L2384)

Example usages in identity:\
&#x20; \- Make request to [Get Transaction Spending](https://github.com/deso-protocol/identity/blob/9dad527dc46498b9aaa0344abd70dc8895acf246/src/app/backend-api.service.ts#L199)\
&#x20; \- Use Get Transaction Spending to [show user total spending of transaction](https://github.com/deso-protocol/identity/blob/9dad527dc46498b9aaa0344abd70dc8895acf246/src/app/approve/approve.component.ts#L62)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactionHex" type="String" required="true" %}
The hex of the transaction on which extra data will be appended.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Total amount spent in the transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TotalSpendingNanos": 1000823987
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name               | Type    | Description                                              |
| ------------------ | ------- | -------------------------------------------------------- |
| TotalSpendingNanos | Integer | Total amount spent by the transactor in this transaction |
{% endtab %}
{% endtabs %}
{% endswagger-response %}
{% endswagger %}
