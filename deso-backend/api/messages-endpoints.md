# Messages Endpoints



## Get User Direct Message Threads Ordered by Timestamp

<mark style="color:green;">`POST`</mark> `/api/v0/get-user-dm-threads-ordered-by-timestamp`

Get User Direct Message Threads Ordered by Timestamp returns an array of NewMessageEntryResponse objects for the public key provided in the request body. Each NewMessageEntryResponse object represents the most recent message each in DM conversation a user has. This is useful for showing a list of DM conversations in a user's inbox. The first NewMessageEntryResponse object is the most recent conversation and the last one is the old.

Additionally, a map of public key to [#profileentryresponse](./#profileentryresponse "mention") objects for convenience so you don't need to make an extra request to get profile entry responses for the public keys in the response.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L459).

#### Request Body

| Name                                                       | Type   | Description                                                           |
| ---------------------------------------------------------- | ------ | --------------------------------------------------------------------- |
| UserPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the user for whom we want to fetch all DM conversations |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
  "MessageThreads": [
    {
      "ChatType": "DM",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKUr3CEsbbg95oH6nauciz3HKxoExX6DgpcFskffGFFXHy5mtrvt",
        "AccessGroupPublicKeyBase58Check": "tBCKVSfJQqcm88YQANuuRDqxHGRZ6pERLmgG2YsbuTJQ66CypHXc2P",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKVNhD9Kn6WzxT1EdgR3Tf3Yop6CXQSDZnvMLbST6C33DTbsnku4",
        "AccessGroupKeyName": "default-key"
      },
      "MessageInfo": {
        "EncryptedText": "04bec21d8b7ebb8bef474b01adf6a128d4984ba1f2f1f03abeb1612c78477cac0e6bdba24924631a9b4a795d9fd5e82dac8306a0193ec0c8c8c8cee9ca8f8313ec0d393178c1241c00e9aafa380813d0296aefd78eef8eff974dae964b330d3f9e838b1848086c98c1778f434bd6569d7d9de09eb6102fff028f8ca98304a3f7c06833b178f358de11072a05a07f7d77984321",
        "TimestampNanos": 1674622684987966700,
        "TimestampNanosString": "1674622684987966645",
        "ExtraData": null
      }
    }
  ],
  "PublicKeyToProfileEntryResponse": {
    "tBCKUr3CEsbbg95oH6nauciz3HKxoExX6DgpcFskffGFFXHy5mtrvt": null,
    "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2": {
      "PublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
      "Username": "cloutchaser",
      "Description": "",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 1124400018,
        "NumberOfHolders": 1,
        "CoinsInCirculationNanos": 9999331379,
        "CoinWatermarkNanos": 9999331379,
        "BitCloutLockedNanos": 1124400018
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 3,
        "CoinsInCirculationNanos": "0xd96914214a6b400",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "profile_owner_only"
      },
      "CoinPriceDeSoNanos": 337342594,
      "CoinPriceBitCloutNanos": 337342594,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "BlogSlugMap": "{\"da39a3ee5e\":\"7f9b91cd09ed5cefa0e2bbe2d70698dc665f3d4d31ee2a3a64c91ead3552ed51\"}"
      },
      "DESOBalanceNanos": 5914499708,
      "BestExchangeRateDESOPerDAOCoin": 0
    }
  }
}
```
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}

## Get Paginated Messages for a Direct Message Thread

<mark style="color:green;">`POST`</mark> `/api/v0/get-paginated-messages-for-dm-thread`

Get Paginated Messages For DM Thread returns an array of NewMessageEntryResponse objects based on the conversation defined in the request body. Each NewMessageEntryResponse object represent a message in the a DM conversation. This is useful for showing all messages in a conversation. This first NewMessageEntryResponse object is the most recent message and the last one is the oldest. This endpoint supports pagination.

Additionally, a map of public key to [#profileentryresponse](./#profileentryresponse "mention") objects for convenience so you don't need to make an extra request to get profile entry responses for the public keys in the response.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L496).

#### Request Body

| Name                                                                  | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------------------------------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UserGroupOwnerPublicKeyBase58Check<mark style="color:red;">\*</mark>  | String | Public key of one of the users in a DM conversation                                                                                                                                                                                                                                                                                                                                                              |
| UserGroupKeyName<mark style="color:red;">\*</mark>                    | String | Access group key name of UserGroupOwnerPublicKeyBase58Check in the DM conversation                                                                                                                                                                                                                                                                                                                               |
| PartyGroupOwnerPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the other user in a DM conversation                                                                                                                                                                                                                                                                                                                                                                |
| PartyGroupKeyName<mark style="color:red;">\*</mark>                   | String | Access group key name of PartyGroupOwnerPublicKeyBase58Check in the DM conversation                                                                                                                                                                                                                                                                                                                              |
| StartTimestampString                                                  | String | <p>String version of a timestamp in nanos. This defines the start point of the page. Messages newer than this timestamp are excluded.</p><p>To get the most recent (but not in the future) messages, set TimestampNanosString to (Date.now()*1e6).toString()</p><p>To get the next page of messages, take TimestampNanosString from the last NewMessageEntryResponse object in the previous page's response.</p> |
| StartTimestamp                                                        | uint64 | Timestamp in nanos. This is less preferred than passing StartTimestamp string as JSON encoding and decoding can lose precision on these values.                                                                                                                                                                                                                                                                  |
| MaxMessagesToFetch                                                    | int    | Maximum number of messages to fetch. You will always receive this amount of messages or fewer.                                                                                                                                                                                                                                                                                                                   |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
  "ThreadMessages": [
    {
      "ChatType": "DM",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
        "AccessGroupPublicKeyBase58Check": "tBCKYe8fNF8pk8Gn3hfD7uYd34a7bz8NVeQmcxaSvqsogtzFkTWEEV",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKVv5H1Gz6RTRhjxJwdzcfwfwoUo8b4PYWSKkayG4dy76Jsjt2Ro",
        "AccessGroupPublicKeyBase58Check": "tBCKYdH5NpkaafHkU6foem4qx5rDDd9N8nCXXPUecTfHetEarueJJ6",
        "AccessGroupKeyName": "default-key"
      },
      "MessageInfo": {
        "EncryptedText": "043aaf6732c852ce7b91fbf8556d8a3a4a237852a3550f3e7617b5f5514d8873f152de0fa1157b32f39e42348876852d0ab641f05e1b9775088c635a480952e9ba4b55909cbb0aa073c9bcd09d9c3a4a419cd488d0290dd0b71cf8700bb8728854be062aeafccb5b35a937b48b4e85ead1fec6",
        "TimestampNanos": 1675459724661132500,
        "TimestampNanosString": "1675459724661132641",
        "ExtraData": null
      }
    }
  ],
  "PublicKeyToProfileEntryResponse": {
    "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV": {
      "PublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
      "Username": "DeSoMessagingDemo",
      "Description": "The coolest demo on the planet",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 178459749,
        "NumberOfHolders": 3,
        "CoinsInCirculationNanos": 3387141072,
        "CoinWatermarkNanos": 3650090519,
        "BitCloutLockedNanos": 178459749
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 7,
        "CoinsInCirculationNanos": "0x152e2c1689476aa1d680",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "permanently_unrestricted"
      },
      "CoinPriceDeSoNanos": 158062297,
      "CoinPriceBitCloutNanos": 158062297,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "DAOPublicKeysPurchased": "tBCKWGGQhAwaH1ZhEFbLA1WoaNeErr3qQuqXSw3LZB3R357m1vkS1E,tBCKY3nVGx7M9FT7h1RcpJyWSUpnjEzJQRXSqwAPaqcAF42W9TEwt8,tBCKYdgcgaCgu53xhwT4J2dyfnjP8M1pXogEgeP2rgp2qDhdWhkMpd",
        "DaoDaoURL": "",
        "DerivedPublicKey": "tBCKXpjxNnoe9x6EBVJTdcrTu9NS2mkJ3va7HY9a52CrEPXZQGWfsk",
        "DiscordURL": "",
        "DisplayName": "",
        "FeaturedImageURL": "",
        "LargeProfilePicURL": "",
        "MarkdownDescription": "2320776f6f0a686f6f",
        "TelegramURL": "",
        "TwitterURL": "",
        "WebsiteURL": ""
      },
      "DESOBalanceNanos": 719569565,
      "BestExchangeRateDESOPerDAOCoin": 0.07468259895444361
    },
    "tBCKVv5H1Gz6RTRhjxJwdzcfwfwoUo8b4PYWSKkayG4dy76Jsjt2Ro": {
      "PublicKeyBase58Check": "tBCKVv5H1Gz6RTRhjxJwdzcfwfwoUo8b4PYWSKkayG4dy76Jsjt2Ro",
      "Username": "lazynina",
      "Description": "",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 6834043772,
        "NumberOfHolders": 1,
        "CoinsInCirculationNanos": 5448485463,
        "CoinWatermarkNanos": 5448485463,
        "BitCloutLockedNanos": 6834043772
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 2,
        "CoinsInCirculationNanos": "0x1794bb7c13520200",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "profile_owner_only"
      },
      "CoinPriceDeSoNanos": 3762905032,
      "CoinPriceBitCloutNanos": 3762905032,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "DAOPublicKeysPurchased": "tBCKY3nVGx7M9FT7h1RcpJyWSUpnjEzJQRXSqwAPaqcAF42W9TEwt8",
        "DerivedPublicKey": "tBCKUoDRjbVj2JMWkMqiDzvbFrSGSdD9nGty4YXsNu4zZW5cySUrbG",
        "DiscordURL": "",
        "DisplayName": "",
        "FeaturedImageURL": "",
        "LargeProfilePicURL": "",
        "MarkdownDescription": "",
        "TelegramURL": "",
        "TwitterURL": "",
        "WebsiteURL": ""
      },
      "DESOBalanceNanos": 16516822968844,
      "BestExchangeRateDESOPerDAOCoin": 0
    }
  }
}
```
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}

