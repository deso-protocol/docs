# Post Endpoints

## Get Posts Stateless

```
POST /api/v0/get-posts-stateless
```

Get Posts Stateless returns an array of posts based on the request body.  This endpoint is used to fetch posts for many different kinds of feeds.

To fetch the global feed, set `GetPostsForGlobalWhitelist` to `true`

To fetch the following feed for a user, set `GetPostsforFollowFeed` to `true`

To fetch the admin view of posts ordered by creator coin price, set `GetPostsByDeSo` to `true` and provide a value for `PostsByDESOMinutesLookback`

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L770).

Example usages in frontend:

* Make request to [Get Posts Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1066)
* Use Get Posts Stateless to get the [global feed](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L310)
* Use Get Posts Stateless to [get the following feed for a user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L391)
* Use Get Posts Stateless to [get posts ordered by time for an admin to curate the global feed](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/admin/admin.component.ts#L290)
* Use Get Posts Stateless to [get posts from user's with the high coin prices in the last hour](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/admin/admin.component.ts#L241)

**Parameters**

| Name                       | Type   | Description                                                                                                                                                                | Required | Restrictions                                          |
| -------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ----------------------------------------------------- |
| PostHashHex                | string | Start paginated look-up of posts after this post hash                                                                                                                      | n        |                                                       |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                                                                                                                                   | n        |                                                       |
| OrderBy                    | string | Order the posts by certain attributes                                                                                                                                      | n        | Must be one of "newest", "oldest", or "last\_comment" |
| StartTstampSecs            | uint64 | deprecated                                                                                                                                                                 | n        |                                                       |
| PostContent                | string | Filters out posts that do not match the text provided in PostContent (case-insensitive)                                                                                    | n        |                                                       |
| NumToFetch                 | int    | Maximum Number of posts to return                                                                                                                                          | y        |                                                       |
| FetchSubcomments           | bool   | If true, fetches comments on comments of each post. Note: not implemented for the following feed                                                                           | n        |                                                       |
| GetPostsForFollowFeed      | bool   | If true, get posts from creators that the reader follows                                                                                                                   | n        |                                                       |
| GetPostsForGlobalWhitelist | bool   | If true, get posts for the global feed - these are posts that have been manually selected for the global feed                                                              | n        |                                                       |
| GetPostsByDESO             | bool   | Get posts from the creators with the highest DESO locked within a certain timeframe                                                                                        | n        |                                                       |
| GetPostsByClout            | bool   | deprecated - use GetPostsByDESO instead                                                                                                                                    | n        |                                                       |
| MediaRequired              | bool   | If true, filter out posts that do not have images or video in them                                                                                                         | n        |                                                       |
| PostsByDESOMinutesLookback | uint64 | If GetPostsByDESO is true, get all posts within PostsByDESOMinutesLookback, order them by the creator's DESO locked, and take the top NumToFetch posts. Must be 60 or less | n        |                                                       |
| AddGlobalFeedBool          | bool   | If set to true, then the posts in the response will contain a boolean about whether they're in the global feed                                                             | n        |                                                       |

**Response**

```json5
{
  "PostsFound": [<PostEntryResponse>, <PostEntryResponse>], // Array of PostEntryResponses that were found for the provided request body.
}
```

## Get Single Post

```
POST /api/v0/get-single-post
```

Gets a single post, optionally including parents and children. This endpoint is used to display a thread view of a post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L968).

Example usages in frontend:

* Make request to [Get Single Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1099)
* Use GetSinglePost to [get a thread view](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/post-thread-page/post-thread/post-thread.component.ts#L289) of a post

**Parameters**

| Name                       | Type    | Description                                                                                                    | Required | Restrictions |
| -------------------------- | ------- | -------------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| PostHashHex                | string  | Hex of Post Hash to fetch                                                                                      | y        |              |
| FetchParents               | boolean | if true, fetch all parents of this post, up to 100 parents.                                                    | n        |              |
| CommentOffset              | uint32  | Offset at which to begin result set of comments returned                                                       | n        |              |
| CommentLimit               | uint32  | number of comments to return, starting at CommentOffset                                                        | y        |              |
| ReaderPublicKeyBase58Check | string  | public key of the user reading this single post                                                                | n        |              |
| AddGlobalFeedBool          | boolean | if set to true, then the posts in the response will contain a boolean indicating if they're in the global feed | n        |              |

**Response**

```json5
{
 "PostFound": <PostEntryResponse>, // A single PostEntryResponse that optionally contains all children and parent of itself.
}
```

## Get Posts For Public Key

```
POST /api/v0/get-posts-for-public-key
```

Get posts created by a public key or username. This endpoint is used to populate the posts on a user's profile page.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1321).

Example usages in frontend:

* Make request to [Get Posts For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1168)
* Use GetPostsForPublicKey to [get posts to display on a user's profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-posts/creator-profile-posts.component.ts#L51)

**Parameters**

| Name                       | Type    | Description                                                               | Required                                     | Restrictions |
| -------------------------- | ------- | ------------------------------------------------------------------------- | -------------------------------------------- | ------------ |
| PublicKeyBase58Check       | string  | Public key of the user whose posts we will fetch                          | Only if Username is not provided             |              |
| Username                   | string  | Username of the user whose posts we will fetch                            | Only if PublicKeyBase58Check is not provided |              |
| ReaderPublicKeyBase58Check | string  | public key of the user reading the posts                                  | n                                            |              |
| LastPostHashHex            | string  | Hex of the Post Hash that ended the previous page of results              | n                                            |              |
| NumToFetch                 | uint64  | Number of posts for fetch                                                 | y                                            |              |
| MediaRequired              | boolean | if true, only return posts that have images, videos, or embed video URLs. | n                                            |              |

**Response**

```json5
{
  "Posts": [<PostEntryResponse>, <PostEntryResponse>], // Array of PostEntryResponses representing a page of results for posts created by the provided public key.
  "LastPostHashHex": "21285bd8c0ed74125cd82a65a74606d8fcb84e309f58bb44b6cb0b76489897b5", //Hex of the last Post Hash in the array of Posts above.  
}
```

## Get Hot Feed

```
POST /api/v0/get-hot-feed
```

Get Hot Feed returns a page of Posts that are currently "hot". A post's hotness is determined by the time since the post was created and the number of likes, diamonds, comments, reposts, and quote reposts.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/hot\_feed.go#L605).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:

* Make request to [Get Hot Feed](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L1153)
* Use GetHotFeed to [get posts to display to the user in the Hot Feed tab](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/feed/feed.component.ts#L501)

**Parameters**

| Name                       | Type      | Description                                               | Required | Restrictions |
| -------------------------- | --------- | --------------------------------------------------------- | -------- | ------------ |
| ReaderPublicKeyBase58Check | string    | public key of the user reading the posts                  | n        |              |
| SeenPosts                  | string\[] | A list of posts that have already been seen by the reader | n        |              |
| ResponseLimit              | uint64    | Number of posts for fetch                                 | y        |              |

**Response**

```json5
{
  "HotFeedPage": [<PostEntryResponse>, <PostEntryResponse>,...] // Array of PostEntryResponses that represent the next page of the hot feed for the reader
}
```

## Get Diamonded Posts

```
POST /api/v0/get-diamonded-posts
```

Get all posts on which sender sent diamonds to the receiver. Posts are sorted by the number of diamonds given from the sender to the receiver and then by timestamp.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1466).

Example usages in frontend:

* Make request to [Get Diamonded Posts](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1187)
* Use GetDiamondedPosts to [get posts in which a specific user received diamonds from another specific user](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamond-posts-page/diamond-posts/diamond-posts.component.ts#L56). Example on [node.deso.org](https://node.deso.org/u/LazyNina/diamonds/diamondhands)

**Parameters**

| Name                         | Type    | Description                                                               | Required                                             | Restrictions |
| ---------------------------- | ------- | ------------------------------------------------------------------------- | ---------------------------------------------------- | ------------ |
| ReceiverPublicKeyBase58Check | string  | Public key of the user who received diamonds from sender                  | Only if ReceiverUsername is not provided             |              |
| ReceiverUsername             | string  | Username of the user who received diamonds from sender                    | Only if ReceiverPublicKeyBase58Check is not provided |              |
| SenderPublicKeyBase58Check   | string  | Public key of the user who sent diamonds to the receiver                  | Only if SenderUsername is not provided               |              |
| SenderUsername               | string  | Username of the user who sent diamonds to the receiver                    | Only if SenderPublicKeyBase58Check is not provided   |              |
| ReaderPublicKeyBase58Check   | string  | public key of the user reading the posts                                  | n                                                    |              |
| StartPostHashHex             | string  | Hex of the first Post Hash to include in this page of results             | n                                                    |              |
| NumToFetch                   | uint64  | Number of posts for fetch                                                 | y                                                    |              |
| MediaRequired                | boolean | if true, only return posts that have images, videos, or embed video URLs. | n                                                    |              |

**Response**

```json5
{
  "DiamondedPosts": [<PostEntryResponse>, <PostEntryResponse>] // Array of PostEntryResponses. Each post is a post created by the receiver AND received diamonds from the sender. The DiamondsFromSender attribute is populated in each PostEntryResponse. Posts are ordered by DiamondsFromSender and then by timestamp.
}
```

## Get Likes For Post

```
POST /api/v0/get-likes-for-post
```

Get Profiles of users who liked a given post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629).

Example usages in frontend:

* Make request to [Get Likes For Post](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1629)
* Use GetLikesForPosts to [show all users who have liked a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/likes-modal/likes-modal.component.ts#L40)

**Parameters**

| Name                       | Type   | Description                                          | Required | Restrictions |
| -------------------------- | ------ | ---------------------------------------------------- | -------- | ------------ |
| PostHashHex                | string | Hex of Post hash for which we want to retrieve likes | y        |              |
| Offset                     | uint32 | Position at which to return this page of results     | y        |              |
| Limit                      | uint32 | Number of profiles to return in this page of results | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                             | y        |              |

**Response**

```json5
{
  "Likers": [<ProfileEntryResponse>, <ProfileEntryResponse>] // ProfileEntryResponses for users who liked this post.
}
```

## Get Diamonds For Post

```
POST /api/v0/get-diamonds-for-post
```

Get Profiles and number of diamonds for users who gave diamonds to a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1750).

Example usages in frontend:

* Make request to [Get Diamonds For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1440)
* Use GetDiamondsForPosts to [show all users who have diamonded a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/diamonds-modal/diamonds-modal.component.ts#L40) and how many diamonds the user gave

**Parameters**

| Name                       | Type   | Description                                                          | Required | Restrictions |
| -------------------------- | ------ | -------------------------------------------------------------------- | -------- | ------------ |
| PostHashHex                | string | Hex of Post hash for which we want to retrieve profiles and diamonds | y        |              |
| Offset                     | uint32 | Position at which to return this page of results                     | y        |              |
| Limit                      | uint32 | Number of profiles to return in this page of results                 | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                             | y        |              |

**Response**

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

## Get Reposts For Post

```
POST /api/v0/get-reposts-for-post
```

Get Profiles of users who reposted (without a quote) a given post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L1885).

Example usages in frontend:

* Make request to [Get Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1455)
* Use GetRepostsForPosts to [show all users who have reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/reposts-modal/reposts-modal.component.ts#L40)

**Parameters**

| Name                       | Type   | Description                                              | Required | Restrictions |
| -------------------------- | ------ | -------------------------------------------------------- | -------- | ------------ |
| PostHashHex                | string | Hex of Post hash for which we want to retrieve reposters | y        |              |
| Offset                     | uint32 | Position at which to return this page of results         | y        |              |
| Limit                      | uint32 | Number of profiles to return in this page of results     | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                 | y        |              |

**Response**

```json5
{
  "Reposters": [<ProfileEntryResponse>, <ProfileEntryResponse>] // Profiles of users who reposted (without a quote) this post.
}
```

## Get Quote Reposts For Post

```
POST /api/v0/get-quote-reposts-for-post
```

Get Profiles of users who quote reposted a given post

Endpoint implementation in backend.

Example usages in frontend:

* Make request to [Get Quote Reposts For Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1470)
* Use GetQuoteRepostsForPost to [show all users who have quoted reposted a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/quote-reposts-modal/quote-reposts-modal.component.ts#L40) and what the quote said

**Parameters**

| Name                       | Type   | Description                                                    | Required | Restrictions |
| -------------------------- | ------ | -------------------------------------------------------------- | -------- | ------------ |
| PostHashHex                | string | Hex of Post hash for which we want to retrieve quote reposters | y        |              |
| Offset                     | uint32 | Position at which to return this page of results               | y        |              |
| Limit                      | uint32 | Number of profiles to return in this page of results           | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                       | y        |              |

**Response**

```json5
{
  "QuoteReposts": [<PostEntryResponse>, <PostEntryResponse>] // Post that quote reposted this quote. Each PostEntryResponse will have a ProfileEntryResponse in it.
}
```
