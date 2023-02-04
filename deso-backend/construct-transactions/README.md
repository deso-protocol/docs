---
description: Descriptions of all Transaction Construction Endpoints
---

# 5⃣ Construct: API

This section describes the endpoints used to construct transactions. All transactions must be signed — you can read about signing transactions in the [Identity Documentation](../../deso-identity/iframe-api/endpoints.md#sign).

For a reference implementation of constructing, signing, and submitting a transaction, see [signAndSubmitTransaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L483) in frontend.

Transactions submit data to the DeSo blockchain. Transactions on the DeSo blockchain can represent social data, such as a profile update or a post, NFT actions (minting, selling, burning, etc.), and financial/creator coin transactions.

### Structure of response

Each transaction construction endpoint returns a similar response, so descriptions are included here instead of in each sample response.

Only `TxnMeta` is commented in the sample responses. Every transaction construction endpoint returns the following fields:

* `TotalInputNanos`: This is the total value in nanos of all the inputs specified in the transaction.\

* `ChangeAmountNanos`: This is the amount of change the transactor will receive when submitting this transaction.\

* `FeeNanos`: This is the amount the user pays in fees. `TotalInputNanos - ChangeAmountNanos - any other DeSo spent in transaction`.\

* `Transaction`:
  * `TxInputs`: This is an array of objects with two keys, TxID and Index. For the sake of brevity, the value of TxID is replace with ellipses in response samples provided on this page.\

  * `TxOutputs`: This is an array of objects with two keys, PublicKey and AmountNanos that specifies where the outputs will go and how much.\

  * `TxnMeta`: This is an object that varies based on the transaction type. It contains the metadata that describes the transaction. For example, in an Update Profile transaction, `TxnMeta` will include `NewUsername` and `NewProfilePic`.\

  * `ExtraData`: This is an object that can contain any key-value pairs that add additional information about a transaction.\

  * `Signature`: This will be null when received from these endpoints. Identity will provide a signature.\

  * `TxnTypeJSON`: This is an integer representing the type of the transaction.\

  * `PublicKey`: This is the public key of the transactor.\

* `TransactionHex`: Hex of the transaction. This is passed to identity to generate a signature.

Some transactions will have the following attributes:

* `TxnHashHex`: Hex of the transaction hash. This is used to check if a transaction has been successfully broadcast to the network with the Get Txn endpoint.\

* `SpendAmountNanos`: The amount of DeSo spent in a transaction not on fees. For example, in a creator coin purchase, this would be the amount of DeSo spent on buying creator coins.

## Data Types

### TransactionFee

`TransactionFees` are additional transaction outputs.&#x20;

These additional outputs are a way for both node operators (who can specify additional fees for all transactions of a certain type on their node) and app developers (who can specify additional fees when making a request to construct a transaction).

```json5
{
  "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user who will receive the additional output,
  "ProfileEntryResponse": <ProfileEntryResponse>, // This is only provided when TranssactionFees are retrieved through admin endpoints when managing node-leve transaction fees
  "AmountNanos": 10000, // The amount of DeSo in nanos this user should receive
}
```

For reference, `TransactionFee` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/admin\_fees.go#L16).

## AccessGroupMember

`AccessGroupMembers`  are objects used when performing Access Group Member transactions which add, remove, or update members of an access group. These objects are only using in the construction of these transaction.

#### Attributes

* AccessGroupMemberPublicKeyBase58Check: the public key of the user who is being added, removed, or updated
* AccessGroupMemberKeyName: the access group key name belong to AccessGroupMemberPublicKeyBase58Check by which the user is being added to or removed from the group. &#x20;
* EncryptedKey: the private key of the access group is encrypted to the AccessGroupPublicKey of the member's access group. This must be left empty when removing a member from a group
* ExtraData: arbitrary key value data used to add details about this access group member&#x20;

```json5
{
  "AccessGroupMemberPublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s",
  "AccessGroupMemberKeyName": "default-key",
  "EncryptedKey": "someencryptedhexstring",
  "ExtraData": { "key": "value" },
}
```

##
