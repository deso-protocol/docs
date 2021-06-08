<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Backend API](#backend-api)
    - [General Endpoints](#general-endpoints)
        - [Index](#index)
        - [Health Check](#health-check)
        - [Get Exchange Rate](#get-exchange-rate)
        - [Get App State](#get-app-state)
    - [Transaction Endpoints](#transaction-endpoints)
        - [Get Txn](#get-txn)
        - [Submit Transaction](#submit-transaction)
        - [Update Profile](#update-profile)
        - [Burn Bitcoin](#burn-bitcoin)
        - [Send BitClout](#send-bitclout)
        - [Submit Post](#submit-post)
        - [Create Follow Txn Stateless](#create-follow-txn-stateless)
        - [Creator Like Stateless](#creator-like-stateless)
        - [Buy or Sell Creator Coin](#buy-or-sell-creator-coin)
        - [Transfer Creator Coin](#transfer-creator-coin)
        - [Send Diamonds](#send-diamonds)
    - [User Endpoints](#user-endpoints)
        - [Get Users Stateless](#get-users-stateless)
        - [Delete Identities](#delete-identities)
        - [Get Profiles](#get-profiles)
        - [Get Single Profile](#get-single-profile)
        - [Get Hodlers For Public Key](#get-hodlers-for-public-key)
        - [Get Diamonds for Public Key](#get-diamonds-for-public-key)
        - [Get Follows Stateless](#get-follows-stateless)
        - [Get User Global Metadata](#get-user-global-metadata)
        - [Update User Global Meta](#update-user-global-meta)
        - [Get Notifications](#get-notifications)
        - [Block Public Key](#block-public-key)
    - [Post Endpoints](#post-endpoints)
        - [Get Posts Stateless](#get-posts-stateless)
        - [Get Single Post](#get-single-post)
        - [Get Posts For Public Key](#get-posts-for-public-key)
        - [Get Diamonded Posts](#get-diamonded-posts)
    - [Media Endpoints](#media-endpoints)
        - [Upload Image](#upload-image)
        - [Get Full TikTok URL](#get-full-tiktok-url)
    - [Message Endpoints](#message-endpoints)
        - [Send Message Stateless](#send-message-stateless)
        - [Get Messages Stateless](#get-messages-stateless)
        - [Mark Contact Messages Read](#mark-contact-messages-read)
        - [Mark All Messages Read](#mark-all-messages-read)
    - [Verify Endpoints](#verify-endpoints)
        - [Send Phone Number Verification Text](#send-phone-number-verification-text)
        - [Submit Phone Number Verification Text](#submit-phone-number-verification-text)
    - [Wyre Endpoints](#wyre-endpoints)
        - [Get Wyre Wallet Order Quotation](#get-wyre-wallet-order-quotation)
        - [Get Wyre Wallet Order Reservation](#get-wyre-wallet-order-reservation)
        - [Wyre Wallet Order Subscription](#wyre-wallet-order-subscription)
        - [Admin Get Wyre Orders For Public Key](#admin-get-wyre-orders-for-public-key)
    - [Miner Endpoints](#miner-endpoints)
        - [Get Block Template](#get-block-template)
        - [Submit Block](#submit-block)
    - [Admin Node Endpoints](#admin-node-endpoints)
        - [Node Control](#node-control)
        - [Reprocess Bitcoin Block](#reprocess-bitcoin-block)
        - [Get Mempool Stats](#get-mempool-stats)
        - [Evict Unmined Bitcoin Txns](#evict-unmined-bitcoin-txns)
    - [Admin Transaction Endpoints](#admin-transaction-endpoints)
        - [Get Global Params](#get-global-params)
        - [Update Global Params](#update-global-params)
        - [Swap Identity](#swap-identity)
    - [Admin User Endpoints](#admin-user-endpoints)
        - [Update User Global Metadata](#update-user-global-metadata)
        - [Get All User Global Metadata](#get-all-user-global-metadata)
        - [Get User Global Metadata](#get-user-global-metadata-1)
        - [Grant Verification Badge](#grant-verification-badge)
        - [Remove Verification Badge](#remove-verification-badge)
        - [Get Verified Users](#get-verified-users)
        - [Get Username Verification Audit Logs](#get-username-verification-audit-logs)
    - [Admin Feed Endpoints](#admin-feed-endpoints)
        - [Update Global Feed](#update-global-feed)
        - [Pin Post](#pin-post)
        - [Remove Nil Posts](#remove-nil-posts)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Backend API
## General Endpoints
### Index
```
GET /
```
Basic endpoint to test if your BitClout node is running.
#### Parameters
None
#### Response
```
Your BitClout node is running!
```

------------


### Health Check
```
GET /api/v0/health-check
```
Check if your BitClout node is synced
#### Parameters:
None
#### Response:
If node is synced and received all transactions.
```
200
```

------------

### Get Exchange Rate
```
GET /api/v0/get-exchange-rate
```
Get BitClout exchange rate, total amount of nanos sold, and Bitcoin exchange rate.
#### Parameters:
None
#### Response:
```
{
	"SatoshisPerBitCloutExchangeRate":498484,
	"NanosSold":8491518125648433,
	"USDCentsPerBitcoinExchangeRate":3608200
}
```

------------

### Get App State
```
POST /api/v0/get-app-state
```
Get state of BitClout App, such as cost of profile creation and diamond level map. Example use in the [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1106) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/base.go#L86).
#### Parameters
None; however, you need to send an empty JSON `{ }`. Otherwise, you will get 400 - Bad Request. More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/base.go#L63).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | (optional) check public key |

#### Response
```
{
    "AmplitudeKey": "",
    "AmplitudeDomain": "api.amplitude.com",
    "MinSatoshisBurnedForProfileCreation": 50000,
    "IsTestnet": false,
    "SupportEmail": "node.admin@protonmail.com",
    "ShowProcessingSpinners": true,
    "HasStarterBitCloutSeed": false,
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
```
POST /api/v0/get-txn
```
Check if Txn is currently in mempool. Example use in the [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/app.component.ts#L291) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L34).
#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L25).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  TxnHashHex | string | Transaction hash |

#### Response
```
{
    "TxnFound": true
}
```

------------
### Submit Transaction
```
POST /api/v0/submit-transaction
```
Submit transaction to BitClout blockchain. Example use in [frontend](`https://github.com/bitclout/frontend/blob/96bdf0c40e05010ec62a1b1cdc78bf0d0fb2ef44/src/app/backend-api.service.ts#L496`) and endpoint implementation in [backend](`https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L81`).
#### Parameters
Read more on transaction format [here](https://github.com/bitclout/docs/blob/main/code/walkthrough.md#transaction-format). More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L69).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  TransactionHex | string | Transaction hash |

#### Response
```
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


------------

### Update Profile
```
POST /api/v0/update-profile
```
Update profile fields and receive corresponding Txn. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L816) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L247).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L214).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  UpdaterPublicKeyBase58Check | string |  Public key of updater |
|  ProfilePublicKeyBase58Check | string |  (optional) Public key of the profile if different from updater |
|  NewUsername | string |  Username |
|  NewDescription | string |  Description |
|  NewProfilePic | string |  Profile picture |
|  NewCreatorBasisPoints | uint64 |  Creator Reward |
|  NewStakeMultipleBasisPoints | uint64 |  Staking Reward |
|  IsHidden | bool |   |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB |

#### Response
```
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

------------

### Burn Bitcoin
TODO


------------

### Send BitClout
```
POST /api/v0/send-bitclout
```
Prepare transaction for sending BitClout. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L470) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L837).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L817).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  SenderPublicKeyBase58Check | string |  Public key of the sender |
|  RecipientPublicKeyOrUsername | string |  Public key of the recipient |
|  AmountNanos | int64 | transaction amount in nanos  |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB |

#### Response
```
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

------------

### Submit Post
```
POST /api/v0/submit-post
```
Prepare transaction for submiting a post. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L644) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1100).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1061).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  UpdaterPublicKeyBase58Check | string |  Public key of the updater |
|  PostHashHexToModify | string |  (optional) Modified post's hash |
|  ParentStakeID | string | (optional)  |
|  Title | string |  (optional) |
|  BodyObj | json |  {Body: STRING, ImageURLs: []} |
|  RecloutedPostHashHex | string |  (optional) hash of post to modify |
|  PostExtraData | json | (optional) extra data  |
|  IsHidden | bool |  |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB |

#### Response

```
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

------------

### Create Follow Txn Stateless
```
POST /api/v0/create-follow-txn-stateless
```
Prepare a follow/unfollow transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L855) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1331).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1314).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  FollowerPublicKeyBase58Check | string |  Public key of creator followed |
|  FollowedPublicKeyBase58Check | string |  Public key of follower |
|  IsUnfollow | bool | false if follow / true if unfollow |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB  |

#### Response
```
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

------------

### Creator Like Stateless
```
POST /api/v0/create-like-stateless
```
Prepare a like/unlike transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L936) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1002).
#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L985).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  ReaderPublicKeyBase58Check | string |  Public key of reader |
|  LikedPostHashHex | string |  Hash of liked post |
|  IsUnlike | bool | false if like / true if unlike |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB  |

#### Response
```
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

------------

### Buy or Sell Creator Coin
```
POST /api/v0/buy-or-sell-creator-coin
```
Prepare transaction for buying/selling creator coin. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1012) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1454).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1401).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  UpdaterPublicKeyBase58Check | string |  Public key of updater |
|  CreatorPublicKeyBase58Check | string |  Public key of creator |
|  OperationType | string | "buy" or "sell" |
|  BitCloutToSellNanos | uint64 |  Amount of BitClout to spend  |
|  CreatorCoinToSellNanos | uint64 |  Amount of Creator Coin to spend |
|  BitCloutToAddNanos | uint64 |  0 |
|  MinBitCloutExpectedNanos | uint64 | 0 |
|  MinCreatorCoinExpectedNanos | uint64 |  0  |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB  |

#### Response
```
{
	ExpectedBitCloutReturnedNanos: 0,
	ExpectedCreatorCoinReturnedNanos: 220387140
	FounderRewardGeneratedNanos: 0,
	FounderRewardGeneratedNanos	0
	SpendAmountNanos	285038185
	TotalInputNanos	1220385962
	ChangeAmountNanos	935347512,
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

------------

### Transfer Creator Coin
```
POST /api/v0/transfer-creator-coin
```
Prepare transaction for transfering creator coin. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1042) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1615).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1587).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  SenderPublicKeyBase58Check | string |  Public key of sender |
|  CreatorPublicKeyBase58Check | string |  Public key of creator |
|  ReceiverUsernameOrPublicKeyBase58Check | string | username or public key of receiver |
|  CreatorCoinToTransferNanos | uint64 |  Amount of Creator Coin to transfer  |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB  |

#### Response
```
{
	SpendAmountNanos: 0,
	TotalInputNanos	355031025
	ChangeAmountNanos	355030764
	FeeNanos	261
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

------------

### Send Diamonds
```
POST /api/v0/send-diamonds
```
Prepare transaction for sending diamonds 💎. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L954) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1750).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/transaction.go#L1739).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  SenderPublicKeyBase58Check | string |  Public key of sender |
|  ReceiverPublicKeyBase58Check | string |  Public key of receiver |
|  DiamondPostHashHex | string | Hash of post receiving diamond |
|  DiamondLevel | int64 |  Diamond level |
|  MinFeeRateNanosPerKB | uint64 |  Rate per KB  |

#### Response
```
{
	SpendAmountNanos: 0,
	TotalInputNanos	355031025
	ChangeAmountNanos	355030764
	FeeNanos	261
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
```
POST /api/v0/get-users-stateless
```
Get information about users. Request contains a list of public keys of users to fetch. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L520) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L34).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L21).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeysBase58Check | []string |  list of public keys  |
|  SkipHodlings | bool |  HODL |

#### Response
```
{
	UserList:[
		{
			PublicKeyBase58Check	"BC1YLg3FS19Syz9h6fqErZEtsKkRxBfkzqp75PiGwMUXJ1fLrytRVVk"
			ProfileEntryResponse	null
			Utxos	null
			BalanceNanos	0
			UnminedBalanceNanos	0
			PublicKeysBase58CheckFollowedByUser	[]
			UsersYouHODL	null
			UsersWhoHODLYou	null
			HasPhoneNumber	false
			CanCreateProfile	true
			BlockedPubKeys	Object { }
			IsAdmin	true
			IsBlacklisted	false
			IsGraylisted	false
		}, ...
	],
	DefaultFeeRateNanosPerKB: 100,
	ParamUpdaters: {...}
}
```

------------

### Delete Identities
```
POST /api/v0/delete-identities
```
Temporary route to wipe [seedinfo cookies](https://github.com/bitclout/docs/blob/main/code/walkthrough.md#seed-creation-and-transaction-construction). This endpoint relies on [identity api](https://github.com/bitclout/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L408) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L424).

#### Parameters
None

#### Response
None

------------

### Get Profiles
```
POST /api/v0/get-profiles
```
Get user profile information. Default number of returned profiles is 20. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L723) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L513).

#### Parameters
OrderBy possible values: `{"influencer_stake",  "influencer_post_stake", "newest_last_post", "newest_last_comment", "influencer_coin_price"}`. More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L455).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | (optional) Check public key |
|  Username | string |  (optional) reader username |
|  UsernamePrefix | string |  (optional) username prefix  |
|  Description | string |  (optional) description |
|  OrderBy | string |  Order ENUM  |
|  NumToFetch | uint32 |  (optional) number of profiles to fetch |
|  ReaderPublicKeyBase58Check | string |  Reader public key  |
|  ModerationType | string |  (optional) empty string or "leaderboard" |
|  FetchUsersThatHODL | bool |  If single profile is requested, return a list of HODLers |
|  AddGlobalFeedBool | bool |  If set to true posts in response will contain boolean if they are in global feed |

#### Response
```
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
				BitCloutLockedNanos: 0,
				NumberOfHolders: 0,
				CoinsInCirculationNanos: 0,
				CoinWatermarkNanos: 0
			},
			CoinPriceBitCloutNanos: 0,
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

------------

### Get Single Profile
```
POST /api/v0/get-single-profile
```
Get information about single profile. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L736) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L935).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L923).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | (optional) Check public key |
|  Username | string |  profile username |

#### Response
```
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
				BitCloutLockedNanos: 0,
				NumberOfHolders: 0,
				CoinsInCirculationNanos: 0,
				CoinWatermarkNanos: 0
			},
			CoinPriceBitCloutNanos: 0,
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

------------

### Get Hodlers For Public Key
```
POST /api/v0/get-hodlers-for-public-key
```
Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L736) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1030).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L996).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | (optional) check public key |
|  Username | string |  profile username |
|  LastPublicKeyBase58Check | string | (optional) last public key check |
|  NumToFetch | uint64  |  number of records to fetch |
|  FetchHodlings | bool | if true fetch balance for hodlings |
|  FetchAll | bool |  if true fetch all |

#### Response
```
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

------------

### Get Diamonds for Public Key
```
POST /api/v0/get-diamonds-for-public-key
```
Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L970) and implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1171).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1158).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | Check public key |
|  FetchYouDiamonded | bool | If true fetch diamonds this user gave out |

#### Response
```
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

------------

### Get Follows Stateless
```
POST /api/v0/get-follows-stateless
```
Get followers. Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L839) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1440).

#### Parameters
Either publickey or username can be set. More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1288).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | Check public key |
|  Username | string | username |
|  GetEntriesFollowingUsername | bool | Get entries following username |
|  LastPublicKeyBase58Check | string  | public key of last follower/followee |
|  NumToFetch | uint64 | number of records to fetch |

#### Response
```
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
				BitCloutLockedNanos: 0,
				NumberOfHolders: 0,
				CoinsInCirculationNanos: 0,
				CoinWatermarkNanos: 0
			},
			CoinPriceBitCloutNanos: 0,
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

------------

### Get User Global Metadata
```
POST /api/v0/get-user-global-metadata
```
Get user metadata such as email and phone. This endpoint relies on [identity api](https://github.com/bitclout/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1131) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1523).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1507).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  UserPublicKeyBase58Check | string | user public key |
|  JWT | string | JSON web token authenticating user |

#### Response
```
{
	Email: "...",
	PhoneNumber: "..."
}
```

------------

### Update User Global Meta
```
POST /api/v0/update-user-global-metadata
```
TODO


------------

### Get Notifications
```
POST /api/v0/get-notifications
```
Get user notifications. This endpoint relies on [identity api](https://github.com/bitclout/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1099) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1670).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1655).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | user public key |
|  FetchStartIndex | int64 | Index of notification at which to start paginated lookup. Can set to -1 |
|  NumToFetch | int64 | Number of notifications to fetch |

#### Response
```
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
				BitCloutLockedNanos: 0,
				NumberOfHolders: 0,
				CoinsInCirculationNanos: 0,
				CoinWatermarkNanos: 0
			},
			CoinPriceBitCloutNanos: 0,
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
			RecloutedPostEntryResponse: null,
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
			RecloutCount: 0,
			ParentPosts: null,
			DiamondsFromSender: 0
		}
	}
}
```

------------

### Block Public Key
```
POST /api/v0/block-public-key
```
Block user. This endpoint relies on [identity api](https://github.com/bitclout/docs/blob/main/devs/identity-api.md). Example use in [frontend](https://github.com/bitclout/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1099) and endpoint implementation in [backend](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L1670).

#### Parameters
More info on the request [here](https://github.com/bitclout/backend/blob/47bcc8a/routes/user.go#L2153).

| Name  | Type  | Description |
| :------------ |:---------------| -----|
|  PublicKeyBase58Check | string | user public key |
|  BlockPublicKeyBase58Check | string | blocked user public key |
|  Unblock | bool | false if block, true if unblock |
|  JWT | string | JSON web token authenticating user |

#### Response
```
{
	BlockedPublicKeys: {
		"BC1YLhqEhWvNnwW9TBqXURFqwkdpUYKrMVgTHQzopF5rRBDcD1LLSUp": {...},
		...
	}
}
```

## Post Endpoints
### Get Posts Stateless
```
POST /api/v0/get-posts-stateless
```
TODO

------------

### Get Single Post
```
POST /api/v0/get-single-post
```
TODO

------------

### Get Posts For Public Key
```
POST /api/v0/get-posts-for-public-key
```
TODO

------------

### Get Diamonded Posts
```
POST /api/v0/get-diamonded-posts
```
TODO

## Media Endpoints
### Upload Image
```
POST /api/v0/upload-image
```
TODO

------------

### Get Full TikTok URL
```
POST /api/v0/get-full-tiktok-url
```
TODO

## Message Endpoints
### Send Message Stateless
```
POST /api/v0/send-message-stateless
```
TODO

------------

### Get Messages Stateless
```
POST /api/v0/get-messages-stateless
```

------------

### Mark Contact Messages Read
```
POST /api/v0/mark-contact-messages-read
```
TODO

------------

### Mark All Messages Read
```
POST /api/v0/mark-all-messages-read
```
TODO

## Verify Endpoints
### Send Phone Number Verification Text
```
POST /api/v0/send-phone-number-verification-text
```
TODO

------------

### Submit Phone Number Verification Text
```
POST /api/v0/submit-phone-number-verification-code
```
TODO

## Wyre Endpoints
### Get Wyre Wallet Order Quotation
```
POST /api/v0/get-wyre-wallet-order-quotation
```

------------

### Get Wyre Wallet Order Reservation
```
POST /api/v0/get-wyre-wallet-order-reservation
```
TODO

------------

### Wyre Wallet Order Subscription
```
POST /api/v0/wyre-wallet-order-subscription
```
TODO

------------

### Admin Get Wyre Orders For Public Key
```
POST /api/v0/admin/get-wyre-wallet-orders-for-public-key
```
TODO

## Miner Endpoints
### Get Block Template
```
POST /api/v0/get-block-template
```
TODO

------------

### Submit Block
```
POST /api/v0/submit-block
```
TODO

## Admin Node Endpoints
### Node Control
```
POST /api/v0/admin/node-control
```
TODO

------------

### Reprocess Bitcoin Block
```
POST /api/v0/admin/reprocess-bitcoin-block
```
TODO

------------

### Get Mempool Stats
```
POST /api/v0/admin/get-mempool-stats
```
TODO

------------

### Evict Unmined Bitcoin Txns
```
POST /api/v0/admin/evict-unmined-bitcoin-txns
```
TODO

------------

## Admin Transaction Endpoints
### Get Global Params
```
POST /api/v0/admin/get-global-params
```
TODO

------------

### Update Global Params
```
POST /api/v0/admin/update-global-params
```
TODO

------------

### Swap Identity
```
POST /api/v0/admin/swap-identity
```
TODO

------------

## Admin User Endpoints
### Update User Global Metadata
```
POST /api/v0/admin/update-user-global-metadata
```
TODO

------------

### Get All User Global Metadata
```
POST /api/v0/admin/get-all-user-global-metadata
```
TODO

------------

### Get User Global Metadata
```
POST /api/v0/admin/get-user-global-metadata
```
TODO

------------

### Grant Verification Badge
```
POST /api/v0/admin/grant-verification-badge
```
TODO

------------

### Remove Verification Badge
```
POST /api/v0/admin/remove-verification-badge
```
TODO

------------

### Get Verified Users
```
POST /api/v0/admin/get-verified-users
```
TODO

------------

### Get Username Verification Audit Logs
```
POST /api/v0/admin/get-username-verification-audit-logs
```
TODO

## Admin Feed Endpoints
### Update Global Feed
```
POST /api/v0/admin/update-global-feed
```
TODO

------------

### Pin Post
```
POST /api/v0/admin/pin-post
```
TODO

------------

### Remove Nil Posts
```
POST /api/v0/admin/remove-nil-posts
```
TODO
