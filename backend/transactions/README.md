---
description: Introduction to the transactions section of the API documentation
---

# Transactions

## Introduction

Transactions are the building material of every blockchain. Transactions allow users to submit data to the blockchain which allows user to perform actions such as transferring DeSo, creating posts and profiles, and minting NFTs.

Transactions have three steps in their lifecycle

1. **Construct:** The first step for a developer is to interact with the DeSo Backend API through a transaction construction endpoint to get an unsigned user transaction. [social-transactions-api.md](construct-transactions/social-transactions-api.md "mention"), [nft-transactions-api.md](construct-transactions/nft-transactions-api.md "mention"), [financial-transactions-api.md](construct-transactions/financial-transactions-api.md "mention"), and [derived-keys-transaction-api.md](construct-transactions/derived-keys-transaction-api.md "mention") explain the endpoints that will get you an unsigned transaction.
2. **Sign:** The developer will then take the output `TransactionHex` from the construct step's response, which encodes the user transaction, and signs it using the DeSo Identity. You can read about signing transactions in the[#sign](../../identity/iframe-api/endpoints.md#sign "mention") section of the [Broken link](broken-reference "mention") documentation.
3. **Broadcast:** The signed transaction will be sent through the `/api/v0/submit-transaction` by the developer so that it can be added to the blockchain ledger. The [#submit-transactions](transaction-utilities.md#submit-transactions "mention") explains how this endpoint works.

You can read more about Transactions in this section of the [Identity documentation. ](../../identity/concepts.md#transactions)

## Sections

1. The [basics](basics/ "mention") section
   * outlines the the various aspects of the response body you'll receive from all endpoints that [construct-transactions](construct-transactions/ "mention")&#x20;
   * In Basics, you'll find the [data-types.md](basics/data-types.md "mention") section will explain commonly reused data types.&#x20;
2. The [transaction-utilities.md](transaction-utilities.md "mention") section
   1. outlines important endpoints that are used to handle transactions, most importantly [#submit-a-transaction](transaction-utilities.md#submit-a-transaction "mention")
3. The [construct-transactions](construct-transactions/ "mention") section
   1. introduces transaction construction and provides a high level overview
   2. [social-transactions-api.md](construct-transactions/social-transactions-api.md "mention") describes endpoints related to constructing transactions that add social data to the DeSo blockchain. The types of transactions included are:
      * [#update-profile](construct-transactions/social-transactions-api.md#update-profile "mention")
      * [#submit-post](construct-transactions/social-transactions-api.md#submit-post "mention")
      * [#follow](construct-transactions/social-transactions-api.md#follow "mention")
      * [#send-diamonds](construct-transactions/social-transactions-api.md#send-diamonds "mention")
      * [#like](construct-transactions/social-transactions-api.md#like "mention")
      * [#send-message](construct-transactions/social-transactions-api.md#send-message "mention")
   3. [nft-transactions-api.md](construct-transactions/nft-transactions-api.md "mention") describes endpoints related to constructing transactions that interact with NFT data on the DeSo blockchain. The types of transactions included are:
      * [#create-nft](construct-transactions/nft-transactions-api.md#create-nft "mention")
      * [#update-nft](construct-transactions/nft-transactions-api.md#update-nft "mention")
      * [#create-nft-bid](construct-transactions/nft-transactions-api.md#create-nft-bid "mention")
      * [#accept-nft-bid](construct-transactions/nft-transactions-api.md#accept-nft-bid "mention")
      * [#transfer-nft](construct-transactions/nft-transactions-api.md#transfer-nft "mention")
      * [#accept-nft-transfer](construct-transactions/nft-transactions-api.md#accept-nft-transfer "mention")
      * [#burn-nft](construct-transactions/nft-transactions-api.md#burn-nft "mention")
   4. [financial-transactions-api.md](construct-transactions/financial-transactions-api.md "mention") describes endpoints related to constructing transactions that send DeSo or buy/sell/transfer Creator coins on the DeSo blockchain. The types of transactions included are:
      * [#send-deso](construct-transactions/financial-transactions-api.md#send-deso "mention")
      * [#buy-or-sell-creator-coin](construct-transactions/financial-transactions-api.md#buy-or-sell-creator-coin "mention")
      * [#transfer-creator-coin](construct-transactions/financial-transactions-api.md#transfer-creator-coin "mention")
   5. Lastly, the [derived-keys-transaction-api.md](construct-transactions/derived-keys-transaction-api.md "mention") describes a single endpoint, [#authorize-derived-key](construct-transactions/derived-keys-transaction-api.md#authorize-derived-key "mention"), which constructs a transaction to authorize or deauthorize a derived key. For more information on Derived Keys, you can go read the [#derived-keys](../../identity/mobile-integration.md#derived-keys "mention")sections under [mobile-integration.md](../../identity/mobile-integration.md "mention")

