---
description: Social features on highly-scalable bare-metal architecture
---

# 1⃣ Bare Metal

In terms of architecture, a good way to understand DeSo is to imagine a Bitcoin node, only evolved to be able to handle a much wider array of transaction types than just sending/receiving money, with a vast amount of custom storage and indexing logic tailor-made to support social features at scale.

### Code Walkthrough

For developers who are interested in diving into the lower-level specifics, [this developer guide](../deso-repos/architecture-overview/dev-setup.md) is a good starting point, and this [code walkthrough](../deso-repos/architecture-overview/) is the best way to fully internalize how everything fits together. It may look dense, but it is written in plain English, and shouldn't take more than an hour or two to fully internalize.\
\
For non-developers, the best way to understand DeSo's architecture and its advantages is to continue to the next section, which explains things in high-level terms.

### Storage & Indexing at Scale

While traditional blockchains like Ethereum are extraordinary for creating open financial ecosystems, they are not designed to scale to handle the storage and indexing requirements of running competitive social media applications.

For example, DeFi applications typically require the updating of balance entries in-place, without creating a new "state," whereas every post on a social platform creates a new state that needs to be stored and indexed in a certain way.

Many issues like this make social media a special use case that we believe needs to be unbundled, and given its own dedicated architecture, in order to be properly served.

DeSo's biggest advantage lies in the fact that it is _**not**_ a general-purpose blockchain.

Instead, it supports a narrow set of social-oriented features that it implements on bare metal, using custom indexes that every node builds during consensus when it syncs from its peers.

In contrast, general-purpose blockchains must run all functions through a virtual machine, which is typically orders of magnitude slower than running on bare metal, and even then they cannot build custom indexes for querying as they sync.

As a very simple example, consider a social transaction that updates one's username.

A node needs to check that the username is not currently held by another user before it allows this transaction to go through.

_Simple, right_? Except that when you have just a million users, this lookup becomes prohibitively expensive on even the most advanced general-purpose blockchains today.

In contrast, because DeSo can support this lookup with access to bare metal, it can cheaply and efficiently create a simple key-value index that is as fast as it would be for a centralized social application, and that can even be sharded across multiple disks or nodes as the user-base grows.&#x20;

### Advantages of Bare Metal

The advantages of bare metal only increase as usage increases and as more use-cases are considered.

For example, checking that a parent post exists before allowing someone to reply, or even checking that an NFT is for sale before allowing someone to place a bid (noting that Ethereum's lack of support for on-chain bidding has caused significant centralization and concentration to occur around NFT marketplaces).

As another simple example, consider displaying a simple list of a user's most recent posts.

Because general-purpose blockchains do not generally support ordered lists, this is not even possible without building an off-chain index.

In contrast, DeSo natively supports indexes such as posts ordered by timestamp, profiles ordered by the value of their coin, NFT bids organized by which NFT they're associated with, and much more, and all of these indexes can scale as the user-base grows.

This significantly reduces the complexity of running a node, which in turn can significantly increase the decentralization of the ecosystem, and the number of apps that can be built on top of DeSo.

As one final example, even the mempool of DeSo nodes was written from scratch to support queries for social data, without requiring users to have to wait for blocks to mine.

This seemingly minor optimization is critical in order for DeSo apps to feel "**instant**," and we believe DeSo would not be competitive with traditional centralized social apps without it.
