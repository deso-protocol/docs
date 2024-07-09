---
description: Overview of the configuration flags for running your own backend
---

# 1️⃣ Backend: Config

**Core Protocol:** Trying to build the next great social app on top of the DeSo blockchain or running a node and want to know the ins-and-outs of all the endpoints?

This section is for you.

Compared to private, monopolized Web2 social media, all of the data on the DeSo blockchain is public and lives on-chain.

This means that **anybody can access this information and build their own application** using that data.&#x20;

However, blockchains require transactions and cryptography to validate information.&#x20;

We know this can be a hassle, so we've built this API along with the [Broken link](broken-reference "mention") service to make it easy for you, the developer, to focus on what matters: building your application.&#x20;

There is no need to define and maintain your own database schema and write logic to extract data from the chain to get started building and writing and reading new data on-chain.&#x20;

To write data to the blockchain, all you need is the Backend API, which you can use to construct transactions with the[construct-transactions](../construct-transactions/ "mention") and then submit them with [#submit-a-transaction](../transaction-utilities.md#submit-a-transaction "mention") endpoint, and Identity, which you can use to [#sign](../../deso-identity/iframe-api/endpoints.md#sign "mention") transactions.

To read data, you can use our [api](../api/ "mention") endpoints to retrieve everything you need.

As a developer, all you need to do is implement the frontend (or modify the reference implementation) and any custom logic around the data you receive from the backend endpoints.

If you're running your own node and using the reference backend implementation, there are a wide variety of flags that are available to you to manage behavior and functionality on your node.

Each section below describes a set of flags related to certain functionality.

