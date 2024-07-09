---
description: Scaling decentralized social networks with an innovative PoS roadmap
---

# 2️⃣ Scaling Roadmap

## A Simplified Scaling Roadmap

There are currently many different, and suitable, ways that the DeSo architecture can be scaled to support one billion users.

Fundamentally, as long as the feature set is kept sufficiently narrow, with an intense focus on keeping everything as close to bare metal as possible, then most of the tools and lessons of scaling centralized social platforms like Instagram can be directly applied to scaling DeSo.

The above being said, we think it's valuable to provide a scaling roadmap with calculations so that blockchain enthusiasts can take comfort in the existence of a concrete path to one billion users.

Below, we detail a concrete scaling roadmap with four relatively straightforward phases:

* **Phase 1:** Proof of Stake
* **Phase 2:** Bigger Blocks
* **Phase 3:** HyperSync
* **Phase 4:** Sharding

The math below walks through DeSo scalability at each stage:

1. **Proof of Stake**
   * DeSo moved from PoW to it's breakthrough [Revolution PoS](https://revolution.deso.com/) on **July 9th, 2024**
     * Revolution PoS brings many novel innovations including:
       * Content, Identity, Social Graphs, Finance and Assets fully on-chain & decentralized on a single Layer-1, with streamlined onboarding.
       * Support for fully on-chain Twitter-scale consumer apps at 500 posts per second, and thousands of DeFi transactions per second.
       *   <1/10,00th of a cent per post compared to \~$1 on Solana and $100+ on Ethereum.

           1 second confirmation times.
       * Data synced over a thousand permissionless validators with HyperSync, and secured via on-chain E2E-Encryption where needed.
       * Burn-Maximizing Fee (BMF) model built to minimize congestion, and increase value for coin-holders.
       * State-of-the art Fast-HotStuff consensus, the first of it's kind in production.
       * Permissionlessly run validators with no staking minimums, no slashing risks, with commodity hardware (when using min. requirements).
     * You learn learn more about Revolution via [revolution.deso.com ](https://revolution.deso.com)\

2. **Bigger blocks**
   * The average DeSo blockchain post size is **218 bytes**.\

   *   There are 10 other [transaction types](https://github.com/deso-protocol/core/blob/135c03a/lib/network.go#L239) besides `POST`, such as `LIKE` and `FOLLOW`. In a recent block, posts were about **1/3** of the total block size.\


       <img src="https://lh4.googleusercontent.com/YSLyEVtV0Ynx--mta7IP3QS5aVrZiq7MBVmIc9h9bZwbCrLXXTIIDzO2Gm9RYOjaqQONhOju-F7RvaTIVO6vWJ5AMASXIYHMI4z9sjK3acpoXOmhRHX99-35qS4I54KBl2C3zjnH" alt="" data-size="original">
   * The DeSo blockchain currently produces up to 2MB blocks every 5 minutes.\

   * So it can scale to \~30 transactions per second, **which is \~10 posts per second** (= 2e6 bytes/block / (218 bytes/post \* 60 seconds/minute \* 5 minutes/block) \* 1 post / 3 transactions).\

   * If we increase the block size to 16MB blocks every five minutes, we can roughly extrapolate that it scales to \~240 transactions per second = **\~80 posts per second**.\

   * For comparison, Twitter has approximately [6000 posts/second](https://www.dsayce.com/social-media/tweets-day/) on average with 300M users.\

   * So, at 80 posts per second, we should be able to roughly accommodate about 80/6000 = 1.33% of 300M users, or **4M users**.\

   * That’s where we can get with a basic block size increase alone. But we have a few other cards to play.\

3. **HyperSync**
   * With an Ethereum-like [warp or snap sync](https://blog.ethereum.org/2021/03/03/geth-v1-10-0/), we loosen a key constraint, which is the need for all nodes to always validate the entire history of transactions. (You can still run an archival node, but this won’t be necessary for normal operations.)
     * As a concrete example, if all you're downloading is the current creator coin balances for each user, then all that user's trades are effectively compressed into a few integers because you don't care about the history (only the end state).\

   * Instead, we move to a model where nodes by default first sync and validate a snapshot of the current blockchain state and then sync only a few week's worth of blocks on top of that.\

   * Generally, the bottleneck to blockchain performance is validation speed. With DeSo, we've run tests that indicate a node running on an Intel Xeon E-2276M can validate transactions at the rate of **\~12MB/s = 1.04TB/day = \~55,000 txns per second**.\

   * So, if we start by just downloading the state with minimal validation, how big can it be? To download 10TB at 10gbps = 10e12/10e9\*8/60/60 takes about \~2.2h. To download 100TB at 10gbps = 10e12/10e9\*8/60/60 is about **\~22.2h to download the state**.\

   * With warp sync, the block size can be increased beyond 16MB because the number of blocks required to get a node up-to-date can be reduced to only one week's worth of blocks rather than the entire history of blocks from the beginning of time.\

   * Let’s suppose then that using warp sync we increase the block size further to 120MB blocks. How many transactions per second (TPS) would 120MB blocks every 5 minutes facilitate? If we assume 218 bytes per transaction (which is what the value is per post), then (120e6 bytes / (5 minutes \* 60 seconds/min)) / (218 bytes/txn) = **\~1,800 transactions per second**.\

   * How much bandwidth would it take to synchronize one week of 120MB blocks appearing every 5 minutes, and how long would it take in wall clock time?\

     * Bandwidth would be roughly (120e6 bytes/block \* 1 block/5 minutes \* 60 minutes/hour \* 24 hours/day \* 7 days/week) / 1e9 bytes/GB = 238GB/ week\

     * At the aforementioned validation speed of 12MB/s on an Intel Xeon E-2276M, it would take approximately 238e9 bytes/week / (12e6 validated bytes / second \* 60 seconds/minute \* 60 minutes/hour) = **5.5-6 hours to download and validate one week of 120MB blocks** at a validation speed of 12MB/s on good hardware.\

   * Under these assumptions, This means that the warp upgrade can allow us to sync a node in \~5.5h while maintaining 1,800 transactions per second (TPS) long-term.\

     * 1,811 tps vs Twitter with 6,000 posts per second and 300M users (assume only posts, no likes)\

     * Users = 300M \* 1,811 / 6000 / 3 txns per post = **\~30M users.**\

4. **Sharding**
   * All transactions can then be write-sharded to make syncing a node parallelizable, thus providing multiple orders of magnitude in speedup.\
     \
     For example, we could do a very simple optimization, which is to shard all posts into their own sub-chain, and then shard other transactions across two of their own sub-chains.\

   * This would result in a node being capable of syncing 3x faster, meaning that we could support **\~90M users without an increase in sync time**.\

   * Ultimately, all transactions can be sharded into sub-chains by user ID, which would allow for virtually unlimited parallelization.\
     \
     For example, with thirty shards, we achieve another \~10x multiplier on the TPS without an increase in sync time, thus achieving **\~1 billion users**.\

   * This number can be scaled further by increasing the number of shards.
