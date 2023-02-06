# 3⃣ Node: Staying Up-To-Date

This is a step-by-step guide on how to stay up to date with hardforks of the core repository, as well as instructions for core team members on how to execute updates that could cause a hard fork on the network.\
\
This doc should help node operators and the core team release new code in an organized way, particularly so that the developer community has sufficient time and resources to upgrade their nodes.

## Instructions for Node Operators <a href="#_cv2t10tt14ya" id="_cv2t10tt14ya"></a>

1. Subscribe to release notifications on GitHub
   * Which repos?
     * [https://github.com/deso-protocol/core](https://github.com/deso-protocol/core) (important)
     * [https://github.com/deso-protocol/backend](https://github.com/deso-protocol/backend) (important)
     * [https://github.com/deso-protocol/rosetta-deso](https://github.com/deso-protocol/rosetta-deso) (important for exchanges like Coinbase)
     * [https://github.com/deso-protocol/identity](https://github.com/deso-protocol/identity)
     * [https://github.com/deso-protocol/frontend](https://github.com/deso-protocol/frontend)
   * How to subscribe
     1. Click “Watch” at the top
     2. Select “Custom”
     3. Check “Releases”\

2. Whenever a major version update occurs, e.g. moving from 2.9.9 to 3.0.0, be sure to read the release notes and reboot your node within the next week to avoid issues
   * Note that occasionally a resync will be required, which could cause \~20 minutes of downtime unless done in parallel with a second running node. Be sure to read the release instructions to avoid any interruptions.\

3. Follow [@deso](https://diamondapp.com/u/deso) and [@nader](https://diamondapp.com/u/nader) on [node.deso.org](https://node.deso.org/) or [DiamondApp](https://diamondapp.com/) for on-chain announcements and discussion\

4. Follow [@desoprotocol](https://twitter.com/desoprotocol) and [@nadertheory](https://twitter.com/nadertheory) on Twitter

## Instructions for Core Team <a href="#_9wdvp7tesgvk" id="_9wdvp7tesgvk"></a>

### Code preparation <a href="#_2tidf3tg7ql4" id="_2tidf3tg7ql4"></a>

1. Gate all the forking changes by a ForkHeight which initially should be set to `math.MaxUint32` for both mainnet and testnet, and 0 for regtest.\

2. Verify that the code passes all basic tests:
   1. core unit tests
   2. core integration tests
   3. node syncs with hypersync + txindex on testnet and mainnet
   4. node syncs with blocksync + txindex on testnet and mainnet
   5. rosetta can sync mainnet
   6. repeat steps c. and d. for postgres
   7. test that all of the changes work in the reference frontend
   8. make sure to run more comprehensive tests when deploying a complex change\

3. Assuming you’re ready with all the content for the steps in the next sections, you should announce everything at 12:00 PM PT and set the testnet fork height to 1 day from the announcement, and the mainnet fork height to **a minimum of** 7 days from the announcement.
   * We will strive to give as much time for node operators to upgrade as possible, erring on the side of two weeks.\
     \
     However, in the interest of moving quickly, it will sometimes be necessary to make updates with less warning, though generally with no less than one week of warning.

### Release preparation <a href="#_nkce44tidl7s" id="_nkce44tidl7s"></a>

1. Come up with a description of what happens in the fork. Make it a couple sentences, or a couple paragraphs depending on the scope of the change. See what fits based on next steps.\

2. Draft a Fork Preparation Checklist document which is a step-by-step checklist/walkthrough of what node operators need to do in order to upgrade their nodes (inspired by [prism’s merge preparation checklist](https://docs.prylabs.network/docs/prepare-for-merge))\

3. (Optionally) Write a before and after table which describes what happens in the fork. The left column should describe different aspects of the “before” tech and the right shows how they updated / changed “after” the fork. (also [prism’s merge preparation docs](https://docs.prylabs.network/docs/prepare-for-merge#the-merge-before-and-now))\

4. Put all of the above in a new draft release on github, similar to how [consensys does it with teku](https://github.com/ConsenSys/teku/releases), or how [ethereum does it](https://github.com/ethereum/go-ethereum/releases)\

5. Assuming the release number is XX.YY.ZZ (major.minor.patch):\

   * You should increment the major XX
     * \-> if the node operators will be required to upgrade their software within a fixed amount of time, otherwise risking ending up on a stale fork.\

   * You should increment the minor YY
     * \-> if the node operators can do nothing and still properly participate in the network. The code augments the node in a backwards-compatible fashion.\

   * You should increment the patch ZZ
     * \-> If the update fixes a bug in a backwards-compatible fashion.\

6. Cut the new release

### Announcement <a href="#_persa8n1a2dg" id="_persa8n1a2dg"></a>

1. Create twitter announcement for the fork
2. Create diamond announcement for the fork
3. Create discord announcement for the fork
