# AI-Generating Your Code

Although the [DeSo Python SDK](https://github.com/deso-protocol/deso-python-sdk) has many useful features, sometimes it's easiest to create your own transaction flow from observing another app (such as Openfund or Focus). This section teaches you how to do that, and how to use AI to auto-generate your code:

1. Navigate to the "DAO Coin" tab on node.deso.org for Openfund here. Remember that "DAO Coin" is just an old term for "DeSo Token"!
2. Open the web inspector on your browser and navigate to the Network tab.
3. Filter the requests to get-hodlers-for-public-key. Click "Copy as CURL" to get the params used by the request. In addition, note "Copy Response" as we'll be using that too.
4. With the sdk loaded into your favorite AI tool, paste the result of "Copy as CURL" and the result of "Copy Response" into the chat, and ask it to add a function to get all the holders for a given token.
5. CONGRATS! You've just added a NEW FUNCTION to the sdk! You can use this process to automate anything you do on openfund.com, focus.xyz, and any DeSo app!

PS: We may have used this exact method to write some of the [DeSo Python SDK](https://github.com/deso-protocol/deso-python-sdk). Ssshh don't tell anyone! :smiling\_face\_with\_tear:
