---
description: Description of endpoints used in querying for associations
---

# Associations Endpoints

### User Associations

## Get user association by ID

<mark style="color:blue;">`GET`</mark> `/api/v0/user-associations/{{ associationID }}`

Retrieve a single user association by ID.

#### Path Parameters

| Name                                            | Type   | Description                                   |
| ----------------------------------------------- | ------ | --------------------------------------------- |
| associationID<mark style="color:red;">\*</mark> | string | The identifier of the association to retrieve |

{% tabs %}
{% tab title="200: OK Successfully retrieved the association" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "AssociationID": "22b1dabed784a8ada7c630e1539829df21e485608c13e3b461a37ff97185ff69",
    "TransactorPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
    "TargetUserPublicKeyBase58Check": "tBCKXU8pf7nkn8M38sYJeAwiBP7HbSJWy9Zmn4sHNL6gA6ahkriymq",
    "AppPublicKeyBase58Check": "tBCKVUCQ9WxpVmNthS2PKfY1BCxG4GkWvXqDhQ4q3zLtiwKVUNMGYS",
    "AssociationType": "ENDORSEMENT",
    "AssociationValue": "SQL",
    "ExtraData": {
        "PeerID": "A"
    },
    "BlockHeight": 38,
    "TransactorProfile": {
        "PublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
        "Username": "sender",
        "Description": "",
        "IsHidden": false,
        "IsReserved": false,
        "IsVerified": false,
        "Comments": null,
        "Posts": null,
        "CoinEntry": {
            "CreatorBasisPoints": 0,
            "DeSoLockedNanos": 0,
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": 0,
            "CoinWatermarkNanos": 0,
            "BitCloutLockedNanos": 0
        },
        "DAOCoinEntry": {
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": "0x0",
            "MintingDisabled": false,
            "TransferRestrictionStatus": "unrestricted"
        },
        "CoinPriceDeSoNanos": 0,
        "CoinPriceBitCloutNanos": 0,
        "UsersThatHODL": null,
        "IsFeaturedTutorialWellKnownCreator": false,
        "IsFeaturedTutorialUpAndComingCreator": false,
        "ExtraData": null,
        "DESOBalanceNanos": 36999999438,
        "BestExchangeRateDESOPerDAOCoin": 0
    },
    "TargetUserProfile": null,
    "AppProfile": null
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}


{% endtab %}

