---
description: Description of endpoints used to get social data on the DeSo blockchain
---

# Social Endpoints

Please make sure you've read [.](./ "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](./#profileentryresponse "mention")
* [#postentryresponse](./#postentryresponse "mention")
* [#balanceentryresponse](./#balanceentryresponse "mention")
* [#nftentryresponse](./#nftentryresponse "mention")
* [#nftcollectionresponse](./#nftcollectionresponse "mention")

## Get Hodlers For Public Key

<mark style="color:green;">`POST`</mark> `/api/v0/get-hodlers-for-public-key`

Get [#balanceentryresponse](./#balanceentryresponse "mention") objects for users who are holding (or held by) a certain public key's creator coin.\
\
Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/user.go#L1224).

Example usages in frontend:\
&#x20; \- Make request to [Get Hodlers For Public Key](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1366)\
&#x20; \- Use GetHodlersForPublicKey to [show all the users who are holding a creator's coin or a creator's DAO coin](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/creator-profile-page/creator-profile-hodlers/creator-profile-hodlers.component.ts#L37)\
&#x20; \- Use GetHodlersForPublicKey to [see all users who hold your DAO coin](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L112)\
&#x20; \- Use GetHodlersForPublicKey to [see all DAO coins you hold](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/dao-coins.component.ts#L139)

#### Request Body

| Name                     | Type    | Description                                                                                                                   |
| ------------------------ | ------- | ----------------------------------------------------------------------------------------------------------------------------- |
| PublicKeyBase58Check     | String  | <p>Public key for which you want to fetch hodlings or hodlers<br><br>Required only if Username is not provided</p>            |
| Username                 | String  | <p>Username for which you want to fetch hodlings or hodlers<br><br>Required only if PublicKeyBase58Check is not provided</p>  |
| LastPublicKeyBase58Check | String  | Public key of the last hodler/hodlee from the previous page. Indicates the point at which we want to start returning results. |
| NumToFetch               | uint64  | number of records to fetch                                                                                                    |
| FetchHodlings            | Boolean | If true, fetch balance entries for hodlings of the user instead of balance entries for hodlers of the user's coin             |
| FetchAll                 | Boolean | if true, fetch all results. Supercedes NumToFetch.                                                                            |
| IsDAOCoin                | Boolean | If true, fetch hodlers of DAO coin instead of creator coin                                                                    |

{% tabs %}
{% tab title="200: OK Successfully retrieved BalanceEntryResponses for each user who are holding/are held by the provided public key/username" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    Hodlers: [<BalanceEntryResponse>, <BalanceEntryResponse>],
    LastPublicKeyBase58Check: "BC1YLianxEsskKYNyL959k6b6UPYtRXfZs4MF3GkbWofdoFQzZCkJRB" 
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
<table><thead><tr><th width="280.3333333333333">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>Hodlers</td><td><a data-mention href="broken-reference">Broken link</a>[]</td><td>Array of <a data-mention href="broken-reference">Broken link</a> objects representing the users who hold (or are held by) the provided public key or username. Each <a data-mention href="broken-reference">Broken link</a> tells you how much that user is holding.</td></tr><tr><td>LastPublicKeyBase58Check</td><td>String</td><td>Public key of the last <a data-mention href="broken-reference">Broken link</a>object from this page of results. Used to fetch the next page of results.</td></tr></tbody></table>
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

## Get Diamonds for Public Key

<mark style="color:green;">`POST`</mark> `/api/v0/get-diamonds-for-public-key`

Get a list of objects representing all the diamonds a user has given or received.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1304).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonds For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1414)\
&#x20; \- Use GetDiamondsForPublicKey to [show all users who have given a creator a diamond, how many diamonds they've given to that creator, and the highest level of diamond](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-diamonds/creator-diamonds.component.ts#L41)

#### Request Body

| Name                                                   | Type    | Description                                                                      |
| ------------------------------------------------------ | ------- | -------------------------------------------------------------------------------- |
| PublicKeyBase58Check<mark style="color:red;">\*</mark> | String  | Public key of the user for whom we want to fetch diamonds                        |
| FetchYouDiamonded                                      | Boolean | If true, fetch diamonds this user gave out instead of diamond this user received |

{% tabs %}
{% tab title="200: OK Successfully retrieved diamonds a user gave or received" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    DiamondSenderSummaryResponses: [
        {
            SenderPublicKeyBase58Check: "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public Key of the user who sent the diamond
            ReceiverPublicKeyBase58Check: "BC1YLgxLrxvq5mgZUUhJc1gkG6pwrRCTbdT6snwcrsEampjqnSD1vck", // Public key of the user who received the diamond
            TotalDiamonds: 8, // The number of diamonds the sender has sent to the receiver.
            HighestDiamondLevel: 2, // The highest level of diamond the sender has sent to the receiver.
            DiamondLevelMap: { 1: 4, 2: 2 }, // Map of diamond level to the number of times the sender gave that level of diamond to the receiver.
            ProfileEntryResponse: <ProfileEntryResponse>, // If FetchYouDiamonded is true, this will be the profile of the receiver. If false, this will be the profile of the sender.
        },
    ],
    TotalDiamonds: 555 // Total number of diamonds received or given by the profile that matches PublicKeyBase58Check
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

## Get Follows Stateless

<mark style="color:green;">`POST`</mark> `/api/v0/get-follows-stateless`

Get followers of a certain user/public key or get users followed by a certain user/public key and the total number of followers/followees.&#x20;

Endpoint implementation in backend.

Example usages in frontend:\
&#x20; \- Make request to [Get Follows Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1274)\
&#x20; \- Use GetFollows to show [the total number of users following a creator and the total number of users followed by a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-top-card/creator-profile-top-card.component.ts#L161)\
&#x20; \- Use GetFollows to [see all users who follow (or alternatively are followed by) a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/manage-follows-page/manage-follows/manage-follows.component.ts#L54)

#### Request Body

| Name                        | Type    | Description                                                                                                                                     |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| PublicKeyBase58Check        | String  | <p>Public key for which we want to fetch followers or followees<br><br>Required only if Username is not provided</p>                            |
| Username                    | String  | <p>Username for which we want to fetch followers or following<br><br>Required only if PublicKeyBase58Check is not provided</p>                  |
| GetEntriesFollowingUsername | Boolean | <p>  - If true, get entries that are following the specified user.</p><p>  - If false, get entries that are followed by the specified user.</p> |
| LastPublicKeyBase58Check    | String  | Public key of the last follower/followee from the previous page. Indicates the point at which we want to start returning results.               |
| NumToFetch                  | uint64  | number of records to fetch                                                                                                                      |

{% tabs %}
{% tab title="200: OK Successfully retrieved the requested page of followers/followee" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  PublicKeyToProfileEntry: { // Map of public key to ProfileEntryResponse representing followers or followees
    "BC1YLfuD5AGm2guj3q5wF7WGi3jTUzNhHUHc84GtVsk9kHyxbnk5V1H" : <ProfileEntryResponse>
  }, 
  NumFollowers: 17707 // Total number of followers or followees
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

## Is Following Public Key

<mark style="color:green;">`POST`</mark> `/api/v0/is-following-public-key`

Check if the user is following a public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2669).

#### Request Body

| Name                                                              | Type   | Description                                                         |
| ----------------------------------------------------------------- | ------ | ------------------------------------------------------------------- |
| PublicKeyBase58Check<mark style="color:red;">\*</mark>            | String | Public key of the user that may be following                        |
| IsFollowingPublicKeyBase58Check<mark style="color:red;">\*</mark> | String | Public key of the creator we want to check if the user is following |

{% tabs %}
{% tab title="200: OK Successfully retrieved whether or not the request public key is following IsFollowingPublicKeyBase58Check" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  IsFollowing: true // true if the user is following the IsFollowingPublicKeyBase58Check. Otherwise, false.
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name        | Type    | Description                                                                          |
| ----------- | ------- | ------------------------------------------------------------------------------------ |
| IsFollowing | Boolean | true if the user is following the IsFollowingPublicKeyBase58Check. Otherwise, false  |
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}

## Is Hodling Public Key

<mark style="color:green;">`POST`</mark> `/api/v0/is-hodling-public-key`

Check if the user holds the creator coin of a public key. If user is holding some amount of creator coin, we return the BalanceEntryResponse representing how much the user holds.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/user.go#L2836).

Example usages in frontend:\
&#x20; \- Make request to [Is Hodling Public Key](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/backend-api.service.ts#L1387)\
&#x20; \- Use IsHodlingPublicKey to [check if a user is a DAO member and can be transferred a DAO coin that is restricted to DAO members only.](https://github.com/deso-protocol/frontend/blob/60cf5571269c01b13da618e214d35d7f2b5614f1/src/app/dao-coins/transfer-dao-coin-modal/transfer-dao-coin-modal.component.ts#L53)

#### Request Body

| Name                                                            | Type    | Description                                                                                  |
| --------------------------------------------------------------- | ------- | -------------------------------------------------------------------------------------------- |
| PublicKeyBase58Check<mark style="color:red;">\*</mark>          | String  | Public key of the user that may be holding the creator coin of IsHodlingPublicKeyBase58Check |
| IsHodlingPublicKeyBase58Check<mark style="color:red;">\*</mark> | String  | Public key of the creator we want to check that the user is holding                          |
| IsDAOCoin                                                       | Boolean | If true, check if this public key is hodling the DAO coin instead of creator coin            |

{% tabs %}
{% tab title="200: OK Successfully retrieved whether PublicKeyBas58Check is hodling the creator coin or DAO coin of IsHodlingPublicKeyBase58Check" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  IsHodling: true, // true if the user holds the creator coin of IsHodlingPublicKeyBase58Check, Otherwise, false.
  BalanceEntry: <BalanceEntryResponse> // Balance entry that shows the amount of creator coins the user holds.
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name         | Type                                      | Description                                                                                      |
| ------------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------ |
| IsHodling    | Boolean                                   | true if the user holds the creator coin of IsHodlingPublicKeyBase58Check, Otherwise, false.      |
| BalanceEntry | [Broken link](broken-reference "mention") | [Broken link](broken-reference "mention") that shows the amount of creator coins the user holds. |
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    // Response
}
```
{% endtab %}
{% endtabs %}
