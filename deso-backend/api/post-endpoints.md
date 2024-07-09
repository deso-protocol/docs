---
description: >-
  Description of endpoints used to get data related to posts on the DeSo
  blockchain
---

# Post Endpoints

Please make sure you've read [.](./ "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](./#profileentryresponse "mention")
* [#postentryresponse](./#postentryresponse "mention")
* [#balanceentryresponse](./#balanceentryresponse "mention")
* [#nftentryresponse](./#nftentryresponse "mention")
* [#nftcollectionresponse](./#nftcollectionresponse "mention")

## Get Posts Stateless

<mark style="color:green;">`POST`</mark> `/api/v0/get-posts-stateless`



Get Posts Stateless returns an array of posts based on the request body.  This endpoint is used to fetch posts for many different kinds of feeds.

To fetch the global feed, set `GetPostsForGlobalWhitelist` to `true`

To fetch the following feed for a user, set `GetPostsforFollowFeed` to `true`

To fetch the admin view of posts ordered by creator coin price, set `GetPostsByDeSo` to `true` and provide a value for `PostsByDESOMinutesLookback`

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L770).

Example usages in frontend:\
&#x20; \- Make request to [Get Posts Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1066)\
&#x20; \- Use Get Posts Stateless to get the [global feed](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L310)\
&#x20; \- Use Get Posts Stateless to [get the following feed for a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L391)\
&#x20; \- Use Get Posts Stateless to [get posts ordered by time for an admin to curate the global feed](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/admin/admin.component.ts#L290)\
&#x20; \- Use Get Posts Stateless to [get posts from user's with the high coin prices in the last hour](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/admin/admin.component.ts#L241)

#### Request Body

| Name                                         | Type    | Description                                                                                                                                                                |
| -------------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GetPostsForFollowFeed                        | Boolean | If true, get posts from creators that the reader follows                                                                                                                   |
| OrderBy                                      | String  | Order the posts by certain attributes                                                                                                                                      |
| NumToFetch<mark style="color:red;">\*</mark> | int     | Maximum Number of posts to return                                                                                                                                          |
| ReaderPublicKeyBase58Check                   | String  | Public key of the reader                                                                                                                                                   |
| PostHashHex                                  | String  | Start paginated look-up of posts after this post hash                                                                                                                      |
| GetPostsForGlobalWhitelist                   | Boolean | If true, get posts for the global feed - these are posts that have been manually selected for the global feed                                                              |
| PostContent                                  | String  | Filters out posts that do not match the text provided in PostContent (case-insensitive)                                                                                    |
| StartTstampSecs                              | uint64  | deprecated                                                                                                                                                                 |
| FetchSubcomments                             | Boolean | If true, fetches comments on comments of each post. Note: not implemented for the following feed                                                                           |
| GetPostsByDESO                               | Boolean | Get posts from the creators with the highest DESO locked within a certain timeframe                                                                                        |
| GetPostsByClout                              | Boolean | deprecated - use GetPostsByDESO instead                                                                                                                                    |
| MediaRequired                                | Boolean | If true, filter out posts that do not have images or video in them                                                                                                         |
| PostsByDESOMinutesLookback                   | uint64  | If GetPostsByDESO is true, get all posts within PostsByDESOMinutesLookback, order them by the creator's DESO locked, and take the top NumToFetch posts. Must be 60 or less |
| AddGlobalFeedBool                            | Boolean | If set to true, then the posts in the response will contain a boolean about whether they're in the global feed                                                             |

{% tabs %}
{% tab title="200: OK Successfully retrieved posts requested" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "PostsFound": [<PostEntryResponse>, <PostEntryResponse>], // Array of PostEntryResponses that were found for the provided request body.
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

## Get Single Post

<mark style="color:green;">`POST`</mark> `/api/v0/get-single-post`

Gets a single post, optionally including parents and children. This endpoint is used to display a thread view of a post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L968).

Example usages in frontend:\
&#x20; \- Make request to [Get Single Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1099)\
&#x20; \- Use GetSinglePost to [get a thread view](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/post-thread-page/post-thread/post-thread.component.ts#L289) of a post

#### Request Body

| Name                                           | Type    | Description                                                                                                    |
| ---------------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------- |
| PostHashHex<mark style="color:red;">\*</mark>  | String  | Hex of Post Hash to fetch                                                                                      |
| FetchParents                                   | Boolean | if true, fetch all parents of this post, up to 100 parents.                                                    |
| CommentOffset                                  | uint32  | <p>Offset at which to begin result set of comments returned.<br><br>Defaults to 0</p>                          |
| CommentLimit<mark style="color:red;">\*</mark> | uint32  | number of comments to return, starting at CommentOffset                                                        |
| ReaderPublicKeyBase58Check                     | String  | public key of the user reading this single post                                                                |
| AddGlobalFeedBool                              | Boolean | if set to true, then the posts in the response will contain a boolean indicating if they're in the global feed |

{% tabs %}
{% tab title="200: OK Successfully retrieved the single post and any parents or comments" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
 "PostFound": <PostEntryResponse>, // A single PostEntryResponse that optionally contains all children and parent of itself.
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

## Get Posts For Public Key

<mark style="color:green;">`POST`</mark> `/api/v0/get-posts-for-public-key`

Get posts created by a public key or username. This endpoint is used to populate the posts on a user's profile page.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1321).

Example usages in frontend:\
&#x20; \- Make request to [Get Posts For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1168)\
&#x20; \- Use GetPostsForPublicKey to [get posts to display on a user's profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-posts/creator-profile-posts.component.ts#L51)

#### Request Body

| Name                                         | Type    | Description                                                                                                        |
| -------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------ |
| PublicKeyBase58Check                         | String  | <p>Public key of the user whose posts we will fetch<br><br>Only required if Username is not provided</p>           |
| Username                                     | String  | <p>Username of the user whose posts we will fetch<br><br>Only required if PublicKeyBase58Check is not provided</p> |
| ReaderPublicKeyBase58Check                   | String  | public key of the user reading the posts                                                                           |
| LastPostHashHex                              | String  | Hex of the Post Hash that ended the previous page of results                                                       |
| NumToFetch<mark style="color:red;">\*</mark> | uint64  | Number of posts to fetch                                                                                           |
| MediaRequired                                | Boolean | if true, only return posts that have images, videos, or embed video URLs.                                          |

{% tabs %}
{% tab title="200: OK Successfully retrieved posts for a given public key or username" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "Posts": [<PostEntryResponse>, <PostEntryResponse>], // Array of PostEntryResponses representing a page of results for posts created by the provided public key.
  "LastPostHashHex": "21285bd8c0ed74125cd82a65a74606d8fcb84e309f58bb44b6cb0b76489897b5", //Hex of the last Post Hash in the array of Posts above.  
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

## Get Hot Feed

<mark style="color:green;">`POST`</mark> `/api/v0/get-hot-feed`

Get Hot Feed returns a page of Posts that are currently "hot". A post's hotness is determined by the time since the post was created and the number of likes, diamonds, comments, reposts, and quote reposts.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/hot\_feed.go#L605).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:\
&#x20; \- Make request to [Get Hot Feed](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1153)\
&#x20; \- Use GetHotFeed to [get posts to display to the user in the Hot Feed tab](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/feed/feed.component.ts#L501)

#### Request Body

| Name                       | Type      | Description                                               |
| -------------------------- | --------- | --------------------------------------------------------- |
| ReaderPublicKeyBase58Check | String    | public key of the user reading the posts                  |
| SeenPosts                  | String\[] | A list of posts that have already been seen by the reader |
| ResponseLimit              | uint64    | Number of posts to fetch                                  |

{% tabs %}
{% tab title="200: OK Successfully retrieved posts from the hot feed" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "HotFeedPage": [<PostEntryResponse>, <PostEntryResponse>,...] // Array of PostEntryResponses that represent the next page of the hot feed for the reader
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

## Get Diamonded Posts

<mark style="color:green;">`POST`</mark> `/api/v0/get-diamonded-posts`

Get all posts on which sender sent diamonds to the receiver. Posts are sorted by the number of diamonds given from the sender to the receiver and then by timestamp.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1466).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonded Posts](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1187)\
&#x20; \- Use GetDiamondedPosts to [get posts in which a specific user received diamonds from another specific user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamond-posts-page/diamond-posts/diamond-posts.component.ts#L56). Example on [node.deso.org](https://node.deso.org/u/LazyNina/diamonds/diamondhands)

#### Request Body

| Name                                         | Type    | Description                                                                                                                        |
| -------------------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| ReceiverPublicKeyBase58Check                 | String  | <p>Public key of the user who received diamonds from sender<br><br>Only required if ReceiverUsername is not provided</p>           |
| ReceiverUsername                             | String  | <p>Username of the user who received diamonds from sender<br><br>Only required if ReceiverPublicKeyBase58Check is not provided</p> |
| SenderPublicKeyBase58Check                   | String  | <p>Public key of the user who sent diamonds to the receiver<br><br>Only required if SenderUsername is not provided</p>             |
| SenderUsername                               | String  | <p>Username of the user who sent diamonds to the receiver<br><br>Only required if SenderPublicKeyBase58Check is not provided</p>   |
| ReaderPublicKeyBase58Check                   | String  | public key of the user reading the posts                                                                                           |
| StartPostHashHex                             | String  | Hex of the first Post Hash to include in this page of results                                                                      |
| NumToFetch<mark style="color:red;">\*</mark> | uint64  | Number of posts to fetch                                                                                                           |
| MediaRequired                                | Boolean | if true, only return posts that have images, videos, or embed video URLs.                                                          |

{% tabs %}
{% tab title="200: OK Successfully retrieved the requested diamonded posts between sender and receiver" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "DiamondedPosts": [<PostEntryResponse>, <PostEntryResponse>] // Array of PostEntryResponses. Each post is a post created by the receiver AND received diamonds from the sender. The DiamondsFromSender attribute is populated in each PostEntryResponse. Posts are ordered by DiamondsFromSender and then by timestamp.
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

## Get Likes For Post

<mark style="color:green;">`POST`</mark> `/api/v0/get-likes-for-post`

Get Profiles of users who liked a given post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629).

Example usages in frontend:\
&#x20; \- Make request to [Get Likes For Post](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629)\
&#x20; \- Use GetLikesForPosts to [show all users who have liked a po](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/likes-modal/likes-modal.component.ts#L40)

#### Request Body

| Name                                          | Type   | Description                                          |
| --------------------------------------------- | ------ | ---------------------------------------------------- |
| PostHashHex<mark style="color:red;">\*</mark> | String | Hex of Post hash for which we want to retrieve likes |
| Offset<mark style="color:red;">\*</mark>      | uint32 | Position at which to return this page of results     |
| Limit<mark style="color:red;">\*</mark>       | uint32 | Number of profiles to return in this page of results |
| ReaderPublicKeyBase58Check                    | String | Public key of the reader                             |

{% tabs %}
{% tab title="200: OK Successfully retrieved profiles of users who liked a given post" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "Likers": [<ProfileEntryResponse>, <ProfileEntryResponse>] // ProfileEntryResponses for users who liked this post.
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

## Get Diamonds For Post

<mark style="color:green;">`POST`</mark> `/api/v0/get-diamonds-for-post`

Get Profiles and number of diamonds for users who gave diamonds to a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1750).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonds For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1440)\
&#x20; \- Use GetDiamondsForPosts to [show all users who have diamonded a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamonds-modal/diamonds-modal.component.ts#L40) and how many diamonds the user gave

#### Request Body

| Name                                          | Type   | Description                                                          |
| --------------------------------------------- | ------ | -------------------------------------------------------------------- |
| PostHashHex<mark style="color:red;">\*</mark> | String | Hex of Post hash for which we want to retrieve profiles and diamonds |
| Offset<mark style="color:red;">\*</mark>      | uint32 | Position at which to return this page of results                     |
| Limit<mark style="color:red;">\*</mark>       | uint32 | Number of profiles to return in this page of results                 |
| ReaderPublicKeyBase58Check                    | String | Public key of the reader                                             |

{% tabs %}
{% tab title="200: OK Successfully retrieved profiles and diamond levels of users who gave diamonds the given post" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "DiamondSenders": [
    { 
      "DiamondSenderProfile": <ProfileEntryResponse>, // Profile of the user who gave diamonds to this post
      "DiamondLevel":  2 // Number of diamonds this user gave to this post
    }
  ]
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

## Get Reposts For Post

<mark style="color:green;">`POST`</mark> `/api/v0/get-reposts-for-post`

Get Profiles of users who reposted (without a quote) a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1885).

Example usages in frontend:\
&#x20; \- Make request to [Get Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1455)\
&#x20; \- Use GetRepostsForPosts to [show all users who have reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/reposts-modal/reposts-modal.component.ts#L40)

#### Request Body

| Name                                          | Type   | Description                                              |
| --------------------------------------------- | ------ | -------------------------------------------------------- |
| PostHashHex<mark style="color:red;">\*</mark> | String | Hex of Post hash for which we want to retrieve reposters |
| Offset<mark style="color:red;">\*</mark>      | uint32 | Position at which to return this page of results         |
| Limit<mark style="color:red;">\*</mark>       | uint32 | Number of profiles to return in this page of results     |
| ReaderPublicKeyBase58Check                    | String | Public key of the reader                                 |

{% tabs %}
{% tab title="200: OK Successfully retrieved profiles of users who reposted (without a quote) the provided post" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "Reposters": [<ProfileEntryResponse>, <ProfileEntryResponse>] // Profiles of users who reposted (without a quote) this post.
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

## Get Quote Reposts For Post

<mark style="color:green;">`POST`</mark> `/api/v0/get-quote-reposts-for-post`

Get profiles of users who quote reposted a given post and the content of the quote repost

Endpoint implementation in backend.

Example usages in frontend:\
&#x20; \- Make request to [Get Quote Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1470)\
&#x20; \- Use GetQuoteRepostsForPost to [show all users who have quoted reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/quote-reposts-modal/quote-reposts-modal.component.ts#L40) and what the quote said

#### Request Body

| Name                                          | Type   | Description                                                    |
| --------------------------------------------- | ------ | -------------------------------------------------------------- |
| PostHashHex<mark style="color:red;">\*</mark> | String | Hex of Post hash for which we want to retrieve quote reposters |
| Offset<mark style="color:red;">\*</mark>      | uint32 | Position at which to return this page of results               |
| Limit<mark style="color:red;">\*</mark>       | uint32 | Number of profiles to return in this page of results           |
| ReaderPublicKeyBase58Check                    | String | Public key of the reader                                       |

{% tabs %}
{% tab title="200: OK Successfully retrieved the profiles that quote reposted a given post and the post that is quote reposting" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.{

```
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}{
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}{
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}{
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}
```
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