{% tab title="400: Bad Request Invalid parameter provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Count user associations

<mark style="color:green;">`POST`</mark> `/api/v0/user-associations/count`

Count the number of user associations matching the provided query parameters.

#### Request Body

| Name                           | Type   | Description                                                            |
| ------------------------------ | ------ | ---------------------------------------------------------------------- |
| TransactorPublicKeyBase58Check | string | The public key of the user who created the association                 |
| TargetUserPublicKeyBase58Check | string | The public key of the user to whom the association references          |
| AppPublicKeyBase58Check        | string | The public key of the application on which the association was created |
| AssociationType                | string | The association type (exact match)                                     |
| AssociationTypePrefix          | string | The prefix of the association type (wildcard match)                    |
| AssociationValue               | string | The association value (exact match)                                    |
| AssociationValuePrefix         | string | The prefix of the association value (wildcard match)                   |

{% tabs %}
{% tab title="200: OK Successfully queried for the number of matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Count": 1
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Count user associations by multiple values

<mark style="color:green;">`POST`</mark> `/api/v0/user-associations/counts`

Count the number of user associations matching the provided query. Here, you can provide an array of association values and the count of associations matching any in that list will be returned.

#### Request Body

| Name                                                | Type      | Description                                                            |
| --------------------------------------------------- | --------- | ---------------------------------------------------------------------- |
| TransactorPublicKeyBase58Check                      | string    | The public key of the user who created the association                 |
| TargetUserPublicKeyBase58Check                      | string    | The public key of the user to whom the association references          |
| AssociationType<mark style="color:red;">\*</mark>   | string    | The association type (exact match)                                     |
| AssociationValues<mark style="color:red;">\*</mark> | \[]string | An array of association values                                         |
| AppPublicKeyBase58Check                             | string    | The public key of the application on which the association was created |

{% tabs %}
{% tab title="200: OK Successfully queried for the number of matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Counts": {
        "JAVASCRIPT": 0,
        "SQL": 1
    },
    "Total": 1
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Query for user associations

<mark style="color:green;">`POST`</mark> `/api/v0/user-associations/query`

Retrieve user associations matching the provided query parameters.

#### Request Body

| Name                           | Type      | Description                                                                                                                          |
| ------------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| TransactorPublicKeyBase58Check | string    | The public key of the user who created the association                                                                               |
| TargetUserPublicKeyBase58Check | string    | The public key of the user to whom the association references                                                                        |
| AssociationType                | string    | The association type (exact match)                                                                                                   |
| AssociationTypePrefix          | string    | The prefix of the association type (wildcard match)                                                                                  |
| AssociationValue               | string    | The association value (exact match)                                                                                                  |
| AssociationValuePrefix         | string    | The prefix of the association value (wildcard match)                                                                                 |
| Limit                          | integer   | The maximum number of associations to retrieve (default is 100)                                                                      |
| LastSeenAssociationID          | string    | The identifier of the last retrieved association; this parameter functions like an offset allowing users to paginate through results |
| SortDescending                 | boolean   | If true, results are returned in reverse order                                                                                       |
| IncludeTransactorProfile       | boolean   | If true, include the transactors' user profiles in the response                                                                      |
| IncludeTargetUserProfile       | boolean   | If true, include the target users' profiles in the response                                                                          |
| IncludeAppProfile              | boolean   | If true, include the applications' user profiles in the response                                                                     |
| AssociationValues              | \[]string | An array of association values; associations matching any of the values in this list will be returned                                |
| AppPublicKeyBase58Check        | string    | The public key of the application on which the association was created                                                               |

{% tabs %}
{% tab title="200: OK Successfully retrieved matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Associations": [
        {
            "AssociationID": "88eb5872de1ae8188e2768874b77dedb3d53fe27df5e7af48783ca8f3d3920f7",
            "TransactorPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
            "TargetUserPublicKeyBase58Check": "tBCKXU8pf7nkn8M38sYJeAwiBP7HbSJWy9Zmn4sHNL6gA6ahkriymq",
            "AppPublicKeyBase58Check": "tBCKVUCQ9WxpVmNthS2PKfY1BCxG4GkWvXqDhQ4q3zLtiwKVUNMGYS",
            "AssociationType": "ENDORSEMENT",
            "AssociationValue": "SQL",
            "ExtraData": {
                "PeerID": "A"
            },
            "BlockHeight": 33,
            "TransactorProfile": null,
            "TargetUserProfile": null,
            "AppProfile": null
        }
    ],
    "PublicKeyToProfileEntryResponse": {
        "tBCKVUCQ9WxpVmNthS2PKfY1BCxG4GkWvXqDhQ4q3zLtiwKVUNMGYS": null,
        "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX": {
            "PublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
            "Username": "sender",
            "Description": "",
            "IsHidden": false,
            "IsReserved": false,
            "IsVerified": false,
            "Comments": null,
            "Posts": null,
            "CoinEntry": {
                "CreatorBasisPoints": 0,
                "DeSoLockedNanos": 0,
                "NumberOfHolders": 0,
                "CoinsInCirculationNanos": 0,
                "CoinWatermarkNanos": 0,
                "BitCloutLockedNanos": 0
            },
            "DAOCoinEntry": {
                "NumberOfHolders": 0,
                "CoinsInCirculationNanos": "0x0",
                "MintingDisabled": false,
                "TransferRestrictionStatus": "unrestricted"
            },
            "CoinPriceDeSoNanos": 0,
            "CoinPriceBitCloutNanos": 0,
            "UsersThatHODL": null,
            "IsFeaturedTutorialWellKnownCreator": false,
            "IsFeaturedTutorialUpAndComingCreator": false,
            "ExtraData": null,
            "DESOBalanceNanos": 31999999438,
            "BestExchangeRateDESOPerDAOCoin": 0
        }
    }
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

### Post Associations

## Get post association by ID

<mark style="color:blue;">`GET`</mark> `/api/v0/post-associations/{{ associationID }}`

Retrieve a single post association by ID.

#### Path Parameters

| Name                                            | Type   | Description                                   |
| ----------------------------------------------- | ------ | --------------------------------------------- |
| associationID<mark style="color:red;">\*</mark> | string | The identifier of the association to retrieve |

{% tabs %}
{% tab title="200: OK Successfully retrieved the association" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "AssociationID": "0ed4915dec590cf2c6da7c836971d927d8e682c1b5caf6d7e705e5497ff36746",
    "TransactorPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
    "PostHashHex": "460f8b4125342af8b4de69018d4b07f862bcd0435f63e75cc376cada35845ddc",
    "AppPublicKeyBase58Check": "tBCKVUCQ9WxpVmNthS2PKfY1BCxG4GkWvXqDhQ4q3zLtiwKVUNMGYS",
    "AssociationType": "REACTION",
    "AssociationValue": "HEART",
    "ExtraData": {
        "PeerID": "B"
    },
    "BlockHeight": 37,
    "TransactorProfile": {
        "PublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
        "Username": "sender",
        "Description": "",
        "IsHidden": false,
        "IsReserved": false,
        "IsVerified": false,
        "Comments": null,
        "Posts": null,
        "CoinEntry": {
            "CreatorBasisPoints": 0,
            "DeSoLockedNanos": 0,
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": 0,
            "CoinWatermarkNanos": 0,
            "BitCloutLockedNanos": 0
        },
        "DAOCoinEntry": {
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": "0x0",
            "MintingDisabled": false,
            "TransferRestrictionStatus": "unrestricted"
        },
        "CoinPriceDeSoNanos": 0,
        "CoinPriceBitCloutNanos": 0,
        "UsersThatHODL": null,
        "IsFeaturedTutorialWellKnownCreator": false,
        "IsFeaturedTutorialUpAndComingCreator": false,
        "ExtraData": null,
        "DESOBalanceNanos": 35999998542,
        "BestExchangeRateDESOPerDAOCoin": 0
    },
    "PostEntry": {
        "PostHashHex": "460f8b4125342af8b4de69018d4b07f862bcd0435f63e75cc376cada35845ddc",
        "PosterPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
        "ParentStakeID": "",
        "Body": "Hello, world!",
        "ImageURLs": null,
        "VideoURLs": null,
        "RepostedPostEntryResponse": null,
        "CreatorBasisPoints": 1000,
        "StakeMultipleBasisPoints": 12500,
        "TimestampNanos": 1675274688926964176,
        "IsHidden": false,
        "ConfirmationBlockHeight": 37,
        "InMempool": true,
        "ProfileEntryResponse": null,
        "Comments": null,
        "LikeCount": 0,
        "DiamondCount": 0,
        "PostEntryReaderState": null,
        "IsPinned": false,
        "PostExtraData": {},
        "CommentCount": 0,
        "RepostCount": 0,
        "QuoteRepostCount": 0,
        "ParentPosts": null,
        "IsNFT": false,
        "NumNFTCopies": 0,
        "NumNFTCopiesForSale": 0,
        "NumNFTCopiesBurned": 0,
        "HasUnlockable": false,
        "NFTRoyaltyToCreatorBasisPoints": 0,
        "NFTRoyaltyToCoinBasisPoints": 0,
        "AdditionalDESORoyaltiesMap": {},
        "AdditionalCoinRoyaltiesMap": {},
        "DiamondsFromSender": 0,
        "HotnessScore": 0,
        "PostMultiplier": 0,
        "RecloutCount": 0,
        "QuoteRecloutCount": 0,
        "RecloutedPostEntryResponse": null
    },
    "PostAuthorProfile": {
        "PublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
        "Username": "sender",
        "Description": "",
        "IsHidden": false,
        "IsReserved": false,
        "IsVerified": false,
        "Comments": null,
        "Posts": null,
        "CoinEntry": {
            "CreatorBasisPoints": 0,
            "DeSoLockedNanos": 0,
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": 0,
            "CoinWatermarkNanos": 0,
            "BitCloutLockedNanos": 0
        },
        "DAOCoinEntry": {
            "NumberOfHolders": 0,
            "CoinsInCirculationNanos": "0x0",
            "MintingDisabled": false,
            "TransferRestrictionStatus": "unrestricted"
        },
        "CoinPriceDeSoNanos": 0,
        "CoinPriceBitCloutNanos": 0,
        "UsersThatHODL": null,
        "IsFeaturedTutorialWellKnownCreator": false,
        "IsFeaturedTutorialUpAndComingCreator": false,
        "ExtraData": null,
        "DESOBalanceNanos": 35999998542,
        "BestExchangeRateDESOPerDAOCoin": 0
    },
    "AppProfile": null
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameter provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Count post associations

<mark style="color:green;">`POST`</mark> `/api/v0/post-associations/count`

Count the number of post associations matching the provided query parameters.

#### Request Body

| Name                           | Type   | Description                                                            |
| ------------------------------ | ------ | ---------------------------------------------------------------------- |
| TransactorPublicKeyBase58Check | string | The public key of the user who created the association                 |
| PostHashHex                    | string | The identifier of the post to which the association references         |
| AppPublicKeyBase58Check        | string | The public key of the application on which the association was created |
| AssociationType                | string | The association type (exact match)                                     |
| AssociationTypePrefix          | string | The prefix of the association type (wildcard match)                    |
| AssociationValue               | string | The association value (exact match)                                    |
| AssociationValuePrefix         | string | The prefix of the association value (wildcard match)                   |

{% tabs %}
{% tab title="200: OK Successfully queried for the number of matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Count": 1
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Count post associations by multiple values

<mark style="color:green;">`POST`</mark> `/api/v0/post-associations/counts`

Count the number of post associations matching the provided query. Here, you can provide an array of association values and the count of associations matching any in that list will be returned.

#### Request Body

| Name                                                | Type      | Description                                                             |
| --------------------------------------------------- | --------- | ----------------------------------------------------------------------- |
| TransactorPublicKeyBase58Check                      | string    | The public key of the user who created the association                  |
| PostHashHex                                         | string    | The identifier of the post to which this association references         |
| AppPublicKeyBase58Check                             | string    | The public key of the application on which this association was created |
| AssociationType<mark style="color:red;">\*</mark>   | string    | The association type (exact match)                                      |
| AssociationValues<mark style="color:red;">\*</mark> | \[]string | An array of association values                                          |

{% tabs %}
{% tab title="200: OK Successfully queried for the number of matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Counts": {
        "HEART": 1,
        "LAUGH": 0
    },
    "Total": 1
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}

## Query for post associations

<mark style="color:green;">`POST`</mark> `/api/v0/post-associations/query`

Retrieve post associations matching the provided query parameters.

#### Request Body

| Name                           | Type      | Description                                                                                                                          |
| ------------------------------ | --------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| TransactorPublicKeyBase58Check | string    | The public key of the user who created the association                                                                               |
| PostHashHex                    | string    | The identifier of the post to which this association references                                                                      |
| AppPublicKeyBase58Check        | string    | The public key of the application on which this association was created                                                              |
| AssociationType                | string    | The association type (exact match)                                                                                                   |
| AssociationTypePrefix          | string    | The prefix of the association type (wildcard match)                                                                                  |
| AssociationValue               | string    | The association value (exact match)                                                                                                  |
| AssociationValuePrefix         | string    | The prefix of the association value (wildcard match)                                                                                 |
| AssociationValues              | \[]string | An array of association values; associations matching any of the values in this list will be returned                                |
| Limit                          | integer   | The maximum number of associations to retrieve (default is 100)                                                                      |
| LastSeenAssociationID          | string    | The identifier of the last retrieved association; this parameter functions like an offset allowing users to paginate through results |
| SortDescending                 | boolean   | If true, results are returned in reverse order                                                                                       |
| IncludeTransactorProfile       | boolean   | If true, include the transactors' use profiles in the response                                                                       |
| IncludePostEntry               | boolean   | If true, include the target posts' entries in the response                                                                           |
| IncludePostAuthorProfile       | boolean   | If true, include the target posts' authors' user profiles in the response                                                            |
| IncludeAppProfile              | boolean   | If true, include the applications' user profiles in the response                                                                     |

{% tabs %}
{% tab title="200: OK Successfully retrieved matching associations" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "Associations": [
        {
            "AssociationID": "77cc81b90caadf52bdbcadef72bcf87bdeefc31308779b401141c13e52670caf",
            "TransactorPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
            "PostHashHex": "7bf79e2fe04f8eadb66b9877ce04d55b64e2663fb34b392c2f215ca9d6fba938",
            "AppPublicKeyBase58Check": "tBCKVUCQ9WxpVmNthS2PKfY1BCxG4GkWvXqDhQ4q3zLtiwKVUNMGYS",
            "AssociationType": "REACTION",
            "AssociationValue": "HEART",
            "ExtraData": {
                "PeerID": "B"
            },
            "BlockHeight": 38,
            "TransactorProfile": null,
            "PostEntry": null,
            "PostAuthorProfile": null,
            "AppProfile": null
        }
    ],
    "PublicKeyToProfileEntryResponse": {},
    "PostHashHexToPostEntryResponse": {
        "7bf79e2fe04f8eadb66b9877ce04d55b64e2663fb34b392c2f215ca9d6fba938": {
            "PostHashHex": "7bf79e2fe04f8eadb66b9877ce04d55b64e2663fb34b392c2f215ca9d6fba938",
            "PosterPublicKeyBase58Check": "tBCKXFJEDSF7Thcc6BUBcB6kicE5qzmLbAtvFf9LfKSXN4LwFt36oX",
            "ParentStakeID": "",
            "Body": "Hello, world!",
            "ImageURLs": null,
            "VideoURLs": null,
            "RepostedPostEntryResponse": null,
            "CreatorBasisPoints": 1000,
            "StakeMultipleBasisPoints": 12500,
            "TimestampNanos": 1675278243313851247,
            "IsHidden": false,
            "ConfirmationBlockHeight": 38,
            "InMempool": true,
            "ProfileEntryResponse": null,
            "Comments": null,
            "LikeCount": 0,
            "DiamondCount": 0,
            "PostEntryReaderState": null,
            "IsPinned": false,
            "PostExtraData": {},
            "CommentCount": 0,
            "RepostCount": 0,
            "QuoteRepostCount": 0,
            "ParentPosts": null,
            "IsNFT": false,
            "NumNFTCopies": 0,
            "NumNFTCopiesForSale": 0,
            "NumNFTCopiesBurned": 0,
            "HasUnlockable": false,
            "NFTRoyaltyToCreatorBasisPoints": 0,
            "NFTRoyaltyToCoinBasisPoints": 0,
            "AdditionalDESORoyaltiesMap": {},
            "AdditionalCoinRoyaltiesMap": {},
            "DiamondsFromSender": 0,
            "HotnessScore": 0,
            "PostMultiplier": 0,
            "RecloutCount": 0,
            "QuoteRecloutCount": 0,
            "RecloutedPostEntryResponse": null
        }
    }
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endtab %}
{% endtabs %}
