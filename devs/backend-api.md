# Backend API

## General Endpoints

### Index

```text
GET /
```

Basic endpoint to test if your DeSo node is running.

**Parameters**

None

**Response**

```text
Your DeSo node is running!
```

### Health Check

```text
GET /api/v0/health-check
```

Check if your DeSo node is synced

**Parameters:**

None

**Response:**

If node is synced and received all transactions.

```text
200
```

### Get Exchange Rate

```text
GET /api/v0/get-exchange-rate
```

Get DeSo exchange rate, total amount of nanos sold, and Bitcoin exchange rate.

**Parameters:**

None

**Response:**

```text
{
    "SatoshisPerDeSoExchangeRate":498484,
    "NanosSold":8491518125648433,
    "USDCentsPerBitcoinExchangeRate":3608200
}
```

### Get App State

```text
POST /api/v0/get-app-state
```

Get state of DeSo App, such as cost of profile creation and diamond level map. Example use in the [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1106) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/base.go#L86).

**Parameters**

None; however, you need to send an empty JSON `{ }`. Otherwise, you will get 400 - Bad Request. More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/base.go#L63).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | \(optional\) check public key |

**Response**

```text
{
    "AmplitudeKey": "",
    "AmplitudeDomain": "api.amplitude.com",
    "MinSatoshisBurnedForProfileCreation": 50000,
    "IsTestnet": false,
    "SupportEmail": "node.admin@protonmail.com",
    "ShowProcessingSpinners": true,
    "HasStarterDeSoSeed": false,
    "HasTwilioAPIKey": false,
    "CreateProfileFeeNanos": 10000000,
    "CompProfileCreation": false,
    "DiamondLevelMap": {
        "1": 50000,
        "2": 500000,
        "3": 5000000,
        "4": 50000000,
        "5": 500000000,
        "6": 5000000000,
        "7": 50000000000,
        "8": 500000000000
    },
    "HasWyreIntegration": false,
    "Password": ""
}
```

## Transaction Endpoints

### Get Txn

```text
POST /api/v0/get-txn
```

Check if Txn is currently in mempool. Example use in the [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/app.component.ts#L291) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L34).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L25).

| Name | Type | Description |
| :--- | :--- | :--- |
| TxnHashHex | string | Transaction hash |

**Response**

```text
{
    "TxnFound": true
}
```

### Submit Transaction

```text
POST /api/v0/submit-transaction
```

Submit transaction to DeSo blockchain. Example use in [frontend](https://github.com/deso-protocol/docs/tree/48edcd8f15f30a527a2d6d927e87c83bf10becdb/devs/%60https:/github.com/deso-protocol/frontend/blob/96bdf0c40e05010ec62a1b1cdc78bf0d0fb2ef44/src/app/backend-api.service.ts#L496%60) and endpoint implementation in [backend](https://github.com/deso-protocol/docs/tree/48edcd8f15f30a527a2d6d927e87c83bf10becdb/devs/%60https:/github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L81%60).

**Parameters**

Read more on transaction format [here](https://github.com/deso-protocol/docs/blob/main/code/walkthrough.md#transaction-format). More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L69).

| Name | Type | Description |
| :--- | :--- | :--- |
| TransactionHex | string | Transaction hash |

**Response**

```text
{
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TxnHashHex: "...",
    PostEntryResponse: {...}
}
```

### Update Profile

```text
POST /api/v0/update-profile
```

Update profile fields and receive corresponding Txn. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L816) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L247).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L214).

| Name | Type | Description |
| :--- | :--- | :--- |
| UpdaterPublicKeyBase58Check | string | Public key of updater |
| ProfilePublicKeyBase58Check | string | \(optional\) Public key of the profile if different from updater |
| NewUsername | string | Username |
| NewDescription | string | Description |
| NewProfilePic | string | Profile picture |
| NewCreatorBasisPoints | uint64 | Creator Reward |
| NewStakeMultipleBasisPoints | uint64 | Staking Reward |
| IsHidden | bool |  |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    TotalInputNanos: 999999,
    ChangeAmountNanos: 999420,
    FeeNanos: 579
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
    TxnHashHex: "..."
}
```

### Burn Bitcoin

TODO

### Send DeSo

```text
POST /api/v0/send-deso
```

Prepare transaction for sending DeSo. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L470) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L837).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L817).

| Name | Type | Description |
| :--- | :--- | :--- |
| SenderPublicKeyBase58Check | string | Public key of the sender |
| RecipientPublicKeyOrUsername | string | Public key of the recipient |
| AmountNanos | int64 | transaction amount in nanos |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    TotalInputNanos: 2220387140,
    SpendAmountNanos: 2000000000
    ChangeAmountNanos: 220386848,
    FeeNanos: 579
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
    TxnHashHex: "...",
}
```

