---
description: Guide on integrating with the DeSo Identity Window API
---

# Overview

In this guide, we will explain how to integrate the DeSo Identity window API into your application.&#x20;

When developing applications that handle user authentication, there are always some actions that require user interaction.

In traditional Web2 applications, this often involves the user having to type information such as username and password, or the user clicking on a third-party login such as Google or Apple.

On the other hand, decentralized Web3 applications are powered by blockchains and consequently, they rely on public and private keys.

The underlying mathematics of blockchains can often appear complicated, both for the user and for the developer.

To solve these issues, we designed the DeSo Identity Service which hides the cryptographic complexities of user authentication underneath a simple window API.

With DeSo Identity Service you don't need to worry about implementing any HTML forms or input validation, you just need to make correct calls to the window API.

The window API is responsible for opening Identity either as a pop-up window or as a new tab. Opening Identity in a window context is as simple as launching a `window.open`.&#x20;

Here are examples of some calls that can be made to Identity:

```javascript
const login   = window.open('https://identity.deso.org/log-in');
const signUp  = window.open('https://identity.deso.org/sign-up');
const logout  = window.open('https://identity.deso.org/logout?publicKey=BC123...');
const approve = window.open('https://identity.deso.org/approve?tx=0abf35a...');

// Can be added to any path for testnet deso and bitcoin addresses
const testnet = window.open('https://identity.deso.org/log-in?testnet=true');
```

And here's an example user interface after launching the `log-in` endpoint with a couple of URL parameters, in this case, `accessLevelRequest=4`&#x20;

