---
description: Integrating with the DeSo Identity on Mobile.
---

# Mobile Integration

## Derived Keys

In the world of blockchain, user private keys are extremely sensitive information.

This is because, unlike Web2 password, private keys cannot be modified, which means that if somebody gets a hold of your private key, you're potentially **forever** vulnerable to an attack and there isn't much you can do unless you move your entire account to another private key.

We are firm believers that user primary keys should **never** be shared with third-party applications, regardless of their security practices, and so we created derived keys, which significantly lower attack vectors related to unauthorized access to user credentials.

Derived keys are impermanent and they usually automatically expire about 30 days after being issued.

Derived keys can also be de-authorized at any point, which we believe will allow for the creation of advanced security systems in the future that can mitigate the risks originating from key leakage.

After the `Transaction Spending Limit` block height is reached, derived keys will need to be authorized to perform specific transaction types as well as more specific operations for Creator Coin, DAO Coin, and NFT transaction types.&#x20;

Check out [#transactionspendinglimitresponse](../../deso-backend/api/#transactionspendinglimitresponse "mention") to learn how to construct the Transaction Spending Limit object.

A derived key is a pair of public and private cryptographic keys that are authorized to sign transactions on behalf of another key pair.

That is, if you hold a valid derived key of a user, you can submit a transaction signed by that derived key, and it will be regarded as a valid transaction as if it was made by that user.

This is particularly useful in mobile applications because it means you only have to interact with the DeSo Identity Service once, just to get the derived key of a user.

It also means that derived keys are extremely sensitive information and therefore should be handled in secure storage with the utmost caution, ideally by experienced software engineers.

**The flow of using the derived keys is as follows:**

1. Generate a derived key by making a call to [#derive](../window-api/#derive "mention") window API endpoint\

2. Construct a `AuthorizeDerivedKey` transaction via Backend API through `/api/v0/authorize-derived-key`\
   ``
3. Sign the `AuthorizeDerivedKey` transaction with the derived key\

4. Submit signed `AuthorizeDerivedKey` transaction via `/api/v0/submit-transaction`\
   ``
5. (Optional) Confirm that the derived key was successfully authorized through Backend API in `/api/v0/get-user-derived-keys`

### Generate Derived Key

In case your application requires offline signing e.g. when you’re a mobile client, identity can accommodate you with derived key material.

To get a derived key for a user, launch the [#derive](../window-api/#derive "mention") window API endpoint with a callback at:

```javascript
const derive = window.open('https://identity.deso.org/derive?callback=...');
```

Once the user completes the identity flow, you’ll receive a response containing the derived keypair.

For simplicity, we list the response payload as a JSON object; however, you'll receive it as URL parameters in a callback.&#x20;

#### Response

```javascript
{
    accessSignature: "30440220314ccf7a747922ddb6f8c26821c6f0dc65f0227e15014fb5e728f96abed841e2022033aace1f75eb35479d07273ff8bf1a959af75209743ced23939210f824d5321f",
    derivedPublicKeyBase58Check: "tBCKUx3cYyUcPnXyqLYuAfpChQHzcbzvhLTF6Xujuu5CA8hKaHPwTo",
    derivedSeedHex: "e4c515c30479d116757c56b4224022a5558af243946c075cff6ae2ec67fd3748",
    expirationBlock: 12024,
    derivedJwt: "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MzM5Mjg0MjgsImV4cCI6MTYzNjUyMDQyOH0.dvbNwcOUz2bzEMC79nyxzIJGI_3NoMUw59VAI6qdLGNy6x5YC9u0MsFcrDhuL-i8Y66gIQXq0VzgeIzNThxisg",
    jwt: "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MzM5Mjg0MjgsImV4cCI6MTYzNjUyMDQyOH0.4zyR0kgXIeO6P94TuGWbxxr3fHUoIyJWv4GGMAxP6gfz6UMwSSej85ZJe_N5JYYcvk_vHWhnj0CcXfGQtNMQ8Q",
    network: "testnet",
    messagingKeyName: "default-key",
    messagingKeySignature: "fakemessagingkeysignature",
    messagingPublicKeyBase58Check: "tBC1YLh9Rjy3fLcW1bRDcQ4PXhGocuGnsNqVJx3CESCJknkZ7LJ6mV"
    messagingPrivateKey: "fakemessagingprivatekey"
    publicKeyBase58Check: "tBCKWiTPdkGAiSd2jTx58hRh1TAGVnpeDE78eYqsghEeVFpjkGYNLk",
    transactionSpendingLimitHex: "80d0dbc3f4020205f4b00709ff8705042100000000000000000000000000000000000000000000000000000000000000000000871121032357f6e57297839516ae3fd71e76ce47e43f881e8be86877f00ec456594b10c9017b21032357f6e57297839516ae3fd71e76ce47e43f881e8be86877f00ec456594b10c9029b2021032357f6e57297839516ae3fd71e76ce47e43f881e8be86877f00ec456594b10c903ee4700022001855d9ca9c54d797e53df0954204ae7d744c98fe853bc846f5663459ac9cb7b00010a2001855d9ca9c54d797e53df0954204ae7d744c98fe853bc846f5663459ac9cb7b0003f50300"
}
```

Let’s take a look at these values:

* `accessSignature` is a proof of access, equal to an owner-signed digest of `sha256(derivedPublicKey + expirationBlock +` \
  `transactionSpendingLimitBytes)` (For more details check out [the implementation](https://github.com/deso-protocol/identity/blob/ffcb09a3ba6070d14a43c31a09f7ed0478fb2acf/src/app/account.service.ts#L109))\

* `derivedPublicKeyBase58Check` and `derivedSeedHex` is the derived keypair\

* `expirationBlock` is a future block height, and represents the expiration “date” (block) of the derived key. Derived keys expire after about 30 days.\
  \
  After that period, you will have to generate and authorize another derived key.\
  \
  To check if a derived key is valid you should compare the current block height, e.g. taken from `/api/v0/get-app-state` Backend API endpoint, with the `expirationBlock` that you can find by querying the `/api/v0/get-user-derived-keys` Backend API endpoint.\

* `derivedJwt` is a JWT token with a month-long timeout signed by the `derivedPublicKeyBase58Check`\
  ``
* `jwt` is a JWT token with a month-long timeout signed by the owner `publicKeyBase58Check`\
  ``
* `network`  is the network for which this derived key was generated\

* `messagingKeyName` is the key name used for v3 messaging\

* `messagingKeySignature` is the key signature used for v3 messaging\

* `messagingPublicKeyBase58Check` is the public key used for v3 messaging\

* `messagingPrivateKey` is the private key used for v3 messaging\

* `publicKeyBase58Check` is the public key that is the parent of this derived key.\

* `transactionSpendingLimitHex` is a hex string representing the TransactionSpendingLimit for this derived key.

### Authorize Derived Key

Before any signing can happen, a derived key must first be activated by submitting an [`authorizeDerivedKey` transaction](https://docs.deso.org/devs/backend-api#authorize-derived-key), containing the `accessSignature`, `derivedPublicKeyBase58Check`, `expirationBlock`, `transactionSpendingLimitHex` and `publicKeyBase58Check`.

To make the transaction, make a request to the `/api/v0/authorize-derived-key` Backend API endpoint.

If you set `DerivedKeySignature: true` when making the Backend API request, you can sign the authorize transaction with the derived key right away.

To help you get started with the `authorizeDerivedKey` transaction, we made [this node.js code](https://github.com/deso-protocol/examples/tree/main/identity/authorize-derived-key) snippet in examples repository that shows this flow.

If everything worked, you should see the derived key listed in the response to the `/api/v0/get-user-derived-keys` [endpoint](https://github.com/deso-protocol/backend/blob/f70d89a/routes/user.go#L2559) with a payload of `PublicKeyBase58Check` set to owner user public key.

Additionally, see the implementation of AuthorizeDerivedKey in the DeSo developer hub [here](https://hub.deso.org/#/user/authorize-derived-key).

While powerful, this model has a limitation.

Namely, it requires the user to have some balance to execute the `authorizeDerivedKey` transaction.

This poses a limitation as you won't be able to authorize a derived key for new users who might not have balance in their accounts.

However, in practice, there is no use for derived keys unless the user has some non-zero balance in their account. Otherwise, they wouldn't be able to submit any transactions in the first place.&#x20;

One possible remedy to this limitation is to send the `authorizeDerivedKey` transaction only when the user wants to perform an action and has sufficient balance, such as giving a like, making a follow, etc.

Another approach is to write a hook that sends the `authorizeDerivedKey` transaction in the background whenever user receives sufficient balance.

This simple background mechanism should mitigate most UX issues.

### Signing Transactions

The private key `derivedSeedHex`embedded in the response from Identity's `/derive` can be used to sign transactions on behalf of the `publicKeyBase58Check` owner.

To achieve this, you should construct transactions in the Backend API as if made by the owner’s public key.

You then need to append a field to transaction’s `ExtraData["DerivedPublicKey"]` with value set to the derived public key in compressed byte format (33 bytes array) encoded as hex string.&#x20;

Check out this node.js [code snippet](https://github.com/deso-protocol/examples/tree/main/identity/compress-public-key) in the examples repository for help with compressing the derived public key.

If you have trouble de/serializing transactions to add the `“DerivedPublicKey”` to ExtraData, you can use a backend endpoint `/api/v0/append-extra-data` and pass the hex of the transaction and the derived public key like this:

**Request**

```javascript
{
    "TransactionHex": "01049434a060acca8c05af65207c019e1052f3e29dc677125ce8a1833ac72e2b2d010102a7af43768408e8b8f5bacc8d0658f36bb27c7ecb81b88e210d7be4e54861a40bcf980c0a21669d2ac6caefa5af9c6bb60d28b30f78d918d5b5b9ee3b5ae986818dc07eee84012102a7af43768408e8b8f5bacc8d0658f36bb27c7ecb81b88e210d7be4e54861a40b0000",
    "ExtraData": { 
        "DerivedPublicKey": "03f6f5470d8df61160ccf364851c77b8b803131d3f1e8092301178e2fdcec15206"
    }
}
```

Because of intricacies with transaction fees and ExtraData, you should increase `minFeeRateNanosPerKb` to something like `12500` from `10000` when constructing the transaction.

Otherwise, you might get an error while submitting transactions indicating that transaction fee is too low.&#x20;

Once you have the transaction hex with the derived key in ExtraData, you can sign the transaction with the `derivedSeedHex` you’ve received from identity.

You can use the signing code from the `authorize-derived-key` example [node.js code snippet](https://github.com/deso-protocol/examples/tree/main/identity/authorize-derived-key).

You can also find this [example implementation](https://github.com/deso-protocol/backend/blob/f70d89a196cfc42ca3e32a1b80ed9935380a91be/routes/admin\_transaction.go#L349) in Go of signing with a derived key corresponding to the admin backend endpoint `/api/v0/admin/test-sign-transaction-with-derived-key`.

Once signed, the transaction can be submitted through the `/api/v0/submit-transacton` per usual.

Note that we didn’t need to communicate with the DeSo Identity at any point in this process.

### Messages

You can use derived keys to encrypt/decrypt messages on the DeSo blockchain. For more information on our messaging protocol check out the [Broken link](broken-reference "mention") section.

In order to encrypt/decrypt messages you will need to get shared secrets for each messaging partner of your user.

To get the shared secrets, you can submit requests to the [#get-shared-secrets](../window-api/#get-shared-secrets "mention") endpoint in the window API.

Once you get the shared secrets you can use them to encrypt/decrypt messages.

To help you code this flow, we made a [node.js code snippet](https://github.com/deso-protocol/examples/tree/main/identity/messages-shared-secret) in the examples repository which displays our messaging protocol.

## Webview Support

Identity also offers support for webview, but there are some differences needed in order to fully integrate.

Major differences:

* There is no need to run an iframe context. You will send all messages to one context running in a webview.
* Your webview context will need to have an additional parameter `?webview=true`
* Depending on your mobile development framework, you need to make sure messages to and from the webview are being registered appropriately. Currently iOS, Android, and React Native webviews are supported.

