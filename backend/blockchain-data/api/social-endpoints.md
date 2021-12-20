---
description: Description of endpoints used to get social data on the DeSo blockchain
---

# Social Endpoints

Please make sure you've read [data-types.md](../basics/data-types.md "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](../basics/data-types.md#profileentryresponse "mention")
* [#postentryresponse](../basics/data-types.md#postentryresponse "mention")
* [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention")
* [#nftentryresponse](../basics/data-types.md#nftentryresponse "mention")
* [#nftcollectionresponse](../basics/data-types.md#nftcollectionresponse "mention")

{% swagger method="post" path="" baseUrl="/api/v0/get-hodlers-for-public-key" summary="Get Hodlers For Public Key" %}
{% swagger-description %}
Get [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention")objects for users who are holding (or held by) a certain public key's creator coin.\
\
Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1168).

Example usages in frontend:\
&#x20; \- Make request to [Get Hodlers For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1208)\
&#x20; \- Use GetHodlersForPublicKey to [show all the users who are holding a creator's coin](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-hodlers/creator-profile-hodlers.component.ts#L36)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" %}
Public key for which you want to fetch hodlings or hodlers

\




\


Required only if Username is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Username" type="String" %}
Username for which you want to fetch hodlings or hodlers

\




\


Required only if PublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="LastPublicKeyBase58Check" type="String" %}
Public key of the last hodler/hodlee from the previous page. Indicates the point at which we want to start returning results.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="uint64" %}
number of records to fetch
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchHodlings" type="Boolean" %}
If true, fetch balance entries for hodlings of the user instead of balance entries for hodlers of the user's coin
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchAll" type="Boolean" %}
if true, fetch all results. Supercedes NumToFetch.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved BalanceEntryResponses for each user who are holding/are held by the provided public key/username" %}
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
| Name                     | Type                                                                               | Description                                                                                                                                                                                                                                                                                                          |
| ------------------------ | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hodlers                  | [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention")\[] | Array of [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention") objects representing the users who hold (or are held by) the provided public key or username. Each [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention") tells you how much that user is holding. |
| LastPublicKeyBase58Check | String                                                                             | Public key of the last [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention")object from this page of results. Used to fetch the next page of results.                                                                                                                                      |
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

{% swagger method="post" path="" baseUrl="/api/v0/get-diamonds-for-public-key" summary="Get Diamonds for Public Key" %}
{% swagger-description %}
Get a list of objects representing all the diamonds a user has given or received.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1304).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonds For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1414)\
&#x20; \- Use GetDiamondsForPublicKey to [show all users who have given a creator a diamond, how many diamonds they've given to that creator, and the highest level of diamond](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-diamonds/creator-diamonds.component.ts#L41)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user for whom we want to fetch diamonds
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchYouDiamonded" type="Boolean" %}
If true, fetch diamonds this user gave out instead of diamond this user received
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved diamonds a user gave or received" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-follows-stateless" summary="Get Follows Stateless" %}
{% swagger-description %}
Get followers of a certain user/public key or get users followed by a certain user/public key and the total number of followers/followees.&#x20;

Endpoint implementation in backend.

Example usages in frontend:\
&#x20; \- Make request to [Get Follows Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1274)\
&#x20; \- Use GetFollows to show [the total number of users following a creator and the total number of users followed by a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-top-card/creator-profile-top-card.component.ts#L161)\
&#x20; \- Use GetFollows to [see all users who follow (or alternatively are followed by) a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/manage-follows-page/manage-follows/manage-follows.component.ts#L54)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" %}
Public key for which we want to fetch followers or followees

\




\


Required only if Username is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" type="String" name="Username" %}
Username for which we want to fetch followers or following

\




\


Required only if PublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetEntriesFollowingUsername" type="Boolean" %}
&#x20; \- If true, get entries that are following the specified user.

&#x20; \- If false, get entries that are followed by the specified user.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="LastPublicKeyBase58Check" type="String" %}
Public key of the last follower/followee from the previous page. Indicates the point at which we want to start returning results.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="uint64" %}
number of records to fetch
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the requested page of followers/followee" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/is-following-public-key" summary="Is Following Public Key" %}
{% swagger-description %}
Check if the user is following a public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2669).
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user that may be following
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="IsFollowingPublicKeyBase58Check" type="String" %}
Public key of the creator we want to check if the user is following
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved whether or not the request public key is following IsFollowingPublicKeyBase58Check" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/is-hodling-public-key" summary="Is Hodling Public Key" %}
{% swagger-description %}
Check if the user holds the creator coin of a public key. If user is holding some amount of creator coin, we return the BalanceEntryResponse representing how much the user holds.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2725).
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="PublicKeyBase58Check" type="String" %}
Public key of the user that may be holding the creator coin of IsHodlingPublicKeyBase58Check
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" type="String" name="IsHodlingPublicKeyBase58Check" %}
Public key of the creator we want to check that the user is holding
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved whether PublicKeyBas58Check is hodling the creator coin of IsHodlingPublicKeyBase58Check" %}
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
| Name         | Type                                                                            | Description                                                                                                                            |
| ------------ | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| IsHodling    | Boolean                                                                         | true if the user holds the creator coin of IsHodlingPublicKeyBase58Check, Otherwise, false.                                            |
| BalanceEntry | [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention") | [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention") that shows the amount of creator coins the user holds. |
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