## Get User Group Chat  Threads Ordered by Timestamp

<mark style="color:green;">`POST`</mark> `/api/v0/get-user-group-chat-threads-ordered-by-timestamp`

Get User Group Chat Threads Ordered by Timestamp returns an array of NewMessageEntryResponse objects for the public key provided in the request body. Each NewMessageEntryResponse object represents the most recent message each in a group chat a user has. This is useful for showing a list of group chats in a user's inbox. The first NewMessageEntryResponse object is the most recent conversation and the last one is the oldest.

Additionally, a map of public key to [#profileentryresponse](./#profileentryresponse "mention") objects for convenience so you don't need to make an extra request to get profile entry responses for the public keys in the response.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L656).

#### Request Body

| Name                                                       | Type   | Description                                                           |
| ---------------------------------------------------------- | ------ | --------------------------------------------------------------------- |
| UserPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the user for whom we want to fetch all DM conversations |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
  "MessageThreads": [
    {
      "ChatType": "GroupChat",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKVNhD9Kn6WzxT1EdgR3Tf3Yop6CXQSDZnvMLbST6C33DTbsnku4",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKWmLgvkMGkMuQ47Jhm8aYMhYMokXpFQTnhqBH7JXQsTuX8AYSs7",
        "AccessGroupKeyName": "a super cool groupchat"
      },
      "MessageInfo": {
        "EncryptedText": "04e8cfc4ebd0f55f612e3779e0a224c702ad89acf33c4499c3f82544f2c0ff295e27dd008f6854abe0c6109ec26b1bbaaedc1e4fa7c7f1dc0dff0cfcc028ddf9fab5e400559e5f280a04442d46168ac67061bcb598a27baa50bf202127397070cbbac68401921777a16017089b6b7f77a01447ff96",
        "TimestampNanos": 1675454352913804800,
        "TimestampNanosString": "1675454352913804789",
        "ExtraData": null
      }
    }
  ],
  "PublicKeyToProfileEntryResponse": {
    "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2": {
      "PublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
      "Username": "cloutchaser",
      "Description": "",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 1124400018,
        "NumberOfHolders": 1,
        "CoinsInCirculationNanos": 9999331379,
        "CoinWatermarkNanos": 9999331379,
        "BitCloutLockedNanos": 1124400018
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 3,
        "CoinsInCirculationNanos": "0xd96914214a6b400",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "profile_owner_only"
      },
      "CoinPriceDeSoNanos": 337342594,
      "CoinPriceBitCloutNanos": 337342594,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "BlogSlugMap": "{\"da39a3ee5e\":\"7f9b91cd09ed5cefa0e2bbe2d70698dc665f3d4d31ee2a3a64c91ead3552ed51\"}"
      },
      "DESOBalanceNanos": 5914499708,
      "BestExchangeRateDESOPerDAOCoin": 0
    }
  }
}
```
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}

## Get Paginated Messages For Group Chat Thread

<mark style="color:green;">`POST`</mark> `/api/v0/get-paginated-messages-for-group-chat-thread`

Get Paginated Messages For Group Chat Thread returns an array of NewMessageEntryResponse objects based on the group chat defined in the request body. Each NewMessageEntryResponse object represent a message in the group chat. This is useful for showing all messages in a conversation. This first NewMessageEntryResponse object is the most recent message and the last one is the oldest. This endpoint supports pagination.

Additionally, a map of public key to [#profileentryresponse](./#profileentryresponse "mention") objects for convenience so you don't need to make an extra request to get profile entry responses for the public keys in the response.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L686).

#### Request Body

| Name                                                       | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| UserPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the access group owner who owns this group chat.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| AccessGroupKeyName<mark style="color:red;">\*</mark>       | String | Name of the access group for which we want to fetch messages.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| StartTimestampString                                       | String | <p>String version of a timestamp in nanos. This defines the start point of the page. Messages newer than this timestamp are included. To get the most recent messages of the conversation, pass the current timestamp in nanoseconds converted to a string (not formatted as a timestamp).</p><p>To get the most recent (but not in the future) messages, set TimestampNanosString to (Date.now()*1e6).toString()</p><p>To get the next page of messages, take TimestampNanosString from the last NewMessageEntryResponse object in the previous page's response.</p> |
| StartTimestamp                                             | uint64 | Timestamp in nanos. This is less preferred than passing StartTimestamp string as JSON encoding and decoding can lose precision on these values.                                                                                                                                                                                                                                                                                                                                                                                                                       |
| MaxMessagesToFetch<mark style="color:red;">\*</mark>       | int    | Maximum number of messages to fetch. You will always receive this amount of messages or fewer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
  "GroupChatMessages": [
    {
      "ChatType": "GroupChat",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
        "AccessGroupPublicKeyBase58Check": "tBCKYe8fNF8pk8Gn3hfD7uYd34a7bz8NVeQmcxaSvqsogtzFkTWEEV",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
        "AccessGroupPublicKeyBase58Check": "tBCKYGaV4ePH88zeVCuj4ywWVN8xf6TDuRThuow4d7dsDYD2A9u8g8",
        "AccessGroupKeyName": "chatdemo"
      },
      "MessageInfo": {
        "EncryptedText": "0481860551243b3524f87a62e9d8704539d20869124f01fccfdc458879f3d42d8db05ac605fda1a2ba6a05d72511758a907c91a7c362359645425bd7e2e4137590a00f7f527ca93744c50b8a8d7b7347753a106e16cf18b47d373533c73c6da84f41b0ad4e59b4bed7d75935a78d320361c3b8bcae24ed743d9549e5a38a2f934007f848db152f55f6a2280a407862cd657f42817928d816f873e4",
        "TimestampNanos": 1675457690291604500,
        "TimestampNanosString": "1675457690291604407",
        "ExtraData": null
      }
    }
  ],
  "PublicKeyToProfileEntryResponse": {
    "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV": {
      "PublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
      "Username": "DeSoMessagingDemo",
      "Description": "The coolest demo on the planet",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 178459749,
        "NumberOfHolders": 3,
        "CoinsInCirculationNanos": 3387141072,
        "CoinWatermarkNanos": 3650090519,
        "BitCloutLockedNanos": 178459749
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 7,
        "CoinsInCirculationNanos": "0x152e2c1689476aa1d680",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "permanently_unrestricted"
      },
      "CoinPriceDeSoNanos": 158062297,
      "CoinPriceBitCloutNanos": 158062297,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "DAOPublicKeysPurchased": "tBCKWGGQhAwaH1ZhEFbLA1WoaNeErr3qQuqXSw3LZB3R357m1vkS1E,tBCKY3nVGx7M9FT7h1RcpJyWSUpnjEzJQRXSqwAPaqcAF42W9TEwt8,tBCKYdgcgaCgu53xhwT4J2dyfnjP8M1pXogEgeP2rgp2qDhdWhkMpd",
        "DaoDaoURL": "",
        "DerivedPublicKey": "tBCKXpjxNnoe9x6EBVJTdcrTu9NS2mkJ3va7HY9a52CrEPXZQGWfsk",
        "DiscordURL": "",
        "DisplayName": "",
        "FeaturedImageURL": "",
        "LargeProfilePicURL": "",
        "MarkdownDescription": "2320776f6f0a686f6f",
        "TelegramURL": "",
        "TwitterURL": "",
        "WebsiteURL": ""
      },
      "DESOBalanceNanos": 719570083,
      "BestExchangeRateDESOPerDAOCoin": 0.07468259895444361
    }
  }
}
```
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}

