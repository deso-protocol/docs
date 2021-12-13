---
description: Description of endpoints used to get notification data on the DeSo blockchain
---

# Notification Endpoints

{% swagger method="post" path="" baseUrl="/api/v0/get-notifications" summary="Get Notifications" %}
{% swagger-description %}
Get user notifications.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1877).

Example usages in frontend:\
&#x20; \- Make request to [Get Notifications](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1605)\
&#x20; \- Use GetNotifications to [get the next page of notifications for a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/notifications-page/notifications-list/notifications-list.component.ts#L46)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchStartIndex" type="int64" %}
Index of notification at which to start paginated lookup. Can set to -1
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="int64" required="true" %}
Number of notifications to fetch
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FilteredOutNotificationCategories" type="map[string]bool" %}
map of category name to boolean indicating whether or not to include this category of transaction types. if not provided, no filtering occurs.

\




\


Category names and transaction types included in category:

\


  \- 

`diamond`

: 

`BasicTransfer`

 or 

`CreatorCoinTransfer`

 transactions that have appropriate diamond extra data.

\


  \- 

`transfer`

: 

`BasicTransfer`

 or 

`CreatorCoinTransfer`

 transactions that do not have diamond extra data OR 

`CreatorCoin`

 (buy or sell) transactions.

\


  \- 

`post`

: 

`Post`

 transactions

\


  \- 

`follow`

: 

`Follow`

 transactions

\


  \- 

`like`

: 

`Like`

 transactions

\


  \- 

`nft`

: 

`NFTBid,`

 

`AcceptNFTBid`

,  or 

`NFTTransfer`

 transactions
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the next page of notifications" %}
{% tabs %}
{% tab title="Sample Response" %}