### Submit Post

```text
POST /api/v0/submit-post
```

Prepare transaction for submiting a post. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L644) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1100).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1061).

| Name | Type | Description |
| :--- | :--- | :--- |
| UpdaterPublicKeyBase58Check | string | Public key of the updater |
| PostHashHexToModify | string | \(optional\) Modified post's hash |
| ParentStakeID | string | \(optional\) |
| Title | string | \(optional\) |
| BodyObj | json | {Body: STRING, ImageURLs: \[\]} |
| RepostedPostHashHex | string | \(optional\) hash of post to modify |
| PostExtraData | json | \(optional\) extra data, values must be strings |
| IsHidden | bool |  |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    TstampNanos: 1623106441519911200,
    PostHashHex: "..."
    TotalInputNanos: 96669,
    ChangeAmountNanos: 96434,
    FeeNanos: 235,
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
}
```

### Create Follow Txn Stateless

```text
POST /api/v0/create-follow-txn-stateless
```

Prepare a follow/unfollow transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L855) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1331).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1314).

| Name | Type | Description |
| :--- | :--- | :--- |
| FollowerPublicKeyBase58Check | string | Public key of creator followed |
| FollowedPublicKeyBase58Check | string | Public key of follower |
| IsUnfollow | bool | false if follow / true if unfollow |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    TotalInputNanos: 220387362,
    ChangeAmountNanos: 220387140
    FeeNanos: 235,
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
}
```

### Creator Like Stateless

```text
POST /api/v0/create-like-stateless
```

Prepare a like/unlike transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L936) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1002).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L985).

| Name | Type | Description |
| :--- | :--- | :--- |
| ReaderPublicKeyBase58Check | string | Public key of reader |
| LikedPostHashHex | string | Hash of liked post |
| IsUnlike | bool | false if like / true if unlike |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    TotalInputNanos: 220387362,
    ChangeAmountNanos: 220387140
    FeeNanos: 235,
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
}
```

### Buy or Sell Creator Coin

```text
POST /api/v0/buy-or-sell-creator-coin
```

Prepare transaction for buying/selling creator coin. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1012) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1454).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1401).

| Name | Type | Description |
| :--- | :--- | :--- |
| UpdaterPublicKeyBase58Check | string | Public key of updater |
| CreatorPublicKeyBase58Check | string | Public key of creator |
| OperationType | string | "buy" or "sell" |
| DeSoToSellNanos | uint64 | Amount of DeSo to spend |
| CreatorCoinToSellNanos | uint64 | Amount of Creator Coin to spend |
| DeSoToAddNanos | uint64 | 0 |
| MinDeSoExpectedNanos | uint64 | 0 |
| MinCreatorCoinExpectedNanos | uint64 | 0 |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    ExpectedDeSoReturnedNanos: 0,
    ExpectedCreatorCoinReturnedNanos: 220387140
    FounderRewardGeneratedNanos: 0,
    FounderRewardGeneratedNanos    0
    SpendAmountNanos    285038185
    TotalInputNanos    1220385962
    ChangeAmountNanos    935347512,
    FeeNanos: 265
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
    TxnHashHex: "..."
}
```

### Transfer Creator Coin

```text
POST /api/v0/transfer-creator-coin
```

Prepare transaction for transfering creator coin. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1042) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1615).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1587).

