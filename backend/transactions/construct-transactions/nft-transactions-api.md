---
description: >-
  Description of endpoints used to construct NFT Transactions on the DeSo
  blockchain
---

# NFT Transactions API

## Create NFT

```
POST /api/v0/create-nft
```

Create a create NFT transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

Create NFT transactions mints the post specified by NFTPostHashHex as an NFT.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L84).

Example usages in frontend:

* Make request to [Create NFT](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L841)
* Use CreateNFT to [mint a post as an NFT](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/mint-nft-modal/mint-nft-modal.component.ts#L101)

**Parameters**

| Name                           | Type               | Description                                                                                                            | Required | Restrictions                                                  |
| ------------------------------ | ------------------ | ---------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------- |
| UpdaterPublicKeyBase58Check    | string             | Public key of the user creating the NFT                                                                                | y        | Must be the creator of the post being minted as an NFT        |
| NFTPostHashHex                 | string             | Hash of the Post being minted as an NFT                                                                                | y        | Post must not already be an NFT                               |
| NumCopies                      | int                | Number of copies to mint                                                                                               | y        | Must be less than the global maximum for number of NFT copies |
| NFTRoyaltyToCreatorBasisPoints | int                | Percentage (specified in basis points) of each sale that should be taken as a royalty to the creator of the post       | y        |                                                               |
| NFTRoyaltyToCoinBasisPoints    | int                | Percentage (specified in basis points) of each sale that should be added to the DeSo locked on the post creator's coin | y        |                                                               |
| HasUnlockable                  | bool               | When true, owner must provide unlockable text when selling this NFT                                                    | y        |                                                               |
| IsForSale                      | bool               | When true, put all serial numbers on sale                                                                              | y        |                                                               |
| MinBidAmountNanos              | int                | Minimum bid amount allowed for all serial numbers                                                                      | n        |                                                               |
| MinFeeRateNanosPerKB           | uint64             | Rate per KB                                                                                                            | y        |                                                               |
| TransactionFees                | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction              | n        |                                                               |

**Response**

```json5
{
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of post being minted as NFT
  "TotalInputNanos": 989250441,
  "ChangeAmountNanos": 989250210,
  "FeeNanos": 231,
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
        "AmountNanos": 989250210
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of the Post Hash that is being minted as an NFT
      "NumCopies": 1, // Number of copies of this NFT. Each has a unique serial number.
      "HasUnlockable": false, // If true, this post contains unlockable content that the seller must provide and encrypt when selling this NFT
      "IsForSale": true, // If true, bids can be submitted for any serial number of this NFT. If false, bids are not currently allowed for any serial number of this NFT.
      "MinBidAmountNanos": 106951871, // Minimum amount of DeSo (in nanos) that can be bid for any serial number.
      "NFTRoyaltyToCreatorBasisPoints": 500, // Percentage in basis points of NFT sales that will go to the creator
      "NFTRoyaltyToCoinBasisPoints": 1000 // Percentage in basis points of NFT sales that will be added to the amount locked in the NFT creator's coin
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 15
  },
  "TransactionHex": "0167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45a285dbd7030f2b67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0010001bfe9ff32f403e8072102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```

## Update NFT

```
POST /api/v0/update-nft
```

Create an update NFT transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Update NFT transactions can put a given serial number on sale or take it off sale or update the minimum bid amount.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L226).

Example usages in frontend: &#x20;