You might remember the `accessLevelRequest` URL parameter from the [#access-levels](../identity/concepts.md#access-levels "mention") section in our Introduction guide.

There are other URL parameters that you can use and we will explain them in this document.

![Example usage of Window API](<../../.gitbook/assets/Screenshot from 2021-11-25 01-00-478.png>)

Once the window context is launched, the user has to perform an action, such as login or signup.&#x20;

After the user completes the flow, your app will receive either a `postMessage` or a `callback` containing secure user credentials and payload of the desired API endpoint.

You can then use the user credentials for communicating with the [iframe-api](../iframe-api/ "mention"). We recommend the following format for opening the Identity window context:

```javascript
// center the window.
const h = 1000;
const w = 800;
const y = window.outerHeight / 2 + window.screenY - h / 2;
const x = window.outerWidth / 2 + window.screenX - w / 2;
const win = window.open("https://identity.deso.org/log-in", null, `toolbar=no, width=${w}, height=${h}, top=${y}, left=${x}`);
```

When we pass the public key to the Identity window API, it should always be in **base58check** format:

```javascript
BC1YLgwkd7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ
```

**Note:** <mark style="color:red;">Only one Identity window should be opened at a time.</mark>

## Messages

Normally, the DeSo Identity Service will communicate with your application via [`postMessage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage).

In order to listen to these messages, you have to declare an event handler in the parent window context, as shown below.

The first message sent by Identity is `method: "initialize"`, and your application has to send a response.

We explained this mechanism in more detail in our [#events](../identity/concepts.md#events "mention") and [#initialize](../identity/concepts.md#initialize "mention") guides.

```javascript
window.addEventListener("message", (event) => this.handleMessage(event));
```

When the user completes an action within the window context, Identity will send a `postMessage` to your application containing the payload corresponding to the launched endpoint.

These messages will usually contain a `method: "login"` field so that it's easier to handle closing the identity window as described in the [#termination](./#termination "mention") section.

However, some endpoints will contain a different method, such as the [#derive](./#derive "mention") endpoint.&#x20;

Messages sent by the window API will also include an `id: null` field, which indicates that they do not require a response from your application.

Below is an example `event.data` response from the `log-in` endpoint.

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLj8iwsicimv8ttrPg6rBtizvM7X3KCsiVQwYZKqj6Wj3rT8TD3D: {
        hasExtraText: false,
        btcDepositAddress: "16YqLEpYzeV1aCcp7YwHKh9m5Kg4Sd8eak",
        ethDepositAddress: "0xf8212f8d8E881090653bFF0AC99f499063bCFE77",
        version: 1,
        encryptedSeedHex: "0ac1d9e14c640a26ea3f70095f9b27a50ef06652aea7a2f19f086d750e5f4ecf2d752568b9e3fd3400ad8581d9cf8da5dab3ea29078c1a528f81a51b55514ed5",
        network: "mainnet",
        accessLevel: 4,
        accessLevelHmac: "d66741dcf7a828e7b2b3c44708643de335de28b7a4941eeb687a6d5b1da66e77"
      }
    },
    publicKeyAdded: "BC1YLj8iwsicimv8ttrPg6rBtizvM7X3KCsiVQwYZKqj6Wj3rT8TD3D",
    signedUp: true
  }
}
```

Most responses from the window context will have the same structure as above.

A typical response will contain the `users` object, which reflects all users that have previously logged in to your application.

The `users` object is a map of `publicKey => userCredentials` where the credentials are used primarily for communicating with the `iframe` context.

In such requests, we will be passing the `encryptedSeedHex, accessLevel, accessLevelHmac` which are used by DeSo Identity to ensure that the user has been securely authenticated via the window context.

User credentials should be saved in local storage.

You should always overwrite existing credentials whenever you receive a `method: "login"` response.

This is because some requests can cause Identity to reauthorize all users and their credentials like `encryptedSeedHex` will change.

In the DeSo Protocol implementation, we do this via `this.backend.setIdentityServiceUsers()` on [line #857](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/global-vars.service.ts#L857) in `/src/app/global-vars.service.ts` which uses the [`localStorage` API](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) to store data.

If you're unfamiliar with `localStorage`, below are the three main API calls you'll need.

In the reference implementation, we simply store the entire `payload.users` field under a single key called `IdentityUsersKey = "identityUsersV2"`

```javascript
// Store data in localStorage with (key, value) = (IdentityUsersKey, users)
// We assume the users value is a JSON object so:
localStorage.setItem(IdentityUsersKey, JSON.stringify(userCredentials));

// Read data from localStorage at publicKey
const users = JSON.parse(localStorage.getItem(IdentityUsersKey));

// Remove data from localStorage at publicKey
localStorage.removeItem(IdentityUsersKey);
```

Apart from the `users` field, the response will contain fields that are specific to the requested API endpoint.

When we called the `log-in` endpoint, the response also contained `publicKeyAdded` of the logged user and `signedUp` indicating that the user has signed up.

More information on these fields can be found in the [#endpoints](./#endpoints "mention") section.

## Callbacks

If you're building a mobile application, you most likely want to be using callbacks instead of messages.

Launching the window context with a `callback` URL causes the Identity window to send the payload via URL parameters rather than a `postMessage`.

The callback mechanism works similarly to how OAuth is typically implemented in mobile apps.&#x20;

Callbacks are intended to be used in combination with derived keys so they currently work for [#derive](./#derive "mention") and [#get-shared-secrets](./#get-shared-secrets "mention") endpoints.

Because responses to these endpoints contain sensitive user information, the provided **`callback`** **URL should always be a local address**.

Below is an example call to the `derive` endpoint with a `auth://derive` callback URL:

```javascript
const derive = window.open('https://identity.deso.org/derive?callback=auth://derive');
```

After user finishes the Identity flow the payload will be sent via URL parameters as a GET request to the provided callback like this:

```javascript
GET auth://derive?param1=value1&param2=value2&...
```

Where the `param1=value1` , `param2=value2` fields will match the structure of the response payload of to the `/derive` endpoint as explained in the [#derive](./#derive "mention") section.

## Termination

When a user finishes any action in an Identity window, a `method: "login"` message is sent or a `callback` is called.

After you receive the payload from your desired API endpoint, you should close the Identity window by calling `window.close()` on the window object.

When the Identity window is open, you can also communicate with it just like you would with the `iframe` API. This is sometimes useful if you want to sign a transaction or generate a JWT.
