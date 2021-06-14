# Running a Node

## The Power of Decentralization

BitClout is unlike any existing social network in that the data is fully-decentralized and stored on a blockchain like Bitcoin. **This means that anyone on the internet can run a BitClout "node" and download a** _**full copy**_ **of all the data, with real-time updates, without needing to ask for permission and without the risk of being de-platformed.**

## What Can You Do With a Node?

Running a node gives you full access to the BitClout firehose. Access to every profile, post, follow, creator coin trade, etc... But what can you do with all this power?

### **Running your own feed**

When you run a node, it starts with a blank global feed and an "Admin" panel that you can use to start adding posts to it. **All of the same tools that the bitclout.com team uses to manage their global feed are now available to you to manage a feed of your own.**

Essentially, running a BitClout node allows you to expose your own "view" of the firehose of content. Our bitclout.com node exposes all of the crypto-related content, but when you run your own node you have full control to surface whatever content speaks to you. What will you do with your feed? Here are some ideas for feeds that we think would be popular:

* **A feed for every country and every language.** Isn't it weird that people all over the world consume information curated predominantly by the US? How much does an engineer working in Silicon Valley really know about what people in other countries want to see, or what features they want? In the past, we were stuck with this model because US companies built a data network effect that entrenched them, even in non-US countries. But BitClout can break this status quo because all of its data is open and the barrier to entry to starting a competitive feed is virtually zero. For the first time, people who actually live in a country can curate a feed for their people, no matter how large or small their country is. And this applies to every country that succumbed too quickly to the network effects of the Silicon Valley tech companies. By lowering the barrier to entry to creating a feed, and opening up the data firehose to anyone, we think BitClout has the potential to bring international social media products to a whole new level.
* **The politics-focused feed.** bitclout.com doesn't prioritize political content, but someone should. Imagine a feed where all the posts from the top political figures are highlighted. You could even imagine segmenting the firehose into two feeds: a "red" feed and a "blue" feed that's dedicated to each political party.
* **The sports-focused feed.** Wouldn't it make sense for someone to operate a feed that just highlights all of the best sports content from the best sports influencers? So many people are interested in this content, and we think it deserves its own feed.
* **The NSFW feed.** bitclout.com doesn't prioritize NSFW content in its feed because its user-base is too mainstream. This bias against NSFW content is even stronger with incumbent social media companies. Yet there are so many talented adult influencers on BitClout, with thousands of followers, who are posting every day. It's about time they had their own feed dedicated to them.

We're just scratching the surface here-- it's now up to you, the community, to figure out how best to display the BitClout firehose. Reddit pioneered the concept of a "Subreddit," but the problem with a Subreddit is that every time one is created, it has to solve a "chicken and egg" problem with regard to its content. If nobody is posting, then the subreddit has no content-- but without content, nobody will start posting. BitClout bascially takes the subreddit concept to the next level by solving the "content" part of the equation for everyone. When you run a node, you don't need to bootstrap content because you have full access to the BitClout firehose. All you need to do is curate it in some interesting way and you'll have created value for anyone who visits your node.

### Add social to your platform

