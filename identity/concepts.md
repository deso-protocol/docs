---
description: Fundamentals of working with the DeSo Identity Service
---

# Concepts

## Basics

In this section, we will look into a web-based integration of the DeSo Identity Service. If you're an iOS or Android developer, this guide is still useful. In addition, we recommend reading our [mobile-integration.md](mobile-integration.md "mention") guide afterwards. Web-based applications interact with Identity in two ways:

* embedding Identity in an [`iframe`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe)
* opening Identity as a [`window`](https://developer.mozilla.org/en-US/docs/Web/API/Window/open)

In most cases, a web application will use both contexts concurrently. A general rule of thumb when it comes to Identity APIs is that the `iframe` is used for all background requests such as transaction signing and message decryption, whereas the `window` context serves requests that require user interaction such as log in, sign up, and account management.

### Events

Both `iframe` and `window` contexts communicate with your application by emitting `message` events through [`Window.postMessage()`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). To start listening to these messages, we need to add a listener to the parent window, such as on [line #38](https://github.com/deso-protocol/frontend/blob/main/src/app/identity.service.ts#L38) in the implementation.

```javascript
window.addEventListener("message", (event) => this.handleMessage(event));
```

Here, `this.handleMessage(event)` is a function defined by the developer to handle the logic around incoming messages. This handler function is likely the most important piece of code that you'll have to write when interacting with Identity. When working with Identity for the first time, we recommend a simple `console.log(event)` to get a sense of how it works. The `event.data` field will contain the payload of messages sent by the DeSo Identity Service.

### Initialize

The first message that Identity sends when it is opened is `initialize`. This message is sent in both `iframe` and `window` contexts and will require a response. In this section, we will be assuming that we've already opened either an `iframe` or `window` context, but don't worry about it for now. Each context has a dedicated guide explaining its inner workings in more detail, and we'll get to that later. Below is an example of the `event.data` that your event handler will receive on `initialize`. And here's a corresponding handler logic on line [#226](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L226) in the implementation.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {},
  method: 'initialize',
}
```

The above four fields are present in the majority of Identity messages. Let's take a closer look at each of them:

* The `id` is in [UUID v4](https://en.wikipedia.org/wiki/Universally\_unique\_identifier#Version\_4\_\(random\)) format and is used to identify requests/responses by Identity.
* The `service` field is set to `'identity'` in every message, and should be checked in the event handler (like[ this](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L214)) to make sure the message originated from the DeSo Identity Service.
* The `payload` field will contain the data sent by Identity, such as user information, signed transactions, etc. In the case of `initialize` message, it's left as an empty JSON.
* The `method` field describes the message sent, which is `'initialize'` in our example.

The DeSo Identity Service **requires** a response to the `initialize` message on web-based applications. Whether we're using an `iframe` or a `window`, the response can simply be sent by directly responding to the message event such as in lines [#187](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L187) and [#282](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L282):

```javascript
event.source.postMessage({ 
    id: '21e02080-0ef4-4056-a319-a66403f33768',
    service: 'identity',
    payload: {},
}, "https://identity.deso.org");
```

The `id` field should be set to match the `id` value present in the `initialize` message. We set the `id` field as a string in the above code snippet, but you should set `id: event.data.id` in your code. You may have noticed that in the example above we've added the string `"https://identity.deso.org"` at the end of the `postMessage()` call. This specifies the target origin, or the accepted URL for the destination of the `postMessage`. We can alternatively pass the wildcard `"*"` to accept any URL, which is less safe, but we will use it throughout this documentation for simplicity. However, we recommend setting `"https://identity.deso.org"` for better security.

A few quick notes about message formats:

* Messages with an `id` and `method` are requests that expect a response. (like `initialize`)
* Messages with an `id` and no `method` are responses to requests.
* Messages without an `id` do not expect a response.

Keep this in mind as you read through [window-api](window-api/ "mention") and [iframe-api](iframe-api/ "mention").

## Accounts

The DeSo Identity Service implements account management such as login, signup, and logout. Identity takes care of all cryptographic complexities related to blockchain accounts. In addition, we don't have to deal with passwords or any sensitive data when using the DeSo Identity. All account-related actions are performed via the `window` context API. For now, we will solely focus on the general intuition about account management, and leave the details of each API endpoint for the guide on the [window-api](window-api/ "mention"). The account workflow involves the following steps:

1. App [opens](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L99) the Identity in a `window` context, with the `/log-in` [API endpoint](window-api/#log-in).
2. When user completes the login flow, Identity sends a response containing [`PublicUserInfo`](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/types/identity.ts#L20) that will be used when exchanging messages with the `iframe` context.
3. App [closes](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L196) the Identity `window` and stores the `PublicUserInfo` from step 2. into [local storage](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/global-vars.service.ts#L857) / database.
4. App uses `PublicUserInfo` when sending messages to the `iframe` context to handle transaction [signing](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L125), message [decryption](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L144), and other methods.
5. App might launch more `window` contexts to handle other actions requiring user interaction.

This communication pattern covers almost all of the interactions with the DeSo Identity.

In step 2. we mentioned the cryptic `PublicUserInfo`. This information can be thought of as secure user credentials consisting of three important fields:

* `encryptedSeedHex` is used to verify user accounts between the `window` and the `iframe` contexts
* `accessLevel` and `accessLevelHmac` information is used to verify the permission that the user has given to your application

When handling user account creation or login you will always need to deal with access levels explicitly, so we dedicated an entire section to them next.

### Access Levels

When handling user accounts in the DeSo Identity `window` context, you will always want to add a `accessLevelRequest` URL parameter to the request, such as below:

```javascript
window.open("https://identity.deso.org/log-in?accessLevelRequest=4", null);
```

The DeSo Protocol's implementation uses a `params` variable when handling URL parameters, and `accessLevelRequest` logic can be found at line [#85](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L85). The access level request ranges from `0` to `4` and determines what actions the user has authorized your application for. Higher permission level means higher number of authorized actions. The available access levels are:

```javascript
enum AccessLevel {
  // User revoked permissions
  None = 0,

  // Approval required for all transactions.
  // This means no account action is authorized.
  ApproveAll = 2, /* DEFAULT */

  // Approval required for buys, sends, and sells
  // This authorizes all non-spending actions.
  ApproveLarge = 3,

  // Node can sign all transactions without approval
  // This authorizes all non-spending & spending actions.
  Full = 4,
}
```

Or, here's how the levels `2,3,4` (increasing downwards) look like in the DeSo Identity UI:

![DeSo Identity Access Level UI](<../.gitbook/assets/Screenshot from 2021-11-22 01-13-56.png>)

Access level determines which actions would require an approval from the user. Approval here means we need to launch the Identity in a `window` context so that user can review and manually confirm a transaction. The approval mechanism is explained in more detail in [#approve](window-api/#approve "mention"). You should only require access level `4` if your app really needs it. In general, you should deliberately request the minimal permission level that fits the requirements of your application For an exhaustive list of `accessLevel` requirements for each action, check out our [iframe-api](iframe-api/ "mention") documentation.

## Transactions

Transactions are the building material of every blockchain. When you think of transactions, you might think of them in financial terms, that is, a transaction is an exchange of money between users. While this is true in traditional systems, in the world of blockchain transactions are actually more general. A blockchain transaction means any change to the underlying database, which is called the blockchain state. On DeSo, this includes financial transactions, but also social transactions such as submitting posts, following users, minting NFTs, etc. Since blockchain communication is peer-to-peer, we can't verify who made a transaction purely by looking at network information such as IP addresses. Instead, we use cryptography to have mathematical certainty that the person sending the transaction is really the person they claim to be. To do that we use digital signatures, which are special certificates issued by user's private key. When integrating with the DeSo Identity Service, we don't have to worry about all the cryptographic nuances related to user keys. Instead, we can simply ask Identity to do the hard work such as issuing transaction signatures.

### Lifecycle

Transactions on the DeSo blockchain have a three-step lifecycle:

**Construct:** The first step for a developer is to interact with the DeSo Backend API through endpoints such as `/api/v0/buy-or-sell-creator-coin` to get an unsigned user transaction.

**Sign:** The developer will then take the output `TransactionHex` from the construct step's response, which encodes the user transaction, and signs it using the DeSo Identity.

**Broadcast:** The signed transaction will be sent through the `/api/v0/submit-transaction` by the developer so that it can be added to the blockchain ledger.

In this section, we're mostly interested in the signing step. The DeSo Identity Sevice handles the issuance of transaction signatures through both the `iframe` and `window` contexts. The `AccessLevel` you've requested in`/log-in`, as mentioned in [#access-levels](identity.md#access-levels "mention"), will determine which transactions you can sign using the `iframe` context. The required AccessLevel for each `iframe` API is detailed in the [iframe-api](iframe-api/ "mention") guide. If the transaction you intend to sign matches this AccessLevel, you could simply ask the DeSo Identity Service to issue a signature in the background through the [`iframe` context](iframe-api/#sign). This would be done by sending a `postMessage` to the `iframe` such as in line [#274](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L274).

```javascript
this.iframe.contentWindow.postMessage(req, "*");
```

Note that we're executing the `postMessage` on the iframe `contentWindow` . We will explain the details of what belongs in the `req` variable in the `iframe` context guide, though we will essentially have to pass `method: "sign"` field, the `encryptedSeedHex,` `accessLevel`, and`accessLevelHmac` fields, and pass the `transactionHex` of the transaction we want to sign (example on line [#125](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L125)). The response will contain either the `signedTransactionHex` , meaning the request was successful, or a `approvalRequired: true` field that will indicate we need to launch the Identity window with the approval flow. To do so, we will launch a `window` context with the `/approve` endpoint and pass the desired transaction as URL param:

```javascript
window.open("https://identity.deso.org/approve?tx={transactionHex}", null);
```

After receiving the `signedTransactionHex`, we can then broadcast it to the network. We will further describe the `method: "sign"` iframe API message and `/approve` window API endpoint in the corresponding API documentations.

## Messages

A crucial component of any social network is messaging. That's why the DeSo blockchain was designed to handle messages as transactions that can be securely stored on-chain. Messages are private and can only be read by the sender and the recipient thanks to our public key cryptographic protocols. Consequently, messages are handled by Identity. As an application developer, it isn't crucial to understand the messaging scheme in-depth, but we want to share a few remarks for those crypto-savvy readers in the [Protocol](identity.md#protocol) subsection.

### Protocol

If you're working closely with DeSo messages or are a mobile app developer, you will most likely have to digest this subsection. There are two implemented messaging protocols on the DeSo blockchain: legacy `V1`, and current `V2`; and we're expecting to move to a `V3` version soon. Older blocks will still contain messages following the legacy `V1` messages so they ought to be handled differently than the `V2` messages. If you've read our core documentation, you should remember the transaction `ExtraData` . New message transactions will have a `V: []byte` field in `ExtraData` which should determine the used version (`1` or `2`) - if a message transaction doesn't have the `V` field, it means it's following `V1` scheme. Here's how each version works:

* `V1`: (LEGACY) Messages encrypted to the recipient public key using AES-128-CTR scheme. Messages can be decrypted with the private key of the user specified in transaction metadata field `RecipientPublicKey`.
* `V2`: (CURRENT) Messages encrypted to both the recipient and sender using [AES-128-CTR](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/lib/ecies/index.js#L175) scheme with shared secrets derived via [ECDH](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/lib/ecies/index.js#L100) and run through a simple [SHA256 ConcatKDF](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/lib/ecies/index.js#L23). Under `V1`, once a message was broadcasted to the blockchain, the sender of the message was unable to decrypt it on other devices. On the other hand, shared secrets are available both to the recipient and sender, so both can decrypt messages.
* `V3`: (PLANNED) We intend to expand the `V2` scheme so it uses rotating messaging keys. Keys will be computed via HD wallet scheme with hardened derivation. Messaging keys can then be shared with third-party apps, specifically mobile clients, to simplify message handling.

If you're a mobile app developer using derived keys you will most likely need to implement the message encryption and decryption yourself, or rely on an existing implementation. The best way would be to trace through the node.js example [encryption/decryption code snippet](https://github.com/deso-protocol/examples/tree/main/identity/messages-shared-secret) or look at [encryption](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/app/identity.service.ts#L203) and [decryption](https://github.com/deso-protocol/identity/blob/f211503ea2420cc6e75e48683670d278cc152d8c/src/app/identity.service.ts#L216) Identity code and recreate it in the programming language of your choosing. If you bundle your code in a library for others to use, please do share it and we'll include it in this documentation!

Our cryptographic implementations are based on JavaScript [ECIES-Parity library](https://github.com/sigp/ecies-parity) from Sigma Prime.

### Implementation

Messages are primarily handled through the [`iframe` context](iframe-api/#decrypt), with the exception of mobile clients relying on derived keys. These clients will handle messages through the `window` context. The goal of this subsection is to give you a general intuition about message handling. We leave the communication details to the section on `iframe` API.

#### Encryption

Encryption is a process of concealing information so that only some authorized parties can read it. In order to successfully encrypt and send a message transaction, the following steps should be taken:

1. Message text is first encrypted through the Identity by sending a request with `method: "encrypt"` to the [`iframe` context](iframe-api/#encrypt), as in [line #141](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L141).
2. The encrypted message is used in the Backend API to construct a message transaction via `/api/v0/send-message-stateless` endpoint.
3. After message transaction is prepared, the `transactionHex` will finally be signed by the DeSo Identity and broadcast to the network as described in the Transactions section.

#### Decryption

We previously mentioned concealing information, but what about revealing an encrypted message, or in other words, what about message decryption? In order to read a message, we will pass the encrypted message to the DeSo Identity Service which will handle the message decryption for us. This is done in a single step:

1. Send the encrypted message in a request to the `iframe` context with `method: "decrypt"` , as in [line #150](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/identity.service.ts#L150). Check out the [#decrypt](iframe-api/#decrypt "mention") API for details.

Another use-case for the `decrypt` API is decrypting unlockable text in NFTs. To see how this can be done, we recommend tracing through the [`DecryptUnlockableTexts()`](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/backend-api.service.ts#L945) in the DeSo Protocol repository.

## Conclusion

In this document, we've outlined all the major components of integrating with the DeSo Identity. If you've gone this far, you should have a good understanding of how the Identity works and what is required to efficiently incorporate it in your application.

Throughout this guide, we've intentionally left out many implementation details for the sake of simplicity. If things made sense to you, it means it's a good time to dive into the other guides on the DeSo Identity. Specifically, if you're making a web-based application, you should take a look our [Window API](window-api/) and [iframe API](iframe-api/) docs next. If you're building a mobile application, we recommend taking a look at the [Mobile integration](mobile-integration.md) guide next.
