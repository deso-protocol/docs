# Developer Resources

Here is a list of libraries, examples, SDKs and other resources useful to those developing on the DeSo Blockchain.

## Important

These are unofficial packages and examples provided by the user community and therefore have not been checked for bugs, issues, or security risks.

So use them as examples and Do Your Own Research before running these.

## Opensource Front ends & Extensions

* [Diamond App](https://github.com/diamond-app/frontend)
* [Bitclout](https://github.com/deso-protocol/frontend)
* [CloutFeed](https://github.com/CloutFeed/mobileApp)
* [Plus+ Extension](https://github.com/iPaulPro/BitCloutPlus)
* [CloutCLI](https://github.com/andrewarrow/cloutcli)
* [Netlify Hosted Frontend](https://github.com/DeSoDev/BitFlare)

## SDKs / Libraries 

* [Unity 3D Package](https://github.com/Desonity/Desonity)

    - Supports signing transactions in C# using seed hex and derived keys
    - Supports Identity login in Unity apps through [flask backend](https://github.com/Desonity/Backend-Flask)
    - Supports buying/selling CC/DAO
    - Supports deso backend api functions
    - Supports image uploads to images.deso.org

* [Python Package](https://pypi.org/project/deso/) 

    - Supports all DeSo Backend api functions
    - Upload image to images.bitclout.org
    - Upload image to [Arweave](https://arweave.org)

* [Dart SDK](https://github.com/DeverseSocial/deso_sdk) 

    - Supports all DeSo Backend api functions

* [Typescript SDK](https://github.com/bitclouthunt/bitclout-sdk):

    - Get Exchange Rate
    - Get App State
    - Get coin holders
    - Get single user profile by pubkey or username
    - Get information about 1 or more users
    - Get a users followers
    - Get notifications
    - Get transaction
    - Submit transaction
    - Identity service

* [Cloudflare Workers DESO Embed Code](https://github.com/DeSoDev/EmbeDeSo) - Enables DESO urls from showing good embed info on twitter, discord, and in google search.

## Code examples

* Use Identity login:

    - [Javascript](https://github.com/mubashariqbal/login-with-bitclout)
    - [React](https://github.com/BogdanDidenko/react-bitclout-login) 
    - [Svelte](https://github.com/mvanhalen/bitclout-identity-window-svelte) 
    - [Javascript](https://github.com/PrismWeb3/deso-web-identity) 
    - [Python](https://github.com/neonstoic/BitcloutPythonIdentityExample) 

* [Sign a transaction in NodeJS with seedHex and txnHex](https://gist.github.com/chafreaky/09296ff58b937056d362b69c2ef59a47)

* [Query DeSo Postgres DB with PublicKey in JS](https://gist.github.com/iPaulPro/2255639769eeedb1394921978a6be895)

* [Upload an image to DeSo with Python](https://gist.github.com/sungkhum/291d3d34f8e63b931a312c805ed19b1c)

* [Generate JWT in Python](https://gist.github.com/sungkhum/53b5b3cd050d46387c1f555f4dad18a5) and [Validate JWT in NodeJS](https://github.com/mattetre/bitclout-jwt-validate/)

* [Start up a Deso Node with PostgreSQL DB (BETA & Incomplete)](https://github.com/tijno/bitclout-run/tree/progres#warning)

* [DESO Blockchain Datamodel and Analytics tools in Swift](https://github.com/lludo/BitClout)

* [Bash script to create a node & print out all posts](https://github.com/DeSoDev/cloutprint)
