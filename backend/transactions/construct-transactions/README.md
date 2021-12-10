---
description: Descriptions of all Transaction Construction Endpoints
---

# Construct Transactions

## Introduction

This section describes the endpoints used to construct transactions. All transactions must be signed - you can read about signing transactions in the [Identity Documentation](../../../identity/iframe-api/endpoints.md#sign).

For a reference implementation of constructing, signing, and submitting a transaction, see [signAndSubmitTransaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L483) in frontend.

Transactions submit data to the DeSo blockchain. Transactions on the DeSo blockchain can represent social data, such as a profile update or a post, NFT actions (minting, selling, burning, etc.), and financial/creator coin transactions.

### Structure of response

Each transaction construction endpoint returns a similar response, so descriptions are included here instead of in each sample response. Only `TxnMeta` is commented in the sample responses. Every transaction construction endpoint returns the following fields:

* `TotalInputNanos`: This is the total value in nanos of all the inputs specified in the transaction.
* `ChangeAmountNanos`: This is the amount of change the transactor will receive when submitting this transaction.
* `FeeNanos`: This is the amount the user pays in fees. `TotalInputNanos - ChangeAmountNanos - any other DeSo spent in transaction`.
* `Transaction`:
  * `TxInputs`: This is an array of objects with two keys, TxID and Index. For the sake of brevity, the value of TxID is replace with ellipses in response samples provided on this page.
  * `TxOutputs`: This is an array of objects with two keys, PublicKey and AmountNanos that specifies where the outputs will go and how much.
  * `TxnMeta`: This is an object that varies based on the transaction type. It contains the metadata that describes the transaction. For example, in an Update Profile transaction, `TxnMeta` will include `NewUsername` and `NewProfilePic`.
  * `ExtraData`: This is an object that can contain any key value pairs that adds additional information about a transaction.
  * `Signature`: This will be null when received from these endpoints. Identity will provide a signature.
  * `TxnTypeJSON`: This is an integer representing the type of the transaction.
  * `PublicKey`: This is the public key of the transactor.
* `TransactionHex`: Hex of the transaction. This is passed to identity to generate a signature.

Some transactions will have the following attributes:

* `TxnHashHex`: Hex of the transaction hash. This is used to check if a transaction has been successfully broadcast to the network with the Get Txn endpoint.
* `SpendAmountNanos`: The amount of DeSo spent in a transaction not on fees. For example, in a creator coin purchase, this would be the amount of DeSo spent on buying creator coins.

## Other Transactions

##
