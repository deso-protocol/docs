---
description: >-
  Description of endpoints used to get data related to posts on the DeSo
  blockchain
---

# NFT Endpoints

Please make sure you've read [.](./ "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](./#profileentryresponse "mention")
* [#postentryresponse](./#postentryresponse "mention")
* [#balanceentryresponse](./#balanceentryresponse "mention")
* [#nftentryresponse](./#nftentryresponse "mention")
* [#nftcollectionresponse](./#nftcollectionresponse "mention")

{% swagger method="post" path="" baseUrl="/api/v0/get-nfts-for-user" summary="Get NFTs For User" %}
{% swagger-description %}
Get NFTs that a user owns, optionally filtering on for-sale status and pending (NFT transferred) status.&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L1024).

Example usages in frontend:\
&#x20; \- Make request to [Get NFTs For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L980)\
&#x20; \- Use GetNFTsForUser to [get NFTs to display in a gallery on a user's profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-nfts/creator-profile-nfts.component.ts#L126)
{% endswagger-description %}

{% swagger-parameter in="body" name="UserPublicKeyBase58Check" type="String" required="true" %}
Public key for user who owns NFTs
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsForSale" type="Boolean" %}
&#x20; \- If true, only return NFTs that are for sale.&#x20;

&#x20; \- If false, only return NFTs that are not for sale.

&#x20; \- If not provided, return NFTs regardless of for sale status
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsPending" type="Boolean" %}
&#x20; \- If IsForSale is provided, this value is ignored.

&#x20; \- Otherwise, if true, only return NFTs that are pending acceptance (NFTs that have been transferred but not accepted).

&#x20; \- If false, only return NFTs that are not pending acceptance. If not provided, return NFTs regardless of pending status
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the requested NFTs owned by the provided user" %}
{% tabs %}
{% tab title="Sample Response" %}
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

{% swagger method="post" path="" baseUrl="/api/v0/get-nft-bids-for-user" summary="Get NFT Bids For User" %}
{% swagger-description %}
Get active bids for a user.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/nft.go#L927).

Example usages in frontend:\
&#x20; \- Make request to [Get NFT Bids For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L993)\
&#x20; \- Use GetNFTBidsForUser to [show a user their outstanding bids when they view their own profile](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-nfts/creator-profile-nfts.component.ts#L98)
{% endswagger-description %}

{% swagger-parameter in="body" name="UserPublicKeyBase58Check" type="String" required="true" %}
Public key for user whose bids we want to find
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved all NFT bids for a given user" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
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

{% swagger method="post" path="" baseUrl="/api/v0/get-nft-bids-for-nft-post" summary="Get NFT Bids For NFT Post" %}
{% swagger-description %}
Get all bids for all serial numbers of a given NFT post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L1113).

Example usages in frontend:\
&#x20; \- Make request to [Get NFT Bids For NFT Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L969)\
&#x20; \- Use GetNFTBidsForNFTPost to [show all active bids on all serial numbers of an NFT collection](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/nft-post-page/nft-post/nft-post.component.ts#L152)
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to fetch bids
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved all bids on all serial numbers for the given post" %}
{% tabs %}
{% tab title="Sample Response" %}
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

{% swagger method="post" path="" baseUrl="/api/v0/get-nft-showcase" summary="Get NFT Showcase" %}
{% swagger-description %}
Get summaries of all NFTs included in the NFT showcase.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L760).

Example usage in frontend:\
&#x20; \- Make request to [Get NFT Showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1004)\
&#x20; \- Use GetNFTShowcase to [fetch all the NFTs to display in the NFT showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/nft-showcase/nft-showcase.component.ts#L39)
{% endswagger-description %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved NFT Collection Responses for all NFT posts in the current NFT showcase" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "NFTCollections": [<NFTCollectionResponse>, <NFTCollectionResponse>,...]
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name           | Type                                         | Description                                                                                                     |
| -------------- | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| NFTCollections | [NFTCollectionResponse](broken-reference)\[] | Array of [Broken link](broken-reference "mention")objects representing all the NFTs in the current NFT Showcase |
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

{% swagger method="post" path="" baseUrl="/api/v0/get-next-nft-showcase" summary="Get Next NFT Showcase" %}
{% swagger-description %}
Get the time the next NFT showcase drop so it can be advertised to users

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L858).

Example usages in frontend:\
&#x20; \- Make request to [Get Next NFT Showcase](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1015)\
&#x20; \- Use GetNextNFTShowcase to [show users the time at which the next NFT showcase drops](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed.component.ts#L105)
{% endswagger-description %}

{% swagger-response status="200: OK" description="Successfully retrieved the timestamp at which the next NFT showcase will drop" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "NextNFTShowcaseTstamp": 109872497124 // Time the next NFT showcase will drop
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name                  | Type   | Description                          |
| --------------------- | ------ | ------------------------------------ |
| NextNFTShowcaseTstamp | uint64 | Time the next NFT showcase will drop |
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

{% swagger method="post" path="" baseUrl="/api/v0/get-nft-collection-summary" summary="Get NFT Collection Summary" %}
{% swagger-description %}
Get an [#nftcollectionresponse](./#nftcollectionresponse "mention") that summarizes a single NFT post

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L1195).

Example usages in frontend:\
&#x20; \- Make request to [Get NFT Collection Summary](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1021)\
&#x20; \- Use GetNFTCollectionSummary to [a summary of the current state of the NFT collection and each serial number](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/place-bid-modal/place-bid-modal.component.ts#L49)
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" required="true" type="String" %}
Hex of Post hash for which we want to fetch a 

[#nftcollectionresponse](./#nftcollectionresponse "mention")


{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved the NFT Collection for the requested post" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "NFTCollectionResponse": {
    "ProfileEntryResponse": <ProfileEntryResponse>, // ProfileEntryResponse of the creator of the NFT
    "PostEntryResponse": <PostEntryResponse>, // PostEntryResponse of the post that is an NFT
    "HighestBidAmountNanos": 2000000000, // Highest bid amount currently on any serial number of this Post
    "LowestBidAmountNanos": 0, // Lowest bid amount currently on any serial number of this Post
    "HighestBidAmountNanos": 35000000000, // Highest buy now price currently on any serial number of this Post
    "LowestBidAmountNanos": 0, // Lowest buy now price currently on any serial number of this Post
    "NumCopiesForSale": 1, // Number of serial numbers currently for sale of this NFT post.
    "NumCopiesBuyNow": 1, // Number of serial numbers currently for sale as "Buy Now" NFTs - this means a user can purchase the NFT at the BuyNowPriceNanos without requiring an accept NFT bid transaction from the owner.
    "AvailableSerialNumbers": [15] // Array of integers representing the set of all serial numbers that are for sale of this NFT post.
  },
  SerialNumberToNFTEntryResponse: { // Map of serial number to NFT Entry Response
    1: <NFTEntryResponse>,
    2: <NFTEntryResponse>,
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

{% swagger method="post" path="" baseUrl="/api/v0/get-nft-entries-for-nft-post" summary="Get NFT Entries For Post Hash" %}
{% swagger-description %}
Gets an NFTEntryResponse for each serial number of this NFT post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/036804dc7c182305ceb8172cbb92598dcbd4d102/routes/nft.go#L1275).

Example usages in frontend:\
&#x20; \- Make request to [Get NFT Entries for Post Hash](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1028)&#x20;
{% endswagger-description %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
Hex of Post hash for which we want to fetch all 

[#nftentryresponse](./#nftentryresponse "mention")

 objects
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" %}
Public key of the reader
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved NFTEntryResponse objects for all serial numbers of the NFT post" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "NFTEntryResponses": [<NFTEntryResponse>, <NFTEntryResponse>, ...] // There will be one NFT Entry response for each serial number.
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
| Name              | Type                                         | Description                                                                                                                        |
| ----------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| NFTEntryResponses | [Broken link](broken-reference "mention")\[] | An array of [Broken link](broken-reference "mention") objects representing the current state of each serial number of the NFT post |
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