* Make request to [Update NFT](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L868)
* Use UpdateNFT to [put an NFT on sale](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/create-nft-auction-modal/create-nft-auction-modal.component.ts#L49)
* Use UpdateNFT to [close the auction on an NFT without selling](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/close-nft-auction-modal/close-nft-auction-modal.component.ts#L34)

**Parameters**

| Name                        | Type               | Description                                                                                               | Required | Restrictions                            |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------- |
| UpdaterPublicKeyBase58Check | string             | Public key of the user creating the NFT                                                                   | y        | Must be the owner of this serial number |
| NFTPostHashHex              | string             | Hash of the NFT Post being updated                                                                        | y        |                                         |
| SerialNumber                | int                | serial number to update                                                                                   | y        |                                         |
| IsForSale                   | bool               | When true, put this serial number on sale                                                                 | y        |                                         |
| MinBidAmountNanos           | int                | Minimum bid amount allowed for this serial number                                                         | n        |                                         |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y        |                                         |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |                                         |

**Response**

```json5
{
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of NFT post being updated
  "SerialNumber": 1, // Serial number of NFT post that is being updated
  "TotalInputNanos": 989249984,
  "ChangeAmountNanos": 989249757,
  "FeeNanos": 227,
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
        "AmountNanos": 989249757
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of Post Hash being updated in this transaction
      "SerialNumber": 1, // Serial number of NFT post being updated in this transaction
      "IsForSale": true, // If true, the NFT will now be on sale. If false, the NFT will NOT be on sale
      "MinBidAmountNanos": 1000000000 // The minimum bid amount allowed on this NFT
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 16
  },
  "TransactionHex": "01eef1a4195e53cfaa4804443c2969525494ee9511c840f9f711f5c87d9449f2b5000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45dd81dbd703102767f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c001018094ebdc032102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```

## Create NFT Bid

```
POST /api/v0/create-nft-bid
```

Create an NFT bid transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

NFT Bid transactions submit a bid on an NFT. If the owner of the NFT accepts this bid - by submitting an Accept NFT Bid transaction - the bidder will send the bid amount to the owner and in return receive the NFT.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L364).

Example usages in frontend:

* Make request to [Create NFT Bid](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L889)
* Use CreateNFTBid to [submit a bid for an NFT](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/place-bid-modal/place-bid-modal.component.ts#L102)

**Parameters**

| Name                        | Type               | Description                                                                                               | Required | Restrictions                                                                          |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------- |
| UpdaterPublicKeyBase58Check | string             | Public key of the user submitting the bid                                                                 | y        |                                                                                       |
| NFTPostHashHex              | string             | Hash of the NFT Post being bid on                                                                         | y        |                                                                                       |
| SerialNumber                | int                | serial number to bid on                                                                                   | y        |                                                                                       |
| BidAmountNanos              | int                | Amount UpdaterPublicKeyBase58Check is bidding on this serial number                                       | y        | Must be greater than MinBidAmountNanos for this Post hash - serial number combination |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y        |                                                                                       |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |                                                                                       |

**Response**

```json5
{
  "UpdaterPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // Public key of user submitting bid in this transaction
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of the NFT post being bid on
  "SerialNumber": 1, // Serial number being bid on
  "BidAmountNanos": 1000000000, // Amount bid in DeSo nanos
  "TotalInputNanos": 49578,
  "ChangeAmountNanos": 49352,
  "FeeNanos": 226,
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
        "AmountNanos": 49352
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of Post Hash that is being bid on
      "SerialNumber": 1, // Serial number being bid on
      "BidAmountNanos": 1000000000 // Amount bid in DeSo nanos
    },
    "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 18
  },
  "TransactionHex": "01b4d67df685cc2938ddb4c5d8c92167dda3e2d0f7c20df693e219cfde1ef2fc30000102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52c88103122667f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0018094ebdc032102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000"
}
```

## Accept NFT Bid

```
POST /api/v0/accept-nft-bid
```

Create an accept NFT Bid transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Accept NFT Bid transactions accepts a bid, divides the proceeds of the bid amount among the owner, creator, and creator coin based on royalties, and updates the owner of the NFT to be the bidder selected.

Note that if you are selling an NFT that has unlockable content, you must encrypt the unlockable content and provide it.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L522).

Example usages in frontend:

* Make request to [Accept NFT Bid](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L907) including encrypting unlockable content if applicable
* Use AcceptNFTBid to [sell an NFT without unlockable content](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/sell-nft-modal/sell-nft-modal.component.ts#L65)
* Use AcceptNFTBid to [sell an NFT with unlockable content](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/add-unlockable-modal/add-unlockable-modal.component.ts#L43)

**Parameters**

| Name                        | Type               | Description                                                                                               | Required                                         | Restrictions |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------ |
| UpdaterPublicKeyBase58Check | string             | Public key of the current NFT Owner                                                                       | y                                                |              |
| NFTPostHashHex              | string             | Hash of the NFT Post being sold                                                                           | y                                                |              |
| SerialNumber                | int                | serial number to being sold                                                                               | y                                                |              |
| BidderPublicKeyBase58Check  | string             | Public key of the bidder being award the NFT                                                              | y                                                |              |
| BidAmountNanos              | int                | Bid amount being accepted                                                                                 | y                                                |              |
| EncryptedUnlockableText     | string             | Text encrypted with a shared secret between the current owner and the bidder                              | Only required if this NFT has unlockable content |              |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y                                                |              |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n                                                |              |

**Response**

```json5
{
  "BidderPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // Bidder whose bid was accepted for this NFT
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of NFT post being sold
  "SerialNumber": 1, // Serial number being sold
  "BidAmountNanos": 1000000000, // Bid amount being accepted
  "TotalInputNanos": 989249757,
  "ChangeAmountNanos": 989249428,
  "FeeNanos": 329,
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
        "AmountNanos": 989249428
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of Post Hash being sold
      "SerialNumber": 1, // Serial number being sold
      "BidderPKID": [2,57,123,26,128,235,160,166,6,68,101,10,241,60,42,111,253,251,191,56,131,12,175,195,73,55,167,93,221,68,184,206,82], // Bytes of Bidder's PKID
      "BidAmountNanos": 1000000000, // Bid amount being accepted
      "UnlockableText": null, // Encrypted unlockableable text if available
      "BidderInputs": [ // Bidder Inputs used for purchasing the NFT
        {
          "TxID": [...],
          "Index": 0
        },
      ]
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 17
  },
  "TransactionHex": "0119a8f2fc3402e0501144ccf9a44d00d6045700ed9036f4770dc46e8297ee1756000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4594ffdad703118c0167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce528094ebdc030002b9f8047e628ff03054d8ffd0abedcd253aece89d9c97cfbcd067eaf5b8d1000a0015cf97ab3fba7c54283874c6928ff05ccf59bf0e1cb3f951cff2f1adee2bea79002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```

## Transfer NFT

```
POST /api/v0/transfer-nft
```

Create a transfer NFT transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Transfer NFT transactions sends an NFT from the sender to the receiver at no cost to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L1399).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:

* Make request to [Transfer NFT](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1068)
* Use Transfer NFT to [send NFT to another user](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/transfer-nft/transfer-nft.component.ts#L93)

**Parameters**

| Name                         | Type               | Description                                                                                               | Required                                         | Restrictions |
| ---------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------ |
| SenderPublicKeyBase58Check   | string             | Public key of the current NFT Owner                                                                       | y                                                |              |
| ReceiverPublicKeyBase58Check | string             | Public key of the recipient of the NFT                                                                    | y                                                |              |
| NFTPostHashHex               | string             | Hash of the NFT Post being transferred                                                                    | y                                                |              |
| SerialNumber                 | int                | serial number being transferred                                                                           | y                                                |              |
| EncryptedUnlockableText      | string             | Text that encrypted with a shared secret between the current owner and the receiver                       | Only required if this NFT has unlockable content |              |
| MinFeeRateNanosPerKB         | uint64             | Rate per KB                                                                                               | y                                                |              |
| TransactionFees              | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n                                                |              |

**Response**

```json5
{
  "SenderPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // Public key of the user sending the NFT
  "ReceiverPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public key of the user receiving the NFT
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of NFT being transferred
  "SerialNumber": 1, // Serial number being transferred
  "TotalInputNanos": 49352,
  "ChangeAmountNanos": 49096,
  "FeeNanos": 256,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 3
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
        "AmountNanos": 49096
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of Post Hash being transferred
      "SerialNumber": 1, // Serial number being transferred
      "ReceiverPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF", // Public key of user receiving NFT
      "UnlockableText": "" // Encrypted unlockable text
    },
    "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 19
  },
  "TransactionHex": "01dde32daf6f9abcd2994d671e603a13f7c5d56dd97ae31ff07d4192a884792d5f030102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52c8ff02134467f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45002102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce520000"
}
```

## Accept NFT Transfer

```
POST /api/v0/accept-nft-transfer
```

Create an accept NFT Transfer transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Accept NFT Transfer transaction changes a transferred NFT status from pending to not pending. Since anybody can send a user an NFT, the recipient needs to accept the transfer before the NFT can appear on their profile in order to prevent users from sending spam NFTs.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L1557).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:

* Make request to [Accept NFT Transfer](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L922)
* Use AcceptNFTTransfer to [make NFT appear on your profile](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/transfer-nft-accept/transfer-nft-accept.component.ts#L56)

**Parameters**

| Name                        | Type               | Description                                                                                               | Required | Restrictions |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| UpdaterPublicKeyBase58Check | string             | Public key of the user accepting the NFT transfer                                                         | y        |              |
| NFTPostHashHex              | string             | Hash of the NFT Post for which the transfer is being accepted                                             | y        |              |
| SerialNumber                | int                | serial number for which the transfer is being accepted                                                    | y        |              |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y        |              |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |              |

**Response**

```json5
{
  "UpdaterPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public key of user accepting NFT transfer
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of the NFT that is being accepted
  "SerialNumber": 1, // Serial Number whose transfer is being accepted
  "TotalInputNanos": 50000000,
  "ChangeAmountNanos": 49999779,
  "FeeNanos": 221,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 2
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 49999779
      }
    ],
    "TxnMeta": {
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of the Post Hash of the NFT whose transferred is being accepted
      "SerialNumber": 1 // Serial Number whose transfer is being accepted
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 20
  },
  "TransactionHex": "01dde32daf6f9abcd2994d671e603a13f7c5d56dd97ae31ff07d4192a884792d5f020102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45a3dfeb17142167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```

## Burn NFT

```
POST /api/v0/burn-nft
```

Create a Burn NFT transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

NFT Burn transactions burns the NFT, meaning that no user can ever own that Post hash-serial number combination.

Endpoint implementation in backend.

Example usages in frontend:

* Make request to [Burn NFT](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L906)
* Use BurnNFT to [burn the NFT ](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/nft-burn/nft-burn.component.ts#L89)

**Parameters**

| Name                        | Type               | Description                                                                                               | Required | Restrictions |
| --------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| UpdaterPublicKeyBase58Check | string             | Public key of the user burning the NFT transfer                                                           | y        |              |
| NFTPostHashHex              | string             | Hash of the NFT Post that is being burnt                                                                  | y        |              |
| SerialNumber                | int                | serial number that is being burnt                                                                         | y        |              |
| MinFeeRateNanosPerKB        | uint64             | Rate per KB                                                                                               | y        |              |
| TransactionFees             | TransactionFees\[] | Array of Transaction Fee objects that define additional outputs that need to be added to this transaction | n        |              |

**Response**

```json5
{
  "UpdaterPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public key of user burning the NFT
  "NFTPostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Post Hash Hex of NFT being burned
  "SerialNumber": 1, // Serial number being burned
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
      "NFTPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of Post Hash being burned 
      "SerialNumber": 1 // Serial number being burned
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 21
  },
  "TransactionHex": "0161b49620c72975d8397836c6b28981a0257d846e28ee74b296264bf1e2109036000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45c6ddeb17152167f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0012102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```

##
