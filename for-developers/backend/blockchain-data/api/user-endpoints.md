---
description: >-
  Description of endpoints used to get data related to users and profiles on the
  DeSo blockchain
---

# User Endpoints

Please make sure you've read [data-types.md](../basics/data-types.md "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](../basics/data-types.md#profileentryresponse "mention")
* [#postentryresponse](../basics/data-types.md#postentryresponse "mention")
* [#balanceentryresponse](../basics/data-types.md#balanceentryresponse "mention")
* [#nftentryresponse](../basics/data-types.md#nftentryresponse "mention")
* [#nftcollectionresponse](../basics/data-types.md#nftcollectionresponse "mention")

{% swagger method="post" path="" baseUrl="/api/v0/get-users-stateless" summary="Get Users Stateless" %}
{% swagger-description %}
Get information about multiple users. This endpoint is used for retrieving data about a user after they log in,  so the UI can adjust to the attributes of the user.

Request contains a list of public keys of users to fetch.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/user.go#L40).

Example usages in frontend:\
&#x20; \- Make request to [Get Users Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L725)\
&#x20; \- Use GetUsersStateless to [retrieve data about users upon login](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/app.component.ts#L115)\
&#x20; \- Use GetUsersStateless to [retrieve profiles for the Bithunt community projects leaderboard](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/bithunt/bithunt-service.ts#L77)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeysBase58Check" type="String[]" required="true" %}
list of public keys
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SkipForLeaderBoard" type="Boolean" %}
Skips fetching all attributes other than the ProfileEntryResponse and PublicKeyBase58Check
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetUnminedBalance" type="Boolean" %}
If true, get all UTXOs to compute the user's unmined balance. This is slower and not recommended.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully return all user objects requested, param updater public keys, and the default fee" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  UserList: [
    {
      PublicKeyBase58Check: "BC1YLg3FS19Syz9h6fqErZEtsKkRxBfkzqp75PiGwMUXJ1fLrytRVVk",
      ProfileEntryResponse: <ProfileEntryResponse>,
      Utxos: [], // Deprecated
      BalanceNanos: 100, // User's balance. If SkipForLeaderboard, 0 is always returned. 
      UnminedBalanceNanos: 5, // User's balance in nanos that has not been mined into a block yet. If SkipForLeaderboard, 0 is always returned.
      PublicKeysBase58CheckFollowedByUser: ["BC1YLianxEsskKYNyL959k6b6UPYtRXfZs4MF3GkbWofdoFQzZCkJRB"], // List of public keys followed by this user. If SkipForLeaderboard, an empty array is always returned.
      UsersYouHODL: [<BalanceEntryResponse>, <BalanceEntryResponse>], // Array of Balance entry responses representing this user's creator coin holdings. If SkipForLeaderboard, an empty array is always returned.
      UsersWhoHODLYou: 2, // Count of the number of unique public keys that own this user's creator coin. If SkipForLeaderboard, 0 is always returned.
      HasPhoneNumber: false, // If true, this user has verified their phone number for free DeSo. If SkipForLeaderboard, 0 is always returned.
      CanCreateProfile: true, // If true, this user is able to create a profile. Either they have enough DeSo to cover the create profile fee OR they've verified their phone number OR verified through the Jumio flow. If SkipForLeaderboard, false is always returned.
      BlockedPubKeys: { // BlockedPubKeys is a map with keys representing Public Keys this user has blocked. Values are empty object with no significance. A map is used to improve look up performance. If SkipForLeaderboard, an empty object is always returned.
        "BC1YLiUt3iQG4QY8KHLPXP8LznyFp3k9vFTg2mhhu76d1Uu2WMw9RVY": {},
      },
      HasEmail: true, // If true, the user has provided an email. If SkipForLeaderboard, false is always returned.
      EmailVerified: true, // If true, the user has verified their email address by clicking on a link sent to them. If SkipForLeaderboard, false is always returned. 
      JumioStartTime: 1908230921, // The time at which this user began the Jumio flow. If SkipForLeaderboard, 0 is always returned.
      JumioFinishedTime: 1908230949, // The time at which the user finished the Jumio flow. If SkipForLeaderboard, 0 is always returned.
      JumioVerified: true, // If true, the user successfully completed the Jumio flow and Jumio verified their identity. If SkipForLeaderboard, false is always returned.
      JumioReturned: true, // If true, Jumio's callback after scanning ID has been received. If SkipForLeaderboard, false is always returned.
      IsAdmin: true, // If true, this user is an admin on this node. If SkipForLeaderboard, false is always returned.
      IsSuperAdmin: true, // If true, this user is a superadmin on this node. If SkipForLeaderboard, false is always returned.
      IsBlacklisted: false, // If true, this user is blacklisted on this node. If SkipForLeaderboard, false is always returned.
      IsGraylisted: false, // If true, this user is graylisted on this node. If SkipForLeaderboard, false is always returned.
      TutorialStatus: "TutorialStarted", // Indicates where in the tutorial the user currently is. If SkipForLeaderboard, the empty string is always returned.
      CreatorPurchasedInTutorialUsername: "LazyNina", // Username of the creator the user purchased during the tutorial flow. If SkipForLeaderboard, the empty string is always returned.
      CreatorCoinPurchasedInTutorial: 1823789, // The amount of Lazy Nina creator coins purchased in the tutorial. If SkipForLeaderboard, 0 is always returned.false
      MustCompleteTutorial: true, // If true, the user must finish the tutorial before they are able to perform basic transfers on this node. If SkipForLeaderboard, false is always returned.
    },
  ], 
  DefaultFeeRateNanosPerKB: 100, // The minimum network fee rate in Nanos Per KB. 
  ParamUpdaters: { // Map of Public key to Param Updaters.
    BC1YLfoSyJWKjHGnj5ZqbSokC3LPDNBMDwHX3ehZDCA3HVkFNiPY5cQ: true,
    BC1YLfz4GH3Gfj6dCtBi8bNdNTbTdcibk8iCZS75toUn4UKZaTJnz9y: true,
    BC1YLg3oh6Boj8e2boCo1vQCYHLk1rjsHF6jthBdvSw79bixQvKK6Qa: true,
    BC1YLgD1f7yw7Ue8qQiW7QMBSm6J7fsieK5rRtyxmWqL2Ypra2BAToc: true,
    BC1YLgGLKjuHUFZZQcNYrdWRrHsDKUofd9MSxDq4NY53x7vGt4H32oZ: true,
    BC1YLiXwGTte8oXEEVzm4zqtDpGRx44Y4rqbeFeAs5MnzsmqT5RcqkW: true,
    BC1YLj8UkNMbCsmTUTx5Z2bhtp8q86csDthRmK6zbYstjjbS5eHoGkr: true,
  } 
}5
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

{% swagger method="post" path="" baseUrl="/api/v0/get-profiles" summary="Get Profiles" %}
{% swagger-description %}
Get user profiles for searching by name or for the creator leaderboard.&#x20;

Default number of returned profiles is 20.&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L599).

Example usages in frontend:\
&#x20; \- Make request to [Get Profiles](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1118)\
&#x20; \- Use GetProfiles to [search for users by username](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/search-bar/search-bar.component.ts#L94)\
&#x20; \- Use GetProfiles to [get profiles for the creator leaderboard](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creators-leaderboard/creators-leaderboard/creators-leaderboard.component.ts#L59)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" %}
When provided, find the profile that contains the public key. The page of results returned by this endpoint will start at the profile that is next in the ordered list.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Username" type="String" %}
When provided, find the profile that contains this username. The page of results returned by this endpoint will start at the profile that is next in the ordered list.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="UsernamePrefix" type="String" %}
username prefix. When provided, only return profiles with usernames that match this prefix
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Descripton" type="String" %}
Deprecated
{% endswagger-parameter %}

