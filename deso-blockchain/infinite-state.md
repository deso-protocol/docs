# 4⃣ Infinite-State

Many believe that general-purpose blockchains like Ethereum, Cardano, Avalanche, and Solana will come to power everything on the web, including financial apps, social apps, and even Amazon-like marketplaces.

But there's a show-stopping problem that's being widely overlooked: **on-chain storage**.

While today's general-purpose blockchains have worked well for storage-light applications like DeFi, they cannot scale to handle storage-heavy applications like social apps and marketplaces.

Imagine a world in which every "like" or "follow" on a decentralized app cost $1.00+ in storage fees.

Unfortunately, that is the reality now because of the storage limitations of all general-purpose blockchains on the market today.

### Storage Costs[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#storage-costs) <a href="#storage-costs" id="storage-costs"></a>

The numbers don't lie. The simple table below illustrates how the cost of storing just 1 gigabyte of on-chain state varies across blockchains. Importantly, these costs are only expected to increase for general-purpose blockchains, as they become more popular and storage becomes more scarce.

We will discuss DeSo as a special case later on.

<figure><img src="https://uploads-ssl.webflow.com/6148aea00f7f907469e373ad/618492a6cae2196583f44adb_DESO_blog%20graphics_FINAL_7pm.png" alt=""><figcaption><p>Pricing snapshot taken as of Nov 09, 2021. Make sure to check out the Appendix for detailed calculations.</p></figcaption></figure>

These high on-chain storage costs prevent the vast majority of web 2.0 applications from being implementable on today’s general-purpose blockchains, even if using bridges to storage-focused blockchains such as Arweave or Filecoin.

At current prices, even storing a single link to Arweave or Filecoin on any of the chains shown above would cost between $0.10 - $1.00+, which is prohibitively expensive.

And the costs are likely going to go up even more as these chains become more popular because they weren't designed to scale state storage.

Moreover, even though many blockchains claim to be able to handle thousands of transactions per second, aka TPS, this metric does not take into account the storage properties of the application at hand.

There is a big difference between 50,000 DeFi transactions, which may generate zero bytes of new state data, as opposed to 50,000 social transactions, which may generate tens of megabytes that need to be stored, indexed, and queried.

Today's most advanced blockchains fail completely at handling the latter type of transaction, and this limitation is blocking the development of some of the most interesting Web3 applications.

We've been researching this challenge for years, and we believe that all storage-heavy Web3 applications, such as social apps and marketplaces, will require new types of blockchains to develop.

That is because, as we will discuss, these applications are infinite-state applications rather than finite-state applications.

### From Finite-State to Infinite-State[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#from-finite-state-to-infinite-state) <a href="#from-finite-state-to-infinite-state" id="from-finite-state-to-infinite-state"></a>

Today, all general-purpose blockchains on the market were built to power what we call finite-state applications.

These are applications where the amount of data or state that you have to keep on hand for each user is, well, finite.

For example, in order to build a financial app, all you really need to know in order to validate transactions is each user's account balance.

Users could transfer funds between each other millions of times, but in the end, all you need to store is just a few numbers indicating the final balance of each user.

Put another way, the state you have to keep around grows as a function of the number of users rather than as a function of the number of transactions. Perhaps surprisingly, virtually all of decentralized finance, aka DeFi, consists of finite-state applications.

As long as you can store a handful of account balances, you can start to build arbitrarily-complex tools for people to trade, borrow, lend, etc... and you'll never have to store more than the ending balances in the long-term.

This is because the transactions users perform in DeFi applications are state-neutral transactions, meaning they simply modify existing balances rather than append new data to the state.

The problem is that, as blockchains look to disrupt applications beyond the financial sector, they start needing to deal with a completely different class of applications: the infinite-state applications.

**Now, what if we want to look beyond finance?**

Infinite-state applications are ones where the amount of data you need to store grows indefinitely with the number of actions that each user performs.

For example, consider a typical social app.

* Users can create a profile, which adds state...
* Users can make a post, which adds state...
* Users can follow other users, which adds state...
* Users can like posts and comments, which adds stats...

You get the picture.

The difference is that with social applications, all transactions are "_state-augmenting_" rather than "_state-neutral_", as is the case with DeFi.

With social apps, instead of just having to keep a few account balances in your state, you need to be able to store an indefinite amount of data.

Even worse, this state needs to be frequently queried by other users on the network, requiring it to be highly-available. Unfortunately, many applications we use today are like this, including most social apps and marketplaces.

