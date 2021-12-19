---
description: Description of common data types that appear in the Blockchain Data endpoints
---

# Data Types

## ProfileEntryResponse

Every public key on the DeSo blockchain can have a profile that describes who they are. The API returns profiles in the form of an object referred to as a ProfileEntryResponse. Below is an example of a ProfileEntryResponse with explanations about each attributes.

```json5
{
  "PublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public key
  "Username": "test", // Username of the public key
  "Description": "this is an example description", // Description displayed on the user's profile
  "IsHidden": false, // Deprecated - ignore.
  "IsReserved": false, // If IsReserved is true, this is a reserved prole that can be claimed by tweeting out a link.
  "IsVerified": false, // If IsVerified is true, this profile is verified on the node on which you are querying and will appear with a blue checkmark
  "Comments": null, // Comments that have been made by this user. This is rarely populated.
  "Posts": null, // Posts that have been made by this user.
  "CoinEntry": {
    "CreatorBasisPoints": 10000, // Founder reward percentage in basis points. e.g. 10%. User will take 10% of all creator coin purchases.
    "DeSoLockedNanos": 110694117, // Total amount of DeSo locked in this profile's creator coin. When ranking by coin price, always use DeSoLockedNanos and not CoinPriceDeSoNanos.
    "NumberOfHolders": 2, // Number of users who hold this creator coin.
    "CoinsInCirculationNanos": 2203171427, // Total number of creator coin nanos in circulation.
    "CoinWatermarkNanos": 2203171427, // CoinWatermarkNanos is the highest amount of nanos that have ever been locked in this profile at a single point in time.
    "DeSoLockedNanos": 110694117 // Deprecated - use DeSoLockedNanos
  },
  "CoinPriceDeSoNanos": 150729253, // CoinPriceDeSoNanos is the price of this creator's coin in nanos.
  "CoinPriceDeSoNanos": 150729253, // Deprecated - use CoinPriceDeSoNanos
  "UsersThatHODL": [{ // Array of all hodlers of this creator's coin in the form of BalanceEntryResponses (described below). This is not always included.
    "HODLerPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // Public key of user who is holding this creator's coin.
    "CreatorPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public Key of the creator
    "HasPurchased": false, // If true, this user has purchased some amount of creator coins of this creator. If false, they have received these creator coins in a transfer.
    "BalanceNanos": 1000000000, // How many nanos of this creator's coin does the HODLer own.
    "NetBalanceInMempool": 0, // How many nanos of this creator's coin does this HODLer own that are still waiting to be mined into a block.
    "ProfileEntryResponse": <ProfileEntryResponse>, // ProfileEntryResponse of the user that is HODLing 
  }],  
  "IsFeaturedTutorialWellKnownCreator": false,
  "IsFeaturedTutorialUpAndComingCreator": false
}
```

Objects of this type will be denoted as `<ProfileEntryResponse>` in this documentation.

For reference, `ProfileEntryResponse` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L560).

## PostEntryResponse

Posts are the main way creators communicate with the public on DeSo. Below is an example of a `PostEntryResponse` - the object that represents a post and all it's attributes.

```json5
{
  "PostHashHex": "67f80ea6908b93cca921a2a49ef268ad373756b5ba45aff4e06bf7a31f7f20c0", // Hex of the Post Hash. Used as the unique identifier of this post.
  "PosterPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public key of the user who made this post.
  "ParentStakeID": "", // Hex of the Parent Post Hash. If populated, this post is a comment on the parent.
  "Body": "testesteart", // Text body of the post.
  "ImageURLs": ["https://images.deso.org/86c5d55150042af2f56b5ed718f5194a42cb072a9f26f5aee9e3afdc7e609c48.gif"], // URLs to images to include in the post
  "VideoURLs": [], // URLs to videos to include in the post
  "RepostedPostEntryResponse": <PostEntryResponse>, // RepostedPostEntryResponse is another post that this post is reposting (similar to retweeting). 
  "CreatorBasisPoints": 1000, // Deprecated
  "StakeMultipleBasisPoints": 12500, // Deprecated
  "TimestampNanos": 1637776136858394400, // Timestamp of the post
  "IsHidden": false, // If true, post is filtered out everywhere.
  "ConfirmationBlockHeight": 180, // Block height at which this post was confirmed.
  "InMempool": false, // If true, this post is still in the mempool and has not been confirmed in a block yet.
  "ProfileEntryResponse": <ProfileEntryResponse>, // This is the profile of the user who created this post.
  "Comments": [<PostEntryResponse>, <PostEntryResponse>], // Array of comments. These PostEntryResponses reference this post as their parent.
  "LikeCount": 123, // Number of likes on this post.
  "DiamondCount": 1092, // Number of diamonds on this post.
  "PostEntryReaderState": {
    "LikedByReader": false, // True if the reader has liked this post, otherwise false.
    "DiamondLevelBestowed": 2, // Number of diamonds the reader has given this post. 
    "RepostedByReader": false, // True if the reader has reposted this post, otherwise false.
    "RepostPostHashHex": "" // Hex of the Post Hash in which the user has reposted this post.
  },
  "InGlobalFeed": false, // If true, this post is included in the global feed.
  "InHotFeed": false, // If true, this post is in the hot feed.
  "IsPinned": false, // If true, this post is pinned to the top of the feeds.
  "PostExtraData": { // PostExtraData can contain any keys and string values to add metadata to a post.
    "Node": "1",
    "EmbedVideoURL": "https://www.youtube.com/watch?v=X-pqNzHyZbM"
  },
  "CommentCount": 2, // Number of comments on this post.
  "RepostCount": 78, // Number of times this post has been reposted.
  "QuoteRepostCount": 10, // Number of times this post has been quote reposted.
  "ParentPosts": [<PostEntryResponse>, <PostEntryResponse>], // Array of PostEntryResponses that represent the parents of this post.
  "IsNFT": true, // If true, this post has been minted as an NFT. False otherwise.
  "NumNFTCopies": 100, // Number of serial numbers that were minted. 
  "NumNFTCopiesForSale": 0, // Number of serial numbers that are currently for sale.
  "NumNFTCopiesBurned": 0, // Number of serial numbers that have been burned.
  "HasUnlockable": false, // If true, when this post is sold as an NFT, the owner will be required to provide some unlockable content.
  "NFTRoyaltyToCreatorBasisPoints": 500, // Percentage in basis points of the royalty that goes to this post's creator when this NFT is sold.
  "NFTRoyaltyToCoinBasisPoints": 1000, // Percentage in basis points of the royalty that is added to the DeSo locked in this post's creator's coin when this NFT is sold.
  "DiamondsFromSender": 0, // Number of diamonds this post received from a sender. Only populated in get-diamonded-posts
  "HotnessScore": 0, // Hotness score is a measure of how engaging a post is. Posts with the highest hotness scores are featured in the Hot Feed.
  "PostMultiplier": 0, // Multiplier applied to this post in the hot feed algorithm.
}
```

