# Access Group Endpoints

{% swagger method="post" path="" baseUrl="/api/v0/get-all-user-access-groups" summary="Get All User Access Groups" %}
{% swagger-description %}
Get All User Access Groups gets all Access Group Entry Responses representing all access groups owned by the public key as well as access groups of which the public key is a member.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L546).
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user for whom we want to get all access groups
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json5
{
  "AccessGroupsOwned": [
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "AccessGroupKeyName": "",
      "AccessGroupPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": null
    },
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "AccessGroupKeyName": "default-key",
      "AccessGroupPublicKeyBase58Check": "tBCKY3eUFE56gCdAA1reHnbnAr9uw69foeW12N1appXDfaHhte1Xia",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": null
    }
  ],
  "AccessGroupsMember": [
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
      "AccessGroupKeyName": "a super cool groupchat",
      "AccessGroupPublicKeyBase58Check": "tBCKWmLgvkMGkMuQ47Jhm8aYMhYMokXpFQTnhqBH7JXQsTuX8AYSs7",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": {
        "AccessGroupMemberPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
        "AccessGroupMemberKeyName": "default-key",
        "EncryptedKey": "04bfaef01cf09ea7698e9f7f3897b7b5d21fc15ae79f36977f46b0fa9244663cded605cd9cbdf35a4b1f2dbdce151e77a9d0cd61e56db26d2086fa99c7940d2ff5296058a1f93b9abe814d3660455737c7d7962d459bc9eedaa9c433184ab810b4bc3e078c21e9c28280914b9289035df61c4ffbe21f9f9a3324588c26441eecfec3268243e50cb661e942a7d5d58748f183064470c783b1f98dee58f1660933e3e4e3bb375bd3ec85875e414a77120a9e",
        "ExtraData": null
      }
    }
  ]
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-all-user-access-groups-owned" summary="Get All Access Groups Owned" %}
{% swagger-description %}
Get All Access Groups Owned gets all Access Group Entry Responses representing all access groups owned by the public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L554).
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of user for whom we want to fetch all groups they own
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "AccessGroupsOwned": [
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "AccessGroupKeyName": "",
      "AccessGroupPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": null
    },
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
      "AccessGroupKeyName": "default-key",
      "AccessGroupPublicKeyBase58Check": "tBCKY3eUFE56gCdAA1reHnbnAr9uw69foeW12N1appXDfaHhte1Xia",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": null
    }
  ]
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-all-user-access-groups-member-only" summary="Get All Access Groups Member Only" %}
{% swagger-description %}
Get All Access Groups Owned gets all Access Group Entry Responses representing all access groups owned by the public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L562).
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of user for whom we want to fetch all groups of which they are a member
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>{
</strong><strong>  "AccessGroupsMember": [
</strong>    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
      "AccessGroupKeyName": "a super cool groupchat",
      "AccessGroupPublicKeyBase58Check": "tBCKWmLgvkMGkMuQ47Jhm8aYMhYMokXpFQTnhqBH7JXQsTuX8AYSs7",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": {
        "AccessGroupMemberPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
        "AccessGroupMemberKeyName": "default-key",
        "EncryptedKey": "04bfaef01cf09ea7698e9f7f3897b7b5d21fc15ae79f36977f46b0fa9244663cded605cd9cbdf35a4b1f2dbdce151e77a9d0cd61e56db26d2086fa99c7940d2ff5296058a1f93b9abe814d3660455737c7d7962d459bc9eedaa9c433184ab810b4bc3e078c21e9c28280914b9289035df61c4ffbe21f9f9a3324588c26441eecfec3268243e50cb661e942a7d5d58748f183064470c783b1f98dee58f1660933e3e4e3bb375bd3ec85875e414a77120a9e",
        "ExtraData": null
      }
    }
  ]
}
</code></pre>
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/check-party-access-groups" summary="Check Party Access Groups" %}
{% swagger-description %}
Check Party Access Groups checks whether both the sender and receiver have the requested access groups. If they do not, it returns the base key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L633).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderPublicKeyBase58Check" type="String" required="true" %}
Public key of sender
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupKeyName" type="String" required="true" %}
Access Group Key Name of sender
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientPublicKeyBase58Check" type="String" required="true" %}
Public key of recipient
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupKeyName" type="String" required="true" %}
Access Group Key Name of recipient
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "SenderPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
  "SenderAccessGroupPublicKeyBase58Check": "tBCKY3eUFE56gCdAA1reHnbnAr9uw69foeW12N1appXDfaHhte1Xia",
  "SenderAccessGroupKeyName": "default-key",
  "IsSenderAccessGroupKey": true,
  "RecipientPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
  "RecipientAccessGroupPublicKeyBase58Check": "tBCKVNhD9Kn6WzxT1EdgR3Tf3Yop6CXQSDZnvMLbST6C33DTbsnku4",
  "RecipientAccessGroupKeyName": "default-key",
  "IsRecipientAccessGroupKey": true
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-access-group-info" summary="Get Access Group Information" %}
{% swagger-description %}
Get Access Group Information gets a single Access Group Entry Response for the access group as defined in the request body.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L762).
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group owner
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Access group key name
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "AccessGroupOwnerPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2",
  "AccessGroupKeyName": "a super cool groupchat",
  "AccessGroupPublicKeyBase58Check": "tBCKWmLgvkMGkMuQ47Jhm8aYMhYMokXpFQTnhqBH7JXQsTuX8AYSs7",
  "ExtraData": null,
  "AccessGroupMemberEntryResponse": null,
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-access-group-member-info" summary="Get Access Group Member Information" %}
{% swagger-description %}
Get Access Group Member Information gets a single Access Group Member Entry Response for the access group member defined in the request body.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L850).&#x20;
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the group owner
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Access group key name of the group
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupMemberPublicKeyBase58Check" type="String" required="true" %}
Public key of the member for which we want to fetch a AccessGroupMemberEntryResponse
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "AccessGroupMemberPublicKeyBase58Check": "tBCKVqiE8oRZwcSLBJWN4WR5dSLBEkXZeWv5iqCfp2crknhFB6Fk2n",
  "AccessGroupMemberKeyName": "default-key",
  "EncryptedKey": "04bfaef01cf09ea7698e9f7f3897b7b5d21fc15ae79f36977f46b0fa9244663cded605cd9cbdf35a4b1f2dbdce151e77a9d0cd61e56db26d2086fa99c7940d2ff5296058a1f93b9abe814d3660455737c7d7962d459bc9eedaa9c433184ab810b4bc3e078c21e9c28280914b9289035df61c4ffbe21f9f9a3324588c26441eecfec3268243e50cb661e942a7d5d58748f183064470c783b1f98dee58f1660933e3e4e3bb375bd3ec85875e414a77120a9e",
  "ExtraData": null
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-paginated-access-group-members" summary="Get Paginated Access Group Members" %}
{% swagger-description %}
Get Paginated Access Group Members gets a page of Access Group Member Entry responses for the access group defined in the request body. This is useful in identifying all members of a group. A map of public key to profile entry response is provided for convenience.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L932).
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the group owner
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
name of the access group
{% endswagger-parameter %}

