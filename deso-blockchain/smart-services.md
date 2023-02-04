# 6⃣ Smart Services

When Brian Armstrong started Coinbase, he made a very contrarian decision:

He bet that a centralized approach to building a crypto exchange would result in a better user experience in the short-run, and therefore outcompete more decentralized approaches.

Today it seems obvious since you couldn't even build a decentralized exchange back then, but at the time it was heresy.

The decision to build a centralized crypto product was so contentious that Brian [broke up with his first co-founder](http://wired.com/2014/03/what-is-bitcoin/) over it. In the long run, practicality tends to triumph, but in the short run, it can be difficult to put aside ideology in favor of it.

We argue for a similarly-heretical viewpoint: **We believe that most of the computation that smart contracts are built for today will happen off-chain in centralized but composable smart services, as we will describe them.**

Blockchains will still be useful for storing **assets** and **content**, but we believe **computation** will move almost entirely off-chain.

This is because smart services will allow Web2 developers to build without having to learn any new programming languages, will allow native inter-chain communication and composability and will scale much better than smart contracts.

These benefits come at the expense of some centralization and trust compared to smart contracts, but we believe developers, and the market more broadly, will strongly prefer smart services to smart contracts in spite of this.

### Smart Contracts are Hard[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#smart-contracts-are-hard) <a href="#smart-contracts-are-hard" id="smart-contracts-are-hard"></a>

Today, if you want to write Web3 applications, most people in the space will tell you to write smart contracts.

To write them, you won't just have to learn a new programming language, like Solidity or Rust, but you'll have to adapt to a whole new "event-driven" programming paradigm that is rife with gotchas.

After you've learned how to write code and deal with all the gotchas, you still have to minimize storage because smart contract chains are not equipped with robust, cost-effective storage and indexing capabilities.

And even then, once you have a working smart contract, it's limited to a single blockchain. An Ethereum smart contract can't directly call an Avalanche or Solana smart contract, and vice versa.

Smart contracts are really hard for most developers, and we think they've made the barrier to entry for getting into Web3 much higher than it needs to be.

What's more, the lack of interoperability and composability across chains has further hindered what developers can build.

_We think there's a better way._

Specifically, a way of achieving the same functionality that smart contracts give you, but using solely Web2 APIs and Javascript/Python, which millions of Web2 developers are already familiar with.

Can you imagine how much more building there would be if you could build apps with tools already widely-understood by millions of Web2 developers?

What's more, what if your code could seamlessly tap into all blockchains at once, even blockchains that don't natively support smart contracts, like Bitcoin?

We call this new paradigm **smart services**, and we believe they will come to power the vast majority of Web3 applications, effectively replacing smart contracts as the dominant way for developers to build.

Why? Because, as we will discuss, smart services do a much better job of maximizing developer accessibility, interoperability, composability, and scalability than smart contracts do.

### On-Chain vs Off-Chain: The Epic Debate[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#on-chain-vs-off-chain-the-epic-debate) <a href="#on-chain-vs-off-chain-the-epic-debate" id="on-chain-vs-off-chain-the-epic-debate"></a>

Before going into the details behind smart services, it's important to note that, at a high level, smart services trade off decentralization and censorship-resistance for enhanced **developer accessibility**, **cross-chain interoperability**, **composability**, and **scalability**.

This really hits on a more existential question for the blockchain space, which is:

When is it useful to use a blockchain vs doing things on a centralized Web2 server?

As time wears on, more and more of crypto is starting to move off-chain.

Even with NFTs, which are a recently-popular blockchain-based product, the image/video content is nearly always stored off-chain, and auctions are generally all done off-chain as well.

But where will this trend lead us? For example, will Instagram one day host NFTs in a totally-centralized way, the same way they host images and videos today? We don't think so — not quite at least.

Generally, as the future of Web3 unfolds, we think that **assets** and **content** will be stored on-chain, while **computation** will move off-chain, effectively rendering smart contracts far less useful than they are today.

For example, we believe your tokens, your NFTs, and your social graph will benefit significantly from being on-chain. But computation-based services that allow you to lend, trade, stake, etc. will move off-chain into smart services.

In light of this, there are a few reasons we believe blockchains will continue to be useful:

* **Censorship-resistance.**
  * When assets and content are stored on a blockchain, no one company or app can freeze someone's assets or censor their voice. This seems to imply that users will prefer to have important assets like their tokens ultimately stored on a blockchain, even if they use smart services for computations like swaps, crowdsales, or loans.
* **Portability.**
  * Although smart services can interoperate via Web2 APIs, putting your assets on a blockchain virtually guarantees that they will be accessible to you and to third-party developers forever. It also means that a token or post on one smart service will show up in all other smart services in much the same way assets like Bitcoin can move between cryptocurrency exchanges today.
* **Overcoming regulatory constraints.**
  * By eliminating reliance on a centralized party entirely, blockchains can sometimes satisfy regulatory requirements that centralized services cannot. For example, issuing tokens or NFTs on a blockchain can provide extra protection against securities law concerns.

### Web2 Developers[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#web2-developers) <a href="#web2-developers" id="web2-developers"></a>

The above being said, although the benefits of storing things on-chain may seem strong, we believe that the ease of development that smart services provide to Web2 developers makes it much harder to justify using a blockchain in general for anything other than the most important pieces of state that the users need to store.

Why? Well, think about it: If smart services only require the developer to know Javascript and Web2 APIs, then we're tapping into a pool of millions of developers who can't even contribute to web3 yet because they don't know Solidity or Rust.

Given the option of writing a smart service for the first time, we believe the vast majority of these developers will choose to compromise on the blockchain advantages listed above when it comes to the computation piece of their app in favor of moving fast and shipping their product, just like Brian Armstrong did with Coinbase so many years ago.

Moreover, who would you rather bet on to find Web3's killer app:

An army of millions of Web2 developers writing unconstrained javascript — or thousands of Solidity/Rust developers operating under gas optimization constraints?

Given the above, one might reasonably ask: What will smart contracts remain useful for if a lot of computation moves into off-chain smart services?

Generally, we think smart contracts will be useful in defining new standards for assets and content, such as ERC-20 (tokens) or ERC-721 (NFTs).

If you want to do something with an Ethereum token, for example, you will still hit an ERC-20 smart contract on the Ethereum blockchain to move assets around, even if the fancier computations are happening in your smart service.

However, as time wears on, it is important to note that smart contract blockchains will start to compete with custom-built blockchains like DeSo that outperform smart contract implementations on efficiency.

Thus, while smart contracts will likely remain useful vehicles for discovering new blockchain-based products, we think there is a significant risk that apps in need of high throughput will eventually shift their assets and content to blockchains that customize themselves to the scaling needs at hand.

### What is a Smart Service?[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#what-is-a-smart-service) <a href="#what-is-a-smart-service" id="what-is-a-smart-service"></a>

Let's break down our understanding of a smart service.

![Smart Services](https://uploads-ssl.webflow.com/6148aea00f7f907469e373ad/61d5bfa51ac5554fd45895d6\_Deso%20Mocks-05.jpeg)

Smart service might seem like a fancy term, but we're really just referring to a simple centralized web service that conforms to a certain set of basic constraints.

These constraints, as we will discuss, allow for discoverability and interoperability between smart services regardless of which underlying blockchains they're tapping into. We list these constraints below.

Any web service that satisfies the bullets below would count as a smart service, regardless of whether it's written as a NodeJS server or using a popular web framework like Django:

* A smart service has a traditional Web2 domain name that other services can use to call it, `e.g. mysmartservice.com`
* A smart service has a REST API implementing, at minimum, the following critical endpoints:

#### `/get-address`[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#get-address) <a href="#get-address" id="get-address"></a>

*   Every smart service has at least one on-chain wallet that is identified by an address or public key. This allows the smart service to accept funds from users and to do arbitrary things with them the same way an on-chain smart contract would.\


    The `/get-address` endpoint is defined by all smart services, and it simply returns the address that users can send funds to in order to interact with the smart service.

    Put another way, the `/get-address` endpoint makes smart services discoverable to one another.\


    Importantly, smart services are not bound to a particular blockchain. If a smart service wants, it can return multiple addresses, each one corresponding to a different chain.\


    Users can then send ETH to the Ethereum address or DESO to the DeSo address with the assumption that the smart service will do the right thing in each case. Not all smart services will do this, but they have the option to do so.\


    Deposits into the smart service's address can include a generic key-value metadata map that will be utilized by the `trigger()` function described below.

#### `/get-info`[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#get-info) <a href="#get-info" id="get-info"></a>

*   Returns a key-value map with important information about the smart service. This can also include a simple description of how the smart service works and what it's supposed to do.

    Smart services can implement other functions via additional REST API endpoints.\


    Standards can then be defined to introduce interoperability around smart services that implement well-known functions (note that the value of smart services is more about ease of development than it is about standardization of APIs).\


    Deposits into the smart service's address can include a generic key-value metadata map that will be utilized by the `trigger()` function described below.

#### `trigger()`[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#trigger) <a href="#trigger" id="trigger"></a>

*   A smart service defines a `trigger(metadata)` function that can be written in any language that is automatically called every time a deposit is made to one of the smart service's addresses, as returned by `/get-address`.\


    The `trigger()` function gets a metadata argument associated with the deposit that includes the raw transaction, the deposit address, and the amount deposited.\


    The `trigger()` function is the magic of the smart service framework. Normally, a developer would have to scan the blockchain for transactions they're interested in, which is challenging, time-consuming, and inefficient.\


    With a smart service, everything is set up so that the `trigger()` function is called whenever a relevant transaction is detected, so all the developer has to do is fill it in.\


    This function is not technically required, but it's essential to how most smart services will function. Namely: someone deposits money, then the smart service does something.\


    It also causes smart services to be directly analogous to smart contracts in terms of how they function, making them familiar to existing smart contract programmers.\


    Again, the `trigger()` function could also operate in a totally cross-chain fashion, e.g. enabling DESO to move between wallets when ETH is sent to the smart service's ETH address.\


    As the smart services framework matures, developers can start substituting a generic `trigger()` function for event handlers such as `onETHDeposit()` or `onNFTTransaction()` that make the code even easier to write and reason about.

### Examples[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#examples) <a href="#examples" id="examples"></a>

The best way to understand the smart service framework is to walk through a couple of simple examples.

#### Example #1: Token Swap Smart Service[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#example-1-token-swap-smart-service) <a href="#example-1-token-swap-smart-service" id="example-1-token-swap-smart-service"></a>

Imagine you want to implement a smart service that allows someone to deposit ETH and immediately have that ETH exchanged for DESO, and vice versa.

Below is what this smart service would look like:

* The service would run at a domain name like `desoethswapper.com`
* `/get-address` would be implemented, and would return the smart service's ETH and DESO addresses.
* `/get-info` would return the instructions for using the smart service via a key-value map. This map can also include important info like the current exchange rate the smart service is offering between DESO and ETH, any fees it's charging, etc.
  * **description**: Call `/get-address` and send DESO to the DESO address to swap to ETH, send ETH to the ETH address to swap to DESO.
    * Include the destination address in a key named `destination`. The exchange rate is specified in the `/get-info` call and indicates the amount of DESO you will receive per ETH."
  * **exchange\_rate**: 5.0
  * **smart\_service\_type**: "swapper"
    * This field allows other smart services to make assumptions about the behavior of the smart service. More on this later.
    * The `trigger(metadata)` function would be defined roughly as follows in any language, including Javascript or Python:

```
if depositAddress == DESODepositAddress:
  EthAmountToSend = metadata['amount'] / exchange_rate
  EthDestinationAddress = metadata['destination']
  Send EthAmountToSend to EthDestinationAddress

else if depositAddress == ETHDepositAddress:
  DESOAmountToSend = metadata['amount'] * exchange_rate
  DESODestinationAddress = metadata['destination']
  Send DESOAmountToSend to DESODestinationAddress
```

Once a smart service like this is defined, anyone can call it to swap ETH for DESO and vice versa.

All you need is the domain name, `desoethswapper.com`, and then the `/get-info` call tells you how to use the service.

Once you know how to use the service, you simply send funds with the appropriate metadata to one of the smart service's blockchain addresses, which you can get from the `/get-address` endpoint.

What's more, the behavior of a swapper smart service can be standardized to the point where any smart service that identifies itself as a swapper in its `/get-info` call can be composed with any other smart service.

For example, anyone can spin up a Uniswap-like aggregator smart service that combines all the swapper smart services into a clean UI.

Then, if you want to swap currency A for currency B, the Uniswap-like smart service can route your trades through whatever smart services clear your order the most efficiently.

#### Example #2: Crowdsale Smart Service[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#example-2-crowdsale-smart-service) <a href="#example-2-crowdsale-smart-service" id="example-2-crowdsale-smart-service"></a>

Suppose you have an ERC-20 token that you've created, let's call it `$MYDAO`, that you want to sell in some clever fashion for DESO.

That is, you want people to deposit DESO into your smart service and get $MYDAO coin out of it.

What would such a smart service look like?

* First, it would have a Web2 domain name. Let's say it's `mydaocrowdsale.com`
* `/get-address` would simply return the DESO address of the smart service, since this smart service only accepts DESO.
* `/get-info` would describe the terms of the crowdsale, and tell users what metadata needs to be included.
  * **description**: Call `/get-address` and send DESO to the address returned in order to purchase `$MYDAO` coins.
    * Include your `$MYDAO` address in the metadata of your deposit with the `destination` key. The price doubles for every million `$MYDAO` coins sold.
    * The current price of `$MYDAO` as denominated in DESO is returned in the exchange\_rate field: `exchange_rate: 10.0`
  * **smart\_service\_type**: "crowdsaler"
* `trigger(metadata)` would be called on each deposit, and would fulfill the user's purchase.

Its logic would look as follows:

{% code overflow="wrap" %}
```
// Track ToenAmountSold as a global variable outside of the trigger() function so
// so that it can be referenced and incremented as the sale progresses.
// ---
// The exchange rate will be computed based on the amount of the token that has been // sold.

TokenAmountToSend = computeTokenAmountToSend(metadata['amount'], TokenAmountSold)
TokenDestinationAddress = metadata['destination']
Send TokenAmountToSend to TokenDestinationAddress

// Update the amount sold, which will increase the exchange rate for subsequent
// purchases.

TokenAMountSold += TokenAmountToSend
```
{% endcode %}

Clearly, arbitrarily complex crowdsales can be implemented by simply modifying the `trigger()` function.

Moreover, these crowdsales can clearly span multiple blockchains, meaning that someone could raise money for their project using ETH, even if what they're selling is a DESO token, and vice versa.

#### Example #3: ERC-20 Smart Service[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#example-3-erc-20-smart-service) <a href="#example-3-erc-20-smart-service" id="example-3-erc-20-smart-service"></a>

As discussed, because smart services are more centralized than smart contracts, we think they will initially be preferred for computation rather than assets and content.

This means that an ERC-20 token may still be better off on a blockchain like Ethereum or DeSo (via DeSo Tokens), since that would make the balances of its holders portable and censorship-resistant.

The above being said, just to highlight the power of smart services, we think it is important to note that even an ERC-20-like standard could be defined entirely as a smart service that implements standard token transfer functions as REST API endpoints (`/total-supply`, `/balance-of`, `/allowance`, `/transfer`, `/approve`, and `/transfer-from`).

If your smart service implemented this set of endpoints, your smart service could then integrate seamlessly into a Uniswap-like "aggregator" smart service to allow trading of your smart service's token.

You could then plug the Web2 domain name of your smart service into the Uniswap smart service, and the Uniswap smart service would know how to call the standardized REST API endpoints of your smart service in order to enable trading.

Again, the Uniswap smart service would not be bound to a particular blockchain. It could theoretically allow trading across a diverse array of assets across chains in a totally agnostic way.

Moreover, an ERC-20 that is defined entirely as a smart service would not be subject to high gas fees that ETH-based ERC-20 tokens are subject to.

### Deploying Smart Services[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#deploying-smart-services) <a href="#deploying-smart-services" id="deploying-smart-services"></a>

Because smart services can be written and deployed in any language, there is technically no required way to deploy one. You can deploy a web service however you want, and as long as its API is accessible on the internet, other smart services will be able to discover and interoperate with it.

The above being said, we are working on a "one-click deploy" functionality that will allow one to fill in simple Javascript functions, and then have that code instantly deployed to a platform like Firebase.

In the long-term, one can imagine that the Javascript code itself gets automatically deployed to a hosting platform, abstracting away deployment and making the code fully descriptive of the smart service's functionality.

Such a platform can enforce that code that is deployed matches what is actually running in the smart service, yielding even stronger guarantees than smart contracts today.

Competition among smart service hosting providers should be highly-competitive and thus fairly decentralized in the long run in much the same way Web2 hosting is today.

### A Note on Layer-2 Solutions[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#a-note-on-layer-2-solutions) <a href="#a-note-on-layer-2-solutions" id="a-note-on-layer-2-solutions"></a>

Ethereum layer-2 solutions like Arbitrum or ZK-Rollups achieve similar scalability improvements as smart services and improve on trustlessness, but they do so at the expense of being much more difficult to build apps with than smart services.

For example, you cannot write a customized ZK-Rollups app using solely javascript and Web2 APIs.

As such, layer-2 solutions do not engage millions of existing Web2 developers the way that smart services do, which is where we believe most of the value of going off-chain will come from.

### Conclusion[​](https://deso-docs.vercel.app/docs/blockchain/smart-services#conclusion) <a href="#conclusion" id="conclusion"></a>

Smart services outperform smart contracts on **developer accessibility**, **interoperability**, **composability**, and **scalability** at the expense of centralization.

We believe that this makes smart services much more appealing to developers, which makes it highly likely that the killer apps will start to be built on smart services going forward, rather than smart contracts.

Once this starts to happen, we believe blockchains will be used predominantly to store assets and content with computation moving off-chain into smart services.
