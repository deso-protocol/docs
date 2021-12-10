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
   1. outlines

