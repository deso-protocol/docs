# Exchange Listing API

**The dev community recommends using the open source Rosetta API implementation for integrating DeSo on an exchange:** [**https://github.com/deso-protocol/rosetta**](https://github.com/deso-protocol/deso-rosetta)**. The other APIs in this doc are less supported than the Rosetta APIs.**

Multiple major crypto exchanges have expressed interest in listing DeSo. The dev community is working closely with several of these, but, now that anyone in the world can run a DeSo node, we thought we'd democratize and decentralize this effort by publishing a simple public API that any crypto exchange in the world could follow to integrate DeSo.

This guide will cover all of the API endpoints that are needed in order to list DeSo, with detailed descriptions and examples. This includes:

* Setting up a node.
* Using the Exchange API to create unlimited public/private key pairs.
* Using the Exchange API to check the balance of DeSo public keys.
* Using the Exchange API to transfer DeSo between public keys.
* Using the Exchange API to query for transactions by transaction ID.
* Using the Exchange API to query for transactions by public key.
* Using the Exchange API to query for node sync status.
* Using the Exchange API to query for block information by height or block hash.

The [Quick Start](exchange-listing-api.md#quick-start) section provides examples of all of the above using the “curl” command. The [Full API Guide](exchange-listing-api.md#full-api-guide) section provides more detail on each API endpoint shown in the examples.

_**Note: This API is strictly for use by exchanges. The bitclout.com nodes use in-browser signing such that your seed phrase never leaves your browser \(**_[_**learn more**_](https://docs.deso.org/privacy-and-security)_**\). In contrast, exchanges are typically custodial and so some of these endpoints manipulate seeds on behalf of users.**_

## Quick Start

### **Generate a Seed Mnemonic**

To get started, you need to generate a standard [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) mnemonic seed that will be used to generate public/private key pairs. If you don't require that your keys be generated on an air-gapped computer, then you can use the [bitclout.com](https://bitclout.com) signup flow to generate your mnemonic. Note that your seed _never_ leaves your browser when you generate it on bitclout.com. See [Privacy and Security](https://docs.deso.org/privacy-and-security) for more details on this process.

If you need your seed to be generated in an offline fashion, then we recommend that you use [this tool](https://iancoleman.io/bip39/). Either a 12 or 24-word mnemonic should be fine, and standard Bitcoin mnemonics work as well.

What we will use in our examples:

* Mnemonic: _arrive mixture refuse loud people robot dolphin scissors lift curve better demand_
* Passphrase \(also known as "ExtraText"\): _password_

### Run a Node

All of the commands and examples in this guide will assume that you have a DeSo node running on your local machine. To set one up, simply follow the instructions in the open-source /run repository. If you run into any trouble, the [nodes-discussion](https://discord.com/channels/820740896181452841/835273317773869086) Discord channel is always available to help you:

* [https://github.com/deso-protocol/run](https://github.com/deso-protocol/run)

Note that the node software is cross-platform and should run on Linux, Mac, and Windows. However, it seems as though people have had the most success with Linux and Mac machines with at least 32GB of RAM and at least 100GB of free disk space.

_NOTE: You must set `READ_ONLY_MODE` to false in_ [_dev.env_](https://github.com/deso-protocol/run/blob/190a2380b278689a4db844bb52a31d0450db7d46/dev.env#L265) _in order for some API calls to work. However, at the time of this writing, it is not yet recommended to deploy a production node with `READ_ONLY_MODE` set to false. This should change shortly, though. Keep an eye on the_ [_README_](https://github.com/deso-protocol/run/tree/190a2380b278689a4db844bb52a31d0450db7d46) _for updates._

### Check Node Sync Status

This query will return information about a node’s sync status, among other things. See the [Full API Guide](exchange-listing-api.md#full-api-guide) section for more information.

```text
curl --header "Content-Type: application/json" --data-raw '{}' \
    http://localhost:17001/api/v1/node-info | python -m json.tool
```

Notes:

* We pipe the command into “python -m json.tool” so that it will “pretty print” but that you can delete this part of the command if you don’t have Python installed.
* We are assuming the node is running on the same machine on which we’re doing this query. If the node is running on a different machine then the IP of that machine should be substituted for “localhost.”

### Generate a Public/Private Key Pair

This will generate a public/private key-pair that corresponds to index “0” for this account. Each key-pair will map to an index for a particular seed. To generate more key-pairs, simply iterate the “Index” parameter.

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "Mnemonic":"arrive mixture refuse loud people robot dolphin scissors lift curve better demand",
    "ExtraText":"password",
    "Index": 0
}' http://localhost:17001/api/v1/key-pair  | python -m json.tool
```

Notes:

* Under the hood, every public/private key pair maps to derivation path m/44'/0'/0'/0/{index}. **Thus they would be identical to what is generated by any Bitcoin wallet using the same mnemonic, passphrase, and derivation path.**
* The public and private keys returned by this function will be encoded using base58 check encoding described in more detail in the [Full API Guide](exchange-listing-api.md#full-api-guide) section for this endpoint. For now, all that you need to know is that you can pass the public/private key strings to other API endpoints to check balances, spend DeSo, etc…
  * DeSo public keys that are encoded with base58 always start with the prefix “BC”. DeSo private keys that are encoded with base58 always start with the prefix “bc” \(lower-case\).
* Example of DeSo public/private key pair returned by this function. Note that Error being empty string means the endpoint succeeded.
  * ```text
    {
        "Error": "",
        "PrivateKeyBase58Check": "bc6EmekhAbzn2V9BchgRLMRMZW1m8mo7kmvdwjZRB5nnKpgQhWSf4",
        "PrivateKeyHex": "423e1f1fe03469e4173f5a0056f468255358f9200fd5acfa7be8185d2fcb98b4",
        "PublicKeyBase58Check": "BC1YLgAJ2kZ7Q4fZp7KzK2Mzr9zyuYPaQ1evEWG4s968sChRBPKbSV1",
        "PublicKeyHex": "024089f4297576513ce07de8190583154c15b8279a586f7d0663ff3c5391351a1e"
    }
    ```
* We pipe the command into “python -m json.tool” so that it will “pretty print” but that you can delete this if you don’t have Python installed.

_**Note: This API is strictly for use by exchanges. The bitclout.com nodes use a different API that never receives your seed phrase, and your seed phrase never leaves your browser. In contrast, exchanges are typically custodial and so some of these endpoints manipulate seeds on behalf of users.**_

### Check Balance of DeSo Public Key

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "PublicKeyBase58Check":"BC1YLgAJ2kZ7Q4fZp7KzK2Mzr9zyuYPaQ1evEWG4s968sChRBPKbSV1"
}' http://localhost:17001/api/v1/balance | python -m json.tool
```

Notes:

* This will return the balance in “nanos,” where 1 DeSo = 1,000,000,000 “nanos.” For example, if the balance for this public key was “1 DeSo” then this endpoint will return 1,000,000,000 \(or 1e9 nanos\).
* This endpoint also returns UTXO's, but this likely won't be useful to most node operators.

### Transfer DeSo Using a Public/Private Key-Pair

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "SenderPublicKeyBase58Check":"BC1YLgAJ2kZ7Q4fZp7KzK2Mzr9zyuYPaQ1evEWG4s968sChRBPKbSV1", 
    "SenderPrivateKeyBase58Check":"bc6EmekhAbzn2V9BchgRLMRMZW1m8mo7kmvdwjZRB5nnKpgQhWSf4", 
    "RecipientPublicKeyBase58Check":"BC1YLgU67opDhT9bTPsqvue9QmyJLDHRZrSj77cF3P4yYDndmad9Wmx", 
    "AmountNanos": 1000000000
}' http://localhost:17001/api/v1/transfer-deso | python -m json.tool
```

Notes:

* This example will fail unless you send DeSo to the `SenderPublicKeyBase58Check`.
  * You can buy DeSo on bitclout.com and then use the "Send DeSo" page to get some DeSo for testing purposes.
* The amount must be specified in "nanos," where 1 DeSo = 1e9 nanos. This example transfers 1 DeSo from public key `BC1YLgAJ2kZ7Q4fZp7KzK2Mzr9zyuYPaQ1evEWG4s968sChRBPKbSV1` to public key `BC1YLgU67opDhT9bTPsqvue9QmyJLDHRZrSj77cF3P4yYDndmad9Wmx`
  * To do a "dry run" of the transaction without broadcasting it, simply add `DryRun: true` to the params.
* Setting “AmountNanos” to a negative value like -1 will send the maximum amount possible.
  * To implement a UI with a “Max” button, we recommend hitting this endpoint with a negative AmountNanos with DryRun set to true, grabbing the resultant “spend amount,” which will be net of fees, and displaying that to the user.
* This endpoint will return information for the transaction created. See the [Full API Guide](exchange-listing-api.md#full-api-guide) section on this endpoint for more information on what is returned.
* A custom “fee rate” can also be set. See the [Full API Guide](exchange-listing-api.md#full-api-guide) section for this endpoint for more detail on that.

_**Note: This API is strictly for use by exchanges. The bitclout.com nodes use a different API that never receives your seed phrase, and your seed phrase never leaves your browser. In contrast, exchanges are typically custodial and so some of these endpoints manipulate seeds on behalf of users.**_

### Look Up Transactions for a Public Key

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "PublicKeyBase58Check":"BC1YLgAJ2kZ7Q4fZp7KzK2Mzr9zyuYPaQ1evEWG4s968sChRBPKbSV1",
    "IDsOnly": true
}' http://localhost:17001/api/v1/transaction-info | python -m json.tool
```

Notes:

* A transaction ID is a sha256 hash of a transaction, encoded using base58 check encoding, that uniquely identifies a transaction.
* This gets all the transaction IDs for a particular public key ordered from oldest to newest.
  * To fetch full transactions rather than just the IDs, simply set `IDsOnly` to `false` rather than `true` or leave it out of the request entirely.
* This endpoint will only work if the node was started with the [TXINDEX flag](https://github.com/deso-protocol/run/blob/190a2380b278689a4db844bb52a31d0450db7d46/dev.env#L123) set to true, which is the default.
  * You must also wait for your `TXINDEX` to generate, which can take a few hours. Grep your logs for UpdateTxIndex to monitor its progress.
* See the [Full API Guide](exchange-listing-api.md#full-api-guide) section for this endpoint to see what information will be returned by this endpoint.

### Look Up Transaction Using Transaction ID

Get information for a specific transaction using that transaction’s transaction ID. You can get a transaction ID from other endpoints like the [transfer-deso endpoint](exchange-listing-api.md#api-v-1-transfer-deso) described previously.

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "TransactionIDBase58Check": "3JuEUE5QSkjyuLwY8WUjS3MRjMbaNEd4nE63VugpU17HMzJW7vbrJP"
}' http://localhost:17001/api/v1/transaction-info | python -m json.tool
```

Notes:

* This is the same endpoint as the one used to lookup the transactions for a public key. When a `PublicKeyBase58Check` param is set, the `TransactionIDBase58Check` param is expected to be unset and is ignored.
* This endpoint will only work if the node was started with the [TXINDEX flag](https://github.com/deso-protocol/run/blob/190a2380b278689a4db844bb52a31d0450db7d46/dev.env#L123) set to true, which is the default.
* See the [Full API Guide](exchange-listing-api.md#full-api-guide) section for this endpoint to see what information will be returned by this endpoint.

### **Get Block For Block Hash or Height**

This will return all the information associated with the block at height 10715. If the chain is not synced up to this point, an error will be returned.

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "Height":10715
}' http://localhost:17001/api/v1/block | python -m json.tool
```

Same as the previous example, only queries the block by its hash rather than its height.

```text
curl --header "Content-Type: application/json" --request POST --data '{
    "HashHex":"0000000000306a10b85a0bfd801479f1f2227ebaa8bdd5c61da4736dff319362"
}' http://localhost:17001/api/v1/block | python -m json.tool
```

For more information, see the [Full API Guide](exchange-listing-api.md#full-api-guide) section for these endpoints.

## Full API Guide

_**Note: This API is strictly for use by exchanges. The bitclout.com nodes use a different API that never receives your seed phrase, and your seed phrase never leaves your browser. In contrast, exchanges are typically custodial and so some of these endpoints manipulate seeds on behalf of users.**_

_**Note: The dev community is also working to complete an integration with**_ [_**Rosetta**_](https://www.rosetta-api.org/) _**that will further build on this API.**_

### /api/v1/key-pair

You can generate public/private keypairs with a standard BIP39 mnemonic. Each public/private key pair corresponds to a particular index associated with the mnemonic. This means that index “5” for a particular mnemonic, for example, will always generate the same public/private key pair. An infinite number of public/private key pairs can thus be generated by iterating an index over a particular mnemonic.

All public/private keys are inter-operable as Bitcoin public/private keys. Meaning they represent a point on the secp256k1 curve \(same as what is used by Bitcoin\).

Under the hood, DeSo takes the BIP39 mnemonic and generates the public/private key pairs using the BIP32 derivation path m/44'/0'/0'/0/{index}, where "index" is the index of the public/private key being generated. This means that DeSo public/private key pair generated by the node will always line up with the public/private key pairs generated by [this Ian Coleman tool](https://iancoleman.io/bip39/). An engineer can therefore “sanity check” that things are working by generating a mnemonic using bitclout.com or Ian Coleman, creating a key pair with that mnemonic, and then verifying that the public/private key pairs generated line up with what is shown on bitclout.com or Ian Coleman.

```text
PATH: /api/v1/key-pair
METHOD: POST
POST PARAMS:
    // A BIP39 mnemonic and extra text. Mnemonic can be 12 words or
    // 24 words. ExtraText is optional.
    Mnemonic  string
    ExtraText string
    // The index of the public/private key pair to generate
    Index uint32
RETURNS:
  // Blank if successful. Otherwise, contains a description of the
  // error that occurred.
  Error string
  // The DeSo public key encoded using base58 check encoding with
  // prefix = [3]byte{0x11, 0xc2, 0x0}
  // This public key can be passed in subsequent API calls to check
  // balance, among other things. All encoded DeSo public keys start
  // with the characters “BC”
  PublicKeyBase58Check string
  // The DeSo public key encoded as a plain hex string. This should
  // match the public key with the corresponding index generated by the
  // Ian Coleman tool.
  // This should not be passed to subsequent API calls, it is only provided
  // as a reference, mainly as a sanity-check.
  PublicKeyHex string
  // The DeSo private key encoded using base58 check encoding with
  // prefix = [3]byte{0x4f, 0x6, 0x1b}
  // This private key can be passed in subsequent API calls to spend DeSo,
  // among other things. All DeSo private keys start with
  // the characters “bc”
  PrivateKeyBase58Check string
  // The DeSo private key encoded as a plain hex string. Note that
  // this will not directly match what is produced by the Ian Coleman
  // tool because the tool shows the private key encoded using
  // Bitcoin’s WIF format rather than as raw hex. To convert this raw hex
  // into Bitcoin’s WIF format you can use this simple Python script:
  // https://github.com/geniusprodigy/bitcoin-convertpvk
  // This should not be passed to subsequent API calls. It is provided as
  // a reference, mainly as a sanity-check.
  PrivateKeyHex string
```

### /api/v1/balance

One can check the balance of a particular public key by passing the public key to the following endpoint.

Spent transaction outputs are not returned by this endpoint. To perform operations on spent transaction outputs, one must use the “transaction-info” endpoint instead.

```text
PATH: /api/v1/balance
METHOD: POST
POST PARAMS:
  // A DeSo public key encoded using base58 check encoding (starts
  // with “BC”). When this field is provided, the other params are
  // ignored.
  PublicKeyBase58Check string
  // Only consider UTXOs with greater than or equal to the specified number
  // of confirmations. This defaults to zero, which considers all UTXOs,
  // including those in the mempool.
  Confirmations uint32
RETURNS:
  // Blank if successful. Otherwise, contains a description of the
  // error that occurred.
  Error string
  // The balance of the public key queried in “nanos.” Note 
  // there are 1e9 “nanos” per DeSo, so if the balance were “1 DeSo” then
  // this value would be set to 1e9.
  ConfirmedBalanceNanos int64
  // The unconfirmed balance of the public key queried in “nanos.” This field
  // is set to zero if Confirmations is set to a value greater than zero.
  UnconfirmedBalanceNanos int64
  // DeSo uses a UTXO model similar to Bitcoin. As such, querying
  // the balance returns all of the UTXOs for a particular public key for
  // convenience. Note that a UTXO is simply a reference to a particular
  // output index in a previous transaction
  UTXOs [{
    // A string that uniquely identifies a previous transaction. This is
    // a sha256 hash of the transaction’s information encoded using
    // base58 check encoding. Will be empty string if this UTXO is
    // a block reward.
    TransactionIDBase58Check string
    // The index within this transaction that corresponds to an output
    // spendable by the passed-in public key.
    Index int64
    // The amount that is spendable by this UTXO in “nanos” = 1e9 DeSo.
    AmountNanos uint64
    // The pulic key entitled to spend the amount stored in this UTXO.
    PublicKeyBase58Check string
    // The number of confirmations this UTXO has. Set to zero if the
    // UTXO is unconfirmed.
    Confirmations int64
    // Whether or not this UTXO was a block reward.
    IsBlockReward bool
  }, ... ]
```

### /api/v1/transfer-deso

DeSo can be transferred from one public key to another using this simple API call. To transfer DeSo, one must either provide a public/private key pair.

DeSo uses a UTXO model like Bitcoin but DeSo transactions are generally simpler than Bitcoin transactions because DeSo always uses the “from public key” as the “change” public key \(meaning that it does not “rotate” keys by default\). For example, if a transaction sends 10 DeSo from PubA to PubB with 5 DeSo in “change” and 1 DeSo as a “miner fee,” then the transaction would look as follows:

```text
Input: 16 DeSo (10 DeSo to send, 5 DeSo in change, and 1 DeSo as a fee)
PubB: 10 DeSo (the amount being sent from A to B)
PubA: 5 DeSo (change returned to A)

Implicit 1 DeSo is paid as a fee to the miner. The miner fee is implicitly
computed as (total input – total output) just like in Bitcoin.
```

The maximum amount of DeSo can be sent by specifying a negative amount when calling the endpoint. We recommend running the endpoint once with `DryRun` set to `true`, inspecting the output, and then running it with `DryRun` set to `false`, which will actually broadcast the transaction.

```text
PATH: /api/v1/transfer-deso
METHOD: POST
POST PARAMS:
    // A DeSo private key encoded using base58 check encoding (starts
    // with "bc").
    SenderPrivateKeyBase58Check string
    // A DeSo public key encoded using base58 check encoding (starts
    // with “BC”) that will receive the DeSo being sent.
    RecipientPublicKeyBase58Check string
    // The amount of DeSo to send in “nanos.” Note that “1 DeSo” is equal to
    // 1e9 nanos, so to send 1 DeSo, this value would need to be set to 1e9.
    AmountNanos int64
    // The fee rate to use for this transaction. If left unset, a default fee rate
    // will be used. This can be checked using the “DryRun” parameter below.
    MinFeeRateNanosPerKB int64
    // When set to true, the transaction is returned in the response but not
    // actually broadcast to the network. Useful for testing.
    DryRun bool
RETURNS:
  // Blank if successful. Otherwise, contains a description of the
  // error that occurred.
  Error string
  // The transaction that executes the transfer. Will not be broadcast
  // if DryRun is set to true.
  Transaction {
    // A string that uniquely identifies this transaction. This is a sha256 hash
    // of the transaction’s data encoded using base58 check encoding.
    TransactionIDBase58Check string
    // The raw hex of the transaction data. This can be fully-constructed from
    // the human-readable portions of this object.
    RawTransactionHex string
    // The inputs of this transaction.
    Inputs [{
        // An input in a transaction consists of the transaction ID and
        // the index of the output from that transaction.
        TransactionIDBase58Check string
        Index int64
      }, ... ]
    Outputs [
      // A transaction output is simply a public key and the
      // amount that is being allocated to that public key in
      // “nanos” where 1 DeSo = 1e9 nanos.
      {
        PublicKeyBase58Check string
        AmountNanos int64
      }, ... ]
    // The signature of the transaction in hex format.
    SignatureHex string
    // Will always be “0” for basic transfers
    TransactionType int64
    // Will always be empty for basic transfers
    TransactionMeta {}
    // The hash of the block in which this transaction was mined. If the
    // transaction is unconfirmed, this field will be empty. To look up
    // how many confirmations a transaction has, simply plug this value
    // into the "block" endpoint.
    BlockHashHex string
  }
  TransactionInfo {
    // The sum of the inputs
    TotalInputNanos uint64
    // The amount being sent to the “RecipientPublicKeyBase58Check”
    SpendAmountNanos uint64
    // The amount being returned to the “SenderPublicKeyBase58Check”
    ChangeAmountNanos uint64
    // The total fee and the fee rate (in nanos per KB) that was used for this
    // transaction.
    FeeNanos uint64
    FeeRateNanosPerKB uint64
    // Will match the public keys passed as params. Note that
    // SenderPublicKeyBase58Check receives the change from this transaction.
    SenderPublicKeyBase58Check string
    RecipientPublicKeyBase58Check string
  }
```

### /api/v1/transaction-info

If one has a TransactionIDBase58Check, e.g. from calling the “transfer-deso” endpoint, one can get the corresponding human-readable “Transaction object” by passing this transaction id to a node. Note that this endpoint will error if `TXINDEX` is set to false. If `TXINDEX` was passed to the node but it has not finished syncing the blockchain yet, this endpoint may return incomplete results. The `/node-info` endpoint can be used to check where a node is in its sync process \(generally, syncing takes only a minute or two\).

If one has a PublicKeyBase58Check \(starts with “BC”\), one can get all of the TransactionIDs associated with that public key sorted by oldest to newest \(this will include transactions where the address is a sender and a receiver\). One can also optionally get the full Transaction objects for all of the transactions in the same call.

```text
PATH: /api/v1/transaction-info
METHOD: POST
POST PARAMS:
  // A string that uniquely identifies this transaction. E.g. from a previous
  // call to “transfer-deso”. Ignored when PublicKeyBase58Check is set.
  // When a transaction is looked up using its ID directly, we also scan the
  // mempool for it. This makes it so that a “block explorer” can easily
  // surface transactions associated with a particular ID.
  TransactionIDBase58Check string
  // A DeSo public key encoded using base58 check encoding (starts
  // with “BC”) to get transaction IDs for. When set,
  // TransactionIDBase58Check is ignored.
  PublicKeyBase58Check string
  // Whether or not to return full transaction info or just the TransactionIDHex
  // for each transaction. Full transactions are returned when this is unset.
  IDsOnly bool
RETURNS
  // Blank if successful. Otherwise, contains a description of the
  // error that occurred.
  Error string
  // The info for all transactions this public key is associated with from oldest
  // to newest. If “IDsOnly” is set to true, each Transaction object will contain
  // only TransactionIDBase58Check. Otherwise, all other fields will be set as well.
  Transactions [
    Transaction {
      // Always set.
      TransactionIDBase58Check string
      // Rest of fields are as defined previously, but only set if
      // IDsOnly is unset or false.
      ...
  }, ... ]
```

### /api/v1/node-info

General information about the node’s blockchain and sync state can be queried using this endpoint. The blockchain does a “headers-first” sync, meaning it first downloads all DeSo headers and then downloads all blocks. This means that, when the node is first syncing, the tip of the best “header chain” may be ahead of of its most recently downloaded block. In addition to syncing DeSo headers and DeSo blocks, a DeSo node will also sync all of the latest Bitcoin headers to power its built-in decentralized Bitcoin &lt;&gt; DeSo swap mechanism. For this reason, the endpoint also returns information on the node’s best Bitcoin header chain, which is distinct from its DeSo chain.

```text
PATH: /api/v1/node-info
METHOD: POST
RETURNS
  DeSoStatus {
    // A summary of what the node is currently doing.
    State string

    // We generally track the latest header we have and the latest block we have
    // separately since headers-first synchronization can cause the latest header
    // to diverge slightly from the latest block.
    LatestHeaderHeight     uint32
    LatestHeaderHash       string
    LatestHeaderTstampSecs uint32

    LatestBlockHeight     uint32
    LatestBlockHash       string
    LatestBlockTstampSecs uint32

    // This is non-zero unless the main header chain is fully current. It can be
    // an estimate in cases where we don't know exactly what the tstamp of the
    // current main chain is.
    HeadersRemaining uint32
    // This is non-zero unless the main header chain is fully current and all
    // the corresponding blocks have been downloaded.
    BlocksRemaining uint32
  }
  BitcoinStatus {
    // We download Bitcoin headers in order to power the decentralized
    // Bitcoin <> DeSo swap built-in to the app,
    // which allows users to convert Bitcoin into DeSo without needing
    // to trust third-parties.
    //
    // This part of the response has the same schema as DeSoStatus, only 
    // the block information won’t be populated since we only download
    // Bitcoin headers not full Bitcoin blocks.
  }
  DeSoOutboundPeers []PeerResponse {
    IP           string
    ProtocolPort uint16
    JSONPort     uint16
    IsSyncPeer   bool
  }
  DeSoInboundPeers []PeerResponse{
    // Same schema as above
  }
  DeSoUnconnectedPeers []PeerResponse{
    // Same schema as above
  }
  BitcoinSyncPeer []PeerResponse{
    // Same schema as above
  }
  BitcoinUnconnectedPeers []PeerResponse{
    // Same schema as above
  }
  // The public keys the node is currently sending block rewards to.
  // If no public keys have been specified then the node will not be mining.
  MinerPublicKeys []string
```

### /api/v1/block

A block’s information can be queried using either the block hash or height. To get all blocks in the chain, simply query this endpoint by enumerating the heights starting from zero and iterating up to the tip. The tip height and hash can be obtained using the `/node-info` endpoint.

```text
PATH: /api/v1/block
METHOD: POST
POST PARAMS:
  // Block height. 0 corresponds to the genesis block. An error will be
  // returned if the height exceeds the tip. This field is ignored if HashHex is
  // set.
  Height int64
  // Hash of the block to return. Height is ignored if this is set.
  HashHex string
  // When set to false, only returns the header of the block requested
  // not the full block. Otherwise, returns the full block.
  FullBlock bool
RETURNS
  // Blank if successful. Otherwise, contains a description of the
  // error that occurred.
  Error string
  // The information contained in the block’s header.
  Header {
    // The hash of the block that was queried.
    BlockHashHex string
    // Generally set to zero
    Version uint32
    // Hash of the previous block in the chain.
    PrevBlockHashHex string
    // The merkle root of all the transactions contained within the block.
    TransactionMerkleRootHex string
    // The unix timestamp (in seconds) specifying when this block was
    // mined.
    TstampSecs uint32
    // The height of the block this header corresponds to.
    Height uint32
    // The nonce is encoded as a little-endian 32-bit integer. If more than 2^32
    // hashes are required in order to mine a block, the block reward's ExtraData
    // field can be twiddled to change the merkle root to give a miner a fresh set
    // of 2^32 header nonces to try. Note that we don't use 64 bits (or more) because
    // keeping the header small is important for the efficiency of light clients and
    // because it doesn't add much value over over just twiddling the ExtraData
    // every 2^32 values.
    Nonce uint32
  }
  // A list of Transactions, where the Transaction object is as defined previously.
  Transactions [
    Transaction {
    }, ... 
  ]
```