Objects of this type will be denoted as `<PostEntryResponse>` in this documentation.

For reference, `PostEntryResponse` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/post.go#L56).

## BalanceEntryResponse

`BalanceEntryResponses` are another common object you will encounter. `BalanceEntryResponses` describe the amount of a specific creator coin that a user holds.

```json5
{
  "HODLerPublicKeyBase58Check": "tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // Public key of user who is holding this creator's coin.
  "CreatorPublicKeyBase58Check": "tBCKW665XZnvVZcCfcEmyeecSZGKAdaxwV2SH9UFab6PpSRikg4EJ2", // Public Key of the creator
  "HasPurchased": false, // If true, this user has purchased some amount of creator coins of this creator. If false, they have received these creator coins in a transfer.
  "BalanceNanos": 1000000000, // How many nanos of this creator's coin does the HODLer own.
  "NetBalanceInMempool": 0, // How many nanos of this creator's coin does this HODLer own that are still waiting to be mined into a block.
  "ProfileEntryResponse": <ProfileEntryResponse>, // ProfileEntryResponse of the HODLer or creator depending upon the context in which the BalanceEntryResponse was retrieved
}
```

Objects of this type will be denoted as `<BalanceEntryResponse>` in this documentation.

For reference, `BalanceEntryResponse` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/shared.go#L143).

## NFTEntryResponse

`NFTEntryResponses` summarize the current state of a single serial number (copy) of an NFT.

```json5
{
  "OwnerPublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user who owns this serial number
  "SerialNumber": 2, // serial number described by this NFTEntryResponse
  "IsForSale": true, // If true, this serial number is for sale. If false, this serial number is not currently for sale.
  "IsPending": false, // If true, this serial number was transferred to the owner and is pending an acceptance of the NFT transfer. If false, this serial number is not pending an acceptance.
  "MinBidAmountNanos": 0, // Minimum bid amount in nanos allowed on this serial number.
  "LastAcceptedBidAmountNanos": 10000000, // Bid amount in nanos representing the last price at which this serial number was sold.
  "HighestBidAmountNanos": 1150000000, // Highest bid amount currently on this serial number.
  "LowestBidAmountNanos": 97680, // Lowest bid amount currently on this serial number.
  "LastOwnerPublicKeyBase58Check": "BC1YLhkVFp84xfJZqN6jCBsdo6bPyuBoxakChg8DJmmvy2jMhZgBaWK", // Public key of the user who last owned this serial number. This is needed to decrypt Unlockable text.
  "EncryptedUnlockableText": "someencryptedtext" // Unlockable content Text encrypted with a shared secret
}
```

Objects of this type will be denoted as `<NFTEntryResponse>` in this documentation.

For reference, `NFTEntryResponse` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L15).

## NFTCollectionResponse

`NFTCollectionResponses` provide a high-level overview of the current state of an NFT and all its serial numbers.

```json5
{
  "ProfileEntryResponse": <ProfileEntryResponse>, // ProfileEntryResponse of the creator of the NFT
  "PostEntryResponse": <PostEntryResponse>, // PostEntryResponse of the post that is an NFT
  "HighestBidAmountNanos": 2000000000, // Highest bid amount currently on any serial number of this Post
  "LowestBidAmountNanos": 0, // Lowest bid amount currently on any serial number of this Post
  "NumCopiesForSale": 1, // Number of serial numbers currently for sale of this NFT post.
  "AvailableSerialNumbers": [15] // Array of integers representing the set of all serial numbers that are for sale of this NFT post.
}
```

Objects of this type will be denoted as `<NFTCollectionResponse>` in this documentation.

For reference, `NFTCollectionResponse` is defined in the backend repo [here](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L32).
