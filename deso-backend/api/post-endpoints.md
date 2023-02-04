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

{% swagger method="post" path="" baseUrl="/api/v0/get-posts-stateless" summary="Get Posts Stateless" %}
{% swagger-description %}


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
{% endswagger-description %}

{% swagger-parameter in="body" type="String" name="PostHashHex" %}
Start paginated look-up of posts after this post hash
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-parameter in="body" name="OrderBy" type="String" %}
Order the posts by certain attributes
{% endswagger-parameter %}

{% swagger-parameter in="body" name="StartTstampSecs" type="uint64" %}
deprecated
{% endswagger-parameter %}

{% swagger-parameter in="body" name="PostContent" type="String" %}
Filters out posts that do not match the text provided in PostContent (case-insensitive)
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="NumToFetch" type="int" %}
Maximum Number of posts to return
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchSubcomments" type="Boolean" %}
If true, fetches comments on comments of each post. Note: not implemented for the following feed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetPostsForFollowFeed" type="Boolean" %}
If true, get posts from creators that the reader follows
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetPostsForGlobalWhitelist" type="Boolean" %}
If true, get posts for the global feed - these are posts that have been manually selected for the global feed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetPostsByDESO" type="Boolean" %}
Get posts from the creators with the highest DESO locked within a certain timeframe
{% endswagger-parameter %}

{% swagger-parameter in="body" name="GetPostsByClout" type="Boolean" %}
deprecated - use GetPostsByDESO instead
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MediaRequired" type="Boolean" %}
If true, filter out posts that do not have images or video in them
{% endswagger-parameter %}

{% swagger-parameter in="body" name="PostsByDESOMinutesLookback" type="uint64" %}
If GetPostsByDESO is true, get all posts within PostsByDESOMinutesLookback, order them by the creator's DESO locked, and take the top NumToFetch posts. Must be 60 or less
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AddGlobalFeedBool" type="Boolean" %}
If set to true, then the posts in the response will contain a boolean about whether they're in the global feed
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved posts requested" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-single-post" summary="Get Single Post" %}
{% swagger-description %}
Gets a single post, optionally including parents and children. This endpoint is used to display a thread view of a post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L968).

Example usages in frontend:\
&#x20; \- Make request to [Get Single Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1099)\
&#x20; \- Use GetSinglePost to [get a thread view](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/post-thread-page/post-thread/post-thread.component.ts#L289) of a post
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post Hash to fetch
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FetchParents" type="Boolean" %}
if true, fetch all parents of this post, up to 100 parents.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="CommentOffset" type="uint32" %}
Offset at which to begin result set of comments returned.

\




\


Defaults to 0
{% endswagger-parameter %}

{% swagger-parameter in="body" name="CommentLimit" type="uint32" required="true" %}
number of comments to return, starting at CommentOffset
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
public key of the user reading this single post
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AddGlobalFeedBool" type="Boolean" %}
if set to true, then the posts in the response will contain a boolean indicating if they're in the global feed
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the single post and any parents or comments" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-posts-for-public-key" summary="Get Posts For Public Key" %}
{% swagger-description %}
Get posts created by a public key or username. This endpoint is used to populate the posts on a user's profile page.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1321).

Example usages in frontend:\
&#x20; \- Make request to [Get Posts For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1168)\
&#x20; \- Use GetPostsForPublicKey to [get posts to display on a user's profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-posts/creator-profile-posts.component.ts#L51)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" required="false" type="String" %}
Public key of the user whose posts we will fetch

\




\


Only required if Username is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Username" type="String" %}
Username of the user whose posts we will fetch

\




\


Only required if PublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
public key of the user reading the posts
{% endswagger-parameter %}