What's more, as we'll discuss, none of the existing general-purpose blockchains on the market today are equipped to handle these types of applications.

### Congestion of General-Purpose Chains[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#congestion-of-general-purpose-chains) <a href="#congestion-of-general-purpose-chains" id="congestion-of-general-purpose-chains"></a>

All the general-purpose blockchains on the market today, including Ethereum, Cardano, Avalanche, Solana, and others are ill-equipped to handle infinite-state applications such as social apps and marketplaces.

This is because scaling infinite-state applications, even to a small number of users, requires solutions that are inherently tailored to the storage and indexing requirements of the app at hand.

> **Remember:** 50,000 state-neutral transactions per second — is not the same as — 50,000 state-augmenting transactions per second.

For example, most of the newest general-purpose blockchains on the market maintain high TPS by storing all account state in memory.

This doesn't just work great for finite-state applications like DeFi, but it is the optimal choice if you want to be the fastest DeFi blockchain.

However, the second someone tries to build an infinite-state app on your chain, you go from having to store a single number for each user, to having to store potentially megabytes or more, which means things suddenly don't fit in memory anymore.

Moreover, if the blockchain is general-purpose, it can't make intelligent decisions about what account state is needed in-memory vs what isn't, and it certainly can't index the data to make it query-able in real-time.

The end result is that all general-purpose blockchains today have had to impose storage limits in order to remain viable. This has resulted in skyrocketing storage fees that make it prohibitive to build infinite-state apps on them, and that will only worsen as these chains become more popular.

Nobody is trying to build infinite-state applications on a general-purpose blockchain because it's impossible to do so at a low enough cost.

But there's a whole world of interesting applications that are infinite-state. In fact, the vast majority of Web 2 applications are infinite-state (FB, Insta, Amazon, etc).

So how can Web3 take off if you can't even build the majority of Web 2 applications on today's finite-state chains?

Moreover, all it takes is for a single person to build an infinite-state application on a general-purpose blockchain before its storage becomes congested.

To use an analogy, imagine you're sharing a dorm room with three other roommates-- if a single one of them is messy, then your room gets filled with clutter.

Similarly, building a full-scale social app or a marketplace, even on one of the newer general-purpose blockchains, will immediately hit these blockchains' inherent storage limits, causing all infinite-state apps to become virtually unusable relatively quickly.

We have already seen this tragedy of the commons play out with Ethereum and it's starting to happen on Solana as well.

### Scaling Infinite-State Apps[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#scaling-infinite-state-apps) <a href="#scaling-infinite-state-apps" id="scaling-infinite-state-apps"></a>

In order to handle the storage and indexing requirements inherent to infinite-state applications, we believe blockchains will need to be built that are custom-tailored to the application at hand.

This is because, without being able to make assumptions about the type of data that will be stored, _aka the schema_, the costs of storing, indexing, and querying the data will skyrocket, making applications built on the chain uncompetitive.

To give a very concrete example, consider the Decentralized Social blockchain, aka DeSo. DeSo is custom-built from the ground up to power social applications, and that means that all of the data that it stores and indexes follows a known schema.

Profiles are stored and indexed differently than posts, which are stored and indexed differently than follows, etc...

This level of customization not only makes storage costs **10,000x cheaper** than Avalanche or Solana, but it also allows all DeSo nodes to offer instant querying of all relevant data.

Queries like figuring out who liked a post or figuring out who someone follows, which would be expensive if the data was stored in an unstructured way, are virtually instant.

This makes it much easier for developers to build apps on DeSo, and it is part of the reason why there are already over a hundred apps built on it, including Diamond, Pearl, Desofy, DeSocialWorld, Stori, and more.

The simple table below illustrates how the cost of storing just 1 gigabyte of on-chain state varies across blockchains.

Note also that, as time goes on, the storage rents of the general-purpose chains are expected to increase as storage becomes scarce. In contrast, DeSo's costs are expected to remain fixed, and potentially even decrease, since it was built to handle the infinite-state use-case.

Interestingly, even though the DeSo blockchain was designed to support social applications, it is important to note that it can be augmented to support any infinite-state application, as long as the schema is well-defined.

The key is that each new type of application is supported at the bare metal level and customized in such a way that the storage and indexing requirements of that application are optimized.

This means that, over time, as a network effect forms around the DeSo blockchain, it can be extended to support marketplace data structures and more, giving it the potential to disrupt all of Web 2.0, and not just the social media giants.

### Other Storage Blockchains[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#other-storage-blockchains) <a href="#other-storage-blockchains" id="other-storage-blockchains"></a>