| Name | Type | Description |
| :--- | :--- | :--- |
| SenderPublicKeyBase58Check | string | Public key of sender |
| CreatorPublicKeyBase58Check | string | Public key of creator |
| ReceiverUsernameOrPublicKeyBase58Check | string | username or public key of receiver |
| CreatorCoinToTransferNanos | uint64 | Amount of Creator Coin to transfer |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    SpendAmountNanos: 0,
    TotalInputNanos    355031025
    ChangeAmountNanos    355030764
    FeeNanos    261
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
    TxnHashHex: "..."
}
```

### Send Diamonds

```text
POST /api/v0/send-diamonds
```

Prepare transaction for sending diamonds 💎. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L954) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1750).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1739).

| Name | Type | Description |
| :--- | :--- | :--- |
| SenderPublicKeyBase58Check | string | Public key of sender |
| ReceiverPublicKeyBase58Check | string | Public key of receiver |
| DiamondPostHashHex | string | Hash of post receiving diamond |
| DiamondLevel | int64 | Diamond level |
| MinFeeRateNanosPerKB | uint64 | Rate per KB |

**Response**

```text
{
    SpendAmountNanos: 0,
    TotalInputNanos    355031025
    ChangeAmountNanos    355030764
    FeeNanos    261
    Transaction: {
        TxInputs : [ 
            {
                TxID: [...],
                Index: 0
            } , ...
        ],
        TxOutputs : [
            {
                PublicKey: "...",
                AmountNanos: 999420
            }, ...
        ],
        TxnMeta : {...},
        PublicKey: "...",
        ExtraData: {...},
        Signature: {...},
        TxnTypeJSON: 6
    },
    TransactionHex: "...",
    TxnHashHex: "..."
}
```

## User Endpoints

### Get Users Stateless

```text
POST /api/v0/get-users-stateless
```

Get information about users. Request contains a list of public keys of users to fetch. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L520) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L34).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L21).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeysBase58Check | \[\]string | list of public keys |
| SkipForLeaderboard | bool | Skips UsersYouHODL entry |

**Response**

```text
{
    UserList:[
        {
            PublicKeyBase58Check    "BC1YLg3FS19Syz9h6fqErZEtsKkRxBfkzqp75PiGwMUXJ1fLrytRVVk"
            ProfileEntryResponse    null
            Utxos    null
            BalanceNanos    0
            UnminedBalanceNanos    0
            PublicKeysBase58CheckFollowedByUser    []
            UsersYouHODL    null
            UsersWhoHODLYou    null
            HasPhoneNumber    false
            CanCreateProfile    true
            BlockedPubKeys    Object { }
            IsAdmin    true
            IsBlacklisted    false
            IsGraylisted    false
        }, ...
    ],
    DefaultFeeRateNanosPerKB: 100,
    ParamUpdaters: {...}
}
```

### Delete Identities

```text
POST /api/v0/delete-identities
```

Temporary route to wipe [seedinfo cookies](https://github.com/deso-protocol/docs/blob/main/code/walkthrough.md#seed-creation-and-transaction-construction). This endpoint relies on [identity api](https://github.com/deso-protocol/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L408) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L424).

**Parameters**

None

**Response**

None

### Get Profiles

```text
POST /api/v0/get-profiles
```

Get user profile information. Default number of returned profiles is 20. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L723) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L513).

**Parameters**

OrderBy possible values: `{"influencer_stake", "influencer_post_stake", "newest_last_post", "newest_last_comment", "influencer_coin_price"}`. More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L455).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | \(optional\) Check public key |
| Username | string | \(optional\) reader username |
| UsernamePrefix | string | \(optional\) username prefix |
| Description | string | \(optional\) description |
| OrderBy | string | Order ENUM |
| NumToFetch | uint32 | \(optional\) number of profiles to fetch |
| ReaderPublicKeyBase58Check | string | Reader public key |
| ModerationType | string | \(optional\) empty string or "leaderboard" |
| FetchUsersThatHODL | bool | If single profile is requested, return a list of HODLers |
| AddGlobalFeedBool | bool | If set to true posts in response will contain boolean if they are in global feed |

**Response**

```text
{
    ProfilesFound: [
        {
            PublicKeyBase58Check: "...",
            Username: "...",
            Description: "...",
            ProfilePic : "...",
            IsHidden: false,
            IsReserved: false,
            IsVerified: false,
            Comments: null,
            Posts: null,
            CoinEntry: {
                CreatorBasisPoints: 1000,
                DeSoLockedNanos: 0,
                NumberOfHolders: 0,
                CoinsInCirculationNanos: 0,
                CoinWatermarkNanos: 0
            },
            CoinPriceDeSoNanos: 0,
            StakeMultipleBasisPoints: 12500,
            StakeEntryStats: {
                TotalStakeNanos: 0, 
                TotalStakeOwedNanos: 0,
                TotalCreatorEarningsNanos: 0,
                TotalFeesBurnedNanos: 0,
                TotalPostStakeNanos: 0
            }
            UsersThatHODL: {...}
        }, ...
    ],
    NextPublicKey: null
}
```

### Get Single Profile

```text
POST /api/v0/get-single-profile
```

Get information about single profile. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L736) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L935).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L923).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | \(optional\) Check public key |
| Username | string | profile username |

**Response**

```text
{
    ProfilesFound: [
        {
            PublicKeyBase58Check: "...",
            Username: "...",
            Description: "...",
            ProfilePic : "...",
            IsHidden: false,
            IsReserved: false,
            IsVerified: false,
            Comments: null,
            Posts: null,
            CoinEntry: {
                CreatorBasisPoints: 1000,
                DeSoLockedNanos: 0,
                NumberOfHolders: 0,
                CoinsInCirculationNanos: 0,
                CoinWatermarkNanos: 0
            },
            CoinPriceDeSoNanos: 0,
            StakeMultipleBasisPoints: 12500,
            StakeEntryStats: {
                TotalStakeNanos: 0, 
                TotalStakeOwedNanos: 0,
                TotalCreatorEarningsNanos: 0,
                TotalFeesBurnedNanos: 0,
                TotalPostStakeNanos: 0
            }
            UsersThatHODL: {...}
        }, ...
    ],
    NextPublicKey: null
}
```

### Get Hodlers For Public Key

```text
POST /api/v0/get-hodlers-for-public-key
```

Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L736) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1030).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L996).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | \(optional\) check public key |
| Username | string | profile username |
| LastPublicKeyBase58Check | string | \(optional\) last public key check |
| NumToFetch | uint64 | number of records to fetch |
| FetchHodlings | bool | if true fetch balance for hodlings |
| FetchAll | bool | if true fetch all |

**Response**

```text
{
    Hodlers: [
        {
            HODLerPublicKeyBase58Check: "...",
            CreatorPublicKeyBase58Check: "...",
            HasPurchased: false,
            BalanceNanos: 2500509627,
            NetBalanceInMempool: 0,
            ProfileEntryResponse: {...}
        }, ...
    ],
    LastPublicKeyBase58Check: "..."
}
```

### Get Diamonds for Public Key

```text
POST /api/v0/get-diamonds-for-public-key
```

Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L970) and implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1171).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1158).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | Check public key |
| FetchYouDiamonded | bool | If true fetch diamonds this user gave out |

**Response**

```text
{
    DiamondSenderSummaryResponses: [
        {
            SenderPublicKeyBase58Check: "...",
            ReceiverPublicKeyBase58Check: "...",
            TotalDiamonds: "...",
            HighestDiamondLevel: "...",
            DiamondLevelMap: {...},
            ProfileEntryResponse: {...}
        }, ...
    ],
    TotalDiamonds: 555
}
```

### Get Follows Stateless

```text
POST /api/v0/get-follows-stateless
```

Get followers. Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L839) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1440).

**Parameters**

Either publickey or username can be set. More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1288).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | Check public key |
| Username | string | username |
| GetEntriesFollowingUsername | bool | Get entries following username |
| LastPublicKeyBase58Check | string | public key of last follower/followee |
| NumToFetch | uint64 | number of records to fetch |

**Response**

```text
{
    PublicKeyToProfileEntry: {
        "BC1YLfuD5AGm2guj3q5wF7WGi3jTUzNhHUHc84GtVsk9kHyxbnk5V1H" : {
            PublicKeyBase58Check: "...",
            Username: "...",
            Description: "...",
            ProfilePic : "...",
            IsHidden: false,
            IsReserved: false,
            IsVerified: false,
            Comments: null,
            Posts: null,
            CoinEntry: {
                CreatorBasisPoints: 1000,
                DeSoLockedNanos: 0,
                NumberOfHolders: 0,
                CoinsInCirculationNanos: 0,
                CoinWatermarkNanos: 0
            },
            CoinPriceDeSoNanos: 0,
            StakeMultipleBasisPoints: 12500,
            StakeEntryStats: {
                TotalStakeNanos: 0, 
                TotalStakeOwedNanos: 0,
                TotalCreatorEarningsNanos: 0,
                TotalFeesBurnedNanos: 0,
                TotalPostStakeNanos: 0
            },
            UsersThatHODL: {...}
        }, ...
    },
    NumFollowers: 17707
}
```

### Get User Global Metadata

```text
POST /api/v0/get-user-global-metadata
```

Get user metadata such as email and phone. This endpoint relies on [identity api](https://github.com/deso-protocol/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1131) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1523).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1507).

| Name | Type | Description |
| :--- | :--- | :--- |
| UserPublicKeyBase58Check | string | user public key |
| JWT | string | JSON web token authenticating user |

**Response**

```text
{
    Email: "...",
    PhoneNumber: "..."
}
```

### Update User Global Meta

```text
POST /api/v0/update-user-global-metadata
```

TODO

### Get Notifications

```text
POST /api/v0/get-notifications
```

Get user notifications. This endpoint relies on [identity api](https://github.com/deso-protocol/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1099) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1670).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1655).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | user public key |
| FetchStartIndex | int64 | Index of notification at which to start paginated lookup. Can set to -1 |
| NumToFetch | int64 | Number of notifications to fetch |

**Response**

```text
{
    Notifications : [
        {
            Metadata : {
                BlockHashHex: "...",
                TxnIndexInBlock: 3921,
                TxnType: "FOLLOW",
                TransactorPublicKeyBase58Check: "...",
                AffectedPublicKeys: [...],
                BasicTransferTxindexMetadata: {...},
                TotalInputNanos: 89585061,
                TotalOutputNanos: 89584839,
                FeeNanos: 222,
                UtxoOpsDump: "..."
            },
            Txn: null,
            Index: 50
        }, ...
    ],
    ProfilesByPublicKey: {
        "BC1YLfuD5AGm2guj3q5wF7WGi3jTUzNhHUHc84GtVsk9kHyxbnk5V1H" : {
            PublicKeyBase58Check: "...",
            Username: "...",
            Description: "...",
            ProfilePic : "...",
            IsHidden: false,
            IsReserved: false,
            IsVerified: false,
            Comments: null,
            Posts: null,
            CoinEntry: {
                CreatorBasisPoints: 1000,
                DeSoLockedNanos: 0,
                NumberOfHolders: 0,
                CoinsInCirculationNanos: 0,
                CoinWatermarkNanos: 0
            },
            CoinPriceDeSoNanos: 0,
            StakeMultipleBasisPoints: 12500,
            StakeEntryStats: {
                TotalStakeNanos: 0, 
                TotalStakeOwedNanos: 0,
                TotalCreatorEarningsNanos: 0,
                TotalFeesBurnedNanos: 0,
                TotalPostStakeNanos: 0
            },
            UsersThatHODL: {...}
        }, ...
    },
    PostsByHash: {
        "5782badd48b3db0ea3074fa6339e1a726265bdfbd86ed38e1e55691f2d79b296" : {
            PostHashHex: "...",
            PosterPublicKeyBase58Check: "...",
            ParentStakeID : "",
            Body: "...",
            ImageURLs: [],
            RepostedPostEntryResponse: null,
            CreatorBasisPoints: 1000,
            StakeMultipleBasisPoints: 12500,
            TimestampNanos: 1623010583195063300,
            IsHidden: false,
            ConfirmationBlockHeight: 31919,
            InMempool: false,
            StakeEntry: {...},
            StakeEntryStats: {...},
            ProfileEntryResponse: {...},
            Comments: null,
            LikeCount: 1,
            DiamondCount: 1,
            PostEntryReaderState: {...},
            IsPinned: false,
            PostExtraData: {...},
            CommentCount: 0,
            RepostCount: 0,
            ParentPosts: null,
            DiamondsFromSender: 0
        }
    }
}
```

### Block Public Key

```text
POST /api/v0/block-public-key
```

Block user. This endpoint relies on [identity api](https://github.com/deso-protocol/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1099) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L1670).

**Parameters**

More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/user.go#L2153).

| Name | Type | Description |
| :--- | :--- | :--- |
| PublicKeyBase58Check | string | user public key |
| BlockPublicKeyBase58Check | string | blocked user public key |
| Unblock | bool | false if block, true if unblock |
| JWT | string | JSON web token authenticating user |

**Response**

```text
{
    BlockedPublicKeys: {
        "BC1YLhqEhWvNnwW9TBqXURFqwkdpUYKrMVgTHQzopF5rRBDcD1LLSUp": {...},
        ...
    }
}
```

## Post Endpoints

### Get Posts Stateless

```text
POST /api/v0/get-posts-stateless
```

TODO

### Get Single Post

```text
POST /api/v0/get-single-post
```

TODO

### Get Posts For Public Key

```text
POST /api/v0/get-posts-for-public-key
```

TODO

### Get Diamonded Posts

```text
POST /api/v0/get-diamonded-posts
```

TODO

## Media Endpoints

### Upload Image

```text
POST /api/v0/upload-image
```

TODO

### Get Full TikTok URL

```text
POST /api/v0/get-full-tiktok-url
```

TODO

## Message Endpoints

### Send Message Stateless

```text
POST /api/v0/send-message-stateless
```

TODO

### Get Messages Stateless

```text
POST /api/v0/get-messages-stateless
```

### Mark Contact Messages Read

```text
POST /api/v0/mark-contact-messages-read
```

TODO

### Mark All Messages Read

```text
POST /api/v0/mark-all-messages-read
```

TODO

## Verify Endpoints

### Send Phone Number Verification Text

```text
POST /api/v0/send-phone-number-verification-text
```

TODO

### Submit Phone Number Verification Text

```text
POST /api/v0/submit-phone-number-verification-code
```

TODO

## Wyre Endpoints

### Get Wyre Wallet Order Quotation

```text
POST /api/v0/get-wyre-wallet-order-quotation
```

### Get Wyre Wallet Order Reservation

```text
POST /api/v0/get-wyre-wallet-order-reservation
```

TODO

### Wyre Wallet Order Subscription

```text
POST /api/v0/wyre-wallet-order-subscription
```

TODO

### Admin Get Wyre Orders For Public Key

```text
POST /api/v0/admin/get-wyre-wallet-orders-for-public-key
```

TODO

## Miner Endpoints

### Get Block Template

```text
POST /api/v0/get-block-template
```

TODO

### Submit Block

```text
POST /api/v0/submit-block
```

TODO

## Admin Node Endpoints

### Node Control

```text
POST /api/v0/admin/node-control
```

TODO

### Reprocess Bitcoin Block

```text
POST /api/v0/admin/reprocess-bitcoin-block
```

TODO

### Get Mempool Stats

```text
POST /api/v0/admin/get-mempool-stats
```

TODO

### Evict Unmined Bitcoin Txns

```text
POST /api/v0/admin/evict-unmined-bitcoin-txns
```

TODO

## Admin Transaction Endpoints

### Get Global Params

```text
POST /api/v0/admin/get-global-params
```

TODO

### Update Global Params

```text
POST /api/v0/admin/update-global-params
```

TODO

### Swap Identity

```text
POST /api/v0/admin/swap-identity
```

TODO

## Admin User Endpoints

### Update User Global Metadata

```text
POST /api/v0/admin/update-user-global-metadata
```

TODO

### Get All User Global Metadata

```text
POST /api/v0/admin/get-all-user-global-metadata
```

TODO

### Get User Global Metadata

```text
POST /api/v0/admin/get-user-global-metadata
```

TODO

### Grant Verification Badge

```text
POST /api/v0/admin/grant-verification-badge
```

TODO

### Remove Verification Badge

```text
POST /api/v0/admin/remove-verification-badge
```

TODO

### Get Verified Users

```text
POST /api/v0/admin/get-verified-users
```

TODO

### Get Username Verification Audit Logs

```text
POST /api/v0/admin/get-username-verification-audit-logs
```

TODO

## Admin Feed Endpoints

### Update Global Feed

```text
POST /api/v0/admin/update-global-feed
```

TODO

### Pin Post

```text
POST /api/v0/admin/pin-post
```

TODO

### Remove Nil Posts

```text
POST /api/v0/admin/remove-nil-posts
```

TODO

