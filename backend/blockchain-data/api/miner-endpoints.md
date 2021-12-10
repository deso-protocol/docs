# Miner Endpoints

## Get Block Template

```
POST /api/v0/get-block-template
```

Get the template for the next block

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/miner.go#L56).

Example usages in frontend:

* Make request to [Get Block Template](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L584)
* Use GetBlockTemplate to [show stats on the next block in the admin panel](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/admin/admin.component.ts#L405)

**Parameters**

| Name                 | Type   | Description                                       | Required |   |
| -------------------- | ------ | ------------------------------------------------- | -------- | - |
| PublicKeyBase58Check | string | Public key to swap in for the block reward        | y        |   |
| NumHeaders           | int64  | Number of headers (and extra nonces) requested    | y        |   |
| HeaderVersion        | uint32 | Must be 1, version 0 headers have been deprecated | y        |   |

**Response**

```json5
{
  "BlockID": "27c0be6aba42ae4641c95d9a920399f689450f94388a7ac26c11188eb3b689b0", // Hex of latest block template hash
  "DifficultyTargetHex": "000000000000673b615ce18c1f9ddc322745eb8fb6f55b1debefc4bbbaf7db8a", // Hex of the current difficulty target
  "ExtraNonces": [1], // extra nonces in the block reward metadata
  "Headers": [[1,2,3]], // bytes of the headers of the block
  "LatestBlockTemplateStats": {
    "TxnCount": 0, // Number of transactions in the block template
    "FailingTxnError": "You good", // Reason why the final transaction failed to add. If no error, "You good" returned.
    "FailingTxnHash": "Nada", // Hash of final transaction tatempted to be put into the block that failed. If no error, "Nada" returned.
    "FailingTxnMinutesSinceAdded": 0, // Time since the failing transaction was added to the mempool 
    "FailingTxnOriginalTimeAdded": "2021-12-07T16:22:47.018211613Z" // The time of the first block in which the failed transaction was added.
  }
}
```

## Submit Block

```
POST /api/v0/submit-block
```

Submits block to be processed by node's block producer

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/miner.go#L125).

**Parameters**

| Name                 | Type    | Description                                                 | Required |   |
| -------------------- | ------- | ----------------------------------------------------------- | -------- | - |
| PublicKeyBase58Check | string  | Public key to swap in for the block reward                  | y        |   |
| Header               | \[]byte | Bytes of MsgDeSoHeader to be used in block                  | y        |   |
| ExtraNonce           | uint64  | Extra data nonce to be used in the block reward transaction | y        |   |
| BlockID              | string  | ID of block to be looked up from the block producer         | y        |   |

**Response**

```json5
{
  "IsMainChain": true, // If true, this is a block for the mainchain. If false, this is a block for testnet.
  "IsOrphan": false, // If true, this is an orphan block. If false, this block is not an orphan.
}
```