## Get All User Message Threads

<mark style="color:green;">`POST`</mark> `/api/v0/get-all-user-message-threads`

Get All User Message Threads Group returns an array of NewMessageEntryResponse objects for the public key provided in the request body. Each NewMessageEntryResponse object represents the most recent message each in a conversation (DM or group chat) a user has. This is useful for showing a list of all conversations in a user's inbox. The first NewMessageEntryResponse object is the most recent conversation and the last one is the oldest.

Additionally, a map of public key to [#profileentryresponse](./#profileentryresponse "mention") objects for convenience so you don't need to make an extra request to get profile entry responses for the public keys in the response.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L791).

#### Request Body

| Name                                                       | Type   | Description                                                        |
| ---------------------------------------------------------- | ------ | ------------------------------------------------------------------ |
| UserPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the user for whom we want to fetch all conversations |

{% tabs %}
{% tab title="200: OK " %}
```javascript
{
  "MessageThreads": [
    {
      "ChatType": "GroupChat",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKVNhD9Kn6WzxT1EdgR3Tf3Yop6CXQSDZnvMLbST6C33DTbsnku4",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKWmLgvkMGkMuQ47Jhm8aYMhYMokXpFQTnhqBH7JXQsTuX8AYSs7",
        "AccessGroupKeyName": "a super cool groupchat"
      },
      "MessageInfo": {
        "EncryptedText": "04e8cfc4ebd0f55f612e3779e0a224c702ad89acf33c4499c3f82544f2c0ff295e27dd008f6854abe0c6109ec26b1bbaaedc1e4fa7c7f1dc0dff0cfcc028ddf9fab5e400559e5f280a04442d46168ac67061bcb598a27baa50bf202127397070cbbac68401921777a16017089b6b7f77a01447ff96",
        "TimestampNanos": 1675454352913804800,
        "TimestampNanosString": "1675454352913804789",
        "ExtraData": null
      }
    },
    {
      "ChatType": "DM",
      "SenderInfo": {
        "OwnerPublicKeyBase58Check": "tBCKUr3CEsbbg95oH6nauciz3HKxoExX6DgpcFskffGFFXHy5mtrvt",
        "AccessGroupPublicKeyBase58Check": "tBCKVSfJQqcm88YQANuuRDqxHGRZ6pERLmgG2YsbuTJQ66CypHXc2P",
        "AccessGroupKeyName": "default-key"
      },
      "RecipientInfo": {
        "OwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
        "AccessGroupPublicKeyBase58Check": "tBCKVNhD9Kn6WzxT1EdgR3Tf3Yop6CXQSDZnvMLbST6C33DTbsnku4",
        "AccessGroupKeyName": "default-key"
      },
      "MessageInfo": {
        "EncryptedText": "04bec21d8b7ebb8bef474b01adf6a128d4984ba1f2f1f03abeb1612c78477cac0e6bdba24924631a9b4a795d9fd5e82dac8306a0193ec0c8c8c8cee9ca8f8313ec0d393178c1241c00e9aafa380813d0296aefd78eef8eff974dae964b330d3f9e838b1848086c98c1778f434bd6569d7d9de09eb6102fff028f8ca98304a3f7c06833b178f358de11072a05a07f7d77984321",
        "TimestampNanos": 1674622684987966700,
        "TimestampNanosString": "1674622684987966645",
        "ExtraData": null
      }
    }
  ],
  "PublicKeyToProfileEntryResponse": {
    "tBCKUr3CEsbbg95oH6nauciz3HKxoExX6DgpcFskffGFFXHy5mtrvt": null,
    "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2": {
      "PublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
      "Username": "cloutchaser",
      "Description": "",
      "IsHidden": false,
      "IsReserved": false,
      "IsVerified": false,
      "Comments": null,
      "Posts": null,
      "CoinEntry": {
        "CreatorBasisPoints": 10000,
        "DeSoLockedNanos": 1124400018,
        "NumberOfHolders": 1,
        "CoinsInCirculationNanos": 9999331379,
        "CoinWatermarkNanos": 9999331379,
        "BitCloutLockedNanos": 1124400018
      },
      "DAOCoinEntry": {
        "NumberOfHolders": 3,
        "CoinsInCirculationNanos": "0xd96914214a6b400",
        "MintingDisabled": false,
        "TransferRestrictionStatus": "profile_owner_only"
      },
      "CoinPriceDeSoNanos": 337342594,
      "CoinPriceBitCloutNanos": 337342594,
      "UsersThatHODL": null,
      "IsFeaturedTutorialWellKnownCreator": false,
      "IsFeaturedTutorialUpAndComingCreator": false,
      "ExtraData": {
        "BlogSlugMap": "{\"da39a3ee5e\":\"7f9b91cd09ed5cefa0e2bbe2d70698dc665f3d4d31ee2a3a64c91ead3552ed51\"}"
      },
      "DESOBalanceNanos": 5914499708,
      "BestExchangeRateDESOPerDAOCoin": 0
    }
  }
}
```
{% endtab %}

{% tab title="400: Bad Request " %}

{% endtab %}
{% endtabs %}