{% swagger-parameter in="body" name="OrderBy" type="String" %}
Ordering method to be used on the result set

\




\


Must be one of 

`newest_last_post`

, 

`newest_last_comment`

,  or 

`influencer_coin_price`
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="uint32" %}
Number of profiles to fetch. Defaults to 20.

\




\


Must be less than 100
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Reader public key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ModerationType" type="String" %}
empty string, `unrestricted`, or `leaderboard`.&#x20;



If `unrestricted`, return all results. If `leaderboard`, filter out both blacklisted and graylisted users. If empty string, filter out blacklisted users.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchUsersThatHODL" type="Boolean" %}
If single profile is requested, return a list of HODLers
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AddGlobalFeedBool" type="Boolean" %}
If set to true posts in response will contain boolean if they are in global feed
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="List of profiles requested and a NextPublicKey to use to get the next page of results " %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    ProfilesFound: [<ProfileEntryResponse>, <ProfileEntryResponse>], // Array of ProfileEntryResponse Objects.
    NextPublicKey: "BC1YLianxEsskKYNyL959k6b6UPYtRXfZs4MF3GkbWofdoFQzZCkJRB", // This is the PublicKeyBase58Check needed to fetch the next page of results. 
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

{% swagger method="post" path="" baseUrl="/api/v0/get-single-profile" summary="Get Single Profile" %}
{% swagger-description %}
Get information about single profile.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1066).

