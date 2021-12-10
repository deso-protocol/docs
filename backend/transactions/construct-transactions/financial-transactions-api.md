---
description: >-
  Description of endpoints used to construct Financial Transactions on the DeSo
  blockchain
---

# Financial Transactions API

## Send DeSo

```
POST /api/v0/send-deso
```

Create a Basic transfer transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

A Basic Transfer transaction sends DeSo from the sender to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L905).

Example usage in frontend:

* Make request to Send DeSo to get [a preview of the transaction.](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L905)
* Make request to Send DeSo and [sign+submit the transaction.](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L667)
* Use SendDeSo to [transfer DeSo to another user.](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/transfer-deso/transfer-deso.component.ts#L165)

**Parameters**

| Name                         | Type               | Description                                                                                                                             | Required | Restrictions |
| ---------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| SenderPublicKeyBase58Check   | string             | Public key of the sender                                                                                                                | y        |              |
| RecipientPublicKeyOrUsername | string             | Public key or Username of the recipient                                                                                                 | y        |              |
| AmountNanos                  | int64              | transaction amount in nanos - If less than 0, this will create a max spend transaction that will send all funds from Sender to Receiver | y        |              |
| MinFeeRateNanosPerKB         | uint64             | Rate per KB                                                                                                                             | y        |              |
| TransactionFees              | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction                               | n        |              |

**Response**

```json5
{
  "TotalInputNanos": 1999946613,
  "SpendAmountNanos": 1000000000,
  "ChangeAmountNanos": 999946354,
  "FeeNanos": 259,
  "TransactionIDBase58Check": "CbUyAcAiR5C626pSXQ1KCHWxo55FiSsBS66GELxVPPJMT2Q4yjFTA",
  "Transaction": {
    "TxInputs": [ 
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
        "AmountNanos": 1000000000
      },
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 999946354
      }
    ],
    "TxnMeta": {},
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 2
  },
  "TransactionHex": "025051b0eeda78885641a2340f57b908f475d84ce3b8ce2507815c89e857ef1cce0006caf6e4d44a8575fe235d7d3ffdb1020485526118d3379f5a993a31f5a2823f000202397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce528094ebdc0302aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f2f0e7dc0302002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000",
  "TxnHashHex": "df850aef107687e36034e3d298da341fc747d4a663e0a5e98fe3ce05cc10b4be"
}
```

## Buy or Sell Creator Coin

```
POST /api/v0/buy-or-sell-creator-coin
```

Create a buy/sell creator coin transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

A buy creator coin transaction locks DeSo in the creator coin of a creator and in return gives the purchaser creator coins.&#x20;

A sell creator coin transaction unlocks an amount of DeSo commensurate with the amount of creator coins sold.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L1602).

Example usages in frontend:

