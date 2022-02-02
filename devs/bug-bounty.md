# Bug Bounty

We want the DeSo blockchain to be one of the most secure blockchains in the world, but we need your help. If you're skilled at finding exploits and security vulnerabilities, **please email node.admin+security@protonmail.com so we can share some resources with you and include you in our first bug bounty.**‌

## Rewards <a id="rewards"></a>

Hundreds of thousands of users have accounts on the DeSo blockchain, and this section covers the rewards for exploits that compromise the security of those users. Please note that what's in scope may change over time, so **please always check this page for the most up-to-date list of things that are a part of the program.**‌

Below is a list of severity levels and reward amounts for each level, as well as examples of exploits at each level. Please note that all reward amounts are ultimately subject to the discretion of the DeSo developer community, and the example exploits below are just a guide. The actual reward amounts may vary depending on the nature of the exploit, but we will try to be as generous as possible. All rewards are payable in Bitcoin or DeSo.‌

* **Critical: Up to $75,000 USD**
  * Exploit: You can acquire the seed phrases of users using DeSo Identity Service without them having to perform any special action on an external site.
    * The maximum amount will be rewarded if this exploit can impact all currently active users.
    * Example: a post containing an XSS attack that extracts seed phrases for all users who load the post in their browser.
  * Exploit: You are able to print money.
    * The maximum amount will be rewarded if this exploit can result in the actual blockchain becoming corrupted. Corrupting the mempool is a lower-severity issue. In order to be considered critical, the exploit must result in the state of the actual blockchain being corrupted.
    * Example: you create a transaction that prints DeSo and mines into the chain.
    * Example: you buy and sell a creator coin in a way that prints DeSo
* **High: Up to $35,000 USD**
  * Exploit: You can steal money from users using DeSo Identity Service without them having to perform any special action external to the site. However, you are unable to compromise the seed phrases of these users.
    * The maximum amount will be rewarded if this exploit can impact all currently active users.
    * Example: a post containing an XSS attack that sends all of a user's money to your wallet when they view the post in their browser.
    * The reason this is a "High" severity issue rather than a "Critical" severity issue is because an exploit like this could theoretically be reverted by a hard fork, similar to what Ethereum devs implemented during the DAO hack.
  * Exploit: You are able to steal money from users, but they have to perform some trivial action on another website.
    * This will receive a lower amount than an exploit that requires no activity on an external site.
    * Example: a user presses a button on evil.com that steals all of the user's funds.
  * Exploit: You can bring the entire DeSo network down with minimal resources.
    * Example: a corrupt transaction causes all machines that receive it to crash-loop repeatedly.
* **Medium: Up to $10,000 USD**
  * Exploit: You can cause DeSo nodes to lose at least one hour of transactions.
    * The maximum amount will be rewarded if this exploit can be executed with minimal resources. Attacks that require a lot of resources, like the possession of a botnet, are ineligible.
    * Example: submitting a simple corrupt transaction to the mempool that causes all transactions to become invalid.
  * Exploit: You are able to perform low-value actions \(post, like, etc\) on behalf of users on the site.
* **Low: Up to $1,000 USD**
  * Exploit: You can cause weird and confusing behavior that is mostly harmless to the blockchain.
  * * Example: [The Salomon bug](https://diamondapp.com/u/salomon), which causes prices to display incorrectly but doesn't allow users to print DeSo or creator coins.

## Claiming a Bounty <a id="claiming-a-bounty"></a>

In order to be eligible to receive a bounty you must fully disclose an exploit to the DeSo developer community by emailing **node.admin+security@protonmail.com** Below are some guidelines for disclosing your exploit:‌

* You must be the first person to report an issue. If you are not the first person to report an issue the developer community may award a consolation prize at its discretion.
  * Additionally, if you report an issue that the developer community is already aware of or is actively fixing, then you are not eligible to receive a bounty.
* You must include clear steps to reproduce the issue and be willing to answer any questions the developer community has regarding your exploit. This is required in order to fix the exploit and protect DeSo users as quickly as possible.
  * If your exploit cannot be reproduced or you cannot provide some evidence that the exploit is real, you will not be eligible for a bounty.
* You must allow 60 days for the developer community to evaluate and fix whatever you've discovered _before_ disclosing the exploit publicly.
  * Disclosing your exploit publicly in less than 60 days could not only result in harm to users, but it will also make you ineligible for the bounty.
* Once your exploit is verified you will be asked for a DeSo or Bitcoin address to receive payment.

We understand that the disclosure process described above requires you to put some faith in the judgement of the DeSo developer community. For some reports it may not be clear how severe the exploit is or how much of a reward the reporter should receive. In these instances, we ask that you are patient with us as we refine the scope of exploits we're looking for and trust that we will be generous when an exploit is in a grey area.

