# Creating DeSo Testnet Accounts

First, if you’re going to be writing a trading bot, it is best to familiarize yourself with the DeSo node testnet UI, node [accessible here](https://test.deso.org/), and the Openfund testnet UI, [accessible here](https://dev.openfund.com/trade). This will allow you to do everything with “fake money” so that you don’t put capital at risk until you’re sure everything is working properly.

To set up a testnet account, simply execute the following steps:

1. Visit [https://test.deso.org](https://test.deso.org)
   1. This is the reference node for testnet that most developers get started on when testing. Note that anyone can run a DeSo node by following the instructions in [the core repo](https://github.com/deso-protocol/core), this just happens to be a fairly reliable one.
2. Create an account
   1. Note that DeSo wallets are managed locally in your browser. The DeSo wallet works almost exactly like MetaMask, only it doesn’t require the installation of a Chrome extension, and it supports much more granular and transparent permissions at an app level. This means that creating a wallet on one app results in that wallet being accessible to any other DeSo app as long as permissions are confirmed, including [Openfund](https://openfund.com), [Diamond](https://diamondapp.com), and [Focus](https://focus.xyz) (noting these are mainnet links, not testnet ones, so your testnet wallet won’t be accessible there).
      1. Note that when you use an app, a “derived key” is issued that has much more limited permissions than your master key, which you have to accept before using the app. This allows you to use an app without having to hit “confirm” on low-value transactions (e.g. making posts or liking posts) while being 100% sure the app can’t steal funds.
   2. We recommend always using seed phrases for test accounts, as that tends to be easier to manage. You can use one seed phrase and create new accounts with different indexes by hitting “add account” in the wallet.
3. When creating an account you can enter your phone number to get 1 free testnet $DESO from the faucet
   1. Note that testnet $DESO does not have any value outside of the test environment!
   2. If you don’t want to enter your phone number, some apps use the advanced captcha flow, such as [dev.openfund.com](http://dev.openfund.com). Eventually this flow will be integrated back into the reference node.
4. Once you have an account with starter DESO, you can visit the following links to test things:
   1. Testnet explorer: [https://explorer-testnet.deso.com](https://explorer-testnet.deso.com)&#x20;
      1. Here, you can login and stake your DESO if you want.
   2. Testnet Openfund: [https://dev.openfund.com/trade](https://dev.openfund.com/trade)&#x20;
      1. Here, you can place orders and use the inspector to see what transactions are being constructed and submitted to the DeSo blockchain.

For a reference on all useful testnet links, including the testnet block explorer, [see here](https://docs.deso.org/deso-validators/run-a-validator#h.gcb427f1q4hl).

For tips on creating lots of **mainnet** accounts with starter DESO in them, [see here](https://docs.deso.org/deso-tutorial-build-apps#tip-for-creating-lots-of-test-accounts).

For a primer on building DeSo apps, [see here](https://docs.deso.org/deso-applications). Useful as a reference, or if you find something confusing in this guide.

\