Example usages in frontend:\
&#x20; \- Make request to [Get Single Profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1144)\
&#x20; \- Use GetSingleProfile to [get information to display on a user's profile page](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-details/creator-profile-details.component.ts#L170)\
&#x20; \- Use GetSingleProfile to [search for a profile by public key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/search-bar/search-bar.component.ts#L60)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="String" %}
public key of the profile to fetch

\




\


required if Username is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Username" type="String" %}
username of the profile to fetch

\




\


required if PublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NoErrorOnMissing" type="Boolean" %}
If true, do not throw a 404 error if there is no profile found. Instead, nil will be return for the Profile in the response body.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
   Profile: <ProfileEntryResponse>, // ProfileEntryResponse for the username or public key provided.
   IsBlacklisted: false, // If true, this user is blacklisted on this node.
   IsGraylisted: false,  // If true, this user is graylisted on this node.
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

{% swagger-response status="404: Not Found" description="No profile found for the provided username or public key" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="" baseUrl="/api/v0/get-signle-profile-picture/{PublicKeyBase58Check}" summary="Get Single Profile Picture" %}
{% swagger-description %}
Returns the profile picture of the given public key

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1011).

Example usage in frontend:\
&#x20; \- Construct the [Get Single Profile Picture URL](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1158)\
&#x20; \- Use GetSingleProfilePictureURL to [get the profile picture for a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/avatar/avatar.directive.ts#L36)
{% endswagger-description %}

{% swagger-parameter in="path" name="PublicKeyBase58Check" required="true" type="String" %}
Public key of the user for whom we are fetching a profile picture
{% endswagger-parameter %}

{% swagger-parameter in="query" name="fallback" type="String" %}
URL of the image to be used in the event that there is no profile picture for the public key provided
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Profile picture found" %}
Image file of the profile picture is returned
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="No profile picture found for public key provided" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-user-global-metadata" summary="Get User Global Metadata: Email And Phone Number" %}
{% swagger-description %}
Get user metadata such as email and phone.&#x20;

This endpoint requires a JWT, [which can be retrieved from Identity](../../../identity/iframe-api/endpoints.md#jwt).

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1646).

Example usages in frontend:\
&#x20; \- Make request to [Get User Global Metadata](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1637)\
&#x20; \- Use GetUserGlobalMetadata to [fetch email address to display on settings page](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/settings/settings.component.ts#L45)

Note: this data is not stored on-chain.
{% endswagger-description %}

{% swagger-parameter in="body" name="UserPublicKeyBase58Check" required="true" type="String" %}
user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="JWT" required="true" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved user's phone number and email address" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    Email: "test@test.com",
    PhoneNumber: "123346789"
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

{% swagger method="post" path="" baseUrl="/api/v0/update-user-global-metadata" summary="Update User Global Metadata: Email and State of Messages Read" %}
{% swagger-description %}
Update user's email address and the state of messages that have been read.&#x20;

