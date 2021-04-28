# What is BitClout?

**BitClout is a new type of social network that lets you speculate on people and posts with real money, and it’s built from the ground up as its own custom blockchain.** Its architecture is similar to Bitcoin, only it can support complex social network data like posts, profiles, follows, speculation features, and much more at significantly higher throughput and scale. Like Bitcoin, BitClout is a fully open-source project and there is no company behind it-- it’s just coins and code.

## Buying BitClout

The BitClout blockchain has its own native cryptocurrency, called BitClout, that you can use to do all kinds of things on the platform, including buy a new type of asset called [“creator coins,” discussed below.](./#what-are-creator-coins)

Anyone can buy the BitClout cryptocurrency with Bitcoin in minutes through the app’s built-in decentralized “atomic swap” mechanism, available on [the “Buy BitClout” page](https://bitclout.com/buy-bitclout). The price of BitClout doubles for every million BitClout sold. This makes BitClout naturally scarce, resulting in 10 to 19 million BitClout minted in the long run \(less than Bitcoin’s max supply of 21 million\).

## What are Creator Coins?

### Everyone Has a Coin

Every profile on the platform gets its own coin that anybody can buy and sell. We call these coins “creator coins,” and you can have your own coin too simply by creating a profile. The price of each coin goes up when people buy and goes down when people sell.

### You Can Buy Your Favorite Person’s Coin

To buy someone’s coin, you simply navigate to their profile and hit “Buy.” You can find someone’s profile either by searching for it or by visiting the creator coin leaderboard \(shown below\). Profiles for the top 15,000 influencers from Twitter have been pre-loaded into the platform, which means you can buy and sell their coins even though they're not on the platform yet. These “reserved” profiles have a “clock” icon next to their names, indicating the owner of the profile has not joined yet.

![](.gitbook/assets/image.png)

### Tweet to Claim Your Profile

The owner of a reserved profile can claim their profile by navigating to their profile and hitting a button to Tweet their BitClout public key \(shown below\). When they do this, they gain full access to the account, as well as a percentage of the creator coins associated with their profile \(see [Founder Rewards](./#founder-rewards)\). Only the owner of the Twitter account associated with a reserved profile can claim it.

![](.gitbook/assets/image%20%281%29.png)

### What Are Creator Coins Useful For?

Creator coins are a new type of asset class that is tied to the reputation of an individual, rather than to a company or commodity. They are truly the first tool we have as a society to trade “social clout” as an asset. If people understand this, then the value of someone’s coin should be correlated to that person’s standing in society. For example, if Elon Musk succeeds in landing the first person on Mars, his coin price should theoretically go up. And if, in contrast, he makes a racial slur during a press conference, his coin price should theoretically go down. Thus, people who believe in someone’s potential can buy their coin and succeed with them financially when that person realizes their potential. And traders can make money buying and selling the ups and downs.

The above being said, there are many other exciting opportunities for creator coins that we hope will be integrated in the very near future:

#### **The Stakeholder Meeting**

A creator can make it so that only people who own a certain amount of their coin can participate in the comments section of their posts. This forces anyone who wants to have a voice in that creator’s content to first align themselves with the creator by buying their coin. The alignment not only reduces spam significantly, but it could bias conversations to be significantly more positive than on existing platforms. It would also create a lot of demand for one’s coin-- can you imagine if Elon Musk or Chamath did an AMA with a minimum threshold for buying their coin in order to participate? Or if they answered questions in order of coin holdings?

#### Premium Messages

Most creators get a torrent of spam in their message inbox on social media. With BitClout they could make it so that only people who own a certain amount of their coin can message them, or they could simply rank and prioritize messages from the largest holders of their coin. Alternatively, they can make it so that a certain amount of their coin must be paid to them directly in order for the message to actually enter their inbox. All of this would increase demand for their coin while helping to minimize spam for the creator.

#### Sponsored Posts

Creators can have an “inbox” where anyone can “bid” to have them repost \(aka “retweet”\) a particular post. If you want Kim Kardashian to retweet your fashion brand, you can submit an entry into her inbox, and if she retweets it then she keeps your money. The bids can all be made using the creator’s own coin, thus significantly increasing the demand for the coin.

#### Premium Content

People who own a certain amount of a creator’s coin get access to special content. Or, alternatively, people must pay a monthly subscription in the form of the creator’s coin in order to get premium content.

#### Distributions and Engagement 

Creators can also use their coins to distribute scarce resources to the largest holders of their coins. For example, imagine if a famous celebrity offered to have lunch with whoever held the most of their coin at a particular date. Or imagine if they were going to offer 1,000 signed posters to their 1,000 largest holders. This is just the beginning of how creators can engage with their fans using their coins, and all such ideas could increase demand for their coin significantly.

#### Money Likes

Likes can be re-imagined as purchases of the creator’s coin. So it costs money to like something, but you get that person’s coin when you do so \(effectively as a shortcut to buying their coin that’s associated directly with their content\). Such a feature could serve as a stronger signal on what content is high quality as well.

#### Emergent Phenomena

What can happen when you give people the ability to speculate on a person’s reputation? We can’t know for sure, but one of the features that has emerged is what we call “buy and retweet.” Ordinarily, retweeting someone gives you nothing. If that person becomes a superstar because you boosted them, you’ll be lucky if they even remember your name in a few years. In contrast, with BitClout you can buy someone’s coin and then retweet them, which makes it so that you’re not only along for the ride financially if they blow up, but you also get bragging rights. Imagine the difference between being able to say “I retweeted her early on” vs being able to say “I bought her coin when it was $0.50 and now it’s $500-- and by the way I’ve done this hundreds of times, and I can prove it because my track record is on the blockchain.” The latter is clearly a very different game. Moreover, it’s not just a famous person’s game. If you know someone with a lot of clout, or if you know someone who knows someone, you can buy a coin and send it to someone else so that they can buy and retweet them. And thus the incentives go many layers deep. The interesting thing about this mechanic is that it wasn’t even something consciously designed into the product. It exists as an “emergent” phenomenon off of the core creator coin mechanic. What other dynamics could exist that we haven’t yet thought of?

### The Creator Coin Supply Curve

Creator coins are naturally scarce, with generally fewer than 100 to 1,500 coins in existence for each profile. This is because as more people buy a profile’s creator coin, the price of the coin goes up automatically at a faster and faster rate. This means that, eventually, it would take billions of dollars to mint even one more coin.

The formula or “curve” for determining the price of a creator’s coin is as follows. Note that creator coins are normally bought and sold with the BitClout cryptocurrency, but we provide a dollar version of the formula for easy calculating:

$$
price\_in\_bitclout = .003 \times creator\_coins\_in\_circulation^2
\\
price\_in\_usd = .003 \times creator\_coins\_in\_circulation^2 \times bitclout\_price\_in\_usd
$$

When you create a profile, there are initially zero coins in existence and thus the price is zero. If you want to buy coins from the profile, it will happily mint them out of thin air and sell them to you according to the price curve above, making it more and more expensive as more coins are purchased. The money you use to buy the coins gets “locked” in the profile in exchange for the coins. On the flipside, if you want to sell coins, the profile will happily buy them from you according to the curve using the money locked from previous buys. And so buying **creates** coins while pushing the price **up** and **locking** money into the profile, while selling **destroys** coins while pushing the price **down** and **unlocking** money from the profile. This is often referred to as an “automated market-maker,” and it’s the same concept that powers protocols like Uniswap and Bancor.

Below is a graph of what the creator coin price curve looks like as a function of how many creator coins are in circulation for a given profile. We also include a table that shows some of these values. Both of these assume a BitClout price of $16. Note also that “integrating” the price curve yields the amount of money “locked” in a profile, which is equal to the “net” amount of money that has flowed into that particular creator coin \(included as the third column of the table\). If you’d like to play with the numbers yourself, you can do so using [this sheet](https://docs.google.com/spreadsheets/d/1zBEQBBoS12ZhFpPbB13-GTZ8keDVlstRG3l2If78pWM/edit?usp=sharing) \(make a copy to edit it\). You can also learn more about bonding curves [here](https://yos.io/2018/11/10/bonding-curves).

|  **Creator Coins in Circulation** |  **Creator Coin Price \(USD\)** |  **USD Locked in Profile** |
| :--- | :--- | :--- |
|  5 |  $1.20 |  $2 |
|  10 |  $4.80 |  $16 |
|  20 |  $19.20 |  $128 |
|  40 |  $76.80 |  $1,024 |
|  80 |  $307.20 |  $8,192 |
|  160 |  $1,228.80 |  $65,536 |
|  320 |  $4,915.20 |  $524,288 |
|  640 |  $19,660.80 |  $4,194,304 |
|  1280 |  $78,643.20 |  $33,554,432 |

![](.gitbook/assets/image%20%282%29.png)

### Founder Rewards

Every profile allows the creator to keep a certain percentage of the coins that are created as a “founder reward.” For example, if someone sets their founder reward percentage to 10% and then someone buys 100 BitClout of their coin, then 10 BitClout would be used to buy the creator’s coin, and those coins would go to the creator’s wallet rather than the purchaser’s.

The above being said, we think the better way for creators to own a piece of the upside of their coin is simply to buy their coin up-front when they create their profile, and then set their founder reward percentage to zero. This works because the coins are cheapest at the beginning of the curve, and it has the upshot of reducing friction on subsequent purchases of their coin. Nevertheless, the founder reward percentage being 10% is a “sane default” that guarantees creators will maintain a certain percentage of their coin even if they do nothing.

## The Power of Decentralization

Just like Bitcoin, anyone on the internet can run a BitClout “node” that serves the BitClout content, and every node on the network stores a full copy of all the data. This means that anybody can build apps on top of the BitClout data without the risk of being de-platformed, and they can even create their own feed algorithm. When you visit bitclout.com, you’re using our node, but there are already dozens of nodes on the network, all run by people like you. In the long run there will likely be hundreds or thousands of other nodes you could visit to get access to the same content, each with their own moderation policy. It also means that your login on bitclout.com can be used on any domain that runs a BitClout node. In the same way that you can move Bitcoin from one wallet to another, BitClout makes it so that you can move your “clout” in the form of your followers, posts, creator coin balances, etc anywhere as well. Thus, in some sense, BitClout is decentralizing social media in much the same way as Bitcoin is decentralizing the financial system.

