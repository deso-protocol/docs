# Market-Making Bots

Use the [DeSo Python SDK](https://github.com/deso-protocol/deso-python-sdk) to complete the following Challenge Exercises, and build a fully-functioning market-making bot:

1. Get the market mid-price of $openfund on the openfund/deso market by using the get\_limit\_orders function. Beware of ASKs that look like BIDs, and vice versa!
2. Place a market order to buy 0.000001 DESO worth of $openfund. You should be able to do this with just your starter DESO.
3. Check your $openfund balance after doing the market order to confirm that you have the amount of $openfund that you expect to have.
4. Place a LIMIT order to BUY $openfund just below the market mid price, and a LIMIT order to SELL $openfund just above the market mid price. You should be able to do this now that you have both $openfund and $DESO from the previous step! The orders should "rest" on the book, without executing immediately. Save the order\_id from the transaction so you can manage the state of your order! The order\_id is simply the signed txn hash of the transaction you used to place the order.
5. Use get\_limit\_orders to tell if your order has been filled or not. An order will be filled when it no longer appears on the book.
6. Practice cancelling and replacing one of your orders using the sdk, and passing the order\_id from when you placed the order.
7. Write a simple routine to "flip" your buy into a sell once it's been filled (at a slightly higher price so you earn a "spread"). Do the same for your other limit order.
8. ADVANCED: Acquire $100 worth of $openfund and $100 worth of $DESO. Place 10 bids and 10 asks for $10 each around the market mid using an ATOMIC transaction.
9. ADVANCED: Write a routine to "flip" your asks to bids when they're filled (with a spread so you make some money on the volatility!). Do the same for your bids.
10. CONGRATS! If you made it this far, you are officially a market-maker on the DeSo DEX! The AMMs that power Focus and Openfund are essentially a highly-sophisticated and scaled-up version of what you just did.