```json5
{
  "LastSeenIndex": 100, // Index of the last notification the user has seen
  "Notifications": [
    {
      "Index": 99, // Index of this notification in the list of all notifications for the given public key
      "Metadata": { // Metadata that describe the transaction
        "AffectedPublicKeys": [{
          "Metadata": "BasicTransferOutput", // For a list of all AffectedPublicKey Metadata values - see over here...,
          "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", 
        }],
        "BlockHashHex": "0000000000006a8e90b36c0ac36dbe9bc9f97252ab5418abc4d0e2bb2e3d923f", // Hex of the block hash
        "TransactorPublicKeyBase58Check": "BC1YLfhPNqmoAUSDwoQBJrAZRbW4XFA7db89Awpr7GJdN9pWcBZVweQ", // Public key of the user who created the transaction that generated this notification
        "TxnIndexInBlock": 850, // Index of the transaction in the block for which this notification was generated
        "TxnOutputs": [
          {
            "PublicKey": "AgNw/nCxUVblm/dDy0qGleAUqZfdIdiuOBtALN4TdhII", // Public key bytes - this is not particularly useful.
            "AmountNanos": 792289, // Amount of Deso in nanos in this transaction output.
          }
        ],
        "BasicTransferTxindexMetadata": { // Describes basic transfer in transaction. Appears for all notifications
          "TotalInputNanos": 100, // Total DeSo nanos in the inputs of this transaction
          "TotalOutputNanos": 90, // Total Deso nanos in the outputs of this transaction
          "FeeNanos": 20, // Total fees of this transaction
          "UtxoOpsDump": "",
          "UtxoOps": <UtxoOperation>, // UTXO operation to be documented
          "DiamondLevel": 1, // Number of diamonds given by this basic transfer
          "PostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290" // Hex of Post hash for which diamonds were given by this transaction
        },
        "BitcoinExchangeTxindexMetadata": { // Describes bitcoin exchange transaction. Only appears for Bitcoin Exchange transactions
          "BitcoinSpendAddress": "1B9MXT8sME3Z7kNLTfRDqeKdcUKiF1qhPr", // Address from which BTC was burned
          "SatoshisBurned": 100, // Total Satoshis burned by the transactor
          "NanosCreated": 10, // Total nanos created by this transaction. Note: the DeSo supply is now fixed.
          "TotalNanosPurchasedBefore": 10000, // Total nanos minted by the DeSo protocol before this transaction. Note: the DeSo supply is now fixed.
          "TotalNanosPurchasedAfter": 1010, // Total nanos minted by the DeSo protocol after this transaction. Note: the DeSo supply is now fixed. 
          "BitcoinTxnHash": "833ec95b45707667d29d9cecfdc8dd86006ab0fc8214f395e206437902a9cc9f" // Hash of BTC transaction that exchanged BTC for DeSo
        },
        "CreatorCoinTxindexMetadata": { // Describes creator coin buy/sell transaction. Only appears for Creator coin transactions.
          "OperationType": "buy", // Valid values are "buy" and "sell". "buy" means this transaction was a creator coin purchase. "sell" means this transaction was a creator coin sale.
          "DeSoToSellNanos": 1000, // The amount of DeSo in nanos spent to purchase creator coins. This is only populated if OperationType is "buy"
          "CreatorCoinToSellNanos": 0, // The amount of creator coin in nanos sold. This is only populated if the OperationType is "sell"
          "DeSoToAddNanos": 0, // Note: this field is not currently used.
          "DESOLockedNanosDiff": 1000, // diff in the amount of DeSo locked in a creator coin as a result of this transaction
        },
        "CreatorCoinTransferTxindexMetadata": { // Describes creator coin transfer transaction. Only appears for Creator coin transfer transactions
          "CreatorUsername": "LazyNina", // User of the creator whose coins are transferred in this transaction
          "CreatorCoinToTransferNanos": 1000, // Amount of creator coins of CreatorUsername that were transferred in this transaction
          "DiamondLevel": 2, // Number of diamonds given by this creator coin transfer. Note: diamonds are no longer given in Creator Coins
          "PostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290", // Hex of Post hash for which diamonds were given by this transaction. Note: diamonds are no longer given in creator coins.
        },
        "UpdateProfileTxindexMetadata": { // Describe update profile transaction. Only appears for Update profile transactions.
          "ProfilePublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user whose profile was updated
          "NewUsername": "LazyNina", // New username in the user's profile
          "NewDescription": "reading the paper, going to movies", //  New description in the user's profile
          "NewProfilePic": "", // Base64 encoded profile picture in the user's profile
          "NewCreatorBasisPoints": 1000, // Founder reward for the user in basis points.
          "NewStakeMultipleBasisPoints": 0, // Deprecated
          "IsHidden": false, // Whether this profile is hidden or not
        },
        "SubmitPostTxindexMetadata": { // Describes submit post transaction. Only appears for submit post transactions.
          "PostHashBeingModifiedHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290", // Hex of Post hash that is created by or modified by this transaction
          "ParentPostHashHex": "", // Hex of Post hash that is the parent of PostHashBeingModifiedHex if there is one.
        },
        "LikeTxindexMetadata": { // Describes like transaction. Only appears for like transactions.
          "IsUnlike": false, // If true, this transaction remove an existing like on a post. If false, this transaction add a like on a post.
          "PostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290" // Hex of Post hash that is liked (or unliked) by this transaction.
        },
        "FollowTxindexMetadata": { // Describes follow transaction. Only appears for follow transactions.
          "IsUnfollow": false, // If true, this transaction removes a creator from a user's following list. If false, this transaction add a creator to a user's following list.
        },
        "PrivateMessageTxindexMetadata": { // Describes private message transaction. Only appears for private message transactions.
          "TimestampNanos": 1000, // Timestamp of private message
        },
        "SwapidentityTxindexMetadata": { // Describes swap identity transaction. Only appears for swap identity transactions.
          "FromPublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the "from" user being swapped
          "ToPublicKeyBase58Check": "BC1YLgU67opDhT9bTPsqvue9QmyJLDHRZrSj77cF3P4yYDndmad9Wmx", // Public key of the "to" user being swapped
          
          "FromDeSoLockedNanos": 1000, // DeSoLocked in from's creator coin after this transaction. Note this field is only needed for Rosetta
          "ToDeSoLockedNanos": 2000, // DeSoLocked in to's creator coin after this transaction. Note this field is only needed for Rosetta
        },
        "NFTBidTxindexMetadata": { // Describes NFT bid transaction. Only appears for NFT bid transactions.
          "NFTPostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290", // Hex of Post hash that is being bid on in this transaction
          "SerialNumber": 10, // Serial number on which this bid is submitted
          "BidAmountNanos": 1000, // Amount of DeSo (in nanos) bid on the NFT in this transaction.
        },
        "AcceptNFTBidTxindexMetadata": { // Describe Accept NFT bid transactions. Only appears for accept NFT bid transactions.
          "NFTPostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290", // Hex of Post hash that is  sold in this transaction
          "SerialNumber": 10, // Serial number that is sold in this transaction 
          "BidAmountNanos": 1000, // Amount of DeSo (in nanos) of the bid that was accepted in this transaction.
          "CreatorCoinRoyaltyNanos": 100, // Amount of royalties going to the creator coin in this transaction. 
          "CreatorPublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the creator of the NFT
        }
      },
      "Txn": null,
      "TxnOutputResponses": [{ // Outputs from the transaction that generated this notification
        "AmountNanos": 1000, // Amount of nanos in this output
        "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the recipient of this output
      }]
    }
  ],
  "PostsByHash": { // Map of post hash hex to PostEntryResponse that can be used to add post data to a notification
    "63d6b65c4b270cc538ca0cbd2bb538b8d30de1dda9b397e25c68581e3e4bc209": <PostEntryResponse>,
    ...
  },
  "ProfilesByPublicKey": { // Map of public key to ProfileEntryResponse that can be used to add user profile data to a notification
    "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s": <ProfileEntryResponse>,
  }
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
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

{% swagger method="post" path="" baseUrl="/api/v0/get-unread-notifications-count" summary="Get Unread Notification Count" %}
{% swagger-description %}
Gets the number of unread notifications.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1795).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:\
&#x20; \- Make request to [Get Unread Notification Count](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1706)\
&#x20; \- Use GetUnreadNotificationCount to [display an indicator next to notifications representing the number of unread notifications ](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/global-vars.service.ts#L274)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user for whom we want to get the notification count
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the number of unread notifications" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "NotificationsCount": 2, // Number of unread notifications.
  "LastUnreadNotificationIndex": 31147, // Index of the last read notification.
  "UpdateMetadata": false // If true, the frontend should make a call to /api/v0/set-notification-metadata
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name                        | Type    | Description                                                                                                                                                                      |
| --------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NotificationsCount          | uint64  | Number of unread notifications                                                                                                                                                   |
| LastUnreadNotificationIndex | uint64  | Index of the last read notification                                                                                                                                              |
| UpdateMetadata              | Boolean | if true, the frontend should make a call to [#set-notification-metadata](notification-endpoints.md#set-notification-metadata "mention")to update the state of notifications read |
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

{% swagger method="post" path="" baseUrl="/api/v0/set-notification-metadata" summary="Set Notification Metadata" %}
{% swagger-description %}


Update the number of unread notifications, the last notification seen index, and the last unread notification index.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2050).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:\
&#x20; \- Make a request to [Set Notification Metadata](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1691)\
&#x20; \- Use SetNotificationMetadata [when loading the first page of notifications](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/notifications-page/notifications-list/notifications-list.component.ts#L98)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user for whom we are setting notification metadata
{% endswagger-parameter %}

{% swagger-parameter in="body" name="LastSeenIndex" type="int" required="true" %}
The last notification index the user has seen
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="int" name="LastUnreadNotificationIndex" %}
The last notification index that has been scanned
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="UnreadNotifications" type="int" %}
The total count of unread notifications
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="JWT" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully updated notification metadata for user" %}
No response body
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
