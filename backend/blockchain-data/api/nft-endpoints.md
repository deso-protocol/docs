# NFT Endpoints

## Get NFTs For User

```
POST /api/v0/get-nfts-for-user
```

Get NFTs that a user owns, optionally filtering on for-sale status and pending (NFT transferred) status.&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L811).

Example usages in frontend:

* Make request to [Get NFTs For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L980)
* Use GetNFTsForUser to [get NFTs to display in a gallery on a user's profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-nfts/creator-profile-nfts.component.ts#L126)&#x20;

#### **Parameters**

| Name                       | Type   | Description                                                                                                                                                                                                                                                                                                                   | Required | Restrictions |
| -------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| UserPublicKeyBase58Check   | string | Public key for user who owns NFTs                                                                                                                                                                                                                                                                                             | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                                                                                                                                                                                                                                                                                      | n        |              |
| IsForSale                  | bool   | <ul><li>If true, only return NFTs that are for sale. </li><li>If false, only return NFTs that are not for sale.</li><li>If not provided, return NFTs regardless of for sale status</li></ul>                                                                                                                                  | n        |              |
| IsPending                  | bool   | <ul><li>If IsForSale is provided, this value is ignored.</li><li>Otherwise, if true, only return NFTs that are pending acceptance (NFTs that have been transferred but not accepted).</li><li>If false, only return NFTs that are not pending acceptance. If not provided, return NFTs regardless of pending status</li></ul> | n        |              |

**Response**

```json5
{
  "NFTsMap": { // Map of Post Hash Hex to an object containing a PostEntryResponse AND an NFTEntryResponse.
    "4bd205ec36bb13e76620b48ffc601340562c3ad7fa61b5343f0d9edc6ff6e2f8": {
      "PostEntryResponse": <PostEntryesponse>, // PostEntryResponse of the post that is an NFT owned by UserPublicKeyBase58Check
      "NFTEntryResponses": [<NFTEntryResponse>, <NFTEntryResponse>]// NFTEntryResponses describe the serial numbers of this NFT. There may be multiple for a given post if a user owns multiple NFTs
    },
  }
}
```

## Get NFT Bids For User

```
POST /api/v0/get-nft-bids-for-user
```

Get active bids for a user.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L927).

Example usages in frontend:

* Make request to [Get NFT Bids For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L993)
* Use GetNFTBidsForUser to [show a user their outstanding bids when they view their own profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-nfts/creator-profile-nfts.component.ts#L98)

**Parameters**

| Name                       | Type   | Description                                    | Required | Restrictions |
| -------------------------- | ------ | ---------------------------------------------- | -------- | ------------ |
| UserPublicKeyBase58Check   | string | Public key for user whose bids we want to find | y        |              |
| ReaderPublicKeyBase58Check | string | Public key of the reader                       | n        |              |

**Response**

```json5
{
  "NFTBidEntries": [
    {
      "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of the user who submitted this bid
      "PostHashHex": "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290", // Hex of Post hash of the post that is an NFT.
      "SerialNumber": 85, // Serial number on which this bid was submitted.
      "BidAmountNanos": 1000000000, // Amount of DeSo bid (in nanos)
      "HighestBidAmountNanos": 1000000000, // Highest bid amount currently on this serial number
      "LowestBidAmountNanos": 0, // Lowest bid amount currently on this serial number
      "BidderBalanceNanos": 5000000000 // DeSo balance of the bidder
    }
  ],
  "PostHashHexToPostEntryResponse": {
    "43b943880ff21f67b92941553233b9d0e6bcf7c56e9a533416873d4d0798d290": <PostEntryResponse>
  },
  "PublicKeyBase58CheckToProfileEntryResponse": {
    "BC1YLgF9Tt85ADrL4QiyX6xSGwGrqFWnLr9b5Mt8hfdk54Tg1JJzi9e": <ProfileEntryResponse>
  }
}
```

## Get NFT Bids For NFT Post

```
POST /api/v0/get-nft-bids-for-nft-post
```

Get all bids for all serial numbers of a given NFT post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L1016).

Example usages in frontend:

* Make request to [Get NFT Bids For NFT Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L969)
* Use GetNFTBidsForNFTPost to [show all active bids on all serial numbers of an NFT collection](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/nft-post-page/nft-post/nft-post.component.ts#L152)

**Parameters**

| Name                       | Type   | Description                                      | Required | Restrictions |
| -------------------------- | ------ | ------------------------------------------------ | -------- | ------------ |
| ReaderPublicKeyBase58Check | string | Public key of the reader                         | n        |              |
| PostHashHex                | string | Hex of Post hash for which we want to fetch bids | y        |              |

**Response**

```json5
{
  "BidEntryResponses": [ // Array of BidEntryResponses which represents all the bids on all serial numbers of NFTs with the provided PostHashHex.
    {
      "PublicKeyBase58Check": "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public key of user who submitted bid
      "ProfileEntryResponse": <ProfileEntryResponse>,  // ProfileEntryResponse of user who submitted bid.
      "SerialNumber": 85, // Serial number on which this bid was submitted.
      "BidAmountNanos": 1000000000, // Amount of DeSo bid (in nanos).
      "BidderBalanceNanos": 5000000000 // DeSo balance of the bidder.
    }
  ],
  "NFTEntryResponses": [<NFTEntryResponse>, <NFTEntryResponse>, ...],// NFT entry responses for each serial number. There will be one for each serial number.
  "PostEntryResponse": <PostEntryResponse> // PostEntryResponse of the post that is an NFT.
}
```

## Get NFT Showcase

```
POST /api/v0/get-nft-showcase
```

Get summaries of all NFTs included in the NFT showcase.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L663).

Example usage in frontend:

* Make request to [Get NFT Showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1004)
* Use GetNFTShowcase to [fetch all the NFTs to display in the NFT showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/nft-showcase/nft-showcase.component.ts#L39)

**Parameters**

| Name                       | Type   | Description              | Required | Restrictions |
| -------------------------- | ------ | ------------------------ | -------- | ------------ |
| ReaderPublicKeyBase58Check | string | Public key of the reader | n        |              |

**Response**

```json5
{
  "NFTCollections": [<NFTCollectionResponse>, <NFTCollectionResponse>,...]
}
```

## Get Next NFT Showcase

```
POST /api/v0/get-next-nft-showcase
```

Get the time the next NFT showcase drop so it can be advertised to users

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L761).

Example usages in frontend:

* Make request to [Get Next NFT Showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1015)
* Use GetNextNFTShowcase to [show users the time at which the next NFT showcase drops](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L105)

**Parameters**&#x20;

none

**Response**

```json5
{
  "NextNFTShowcaseTstamp": 109872497124 // Time the next NFT showcase will drop
}
```

## Get NFT Collection Summary

```
POST /api/v0/get-nft-collection-summary
```

Get a summary of a single NFT post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L1098).

Example usages in frontend:

* Make request to [Get NFT Collection Summary](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1021)
* Use GetNFTCollectionSummary to [a summary of the current state of the NFT collection and each serial number](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/place-bid-modal/place-bid-modal.component.ts#L49)

**Parameters**

| Name                       | Type   | Description                                           | Required | Restrictions |
| -------------------------- | ------ | ----------------------------------------------------- | -------- | ------------ |
| ReaderPublicKeyBase58Check | string | Public key of the reader                              | n        |              |
| PostHashHex                | string | Hex of Post hash for which we want to fetch a summary | y        |              |

**Response**

```json5
{
  "NFTCollectionResponse": {
    "ProfileEntryResponse": <ProfileEntryResponse>, // ProfileEntryResponse of the creator of the NFT
    "PostEntryResponse": <PostEntryResponse>, // PostEntryResponse of the post that is an NFT
    "HighestBidAmountNanos": 2000000000, // Highest bid amount currently on any serial number of this Post
    "LowestBidAmountNanos": 0, // Lowest bid amount currently on any serial number of this Post
    "NumCopiesForSale": 1, // Number of serial numbers currently for sale of this NFT post.
    "AvailableSerialNumbers": [15] // Array of integers representing the set of all serial numbers that are for sale of this NFT post.
  },
  SerialNumberToNFTEntryResponse: { // Map of serial number to NFT Entry Response
    1: <NFTEntryResponse>,
    2: <NFTEntryResponse>,
  }
}
```

## Get NFT Entries For Post Hash

```
POST /api/v0/get-nft-entries-for-nft-post
```

Gets an NFTEntryResponse for each serial number of this NFT post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L1178).

Example usages in frontend:

* Make request to [Get NFT Entries for Post Hash](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1028)&#x20;

**Parameters**

| Name                       | Type   | Description                                                         | Required | Restrictions |
| -------------------------- | ------ | ------------------------------------------------------------------- | -------- | ------------ |
| ReaderPublicKeyBase58Check | string | Public key of the reader                                            | n        |              |
| PostHashHex                | string | Hex of Post hash for which we want to fetch all NFT Entry Responses | y        |              |

**Response**

```json5
{
  "NFTEntryResponses": [<NFTEntryResponse>, <NFTEntryResponse>, ...] // There will be one NFT Entry response for each serial number.
}
```