{% swagger-parameter in="body" name="StartingAccessGroupMemberPublicKeyBase58Check" type="String" %}
Public key of the last result from the previous page. To get the first page, exclude this value or make it an empty string.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MaxMembersToFetch" type="int" required="true" %}
Maximum number of members to fetch. You will receive at most this number of members.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "AccessGroupMembersBase58Check": [
    "tBCKVv5H1Gz6RTRhjxJwdzcfwfwoUo8b4PYWSKkayG4dy76Jsjt2Ro",
    "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2"
  ],
  "PublicKeyToProfileEntryResponse": {
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
    },
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
      "DESOBalanceNanos": 5914499057,
      "BestExchangeRateDESOPerDAOCoin": 0
    }
  }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-bulk-access-group-entries" summary="Get Bulk Access Group Entries" %}
{% swagger-description %}
Get Bulk Access Group Entries returns an array of AccessGroupEntryResponse objects for the request list of group owner + group key name pairs in the request body.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access\_group.go#L1051).
{% endswagger-description %}

{% swagger-parameter in="body" name="GroupOwnerAndGroupKeyNamePairs" type="GroupOwnerAndGroupKeyNamePair[]" required="true" %}
An array of objects containing the below attributes.



GroupOwnerPublicKeyBase58Check: the owner of the group



GroupKeyName: the name of the group



This endpoint will return the associated AccessGroupEntryResponse for each object. If the AccessGroupEntryResponse is not found, this object will appear in the PairsNotFound array in the response.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "AccessGroupEntries": [
    {
      "AccessGroupOwnerPublicKeyBase58Check": "tBCKVapwwkTTdgpfEKGphh5bGMvcU9aLJTqssRopKX7wQyzwGvoxGL",
      "AccessGroupKeyName": "default-key",
      "AccessGroupPublicKeyBase58Check": "tBCKWgxWqvmmMF1H9K49onuyp1bcYEs1PqAuCFJJbCg8RJPb4jvBTG",
      "ExtraData": null,
      "AccessGroupMemberEntryResponse": null
    }
  ],
  "PairsNotFound": null
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}