Suppose you're a platform with millions of users like Coinbase or Robinhood, or even traditional media companies like ESPN. Your users would probably love it if you could integrate a social component into your products-- but you can't because Twitter and Facebook don't allow it. They [closed down their API's](https://www.google.com/url?q=https://www.theverge.com/2018/8/16/17699626/twitter-third-party-apps-streaming-api-deprecation&sa=D&source=editors&ust=1618805849171000&usg=AOvVaw3zPqxFdHzfISpMoiBG_Yi0) a long time ago because they realized that third-party integrations eat into their ad revenue. Every user who engages on a third-party platform is a user who's engaging less on Twitter and Facebook.

Enter BitClout. With BitClout, you don't need to build a billion-user data moat in order to be able to add social features to your platform. All you need to do is run a BitClout node, and use its API to expose whatever content you want. Suddenly, with just one engineer's worth of effort, any major platform can spin up a social product that's adjacent to its core business. Moreover, it's possible that the best feeds will come from existing publishers that have already built a competency in a particular area. For example, ESPN might be the best entity to run the sports-focused feed because of their relationships and connections, and now they can.

### Analysis tools

Building the best analysis tools requires access to the best data, and running a node is the best way to get full access to the BitClout firehose. Until now, the bitclout.com nodes have had to set up rate limits to avoid having our machines get overloaded. But now, because BitClout is a blockchain that allows anyone to run a full copy of the platform, anyone who wants to build analytics tools can simply run a node and query it in whatever way they want.

### Invent your own features

When you run a node, you have the flexibility to expose the BitClout content in whatever way most resonates with your users. If you wanted to, you could even build a whole new frontend with totally different features than what the "default" node gives you. If you feel like BitClout is missing a feature, like dark mode or paid messages or better filtering for spam for example, now you can build it and run your own node to back it.

## Making Money on Your Node

Incentives are key to making BitClout truly decentralized in the long run. It's not sufficient that nodes be runnable by the community, they must be _profitable_ to run as well. Many cryptocurrencies struggle with this, and even Bitcoin and Ethereum nodes are still largely run by volunteers. BitClout is truly unique in this regard, however, because the social features it introduces give node operators incentives that other blockchains don't have.

The above being said, there are several ways that BitClout node operators can earn a profit:

* **Promoted content.** Because running a node comes with the ability to have a social media product with minimal marginal effort, every node operator has an opportunity to amass and monetize the reach that comes from curating a popular feed. This can be as simple as showing promoted posts that partners pay the node operator to pin to their feed.
* **Trading fees.** Anyone who runs a node can modify their frontend to add trading fees on every creator coin trade, which go to the node operator's wallet. By doing this, any node operator basically doubles as a crypto exchange.
* **Other transaction fees.** Any transaction users complete on one's node can be augmented to contain a small fee that goes to the node operator. Thus there should eventually arise an efficient market for node operator fees that is high enough to justify operating a node.

The above mechanisms don't even factor in profits that could be derived from augmenting the BitClout feature set. For example, if someone creates an app experience for BitClout that is significantly better than alternatives, they could even charge a monthly subscription fee or some other premium to cover costs.

## How to Run a Node

Running a node currently requires a modest amount of technical know-how. For the full instructions on how to run a node, check out this GitHub repository:

* [https://github.com/bitclout/run](https://github.com/bitclout/run)

Once a node is running, it syncs all of the blocks from its peers, as well as the transactions in the "mempool," which have yet to be mined into a block. Every node comes with an Admin panel with a Network tab that allows you to monitor the node's sync state.

![](../.gitbook/assets/image%20%287%29%20%281%29%20%282%29.png)

Once your node is synced, you have access to the full firehose of BitClout data in real time! Below are some tips on how take full advantage of your node.

* Go to your Admin tab and watch the unfiltered feed update as your node syncs. It's like a time machine!
* Try to whitelist some posts in the Admin tab and see that they've made their way onto your global feed.
* Read through the flags available in the [dev.env](https://github.com/bitclout/run/blob/main/dev.env) file. You can adjust these flags however you want, but note that we strongly recommend keeping your node in read-only mode for now. Turning read-only mode off could cause users who visit your node to make transactions that are not ultimately confirmed.
* Set `ADMIN_PUBLIC_KEYS` to your public key so that the Admin tab is only visible to your username.
* Whitelist some posts and verify that they show up on the global feed.
* Deploy your node on any cloud provider with a static IP to make it accessible to anyone on the internet.
* Set a `PASSWORDS_FILE` if you want to restrict read access to your node.
* Add an `SSL_CERT_DIR` and `SSL_DOMAIN` using a letsencrypt cert in order to protect your node with HTTPS.
* Set the `TWILIO*` flags to allow new users to get some starter BitClout.
* Set a `SUPPORT_EMAIL` so your users can contact you if they run into trouble.
* Play with the logging verbosity by increasing `GLOG_V`.

## Managing Your Feed

To manage your feed, start by navigating to the Admin tab as shown below. The Admin tab shows the full firehose of posts in real time, with a button next to each one that allows you to add it to the global feed. You can also sort the posts by clout. These are all the same tools that the bitclout.com mods have, now at your fingertips through the power of decentralization.

![](../.gitbook/assets/image%20%288%29%20%281%29%20%282%29.png)

You can also add any post from anyone's profile to the global feed simply by hitting the dropdown at the top-right of the post. You can also pin posts to your feed, which is a good way of communicating announcements to your user-base.

![](../.gitbook/assets/image%20%283%29.png)

When you run a node, you act as a moderator and have a variety of superpowers that help you manage spam and harmful content.

* **Blacklisting** a profile removes it everywhere except from peoples' wallet pages. This makes it so that anyone who was holding the blacklisted profile can sell out of their holdings.
* **Graylisting** a profile removes it from the leaderboard, removes it from search, removes its comments from threads, and removes its posts from the Admin panel.
* **Whitelisting** a profile makes that user's posts show up on the global feed automatically with some frequency \(currently it allows five posts per day\).
* Finally, a mod can allow a phone number to be re-used to claim starter BitClout. This is useful for various testing situations.

![](../.gitbook/assets/image%20%284%29.png)

When you've set your public key as an `ADMIN_PUBLIC_KEY`, the Admin tab becomes visible only to you. This is a critical step in securing your node. Not doing this would make it so that all your users can add posts to the global feed.

## How Users Login

When a user logs in on your node, they have the ability to sign in with their BitClout identity, without having to re-enter their seed phrase. Once a user signs in, your node can sign transactions on their behalf with varying levels of approval required depending on what kind of permission the user granted. This creates a login mechanism for node operators that is as easy for users as "login with Facebook," but it unlocks a wallet in addition to a user's identity.

## FAQ

Answers to common questions and issues about running your own node:

### What are the minimum requirements for syncing a node?

We recommend having a machine with at least 32GB of RAM and 50GB of storage.

### How do I configure SSL?

There is an example SSL configuration in `nginx.dev`.

### How do I use the BlockCypher API?

BlockCypher will help prevent double-spends in the mempool. You can signup for a [BlockCypher](https://www.blockcypher.com/) account on the BlockCypher website. BlockCypher does offer a free amount of API calls.

Once you have signed up for an account you may copy a token from the [tokens](https://accounts.blockcypher.com/tokens) section of the dashboard.

You will copy this token in your `dev.env` file as the value for `BLOCK_CYPHER_API_KEY`.

### What type of records do I use with custom domains?

You must create two seperate **A** type domain records.

Both records should point to the IP address of your node.

#### Example DNS Records:

| Hostname | Type | TTL | Priority | Content |
| :--- | :--- | :--- | :--- | :--- |
| node.`DOMAIN`.com | A | 299 |  | `IPADDRESS` |
| api.`DOMAIN`.com | A | 299 |  | `IPADDRESS` |

If you do not create both records you will be unable to use a custom domain.

### Can my node write back to the mainnet?

Yes! Every transaction is broadcast to all other nodes on the network, and should eventually be mined into a block.

### What does Twilio provide to my node?

Twilio provides an SMS API that allows you to confirm user phone numbers and thus send them currency from your seed wallet set inside the `dev.env` file. If you do not have this set users will be unable to verify a phone number.

Twilio pricing can be reviewed [here](https://www.twilio.com/sms/pricing/us).