{% swagger-parameter in="body" name="LastPostHashHex" type="String" %}
Hex of the Post Hash that ended the previous page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="uint64" required="true" %}
Number of posts to fetch
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MediaRequired" type="Boolean" %}
if true, only return posts that have images, videos, or embed video URLs.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved posts for a given public key or username" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-hot-feed" summary="Get Hot Feed" %}
{% swagger-description %}
Get Hot Feed returns a page of Posts that are currently "hot". A post's hotness is determined by the time since the post was created and the number of likes, diamonds, comments, reposts, and quote reposts.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/hot\_feed.go#L605).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:\
&#x20; \- Make request to [Get Hot Feed](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1153)\
&#x20; \- Use GetHotFeed to [get posts to display to the user in the Hot Feed tab](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/feed/feed.component.ts#L501)
{% endswagger-description %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
public key of the user reading the posts
{% endswagger-parameter %}

{% swagger-parameter in="body" type="String[]" name="SeenPosts" %}
A list of posts that have already been seen by the reader
{% endswagger-parameter %}

{% swagger-parameter in="body" type="uint64" name="ResponseLimit" %}
Number of posts to fetch
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved posts from the hot feed" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-diamonded-posts" summary="Get Diamonded Posts" %}
{% swagger-description %}
Get all posts on which sender sent diamonds to the receiver. Posts are sorted by the number of diamonds given from the sender to the receiver and then by timestamp.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1466).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonded Posts](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1187)\
&#x20; \- Use GetDiamondedPosts to [get posts in which a specific user received diamonds from another specific user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamond-posts-page/diamond-posts/diamond-posts.component.ts#L56). Example on [node.deso.org](https://node.deso.org/u/LazyNina/diamonds/diamondhands)
{% endswagger-description %}

{% swagger-parameter in="body" name="ReceiverPublicKeyBase58Check" type="String" %}
Public key of the user who received diamonds from sender

\




\


Only required if ReceiverUsername is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReceiverUsername" type="String" %}
Username of the user who received diamonds from sender

\




\


Only required if ReceiverPublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" type="String" name="SenderPublicKeyBase58Check" %}
Public key of the user who sent diamonds to the receiver

\




\


Only required if SenderUsername is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" type="String" name="SenderUsername" %}
Username of the user who sent diamonds to the receiver

\




\


Only required if SenderPublicKeyBase58Check is not provided
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
public key of the user reading the posts
{% endswagger-parameter %}

{% swagger-parameter in="body" name="StartPostHashHex" type="String" %}
Hex of the first Post Hash to include in this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NumToFetch" type="uint64" required="true" %}
Number of posts to fetch
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MediaRequired" type="Boolean" %}
if true, only return posts that have images, videos, or embed video URLs.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the requested diamonded posts between sender and receiver" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-likes-for-post" summary="Get Likes For Post" %}
{% swagger-description %}
Get Profiles of users who liked a given post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629).

Example usages in frontend:\
&#x20; \- Make request to [Get Likes For Post](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629)\
&#x20; \- Use GetLikesForPosts to [show all users who have liked a po](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/likes-modal/likes-modal.component.ts#L40)
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to retrieve likes
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Offset" type="uint32" required="true" %}
Position at which to return this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Limit" type="uint32" required="true" %}
Number of profiles to return in this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved profiles of users who liked a given post" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-diamonds-for-post" summary="Get Diamonds For Post" %}
{% swagger-description %}
Get Profiles and number of diamonds for users who gave diamonds to a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1750).

Example usages in frontend:\
&#x20; \- Make request to [Get Diamonds For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1440)\
&#x20; \- Use GetDiamondsForPosts to [show all users who have diamonded a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamonds-modal/diamonds-modal.component.ts#L40) and how many diamonds the user gave
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to retrieve profiles and diamonds
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Offset" type="uint32" required="true" %}
Position at which to return this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Limit" type="uint32" required="true" %}
Number of profiles to return in this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved profiles and diamond levels of users who gave diamonds the given post" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-reposts-for-post" summary="Get Reposts For Post" %}
{% swagger-description %}
Get Profiles of users who reposted (without a quote) a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1885).

Example usages in frontend:\
&#x20; \- Make request to [Get Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1455)\
&#x20; \- Use GetRepostsForPosts to [show all users who have reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/reposts-modal/reposts-modal.component.ts#L40)
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to retrieve reposters
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Offset" type="uint32" required="true" %}
Position at which to return this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="Limit" type="uint32" required="true" %}
Number of profiles to return in this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved profiles of users who reposted (without a quote) the provided post" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-quote-reposts-for-post" summary="Get Quote Reposts For Post" %}
{% swagger-description %}
Get profiles of users who quote reposted a given post and the content of the quote repost

Endpoint implementation in backend.

Example usages in frontend:\
&#x20; \- Make request to [Get Quote Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1470)\
&#x20; \- Use GetQuoteRepostsForPost to [show all users who have quoted reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/quote-reposts-modal/quote-reposts-modal.component.ts#L40) and what the quote said
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to retrieve quote reposters
{% endswagger-parameter %}

{% swagger-parameter in="body" type="uint32" name="Offset" required="true" %}
Position at which to return this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" type="uint32" name="Limit" required="true" %}
Number of profiles to return in this page of results
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the profiles that quote reposted a given post and the post that is quote reposting" %}
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
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