It is important to mention that there are also blockchains that have focused exclusively on storage, such as Filecoin or Arweave.

Some have suggested that these blockchains can be used in combination with a general-purpose blockchain in order to alleviate the storage issues they face.

In practice, however, the storage costs of general-purpose blockchains are so high that even storing a simple link to Filecoin or Arweave for each piece of content would cost between $0.10 - $1.00+ at today's prices.

This makes it prohibitively expensive to build most infinite-state apps using these bridges, and the cost will only increase as these chains become more popular.

Additionally, data stored in Filecoin or Arweave would not be indexed appropriately, and thus a whole separate indexing layer would need to be created in order to support each app at scale. The indexing layer would then need its own incentive structure in place as it becomes more and more expensive to run at scale.

The above being said, Arweave can be useful for storing data that does not need to be indexed, aka blob storage, and the DeSo blockchain allows for images and videos to be stored on Arweave if the user desires.

The DeSo blockchain then stores a link to Arweave as opposed to storing the image on-chain or in a centralized service.

Notably, because the cost of storing links on DeSo is virtually free ($0.0000184), DeSo can integrate these systems in a way that today’s general-purpose blockchains cannot.

### Conclusion[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#conclusion) <a href="#conclusion" id="conclusion"></a>

We think that the difficulty of storing and indexing data in a scalable way is something that has been underestimated by most of the crypto space.

For a long time, the entire space has been limited to finite-state applications without much consideration for the wide range of infinite-state applications, like social apps and marketplaces, that in fact make up the majority of Web 2.0 applications.

In order for Web3 to reach its full potential to disrupt Web 2.0 and the systems of the past, we believe blockchains that are custom-built to support new use-cases will be required because of the storage and indexing limitations inherent to the existing general-purpose chains.

### Appendix[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#appendix) <a href="#appendix" id="appendix"></a>

In this section we explain the calculations of how much it costs to store 1 GB of state data on each blockchain as of publishing on Nov 09, 2021

#### DeSo[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#deso) <a href="#deso" id="deso"></a>

In DeSo’s transaction cost model, the fee is simply the transaction size in KB multiplied by MinFeeRateNanosPerKB. The current rate is 1000 Nanos per KB, which means 1 GB of storage costs 1 DeSo. As of writing, the price of DeSo is $80. Storage costs in USD are expected to remain fixed or even decrease as the blockchain scales and efficiency increases over time.

#### Cardano[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#cardano) <a href="#cardano" id="cardano"></a>

Cost is calculated using the a + b x size formula. Taking a = 0.155381 ADA, b = 0.000043946 ADA. For simplicity, we skip the costs associated with Cardano’s Plutus execution (which would further increase the price), and instead just focus on the bare minimum transaction cost.

We assume an average state-augmenting transaction appends 500 bytes of data to the state, hence 1GB of storage would cost about 354708 ADA. As of writing, the price of ADA is $1.97, which gives a total cost of $698,775.

#### Avalanche[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#avalanche) <a href="#avalanche" id="avalanche"></a>

Cost is calculated based on today’s gas price of 25 NanoAVAX and one word (32 bytes) costing 20,000 gas or 0.0005 AVAX. For simplicity, we skip the gas costs of smart contract code execution and of allocating the storage and instead only consider the bare minimum cost of SSTORE operations. This makes storing 1GB of data cost about 15625 AVAX. As of writing, the price of AVAX is $63.24, which gives a total cost of $988,125.

#### Solana[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#solana) <a href="#solana" id="solana"></a>

Cost is calculated based on today’s rent fee of 19.05 lamports per byte-epoch and epoch lasts 2 days. For 1 GB of state, this gives the biennial rent equal to 6858 SOL. As of writing, the price of SOL is $200, which gives a total cost of $1,371,600. It’s worth noting that this is only the amount required to be rent-exempt, otherwise Solana will charge a recurring fee of 19.05 SOL every 2 day epoch.

#### Ethereum[​](https://deso-docs.vercel.app/docs/blockchain/infinite-state#ethereum) <a href="#ethereum" id="ethereum"></a>

Cost is calculated based on today’s gas price of 150 Gwei and one word (32 bytes) costing 20,000 gas or 0.003 ETH. For simplicity, we skip the gas costs of smart contract code execution and of allocating the storage and instead only consider the bare minimum cost of SSTORE operations. This makes storing 1GB of data cost about 93750 ETH. As of writing, the price of ETH is $4200, which gives a total cost of $393,750,000.