This endpoint requires a JWT, [which can be retrieved from Identity](../../../identity/iframe-api/endpoints.md#jwt).&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1711).

Example usages in frontend:\
&#x20; \- Make request to [Update User Global Metadata](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1624)\
&#x20; \- Use UpdateUserGlobalMetadata to [update email address](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/settings/settings.component.ts#L78)
{% endswagger-description %}

{% swagger-parameter in="body" name="UserPublicKeyBase58Check" required="true" type="String" %}
user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="JWT" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Email" type="String" %}
new email address. If provided, an email will be sent to verify this email address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MessageReadStateUpdatesByContact" type="Object with string keys and int values" %}
A map with keys that represent public keys that have messaged this user. The values represent the index of the message read in the conversation between the two users.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully updated user global metadata" %}
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

{% swagger method="get" path="" baseUrl="/api/v0/get-user-metadata/{PublicKeyBase58Check}" summary="Get User Metadata" %}
{% swagger-description %}
Get user metadata. Typically, this endpoint is used when reaching node.deso.org to get user metadata that should be merged with user metadata from one's local node.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L438).

Example usages in frontend:\
&#x20; \- Make request to [Get User Metadata](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1667)\
&#x20; \- Use GetUserMetadata to [get additional data about a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/app.component.ts#L117) to merge in with user metadata fetch from local node
{% endswagger-description %}

{% swagger-parameter in="path" name="PublicKeyBase58Check" type="String" required="true" %}
Public key of the user for whom we are fetching user metadata
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved user metadata from this node" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "HasPhoneNumber": false, // Whether the user verified a phone number. Used to know whether to allow user to launch identity to get free DeSo for verifying their phone number 
  "CanCreateProfile": true, // Whether or not this user is capable of creating a profile. Even if a user does not have enough DeSo to create a profile, they may be able to create a profile since it will be comped by the node to which this request was made. Usually profile creation will be comped if a user verified either their phone number or went through the Jumio flow.
  "BlockedPubKeys": { // Map of public keys that are blocked by the user.
    "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s": {},
  },
  "HasEmail": false, // Whether the user provided an email to the node to which this request was made.
  "EmailVerified": false, // Whether the user verified their email address.
  "JumioFinishedTime": 1629943843752943900, // Time user finished the jumio flow
  "JumioVerified": true, // Whether the user was verified by Jumio
  "JumioReturned": true // Whether Jumio returned a callback for this user.
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

{% swagger-response status="404: Not Found" description="Node does not expose its global state so you are unable to get user metadata from the requested node" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/delete-pii" summary="Delete PII - Personal Identifiable Information" %}
{% swagger-description %}
Deletes email address and phone number associated with this user from the node's global state.

This endpoint requires a JWT, [which can be retrieved from Identity](../../../identity/iframe-api/endpoints.md#jwt).

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2873).

Example usages in frontend:\
&#x20; \- Make request to [Delete PII](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1661)\
&#x20; \- Use DeletePII to [remove user's personal information](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/settings/settings.component.ts#L119) at the user's request
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="PublicKeyBase58Check" type="String" %}
user public key that wants to delete their PII
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="JWT" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully deleted personal identifiable information including phone number and email address" %}
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

{% swagger method="post" path="" baseUrl="/api/v0/block-public-key" summary="Block Public Key" %}
{% swagger-description %}
Block another user. Blocking a user hides that user from everything you see and the blocked user's comments on your posts will be hidden for all other users.

Note: Blocks are not currently stored on chain and thus a block only applies to the node on which a user is blocked.

This endpoint requires a JWT, [which can be retrieved from Identity](../../../identity/iframe-api/endpoints.md#jwt).

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2585).

Example usages in frontend:\
&#x20; \- Make request to [Block Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1570)\
&#x20; \- Use BlockPublicKey to [block a user ](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-details/creator-profile-details.component.ts#L136)as described above\
&#x20; \- Use BlockPublicKey to [unblock a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-details/creator-profile-details.component.ts#L91)
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="PublicKeyBase58Check" type="String" %}
user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="JWT" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-parameter in="body" name="BlockPublicKeyBase58Check" type="String" required="true" %}
blocked user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Unblock" type="Boolean" %}
false if block, true if unblock
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully blocked public key and return all public keys currently blocked by this user" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
    BlockedPublicKeys: {
        "BC1YLhqEhWvNnwW9TBqXURFqwkdpUYKrMVgTHQzopF5rRBDcD1LLSUp": {}, // Every key in the map is a blocked pulic key.
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

{% swagger method="post" path="" baseUrl="/api/v0/get-user-derived-keys" summary="Get User Derived Keys" %}
{% swagger-description %}
Get a map of derived public keys to metadata about that derived key for a given master public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2810).
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" required="true" type="String" %}
Public key for which we want to query derived keys
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved derived keys for user" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  DerivedKeys: {
    BC1YLhqEhWvNnwW9TBqXURFqwkdpUYKrMVgTHQzopF5rRBDcD1LLSUp: { // Key is a derived key
      OwnerPublicKeyBase58Check: "BC1YLfuD5AGm2guj3q5wF7WGi3jTUzNhHUHc84GtVsk9kHyxbnk5V1H", // Owner of the derived key.
      DerivedPublicKeyBase58Check: "BC1YLhqEhWvNnwW9TBqXURFqwkdpUYKrMVgTHQzopF5rRBDcD1LLSUp", // The derived key itself.
      ExpirationBlock: 1000000, // THe block height at which the derived key expires.
      IsValid: true // If true, this derived key can still perform actions on behalf of the owner.
    } 
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

{% swagger method="post" path="" baseUrl="/api/v0/delete-identities" summary="Delete Identities" %}
{% swagger-description %}
Temporary route to wipe [seedinfo cookies](../../../../code/walkthrough.md#seed-creation-and-transaction-construction). This endpoint relies on [identity api](../../../../devs/identity-api.md).&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L498).

Example usages in frontend:\
&#x20; \- Make request to [Delete Identities](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L597)\
&#x20; \- Use DeleteIdentities to [clean up legacy seedinfo storage](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/app.component.ts#L360)
{% endswagger-description %}

{% swagger-response status="200: OK" description="Successfully deleted cookies" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