* Make request to [Buy Or Sell Creator Coin](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1485)
* Use BuyOrSellCreatorCoin to get a [preview of purchase/sale of creator coins](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/trade-creator-page/trade-creator-form/trade-creator-form.component.ts#L190)
* Use BuyOrSellCreatorCoin to [buy or sell creator coins](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/trade-creator-page/trade-creator-preview/trade-creator-preview.component.ts#L89)

**Parameters**

| Name                        | Type               | Description                                                                                                                                   | Required                            | Restrictions |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- | ------------ |
| UpdaterPublicKeyBase58Check | string             | Public key of user purchasing/selling creator coins                                                                                           | y                                   |              |
| CreatorPublicKeyBase58Check | string             | Public key of creator whose coin is being purchased                                                                                           | y                                   |              |
| OperationType               | string             | "buy" or "sell"                                                                                                                               | y                                   |              |
| DeSoToSellNanos             | uint64             | Amount of DeSo to spend                                                                                                                       | only required for buy transactions  |              |
| CreatorCoinToSellNanos      | uint64             | Amount of Creator Coin to spend                                                                                                               | only required for sell transactions |              |
| DeSoToAddNanos              | uint64             | deprecated                                                                                                                                    | n                                   |              |
| MinDeSoExpectedNanos        | uint64             | Minimum DeSo expected to be received when selling creator coins                                                                               | only required for sell transactions |              |
| MinCreatorCoinExpectedNanos | uint64             | Minimum amount of Creator Coins expected when buying creator coins                                                                            | only required for buy transactions  |              |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                                                                   | y                                   |              |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction                                     | n                                   |              |
| InTutorial                  | bool               | When true, perform additional checks to ensure user is at the correct point in the tutorial to execute this buy/sell creator coin transaction | n                                   |              |

**Response**

**Buy Creator Coin Response**

```json5
{
  "ExpectedDeSoReturnedNanos": 0, // Expected amount of DeSo received from the sale. Will always be 0 for buys.
  "ExpectedCreatorCoinReturnedNanos": 1200, // Expected amount of creator coins received from the purchase. Will always be 0 for sales.
  "FounderRewardGeneratedNanos": 9999664686, // Founders reward generated from this purchase. Will always be 0 for sales.
  "SpendAmountNanos": 1000000000,
  "TotalInputNanos": 1933081867,
  "ChangeAmountNanos": 933081602,
  "FeeNanos": 265,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [... ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 933081602
      }
    ],
    "TxnMeta": {
      "ProfilePublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // Public key of the creator whose coin was purchased in this transaction
      "OperationType": 0, // 0 means buy, 1 means sell
      "DeSoToSellNanos": 1000000000, // Amount of DeSo used to purchase creator coins in this transaction
      "CreatorCoinToSellNanos": 0, // Amount of creator coins sold in this transaction
      "DeSoToAddNanos": 0, // not in use - ignore
      "MinDeSoExpectedNanos": 0, // Minimum DeSo expected from a creator coin sale. This will always be 0 for buys.
      "MinCreatorCoinExpectedNanos": 1000 // Minimum amount of creator coins expected from this purchase. This will always be 0 for sales.
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 11
  },
  "TransactionHex": "02acc5b6f137be6a9b49fa1e0b20d0b6c5f2a76618628e1cd34fe8dbb198b627f1001ba3390ffb4817f3d5993ef39fd2fc4cfd75312ba67a7ce2f1d083efa0df2e5e000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4582e4f6bc030b2c2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45008094ebdc03000000002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000",
  "TxnHashHex": "69f08986184d99833c40d794d3fdd8861c0be602f15e3021ed771e49c6f7771c"
}
```

**Sell Creator Coin Response**

```json5
{
  "ExpectedDeSoReturnedNanos": 270953971, // Expected amount of DeSo received from the sale. Will always be 0 for buys.
  "ExpectedCreatorCoinReturnedNanos": 0, // Expected amount of creator coins received from the purchase. Will always be 0 for sales.
  "FounderRewardGeneratedNanos": 0, // Founder reward generated from this transaction. Will always be 0 for sales.
  "SpendAmountNanos": 0,
  "TotalInputNanos": 933081602,
  "ChangeAmountNanos": 933081367,
  "FeeNanos": 235,
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
        "AmountNanos": 933081367
      }
    ],
    "TxnMeta": {
      "ProfilePublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // Public key of the creator whose coin was purchased in this transaction
      "OperationType": 1, // 0 means buy, 1 means sell
      "DeSoToSellNanos": 0, // Amount of DeSo used to purchase creator coins in this transaction
      "CreatorCoinToSellNanos": 1000000000, // Amount of creator coins sold in this transaction
      "DeSoToAddNanos": 0, // Not in use - ignore.
      "MinDeSoExpectedNanos": 203215478, // Minimum DeSo expected from a creator coin sale. This will always be 0 for buys.
      "MinCreatorCoinExpectedNanos": 0 // Minimum amount of creator coins expected from this purchase. This will always be 0 for sales.
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 11
  },
  "TransactionHex": "013a58bb26ece8a6abb8f81d4d63359cd42a1602ffc0b99588d70f5278c3e2cb34000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4597e2f6bc030b2f2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4501008094ebdc0300f6a4f360002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000",
  "TxnHashHex": "c4a8163660bc46f5d6b51f55d0b6b6070d72939eeea13ccae422d78cb255859b"
}
```

## Transfer Creator Coin

```
POST /api/v0/transfer-creator-coin
```

Create a transfer creator coin transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

Transfer creator coin transactions sends creator coins owned by the sender to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L1860).

Example usages in frontend:

* Make request to [Transfer Creator Coin](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1543)
* Use TransferCreatorCoin to get a [preview of a creator coin transfer transaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/trade-creator-page/trade-creator-form/trade-creator-form.component.ts#L152).
* Use TransferCreatorCoin to [construct, sign, and submit a creator coin transfer transaction](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/trade-creator-page/trade-creator-preview/trade-creator-preview.component.ts#L167).

**Parameters**

| Name                                   | Type               | Description                                                                                               | Required | Restrictions |
| -------------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| SenderPublicKeyBase58Check             | string             | Public key of user sending creator coins                                                                  | y        |              |
| CreatorPublicKeyBase58Check            | string             | Public key of creator whose coins will be sent                                                            | y        |              |
| ReceiverUsernameOrPublicKeyBase58Check | string             | username or public key of user who will receive creator coins                                             | y        |              |
| CreatorCoinToTransferNanos             | uint64             | Amount of Creator Coin to transfer                                                                        | y        |              |
| MinFeeRateNanosPerKB                   | uint64             | Rate per KB                                                                                               | y        |              |
| TransactionFees                        | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |              |

**Response**

```json5
{
  "SpendAmountNanos": 0,
  "TotalInputNanos": 989250936,
  "ChangeAmountNanos": 989250675,
  "FeeNanos": 261,
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
        "AmountNanos": 989250675
      }
    ],
    "TxnMeta": {
      "ProfilePublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // Public key of the creator whose coin is being transferred in this transaction
      "CreatorCoinToTransferNanos": 1000000000, // Amount of Creator coins being transferred (in nanos)
      "ReceiverPublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S" // Public key of the recipient of the creator coin transfer
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 14
  },
  "TransactionHex": "01f2816e264f381cf153299e0c3cc894ab67687e83b79e55996bdb1c44280aed1d000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f388dbd7030e492102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c458094ebdc032102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce522102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000",
  "TxnHashHex": "a0e90d5786367855ea2960cd4d8cc3ad81a73c056c7ee62a637ab17aa01320fe"
}
```

##
