# BitClout FAQ

## **1.** Overview

### **What is BitClout?**

The BitClout project is a decentralized, blockchain-based, [open source](https://github.com/bitclout/core#about-this-repo) social network. It is not a competitor to Bitcoin or Ethereum, but to existing closed social networks like Twitter and Facebook. And to understand why we built it, it helps to understand the history of these social networks.

Back in 2009, Paul Graham wrote about [Twitter as an open protocol](http://www.paulgraham.com/twitter.html) that was run by a private company that had been “slow to monetize” and hadn’t “tried to control it too much”. Due to this openness, people built many applications on the Twitter API, including applications like [Tweetdeck](https://en.wikipedia.org/wiki/TweetDeck) that replicated the full functionality of Twitter. Then Twitter very much asserted itself as a private company, shutting off API access to countless developers \([1](https://readwrite.com/2011/02/11/twitter_kills_the_api_whitelist_what_it_means_for/), [2](https://techcrunch.com/2011/05/18/twitter-revokes-automatic-3rd-party-dm-access-gives-users-more-details-on-app-permissions/), [3](https://www.theverge.com/2012/7/9/3135406/twitter-api-open-closed-facebook-walled-garden), [4](https://www.theverge.com/2012/8/20/3250218/developers-react-twitter-api-rules), [5](https://www.networkworld.com/article/2200143/twitter-whacks-ubertwitter-company-so-hard-it-s-changing-app-s-name-to-ubersocial.html), [6](https://readwrite.com/2011/02/22/twitter_puts_the_smack_down_on_another_popular_app), [7](https://mashable.com/archive/twitter-killing-tweetdeck), [8](https://venturebeat.com/2015/10/22/10-reasons-why-twitter-ceo-jack-dorsey-apologized-to-developers/), [9](https://thenextweb.com/news/developers-bracing-themselves-for-twitter-api-retrictions-call-todays-post-ominous), [10](https://www.infoworld.com/article/2908869/twitters-firehose-shut-off-is-the-newest-hazard-of-the-api-economy.html)\), deplatforming direct competitors like [Meerkat](https://techcrunch.com/2015/05/06/meerkat-founder-on-getting-the-kill-call-from-twitter/), and generally becoming a Facebook-like [walled garden](https://www.theverge.com/2012/7/9/3135406/twitter-api-open-closed-facebook-walled-garden). This is now far enough in the past that 20-something developers often don’t even know about this history.

Twitter didn’t cut off API access because it was “evil”. Fundamentally, the reason this happened is because \(a\) Twitter was a private company that needed to provide returns to its employees/investors and \(b\) Twitter could not monetize via its API as well as it could via ads run on Twitter.com itself. The incentives of Twitter-the-company, Twitter-the-platform, Twitter developers, and Twitter users were not economically aligned. And so the background issues that Paul Graham had presciently noted — namely that Twitter was a private company that hadn’t tried to monetize or assert control over its API — came to the foreground.  
  
But what if we could build an open source social protocol, based on a blockchain, that aligned all parties behind a different form of innate monetization, and that delegated control back to its users, nodes, and developers? That’s what we’re trying to do with BitClout.

### **What is the difference between BitClout, Bitclout.com, the BitClout Blockchain, the CLOUT cryptocurrency, and individual creator coins?**

The BitClout project has several parts.

* The BitClout Blockchain, the decentralized backend of the whole system.
* Bitclout.com, the first client to that blockchain.
* The CLOUT cryptocurrency, which is the native digital asset of the platform.
* The individual creator coins, which are personal tokens that are automatically set up for each BitClout profile. Each creator can add value to their creator coins, and they are a way for users to back their favorite creators.

Let’s give detail on each of these.

#### The BitClout Blockchain

BitClout’s backend is structured as a blockchain. It is already [open source](https://github.com/bitclout/core#example-1-a-bitclout-website-aka-bitcloutcom) and more decentralized than closed social networks like Twitter. 

To prove this, note that right now you can [run a node](https://docs.bitclout.com/devs/running-a-node) and download the entire BitClout blockchain. Any engineer can then run [simple commands](https://github.com/andrewarrow/cloutcli#quick-start-demo) to print out the entire history of all BitClout messages, visualize the full social graph, search all clouts, send mass DMs to users, and in general gain full access to the entire BitClout backend as you would with any other public blockchain. These actions would be impossible on twitter.com without [corporate permission](https://developer.twitter.com/en/docs/twitter-api/enterprise/historical-powertrack-api/overview) from Twitter.

For this reason we think BitClout as a whole is _already_ significantly more decentralized than Twitter or Facebook. That said, there are still parts of the BitClout project that are centralized or semi-centralized, like [identity.bitclout.com](https://docs.bitclout.com/devs/identity-api) and [images.bitclout.com](https://bitclout.com/posts/db5f007e1c6a3b018bba98362fe5f8488f29f51676aa90ebf0bc2b702aec0f26), which we plan to phase out over time. You can read more about the technical roadmap for progressive decentralization below.

The main difference between BitClout and most previous public blockchains is that BitClout focuses on _social_ rather than financial transactions, like [updating a profile](https://github.com/bitclout/core/blob/main/lib/network.go#L211) or [buying a creator coin](https://github.com/bitclout/core/blob/27426de8fd6dfb5d0d0e98a2f6b82773d92a6288/lib/network.go#L215). Just to make that clear, here’s a [screenshot](https://www.bitcloutpulse.com/explorer/blocks/00000000000099a0306c0bd5b8bbb5eb6e2e716be4a929e20157d2af18189661) of a recent BitClout block from a third party block explorer called bitcloutpulse.com. Note that you can see individual social transaction types, such as `BLOCK_REWARD`, `SUBMIT_POST`, `FOLLOW`, `LIKE`, and so on.  


![](../.gitbook/assets/image.png)

#### The Bitclout.com Frontend

The first frontend UI to the BitClout Blockchain is at bitclout.com. Like the BitClout Blockchain, this is also [open source](https://github.com/bitclout/frontend) and anyone can download and modify it.

And people have. While [Bitclout.com](http://bitclout.com) was the first major node on the BitClout network, and maintains a significant portion of the traffic at the time of this writing, other major nodes have matured recently. Some of them include:

* [bitcloutpulse.com](http://bitcloutpulse.com), which provides analytics tools for the BitClout blockchain
* [Flick](https://www.flickapp.com/bitclout), which serves the dominant iOS and Android apps for BitClout
* [Blockchain.com](http://exchange.blockchain.com), which lists the CLOUT cryptocurrency for trading
* And the many apps building on top of BitClout, listed at [bithunt.com](http://bithunt.com), many of which run their own full nodes

Notably, everything that is not bitclout.com is operated as its own independent entity, and in the roughly three months since the launch of the BitClout project, several of these entities have already raised capital from blue-chip VC firms. For example, Flick was founded by [Nigel Eccles](https://www.youtube.com/watch?v=QM1Bz_eB2Ec), who was formerly the founder of FanDuel, a billion-dollar sports betting company.

These nodes are to the BitClout blockchain what entities like Etherscan and Coinbase are to the Ethereum blockchain. Just like block explorers or exchanges, they allow you to view and interact with the BitClout blockchain. BitClout interactions tend to be social rather than mainly financial, of course.

#### The CLOUT cryptocurrency

As a blockchain, BitClout has its own native cryptocurrency called CLOUT. CLOUT can be purchased and sold at [exchange.blockchain.com](https://medium.com/blockchain/trade-clout-on-the-blockchain-com-exchange-3ee1a2b2aca4), and on any major node using dollars or BTC. This purchase comes out of a pool of CLOUT maintained by each individual node operator.

What can CLOUT be used for?

* _Creator Coins_. CLOUT is also used to buy Creator Coins \(see below\), which are assets native to the BitClout blockchain.
* _Fees_. Every transaction on the BitClout blockchain requires some small amount of CLOUT to cover transaction fees. 
* _Other Applications_. BitClout developers can build applications that require users to hold a certain amount of CLOUT to get in, that permit people to do multi-signature transactions with their CLOUT private keys, that enable people to use CLOUT to run crowdfunders on platform, and more.

In general, every application written on the BitClout blockchain can make use of CLOUT. Think about what you’d do with Twitter or Facebook if every user had a balance.

#### Creator Coins

In addition to CLOUT, the overall currency of the platform, each individual user on BitClout has their own creator coin. Creator coins are described more fully in [this explainer](https://docs.bitclout.com/#what-are-creator-coins), but in brief they allow users to support their favorite creators by buying their coin, a little like a combination of AngelList and Patreon. Creator coins can be thought of as the simplest way to issue a token for an individual, and individual creators can add value to their creator coins in many ways. For example:

* [Bitcloutmembers](https://bitclout.com/u/cloutmembers) is Substack for BitClout. You need to hold a certain amount of a creator coin before you can see the creator’s premium posts.
* [Hyped](https://bitclout.com/u/hyped) is Eventbrite for BitClout. You need to hold a certain amount of creator coins to enter the creator’s live event.
* [Paywithbitclout](https://bitclout.com/u/paywithbitclout) is Gumroad for BitClout. You use creator coins or CLOUT to buy digital goods.

The reason we have creator coins in addition to CLOUT is that creators can add creator-specific utility to their individual coins. As with CLOUT, creator coins are stored on the BitClout Blockchain.

### **What is BitClout’s Twitter account?**

bitclout.com's only official Twitter account is @bitclout \([twitter.com/bitclout](https://twitter.com/bitclout)\). We posted proof of this on the BitClout blockchain [here](https://bitclout.com/posts/d61106b2b0f4fabe621909dbd297b9d7d659c49ba9c76ec2400eca97321033ae).

### **What is BitClout’s GitHub account?**

All of the code for the BitClout reference implementation, as well as tools and other libraries, can be found in the BitClout GitHub project at [github.com/bitclout](https://github.com/bitclout). The key repositories are:

* [github.com/bitclout/core](https://github.com/bitclout/core)
* [github.com/bitclout/backend](https://github.com/bitclout/backend)
* [github.com/bitclout/frontend](https://github.com/bitclout/frontend)
* [github.com/bitclout/identity](https://github.com/bitclout/identity)

If you're an engineer, read the [developer docs](https://docs.bitclout.com/code/dev-setup) for how to clone these repositories, synchronize the BitClout blockchain locally, and stand up your own instance of a BitClout node.

## **2. Bitclout.com Profiles, Seed Phrases, Identity, and Messaging**

### What is a bitclout.com profile?

To first order, a bitclout.com profile is similar to a Twitter profile. You can see one here: [bitclout.com/u/diamondhands](https://bitclout.com/u/diamondhands) and read more about profiles [here](https://docs.bitclout.com/#tweet-to-claim-your-profile).

But there are some key differences between bitclout.com profiles and Twitter profiles. Among them:

1. Recall that bitclout.com is just one client to the BitClout blockchain. Other clients include [bitcloutpulse.com](https://bitcloutpulse.com), [flickapp.com](https://www.flickapp.com/bitclout), and many others listed at [bithunt.com](https://bithunt.com). So a profile can be removed from bitclout.com while the underlying user data is _still present_ on the BitClout blockchain. By contrast, when Twitter [suspends](https://help.twitter.com/en/managing-your-account/suspended-twitter-accounts) a user, that user has no way of gaining access to their messages, followers, or other user data without Twitter's consent.
2. In particular, because the underlying user data for any bitclout.com profile \(including access to funds\) is present on the BitClout blockchain, a user whose profile is removed from bitclout.com retains access to their funds through their seed phrase \(see below\).
3. Moreover, the developers who run individual BitClout clients like bitcloutpulse.com and flickapp.com can decide whether to show or hide any given profile at any given time, independent of bitclout.com's decision.

Put another way, a Twitter profile like twitter.com/bitclout is a view of data that is stored in the closed Twitter database. But a bitclout.com profile is a URL like bitclout.com/u/diamondhands which is a view of data that is stored on the [_public_](https://bitcloutpulse.com/explorer) BitClout blockchain. Anyone else can code a different view, and hide or show different users.

### What is a BitClout seed phrase?

Your BitClout seed phrase is a series of 12 words that is the password to your entire account. It's not just the password to your bitclout.com profile, but to the funds and the data stored on the BitClout blockchain. Your seed phrase can never be changed and sharing your seed phrase with anyone could result in a loss of funds. Read more [here](https://docs.bitclout.com/faq/privacy-and-security).

### How do I create a bitclout.com profile and seed phrase?

For 99% of users, you can simply follow the signup flow on [bitclout.com](https://bitclout.com). However, if you are one of the ~15,000 users with reserved profiles, read on.

### What is a reserved profile and how does it work?

In March of 2021, the BitClout core dev team reserved bitclout.com profiles for thousands of people, mainly to prevent squatting and impersonation of those handles. These were just ordinary profiles created by the dev team in the same way ordinary users create profiles \(namely by broadcasting transactions to the BitClout blockchain\).

Then, as an added benefit to the people for whom they reserved these profiles, the dev team also funded these profiles with BitClout to give them some of their own coin.

The dev team then made it possible for anyone on Twitter to claim their profile by Tweeting out their public key. This conveniently solved an authentication issue, where you need to be sure that the person who’s reaching out to claim the profile is really the person they say they are.

Once a profile is claimed, the dev team transfers the profile to the public key contained in the Tweet. The dev team created these profiles, and so they then transfer these profiles — including control over the reserved creator coin assets — to the party claiming the profile using the [SWAP\_IDENTITY](https://github.com/bitclout/core/blob/2252c71379d9e7d0872c7e4844413134b5d7393c/lib/network.go#L216) transaction type on the blockchain.

### How do I claim my profile at bitclout.com, if it was reserved?

Simply create an account on [bitclout.com](https://bitclout.com), navigate to your reserved profile, and hit the button to claim your profile. This will draft a Tweet with your public key in it that support can use to transfer your profile to you.

![](../.gitbook/assets/image%20%281%29.png)

### How do I remove my profile from bitclout.com, if it was reserved?

Simply DM @bitclout on Twitter from the account with the same handle as the reserved account. This is required in order to verify that you own the Twitter account associated with the reserved profile.

Importantly, removing your account from bitclout.com does not remove it from the underlying BitClout blockchain. This means your profile may still show up on other nodes, such as [bitcloutsignal.com](https://bitcloutsignal.com) or [prosperclout.com](https://prosperclout.com). They may decide to follow bitclout.com or make their own moderation decisions.

An account can be removed from bitclout.com either by request or by the admin of the bitclout.com node. Only the owner of the account can request removal; accounts cannot be removed by anyone other than the owner or the admin of the bitclout.com node.

### How do I un-remove my profile from bitclout.com?

Importantly, profiles cannot be removed from the blockchain, and so this question refers solely to hiding profiles on bitclout.com.

If the account was removed from bitclout.com by the owner, the owner can email [node.admin@protonmail.com](mailto:node.admin@protonmail.com) to request it be unhidden on bitclout.com. They may be asked to provide proof that they control the private keys of the account.

The bitclout.com node admin also has the discretion to hide profiles. In such cases, the profile cannot be unhidden by the owner. This does not affect how other nodes like BitClout Pulse, ProsperClout, BitClout Signal, or Flick show the profile.

### Can reserved profiles be removed prior to being claimed?

Yes. As described in [this answer](bitclout-faq.md#what-is-a-reserved-profile-and-how-does-it-work), reserved profiles are controlled by the BitClout core developers until they are transferred over to the corresponding Twitter users who claim them. This means that the core developers can choose to remove reserved profiles if this is in the best interests of the community. If this happens, users can still message @bitclout to claim their handles, but they will not receive the pre-purchased coins that the core developers allocated to their names. Anyone who holds coins in this profile can still access this profile in their wallet and sell their coins.

In the past, reserved profiles have been removed when it appears as though the person claiming them is going to either immediately sell the pre-purchased coins that the core developers allocated to them, or "rug-pull" on their holders. This is because rug-pulls are not only harmful to the holders of the reserved user’s coins, but also because they promote a short-term mindset that is harmful to the community more generally.

### What is the BitClout seed phrase and is it the same as the BitClout private keys?

Seed phrases were used as a mechanism to log users in prior to the introduction of the “Login with Google” flow. A seed phrase is used to deterministically generate one’s private key, and neither the seed phrase nor the user’s private key ever leave the browser.

### If I signed up with a seed phrase only, how do I switch to login with Google with 2FA?

Currently, we don't have a migration path for people who logged in with a seed phrase and want to "import" that seed phrase into their Google account. We hope to have a fix for this soon so that everyone who wants to use Google to login can import their seed phrase and have the benefit of 2FA and secure storage of their seed phrase. The dev community is working aggressively on improving BitClout Identity to add this, especially the Flick team, which creates the top BitClout mobile apps.

### Can Bitclout.com access my private keys, if I’m a normal user?

No. A user who signs up at bitclout.com fully controls their profiles with their private keys. When a user signs up with a seed phrase, their private keys _never_ leave the browser.

When a user signs in with Google, the user's private keys are backed up to an isolated part of that user's Google Drive that nobody can access other than that user.

For more information on how BitClout private key storage works, see [here](https://docs.bitclout.com/faq/privacy-and-security).

Note: Although a user's private keys never touch bitclout.com's servers, it is important to mention that profiles and creator coins \(not $CLOUT\) can be recovered by certain ParamUpdater public keys using a [SWAP\_IDENTITY](https://github.com/bitclout/core/blob/2252c71379d9e7d0872c7e4844413134b5d7393c/lib/network.go#L216) transaction type that the core dev team intends to remove after an initial bootstrapping phase. This transaction type has only been used to transfer profiles and recover profiles for high-profile accounts that lost their seed phrase prior to the "Login with Google" update.

### Can Bitclout.com access my private keys, if I’m a reserved user?

The private keys for reserved profiles are controlled by the core developers until the profiles are transferred over to the users who claim them. After this transfer is complete, the user fully controls these profiles with their private keys, which never leave their browser.

Note: Although a user's private keys never touch bitclout.com's servers, it is important to mention that profiles and creator coins \(not $CLOUT\) can be recovered by certain ParamUpdater public keys using a [SWAP\_IDENTITY](https://github.com/bitclout/core/blob/2252c71379d9e7d0872c7e4844413134b5d7393c/lib/network.go#L216) transaction type that the core dev team intends to remove after an initial bootstrapping phase. This transaction type has only been used to transfer profiles and recover profiles for high-profile accounts that lost their seed phrase prior to the "Login with Google" update.

### Can developers using identity.bitclout.com access my private keys?

No. identity.bitclout.com only exists as a means of isolating key material to an iFrame in the user’s browser. This isolation not only makes bitclout.com more secure, but it also provides a means for third-party developers to leverage accounts created on other nodes by including identity.bitclout.com as a dependency. Notably, anyone can spin up a clone of identity.bitclout.com since it is [fully open-source](https://github.com/bitclout/identity), and we expect this to be an opportunity for exchanges to custody users’ identities in the future. For more information about BitClout’s identity API, see [here](https://docs.bitclout.com/devs/identity-api).

### Can anyone create an account on the BitClout blockchain without going through Bitclout.com?

Yes, absolutely. Apps such as bitcloutpulse.com and Flick already allow this. Users can also create accounts programmatically by broadcasting the proper transactions to the blockchain. This [toolslib](https://github.com/bitclout/backend/tree/main/scripts/tools/toolslib) directory is useful for that.

### Can random BitClout Blockchain users access my private keys?

No. Nobody other than the user themselves can access their private keys.

When a user signs up with a seed phrase, their private keys _never_ leave the browser.

When a user signs in with Google, the user's private keys are backed up to an isolated part of that user's Google Drive that nobody can access other than that user.

### Where should I store my private keys?

If you use the “Login with Google” signup flow, then no storage is needed. Your private keys are stored in an isolated part of your Google Drive by default that only you can access.

If you are using raw seed phrases to login, see [this doc](https://docs.bitclout.com/faq/privacy-and-security) for information on how to manage them securely.

### When should I enter my seed phrase? 

**You should never enter your seed phrase on any site other than the one that generated it \(typically identity.bitclout.com\).**

[BitClout Identity](https://docs.bitclout.com/devs/identity-api) makes it so that seed phrases never need to be entered directly into third-party apps. There are still some legacy apps that request your seed phrase, but these should be avoided.

### How private are on-chain DMs, and what is the roadmap to making them more private?

Message text is end-to-end encrypted with your private key, which never leaves your device.

[**🚨**](https://emojipedia.org/police-car-light/) ****However, similar to how the Bitcoin blockchain publicly stores **who you’ve sent money to** **and when**, the BitClout blockchain publicly stores **who you’ve messaged and when**. ****[**🚨**](https://emojipedia.org/police-car-light/)

We are working on a fix to this, but the engineering is nontrivial. In the meantime, if you want total DM privacy, we recommend putting your Telegram username in your bio to have people contact you there.

## **3. BitClout Blockchain, Decentralization, Open Source, and Scaling**

### Is Bitclout.com open-source?

Yes, 100% of the code that powers bitclout.com is public [here](https://github.com/bitclout). And we mean 100%. This includes all the code required to [run a node](https://docs.bitclout.com/devs/running-a-node) and to [run the frontend](https://github.com/bitclout/frontend).

Moreover, all of the BitClout data is stored publicly on the blockchain, such that anyone who runs and syncs a node can instantly build on the full firehose of data \(there are multiple popular third-party block explorers such as [explorer.cloutangel.com](https://explorer.cloutangel.com) and [bitcloutpulse.com](https://www.bitcloutpulse.com/explorer)\). This property in particular has the potential to turn social media from a walled garden controlled by a handful of companies to a utility that anyone in the world can build and innovate on.

### **Is the BitClout Blockchain open-source?**

Yes, the code that powers the BitClout blockchain is mainly the core repo [here](https://github.com/bitclout/core).

### **What is the difference between Bitclout.com and the BitClout Blockchain?**

Bitclout.com is a BitClout node, which means it downloads all transactions from its peers and writes transactions to the blockchain.

The BitClout blockchain is the set of all transactions that have occurred on the BitClout network, which is a superset of the transactions that happened on bitclout.com, which is just one node on the network.

### **What parts of BitClout are centralized at bitclout.com and what parts are decentralized on the BitClout Blockchain, and what parts of the BitClout Blockchain are still semi-centralized?**

All code is public, and nearly all data utilized by bitclout.com is publicly stored on the blockchain.com. There are some exceptions, however, for good reasons:

* **Removals.** Which profiles have been removed from bitclout.com is not stored on the chain. This is intentional in order to prevent bitclout.com from having the power to unilaterally deplatform someone from the BitClout network. It means that decisions about what profiles should and shouldn’t be exposed from the blockchain are made at the node level, creating a significantly more decentralized approach to moderation than the status quo for social media provides today.
* **Verifications.** Verified checkmarks are not stored on the blockchain. This is for the same reason that removed profiles aren’t. Instead, the core dev team has plans to introduce an “association” primitive that will allow profiles to associate with one another, effectively creating a more decentralized version of the blue checkmark. For example, your profile can list associations with universities or workplaces that have been cryptographically signed.
* **Emails and phones.** Emails and phone numbers are not stored on-chain for privacy reasons currently. However, we are working on storing them in an encrypted fashion to support features like sharing one’s email or phone number with other users on other nodes.
* **Images and videos.** Although the BitClout blockchain stores all text posts directly, images and videos are stored in a centralized fashion at images.bitclout.com such that only links to the images are actually stored on the blockchain. This is because images and videos are still too large to efficiently store on blockchains currently, though technologies like IPFS and Arweave are promising.
* **Identity iFrame \(for now\).** Identity.bitclout.com is currently the dominant domain used to serve a BitClout Identity app. However, we expect this to decentralize more as exchanges enter the Identity game.
* In consensus, there are two transaction types that are temporarily only executable by certain [ParamUpdater public keys](https://github.com/bitclout/core/blob/5ce03b9318b6b447f895cc9db7d61de54736c1a6/lib/constants.go#L449). They are [UpdateGlobalParams](https://github.com/bitclout/core/blob/5ce03b9318b6b447f895cc9db7d61de54736c1a6/lib/network.go#L217) \(for fees\) and [SwapIdentity](https://github.com/bitclout/core/blob/5ce03b9318b6b447f895cc9db7d61de54736c1a6/lib/network.go#L216) \(for reserved profiles\).
  * UpdateGlobalParams is a tool that can be used to set a global transaction "fee rate." This is useful for when the network is getting spam attacked, as it allows [a global fee rate](https://github.com/bitclout/core/blob/5ce03b9318b6b447f895cc9db7d61de54736c1a6/lib/block_view.go#L287) to be increased fairly quickly \(in contrast to other chains where node operators must individually adjust their fees, leading to consensus issues\). It also allows for the adjustment of [CreateProfileFeeNanos](https://github.com/bitclout/core/blob/5ce03b9318b6b447f895cc9db7d61de54736c1a6/lib/block_view.go#L284), which is the fee required in order to create a profile on the BitClout blockchain. The core devs hope to make both of these parameters override-able in the near future.
  * SwapIdentity is used to transfer reserved profiles from the core devs to the users who are claiming them. It is also useful in restoring someone's account if they lost their private key. It transfers creator coin holdings but, importantly, **it does not affect BitClout balances --** _**nothing can change BitClout balances without a user's signature**_**.** Soon, this will be usable by anyone, not just the ParamUpdater public key holders, to implement things like third party marketplaces for usernames. Once this happens, the ParamUpdater public keys will no longer have the ability to transfer arbitrary profiles.

### **How decentralized is the BitClout Blockchain, and what is the roadmap for further decentralization?**

Today, the BitClout blockchain is fully replicated across dozens of independent nodes all over the world. [Anyone can run a node](https://docs.bitclout.com/devs/running-a-node) and join the network, and because it is currently based on proof of work, anyone can mine blocks as well. This has already resulted in [hundreds of apps](http://bithunt.com) being built on the BitClout blockchain, including:

* [Flick](https://www.flickapp.com/bitclout) is an amazing BitClout iOS and Android app that was created by @nigeleccles, who co-founded FanDuel previously. They and others like [Cloutfeed](https://bitclout.com/u/cloutfeed) and [Cloutie](https://bitclout.com/u/CloutieApp) did such a good job that core devs will never build an app. Third-party apps are first-party apps on BitClout.
* [BitClout Pulse](http://bitcloutpulse.com) is an awesome "Bloomberg Terminal" for BitClout. [BitClout Signal](https://bitcloutsignal.com) is amazing for transaction history and analytics as well. They both built timeseries tech that the core devs couldn't, and they blow the reference implementation’s indexes out of the water.
* Our favorite block explorer is NOT the one we built, it's [explorer.cloutangel.com](http://explorer.cloutangel.com). Just compare it to our block explorer at [explorer.bitclout.com](https://explorer.bitclout.com). Head to head, ours is a joke. CloutAngel is better in literally every way.
* And then of course there's [Moonbounce](https://getmoonbounce.com/), which is creating amazing engagement tools for creators on BitClout that will unlock new behaviors and interactions between creators and their followers.

Additionally, although we don’t track the number of nodes currently running, it is important to mention that the node Docker images have been downloaded over 10,000 times, and we have seen hundreds of nodes on the network at any given time.

BitClout’s model differs from pure proof of work, however, in that it allows node operators to specify a set of [trusted block producers](https://github.com/bitclout/core/blob/27426de8fd6dfb5d0d0e98a2f6b82773d92a6288/cmd/run.go#L165). When a node sets a set of trusted block producers, it will only accept blocks if they have been signed by at least one of the block producers in the set. So, for example, all the major exchanges could include each others’ public keys as trusted block producers. If they did this, then the network would be more centralized, but it would also be more robust to censorship by miners.

In the short term, this mechanism is useful in preventing 51% attacks while BitClout is in such an early stage. When bootstrapping a proof of work network, there is a problem whereby early in the network’s development, the hash power is not high enough for it to be secure against 51% attacks. Thus, proof of work networks generally suffer from a chicken and egg problem: They can’t get enough hash power to defend against 51% attacks. When, instead, blocks require both miners and block producers to agree on each block, the system is safe as long as at least one of the two groups is behaving. And if either group starts misbehaving, then the other group can fork it out. For this reason, the core developers don’t operate any mining hardware; it is fully community-run, and the hash power can be tracked on [bitpool.me](http://bitpool.me) \(which is also completely unaffiliated with the core devs\).

Importantly, in the long run, which is likely a matter of months, BitClout intends to move to a full proof of stake system, whereby the need for the trusted block producer model will be eliminated.

### **Can BitClout scale?**

The technical project of scaling centralized social networks occurred about 10-15 years ago, during the mid/late 2000s, when social networks rose from ~0 to 1B users. It’s now been about 10 years since that hypergrowth phase. With the lessons learned, we think it may now be feasible to scale a decentralized social network.

In the following discussion, an important point is that BitClout gives strong incentives for people and companies to run full BitClout nodes with significant hardware resources, because they can run custom frontends on top of the nodes \(essentially their own versions of bitclout.com\) and monetize them through fees and added features.

We have four phases currently planned for scaling BitClout.

* Phase 1: Bigger blocks.
* Phase 2: Warp sync.
* Phase 3: Sharding.
* Phase 4: Advanced Cryptography.

The math below walks through BitClout's scalability at each stage:

1. Bigger blocks
   * The average BitClout blockchain post size is **218 bytes**.
   * There are 10 other [transaction types](https://github.com/bitclout/core/blob/135c03a/lib/network.go#L239) besides `POST`, such as `LIKE` and `FOLLOW`. In a recent block, posts were about **1/3** of the total block size.  ![](https://lh4.googleusercontent.com/YSLyEVtV0Ynx--mta7IP3QS5aVrZiq7MBVmIc9h9bZwbCrLXXTIIDzO2Gm9RYOjaqQONhOju-F7RvaTIVO6vWJ5AMASXIYHMI4z9sjK3acpoXOmhRHX99-35qS4I54KBl2C3zjnH)
   * The BitClout blockchain currently produces up to 2MB blocks every 5 minutes, as you can see from [bitcloutpulse.com/explorer](https://bitcloutpulse.com/explorer).
   * So it can scale to ~30 transactions per second, **which is ~10 posts per second** \(= 2e6 bytes/block / \(218 bytes/post \* 60 seconds/minute \* 5 minutes/block\) \* 1 post / 3 transactions\).
   * If we increase the block size to 16MB blocks every five minutes, we can roughly extrapolate that it scales to ~240 transactions per second = **~80 posts per second**.
   * For comparison, Twitter has approximately [6000 posts/second](https://www.dsayce.com/social-media/tweets-day/#:~:text=Every%20second%2C%20on%20average%2C%20around%206%2C000%20tweets%20are%20tweeted%20on,August%202014%20with%20661%20million.) on average with 300M users.
   * So, at 80 posts per second, we should be able to roughly accommodate about 80/6000 = 1.33% of 300M users, or **4M users**.
   * That’s where we can get with a basic block size increase alone. But we have a few other cards to play.
2. Warp sync
   * With an Ethereum-like [warp or snap sync](https://blog.ethereum.org/2021/03/03/geth-v1-10-0/), we loosen a key constraint, which is the need for all nodes to always validate the entire history of transactions. \(You can still run an archival node, but this won’t be necessary for normal operations.\)
     * As a concrete example, if all you're downloading is the current creator coin balances for each user, then all that user's trades are effectively compressed into a few integers because you don't care about the history \(only the end state\).
   * Instead, we move to a model where nodes by default first sync and validate a snapshot of the current blockchain state and then sync only a few week's worth of blocks on top of that.
   * Generally, the bottleneck to blockchain performance is validation speed. With BitClout, we've run tests that indicate a node running on an Intel Xeon E-2276M can validate transactions at the rate of **~12MB/s = 1.04TB/day = ~55,000 txns per second**.
   * So, if we start by just downloading the state with minimal validation, how big can it be? To download 10TB at 10gbps = 10e12/10e9\*8/60/60 takes about ~2.2h. To download 100TB at 10gbps = 10e12/10e9\*8/60/60 is about **~22.2h to download the state**. 
   * With warp sync, the block size can be increased beyond 16MB because the number of blocks required to get a node up-to-date can be reduced to only one week's worth of blocks rather than the entire history of blocks from the beginning of time.
   * Let’s suppose then that using warp sync we increase the block size further to 120MB blocks. How many transactions per second \(TPS\) would 120MB blocks every 5 minutes facilitate? If we assume 218 bytes per transaction \(which is what the value is per post\), then \(120e6 bytes / \(5 minutes \* 60 seconds/min\)\) / \(218 bytes/txn\) =  **~1,800 transactions per second**.
   * How much bandwidth would it take to synchronize one week of 120MB blocks appearing every 5 minutes, and how long would it take in wall clock time?
     * Bandwidth would be roughly \(120e6 bytes/block \* 1 block/5 minutes \* 60 minutes/hour \* 24 hours/day \* 7 days/week\) / 1e9 bytes/GB = 238GB/ week
     * At the aforementioned validation speed of 12MB/s on an Intel Xeon E-2276M, it would take approximately 238e9 bytes/week / \(12e6 validated bytes / second \* 60 seconds/minute \* 60 minutes/hour\) = **5.5-6 hours to download and validate one week of 120MB blocks** at a validation speed of 12MB/s on good hardware.  
   * Under these assumptions, This means that the warp upgrade can allow us to sync a node in ~5.5h while maintaining 1,800 transactions per second \(TPS\) long-term.
     * 1,811 tps vs Twitter with 6,000 posts per second and 300M users \(assume only posts, no likes\)
     * Users = 300M \* 1,811 / 6000 / 3 txns per post = **~30M users.**
3. Sharding
   * All transactions can then be write-sharded to make syncing a node parallelizable, thus providing potentially another order of magnitude speedup. For example, we could do a very simple optimization, which is to shard all posts into their own sub-chain, and then shard other transactions across two of their own subchains.
   * This would result in a node being capable of syncing 3x faster, meaning that we could support **~90M users without an increase in sync time**.
   * Ultimately, all transactions can be sharded into sub-chains by user ID, which would allow for virtually unlimited parallelization. For example, with ten shards, we achieve another 3.3x multiplier on the TPS without an increase in sync time, thus achieving **~300M users**.
   * This number can be scaled further by increasing the number of shards.
4. Advanced cryptography
   * This is highly speculative, but current research into zk-SNARKs is showing positive results, indicating that we could one day be able to use these primitives to scale without requiring nodes to sync the full state from one another.

### **Is there obfuscated code in the BitClout blockchain miner?**

No. The BitClout reference implementation contains a fully open-source CPU miner with good comments [here](https://github.com/bitclout/core/blob/main/lib/miner.go). 

Some independent developers offered obfuscated binaries tailored to GPU mining, offering faster hash rates using proprietary software, but the BitClout core developers were not involved in this \(and we take no issue with this since it allows third-party developers to make money off of their IP\). Moreover, awesome developers like @lobovkin have since [open-sourced their GPU code](https://github.com/lobovkin/BitPoolMiner)!

### **Why does signing transactions have to go through an iframe at identity.bitclout.com, and is there a roadmap for local /offline signing?**

It doesn’t have to, and the BitClout core devs provide [a whole suite of tools](https://github.com/bitclout/backend/tree/main/scripts/tools/toolslib) that allow independent developers to construct and sign transactions without needing to rely on identity.bitclout.com. Moreover, web clients can use whatever identity provider they want since all the code that powers identity.bitclout.com is open-source.

### **How does code for the BitClout blockchain get updated?**

Below are the steps to update nodes on the BitClout network, as it exists today:

* Code is merged on [GitHub](https://github.com/bitclout).
* A release tag is updated to some commit by the core devs.
* All node operators who are running Docker images that point to the github.com/bitclout/&lt;repo&gt; reboot to pick up the updates.

This process is relatively centralized, but it’s how most other chains work currently. The check against centralization is the fact that _**any group of sufficiently-motivated developers can hard fork all of the code and data at any time because both the code and data are totally open.**_

### **How does Bitclout.com moderate content?**

All users have the right to post, like, comment, message and do all basic BitClout functions forever, as long as they are willing to pay mining fees for their transactions. This is because anybody in the world can [run a BitClout node](https://docs.bitclout.com/devs/running-a-node), which means anybody in the world can access the full range of social network features at all times. Users can also choose from a [long list](https://bithunt.com/explore) of nodes other than bitclout.com such as [bitcloutpulse.com](https://bitcloutpulse.com) and Flick, which have their own distinct moderation policies.

Each individual node on the BitClout network has its own distinct moderation policy. If you want full freedom, you can run your own node fairly easily; however, if you’re using someone else’s node then you are subject to their moderation.

We believe this model of “node-level” moderation presents a significantly more decentralized approach to curating public discourse than what is offered by existing social media today. This is because the barrier to entry for running a node is so low, that we can expect thousands of them to exist, and thus a wide range of diversity in how discourse is curated.

See [running a node](https://docs.bitclout.com/devs/running-a-node). Also see [here]().

### **Can I view, sell, and access my $CLOUT and creator coins on another BitClout node if my profile was removed at Bitclout.com?**

Yes. With BitClout, all profiles and posts are stored on the blockchain forever. You cannot remove a profile or post from the blockchain once it has been created and mined into a block.

Individual nodes are responsible for serving their own curated “view” over the blockchain, and they can choose what content they want to show at the node level.

bitclout.com is one node on the network, but there are already many others that do their own totally distinct moderation, such as bitcloutpulse.com, bitcloutsignal.com, prosperclout.com, and tijn.club. The decisions behind what bitclout.com shows do not affect these other nodes.

For example, bitclout.com does not surface all profiles in its UI; it filters out certain profiles that are spamming, scamming, or impersonating legitimate users. To see any profile that is not available on bitclout.com, you can generally just go to any node that is not bitclout.com. For example, any of the following nodes currently show all profiles:

* [https://bitcloutsignal.com/history](https://bitcloutsignal.com/history) 
* [https://www.prosperclout.com](https://www.prosperclout.com) 
* [https://tijn.club](https://tijn.club)

Note that reserved profiles are controlled by the core devs until they are transferred, and so the coins owned by those profiles are not in the custody of the corresponding Twitter user until they are claimed. This means the core devs reserve the right to remove profiles prior to transferring them, as discussed [here](https://app.gitbook.com/@bitclout-1/s/diamondhands-drafts/~/drafts/-Mda9JRswSJvc85UwFlq/bitclout-faq).

### How does the global feed work?

Every node operator gets to curate what shows up on their own global feed, as described and shown [here](https://docs.bitclout.com/devs/running-a-node#managing-your-feed). This means that, in the very near future, we will have many feeds, not just the global feed on bitclout.com, as described in more detail [here](https://docs.bitclout.com/devs/running-a-node#running-your-own-feed).

In addition, the follow feed will _always_ shows you every post from all the people you follow, which reduces the reliance on node-level curation. Once you're following enough people, there ceases to be a need to check the global feed anymore.

This being said, there are no strict guidelines on anything that shows up in the global feed on bitclout.com. In the short-term, because bitclout.com aims to appeal to as wide an audience as possible, the content will generally bias more toward things that have mass appeal. But that's about it.

## **4. $CLOUT**

### **Can you cash out your $CLOUT for BTC?**

Yes! The BitClout cryptocurrency recently listed on [exchange.blockchain.com](http://exchange.blockchain.com). There, you can exchange BitClout for USD and Bitcoin. Other exchange listings are coming soon!

### **Why are there so many coins in the Genesis Block?**

This is a commonly-misunderstood issue, and it is due to the fact that there was a hard fork in March 2021 that compressed all transactions between November 2020 and March 2021 into a single block. This made it look as though the BitClout core developers gifted coins to early purchasers, when in reality everyone bought right off the bonding curve defined in [supply.go](https://github.com/bitclout/core/blob/738005f9746454374121224b6ca646ce4d0ffb68/lib/supply.go#L1).

### **What happens to the BTC people use to buy $CLOUT?**

Today, node operators can specify a [CLOUT wallet](https://github.com/bitclout/backend/blob/527751f1358aae19b13450cad2c0d270b9ba226b/cmd/run.go#L129) and a [Bitcoin wallet](https://github.com/bitclout/backend/blob/527751f1358aae19b13450cad2c0d270b9ba226b/cmd/run.go#L128) and allow their users to seamlessly purchase CLOUT for Bitcoin through their node. The node operator can then use the Bitcoin they accumulate purchase CLOUT from an exchange to refill their CLOUT wallet and earn a "slippage fee" for this service \(basically acting as a market maker\).

Prior to the [Deflation Bomb](https://bitclout.com/posts/3a13a7e4342148e76e1de957f22775a4f6916ed809a90e77a035bb7cefaaaf44?feedTab=Global), CLOUT was purchased via a [BitcoinExchange](https://github.com/bitclout/core/blob/31735a82b8446421176df151dd5f80d9dac2eabd/lib/network.go#L208) transaction on the BitClout blockchain, which resulted in BTC accumulating in a treasury wallet. For security reasons, we cannot publicly explain exactly how these funds are custodied. This being said, the core dev team is working on clarifying the governance around this wallet and should have some updates very soon.

## 5. Community Questions

A community AMA recently resulted in some [amazing questions](https://bitclout.com/posts/d4d29c0adb284da5190d61593e9dafceddeb92ea02e9d0771f0e6c5f05b6d44c). We decided to give them their own section here to make sure they were all covered.

### Roadmap

#### Can we have more visibility of the product roadmap? This will help the dev community to build appropriately. Would you consider making the roadmap public? \(@GeneGMB @Davidsun @usmansheikh @CloutCurator\)

We are working on formalizing this. The priorities fluctuate, but below are some priorities for the next month or two as of July 2021 \(which is a long time for BitClout!\). These are from memory as I'm typing fast, and I'm probably missing several:

* **NFTs** We will be getting feedback on the community for some proposals prior to this, and are looking to develop and launch this a little more collaboratively than other features.
* **Improve the onboarding and overall look and feel of the reference client.** The core dev team has retained a professional design firm to work on these. And all improvements become instantly accessible to the community via the open-source repos.
* **Notifications.** Currently, bitclout.com sends precisely zero email and phone notifications, even if users have opted into them. It's somewhat ridiculous that this is the case, and that BitClout has the traction that it does given the lack of a "notification loop," but it will be fixed soon \(and all node operators will benefit\).
  * Part of this work requires txindex to be more efficient, which is a top priority in its own right. There is currently an experiment underway to move it to Postgres, which will make it easier to query as well.
* **Derived keys \(aka permissions\).** Third-party apps on iOS currently have issues using identity.bitclout.com due to app store restrictions. Separately, many have expressed a desire to have someone else manage their account without giving them access to their funds or their seed phrase. Both of these concerns will be addressed by a proposal we're working on to allow "derived keys" to sign transactions on behalf of a "master key" for some period of time. We are working with Flick and Gem on this in particular.
  * Along with this work is implementing a way for ordinary users to transfer their profile to a new key, which will allow us to hopefully remove/sunset SwapIdentity.
* **Referral program and gas on the fire.** Almost no money has been spent to acquire BitClout users, and the core dev team has yet to tap into a deep bench of high-powered connections to grow BitClout. This is because the core devs have wanted to refine everything before "pouring gas on the fire." This being said, we believe that after completing the other priorities, we will be ready. And every user we acquire will create value not just for bitclout.com, but for all third-party apps.
* **Scaling.** Soon we will hit the limits of "big block" scaling \(see [here](https://app.gitbook.com/@bitclout-1/s/diamondhands-drafts/~/drafts/-MdaSuHgyhxyqnpDFNo0/bitclout-faq)\). We will be working very hard with the community to make sure the warp upgrade is in place prior to this point.
  * Along with this work is investigation into the optimal way to move to full proof of stake.
* **Listings.** In the background of all of this, expect more listings. Smaller exchanges will be faster to list than bigger exchanges, but we are working with as many as we can to list CLOUT on as many venues as possible. Every listing brings with it new users on these exchanges that have never heard of BitClout before, serving as a great growth hack in and of itself.

#### When will decisions & timescales for move to Proof of Stake be open for review? \(@Davidsun @tijn\)

There is no designated time on this yet, but hoping within 3 months of July 2021.

#### When will verification and profile blocks move on chain and how will this work? \(@ItsAditya @Tijn\)

There is a PR to move blocks on chain that will be merged soonish. For verification, we have a proposal that we call "associations" that will allow any profile to "associate" with another profile, creating a directed graph of "associations." The blue checkmark on bitclout.com will then become a special case of this, as simply an association between a bitclout.com profile and a user, and each node will be able to provide its own blue checkmark via its own on-chain association.

This same primitive can also be used by universities or other credentialing authorities in the future as a means of doing cryptographically verifiable diplomas. This would cause BitClout to become a sort of "identity hub," and eat into the LinkedIn use-case.

But we need to write it up and run it by the community first :\)

#### Was Bitclout originally intended to focus on coin price, gains, trading ? And what consideration was given to this maybe encouraging bad behavior and bad actors by design? \(@GeneGMB\)

It was intended to promote positivity and unity over division and toxicity, while also allowing creators to earn more per follower than on any other platform. Coin price, gains, and trading, to the extent they are a big part of BitClout, are a means to that end. 

#### Will there be a function to lock-up an X part of your coin for an X part of time in our profile? \(@Nigels\)

The amount of time we've spent thinking about lock-ups... There is a tremendously long doc that walks through many proposals and explains why we haven't publicly proposed something here yet, but it's not properly anonymized. I will share it soon...

### Community Engagement

#### Can we have a regular community engagement event, like Clubhouse has their weekly Townhalls \(@Davidsun @GeneGMB @CloutCurator\)

I would love to do this, but we're very focused on shipping. Once we hit the "gas on the fire" point above, however, I think doing more community stuff will make more sense.

#### What is the status of CIPs and can you give us a top line summary of what scope the CIPs will focus on? \(@tijn\)

We've been thinking a lot about how to do them. Right now, because speed is essential to the development of BitClout, we are leaning toward a more casual process, whereby changes can just be submitted via GitHub issues or pull requests and merged with two approvals from the core devs \(with some process for making sure your change is something that's needed\). But we are hoping to write this out more coherenly soon. Just a lot to do.

#### Will there be hackathons, covering product, user experience, and dev? \(@Davidsun @GeneGMB\)

We need the community to do this right now. Again, there are some very high-value things the core dev team wants to focus on, and so we need to put all of our energy on those, and only do very scalable things like this AMA.

#### Will there be a fund to support the developer eco-system \(@Davidsun\)

This would be great, and we hope to develop this more. In the short-term, everyone who we've spoken to about funding has been able to raise tons of money from venture capitalists and hasn't needed any help, which is a very good sign that the organic incentives to decentralize are working as planned.

#### In work underway to implement a badges / profile system on Bitclout? Eg designate profiles ad user, trader, creator, developer, project? \(@brootle @GeneGMB\)

Good idea! We haven't thought about this, but it could be interesting. I would put it as a lower priority than the things mentioned in the roadmap, though, and it does seem to require quite a bit of manual labor to curate.

#### What criteria are or will be used for user verification as it seems inconsistent at times ? And will there be away to get verified without social profiles \(@ItsAditya @lukasjakson\)

See my answer about "associations" previously. The long-term answer is that every node should do its own verification to avoid concentration, and this is precisely why bitclout.com verifications don't carry over to other nodes. Today, bitclout.com verifies users if they have a verified checkmark on Twitter, with very rare exceptions in cases where it looks like a user will add a lot of value to the community. Again, the long-term answer, though, is that no one entity has universal power over this, so that many different standards can compete.

#### Considering the official twitter account, how do you balance this with being a decentralized network - how do you balance anonymous vs public involvement? \(@zopel\)

In a perfect world, the core devs could just focus on shipping. But we're practical and will do public stuff \(anonymously\) as needed to make sure the vision is being communicated appropriately.

#### What can non dev's do to help / support the project? \(@luce\)

Keep doing what you're doing! We are doing our best to read all the PRs, and hopefully it will become easier once the priorities are more known and the process for submitting changes is more fleshed out. **Additionally, onboarding as many devs as you can to BitClout is probably the number one thing you can do to grow the community!**

### Growth

#### What is the plan for growth ? Many devs funding projects based on anticipated growth. Crypto bear market may cause considerable reduction in engagement. \(@Davidsun @GeneGMB\)

See the [roadmap](bitclout-faq.md#roadmap). Gas on the fire will come once a few more big product improvements ship.

#### What are the plans for improving user onboarding? Could there be a welcome page for new users with trusted resources, creators / investors to follow, and best practices? Maybe have a user support centre for reporting and tracking issues? \(@GeneGMB\)

Yes, it's a top priority. See [roadmap](bitclout-faq.md#roadmap).

#### ~~What happens on 10 August when Bitclout.com expires?~~ Bitclout.com has been renewed until 2022. \(@tijn\)

Saw that; thanks guys. We want bitclout.com to eventually shut down full-stop, and it would be great if we could make it happen that quickly, but it will take a little bit more time than that.

#### Are you hiring ? \(@Davidsun\)

Yes! We've had issues hiring since we're not a company and everyone is pseudonymous, but hoping to at least put up a resume drop or something soon for people who are interested.

#### What efforts are being made to convince a major social media platform to test-run a Bitclout front end; either as an optional mode or standalone app? \(@AndyFazliu\)

I can't say anything about this just yet, but I'll give you a hint: Exchanges &gt;&gt; Social Media Companies for this because it allows them to break into social as an adjacent business.

### Reserved Accounts

#### Why can’t I access the formerly reserved profiles of many founders like @conaw, VCs like @ariannasimpson, of whom I hold coins in? Did they asked to have their profiles moved? What happens to those that invested in those coins before removal? \(@Marnimelrose @Davidsun\)

See [here](bitclout-faq.md#can-reserved-profiles-be-removed-prior-to-being-claimed).

#### A big problem is reserved profiles just activating their accounts so they can dump. Would you be open to locking the "gifted" coins of pre-reserved profiles for at least 3 months? Or maybe requiring a month of active posting ? \(@lukasjakson @OscarArgaez\)

See the answer on locking.

#### Will there be an option for reserved accounts to delete their profile if they dont want it? And in that case what will happen to the creator coins & investors? \(@OscarArgaez\)

Yes. See [here](bitclout-faq.md#how-do-i-remove-my-profile-from-bitclout-com-if-it-was-reserved). Their coins remain locked in the profile once they are removed, but will eventually be sold after everyone else has pulled their money out.

### Exchange

#### @diamondhands indicated any Bitclout sold by them on bitclout.com is purchased on blockchain exchange. Does this mean its 1:1 purchase, or does the "Bank" pre-purchase clout on site prior to selling. Do you market buy on Blockchain or place limit orders. Can we have more transparency about $clout purchases on site vs on exchange for the bitclout.com bank? \(@lukasjakson @Krassenstein @Davidsun @tijn @Shhubham\)

Good question. It's not perfect because we don't have a great way to know exactly what price all the Bitcoin came in at. We try and put in limit orders at approximately the price we sold it at. Sometimes we can't execute at those prices and have to raise it, in which case we lose some CLOUT. It's hard to give much more transparency than this here, but hopefully this makes sense.

### Investing

#### What criteria do you \(@diamondhands\) use when you invest in a coin \(@PAZAN\)

When someone creates a lot of value for BitClout or when they're doing something that speaks to me, I invest.

#### What factors influence the BitClout price? \(@PAZAN\)

Our good friends supply and demand, and supply is fixed as of the Deflation Bomb :\)

#### Are there vesting schedules for the original VCs? Is there any deal contractual or otherwise with large early investors on when and or how to sell ? If not what deterrent do they have not to dump early? \(@Davidsun @flanagan\)

The VCs bought off the bonding curve just like everyone else, so technically no. That said, all of the early coin-holders were selected on the basis that they are long-term holders, and I'm not aware of a single one that has sold or plans to sell in the near future \(and we watch the blockchain.com deposits\). VCs in particular typically have 5-10 year investment horizons by default, and they literally can't sell until their funds are up.

### Blockchain / Genesis

#### Why does the transaction ID of genesis block reward not show up as an input for transactions spending clout from genesis block? \(@carsenk @tijn\)

It's a bug. I think it's fixed in the rosetta-bitclout repo but I can't remember if we merged the change back into core.

#### BitClout wallet contains over 4k BTC from clout sales prior to genesis, and after. What is its purpose / role ? \(@Davidsun @Tijn\)

See [here](bitclout-faq.md#what-happens-to-the-btc-people-use-to-buy-usdclout).

#### Whats the role of the 2m clout generated by the devteam as result of FR on 8m genesis clout? \(@tijn @AndyFazliu\)

Can't say anything about this yet, but we're working on it and should have an update soon.

#### What is the long term plan for the SWAP\_IDENTITY god power transaction ? It would allow core team to take over any account. While its in place, what protocols are in place to secure & limit abuse of this transaction? Related, An open SWAP transaction signed by two parties would be very useful \(@CloutAngel\)

The plan is to implement "transfer profile" as a transaction type that any user can use to move their profile from one key to another. Once we have this, we don't need SWAP\_IDENTITY anymore since we will be able to transfer reserved profiles using this non-god-mode transaction type. The reason we didn't come out the gate with a fully-functional transfer-profile is because making it work efficiently is hard, and requires re-indexing posts and other content to use a PKID rather than a public key \(grep the code for PKID to see what I mean\).

#### What was your thinking and reasoning behind not storing creator coins directly on chain, but instead just storing the purchase/sell transactions? This introduces possible issues with financial accounting & taxation. \(@CloutAngel\)

I'm not sure what this is asking. Creator coins are stored directly on-chain.

#### When will independent nodes be able to "decide" what transactions are included in blocks? \(@CloutAngel\)

I'm not sure exactly what this is referring to. Block producers and miners, together, decide what transactions go into blocks.

### Behind the Curtain

#### Who is the funniest and who is the most sarcastic in the core dev group? \(@cloutviz\)

Ha. Good question. @maebeam is the most playful for sure. Beyond that, everyone has a pretty good sense of humor. @diamondhands has a very witty, deep cut style of humor if you can imagine it.

#### How did the core devs all come to work together \(@tijn\)

Without revealing too much, two of the core devs have known each other for over a decade, and everyone is a good friend.

#### How long has Bitclout been in development before it was released earlier this year? \(@tijn\)

@diamondhands started working on it religiously in early 2019, alone, in total isolation. He didn't tell anyone about it until very late in 2020. At that point, he felt it was finally ready to tell people about, and brought on the second core dev. The rest is history.

#### Will the core devs always remain pseudononymous or will there be a face reveal \(maybe at $1k clout price\) ? \(@tijn\)

Pseudonymous for sure. Not only because we prefer it, but also because we think it's the best way to build a truly decentralized platform and to empower third-party devs. BitClout has no CEO or board of directors or shareholders, and pseudonymity helps reinforce that. Nobody will ever de-platform third-party devs, and no company will ever get between a creator and their followers.

#### What does the core dev team have at stake / invested? \(@AndyFazliu\)

A lot :\)

