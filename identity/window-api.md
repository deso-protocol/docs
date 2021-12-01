---
description: Documentation of the Window Context API
---

# Window API

## Basics

In this guide, we will explain how to integrate the DeSo Identity window API into your application. When developing applications that handle user authentication, there are always some actions that require user interaction. In traditional Web2 applications, this often involves the user having to type information such as username and password, or the user clicking on a third-party login such as Google or Apple. On the other hand, decentralized Web3 applications are powered by blockchains and consequently, they rely on public and private keys. The underlying mathematics of blockchains can often appear complicated, both for the user and for the developer. To solve these issues, we designed the DeSo Identity Service which hides the cryptographic complexities of user authentication underneath a simple window API. With DeSo Identity Service you don't need to worry about implementing any HTML forms or input validation, you just need to make correct calls to the window API.

The window API is responsible for opening Identity either as a pop-up window or as a new tab. Opening Identity in a window context is as simple as launching a `window.open`. Here are examples of some calls that can be made to Identity:

```javascript
const login   = window.open('https://identity.deso.org/log-in');
const signUp  = window.open('https://identity.deso.org/sign-up');
const logout  = window.open('https://identity.deso.org/logout?publicKey=BC123...');
const approve = window.open('https://identity.deso.org/approve?tx=0abf35a...');

// Can be added to any path for testnet deso and bitcoin addresses
const testnet = window.open('https://identity.deso.org/log-in?testnet=true');
```

And here's an example user interface after launching the `log-in` endpoint with a couple of URL parameters, in this case, `accessLevelRequest=4` and `hideJumio=true`. You might remember the `accessLevelRequest` URL parameter from the [#access-levels](identity-api.md#access-levels "mention") section in our Introduction guide. There are other URL parameters that you can use and we will explain them in this document.&#x20;

![Example usage of Window API ](<../.gitbook/assets/Screenshot from 2021-11-25 01-00-478.png>)

Once the window context is launched, the user has to perform an action, such as login or signup. After the user completes the flow, your app will receive either a `postMessage` or a `callback` containing secure user credentials and payload of the desired API endpoint. You can then use the user credentials for communicating with the [iframe-api.md](iframe-api.md "mention"). We recommend the following format for opening the Identity window context:&#x20;

```javascript
// center the window.
const h = 1000;
const w = 800;
const y = window.outerHeight / 2 + window.screenY - h / 2;
const x = window.outerWidth / 2 + window.screenX - w / 2;
const win = window.open("https://identity.deso.org/log-in", null, `toolbar=no, width=${w}, height=${h}, top=${y}, left=${x}`);
```

When we pass public key to the Identity window API, it should always be in base58check format:

```javascript
BC1YLgwkd7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ
```

**Note: Only one Identity window should be opened at a time.**&#x20;

### Messages

Normally, the DeSo Identity Service will communicate with your application via [`postMessage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage). In order to listen to these messages, you have to declare an event handler in the parent window context, as shown below. The first message sent by Identity is `method: "initialize"`, and your application has to send a response. We explained this mechanism in more detail in our [#events](identity-api.md#events "mention") and [#initialize](identity-api.md#initialize "mention") guides.

```javascript
window.addEventListener("message", (event) => this.handleMessage(event));
```

When the user completes an action within the window context, Identity will send a `postMessage` to your application containing the payload corresponding to the launched endpoint. These messages will usually contain a `method: "login"` field so that it's easier to handle closing the identity window as described in the [#termination](window-api.md#termination "mention") section. However, some endpoints will contain a different method, such as the [#derive](window-api.md#derive "mention") endpoint. Messages sent by the window API will also include an `id: null` field, which indicates that they do not require a response from your application. Below is an example `event.data` response from the `log-in` endpoint.

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

Most responses from the window context will have the same structure as above. A typical response will contain the `users` object, which reflects all users that have previously logged in to your application. The `users` object is a map of `publicKey => userCredentials` where the credentials are used primarily for communicating with the `iframe` context. In such requests, we will be passing the `encryptedSeedHex, accessLevel, accessLevelHmac` which are used by  DeSo Identity to ensure that the user has been securely authenticated via the window context. User credentials should be saved in local storage. You should always overwrite existing credentials whenever you receive a `method: "login"` response. This is because some requests can cause Identity to reauthorize all users and their credentials like `encryptedSeedHex` will change. In the DeSo Protocol implementation, we do this via `this.backend.setIdentityServiceUsers()` on [line #857](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/global-vars.service.ts#L857) in `/src/app/global-vars.service.ts` which uses the [`localStorage` API](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) to store data. If you're unfamiliar with the `localStorage`, here are the three main API calls you'll need:

```javascript
// Store data in localStorage with (key, value) = (publicKey, userCredentials)
// We assume the userCredentials value is a JSON object so:
localStorage.setItem(publicKey, JSON.stringify(userCredentials));

// Read data from localStorage at publicKey
const data = JSON.parse(localStroage.getItem(publicKey));

// Remove data from localStorage at publicKey
localStorage.remove(publicKey);
```

Apart from the `users` field, the response will contain fields that are specific to the requested API endpoint. When we called the `log-in` endpoint, the response also contained `publicKeyAdded` of the logged user, and `signedUp` indicating that the user has signed up. More information on these fields can be found in the [#endpoints](window-api.md#endpoints "mention") section.

### Callbacks

If you're building a mobile application, you most likely want to be using callbacks instead of messages. Launching the window context with a `callback` URL causes the Identity window to send the payload via URL parameters rather than a `postMessage`. The callback mechanism works similarly to how OAuth is typically implemented in mobile apps. Callbacks are intended to be used in combination with derived keys so they currently work for [#derive](window-api.md#derive "mention") and [#get-shared-secrets](window-api.md#get-shared-secrets "mention") endpoints. Because responses to these endpoints contain sensitive user information, the provided **`callback`** **URL should always be a local address**. Below is an example call to the `derive` endpoint with a `auth://derive` callback URL:

```javascript
const derive = window.open('https://identity.deso.org/derive?callback=auth://derive');
```

After user finishes the Identity flow the payload will be sent via URL parameters as a GET request to the provided callback like this:

```javascript
GET auth://derive?param1=value1&param2=value2&...
```

Where the `param1=value1` , `param2=value2` fields will match the structure of the response payload of to the `/derive` endpoint as explained in the [#derive](window-api.md#derive "mention") section.

### Termination

When a user finishes any action in an Identity window, a `method: "login"` message is sent or a `callback` is called. After you receive the payload from your desired API endpoint, you should close the Identity window by calling `window.close()` on the window object. When the Identity window is open, you can also communicate with it just like you would with the `iframe` API. This is sometimes useful if you want to sign a transaction or generate a JWT.

## Endpoints

This section describes all endpoints for the window API. Endpoints are called with URL parameters, some of which are optional. After user completes the flow within the window, Identity will send a `postMessage` response.

### log-in

This endpoint is used to handle user login or account creation.

#### Request

```javascript
const login = window.open('https://identity.deso.org/log-in');
```

#### URL Parameters

| Name                          | Type   | Description                                                                                                      |
| ----------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------- |
| accessLevelRequest (optional) | int    | Requested access level, as in[#access-levels](identity-api.md#access-levels "mention") Default is Access Level 2 |
| testnet (optional)            | bool   | Whether we're on testnet or mainnet. Default is `false`                                                          |
| webview (optional)            | bool   | Whether we're using webview. Default is `false`                                                                  |
| jumio (optional)              | bool   | Whether to show "get free deso" during signup. Default is `false`                                                |
| referralCode (optional)       | string | Referral code to be used in Jumio, similar to [#get-free-deso](window-api.md#get-free-deso "mention")            |
| hideGoogle (optional)         | bool   | Hide Google login from the Identity UI. Default is `false`                                                       |

`accessLevelRequest` can take values from {0, 2, 3, 4}. You should only ask for the lowest permission that fits the requirements of your application. Here's what each level means:

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

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
        accessLevel: 4,
        accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
        btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
        encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
        hasExtraText: false,
        network: "mainnet",
      },
      BC1...
    },
    publicKeyAdded: 'BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX',
    signedUp: false,
  }
}
```

For a log in or sign up action, the selected `publicKey` will be included in `publicKeyAdded`. When a user signs up the `signedUp` field will be set to `true`, otherwise it'll be `false` for log in.

An application should store the current `publicKeyAdded` and `users` objects in its local storage. You should always overwrite the existing, saved users. Check out the [#messages](window-api.md#messages "mention") section for more information. When an application wants to sign a transaction or decrypt a message, the `accessLevel`, `accessLevelHmac`, and `encryptedSeedHex` values will be required.

### logout

Logout is used to reset the `accessLevel` of an individual user. You should handle user logout by calling this endpoint. When you logout a user, you should delete the corresponding `userCredentials` entry form the local storage/database. Consult the [#messages](window-api.md#messages "mention") section for more information.

#### Request

```javascript
const logout = window.open('https://identity.deso.org/logout');
```

#### URL Parameters

| Name               | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| publicKey          | string | Public key of the user that is logging out              |
| testnet (optional) | bool   | Whether we're on testnet or mainnet. Default is `false` |
| webview (optional) | bool   | Whether we're using webview. Default is `false`         |

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
        accessLevel: 4,
        accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
        btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
        encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
        hasExtraText: false,
        network: "mainnet",
      },
      BC1...
    }
  }
}
```

### approve

The approve endpoint is used for transaction signing. If you're unsure what this means, make sure to check out the [#transactions](identity-api.md#transactions "mention") section to see how transactions work in the DeSo blockchain. Usually, the approve endpoint is called when you want to sign a transaction that's outside the scope of the [`accessLevel`](identity-api.md#access-levels) you have requested during [#log-in](window-api.md#log-in "mention"). For example, if you requested `accessLevel=3` and want to sign a `BasicTransfer` transaction (constructed via `api/v0/send-deso`Backend API), you would need to use the approve endpoint because `BasicTransfer` requires `accessLevel=4`. If the transaction you want to sign is within the scope of the `accessLevel` you have, you should sign it through the [iframe-api.md](iframe-api.md "mention").&#x20;

#### Request

```javascript
const approve = window.open('https://identity.deso.org/approve');
```

#### URL Parameters

| Name                | Type   | Description                                             |
| ------------------- | ------ | ------------------------------------------------------- |
| tx                  | string | Transaction hex of the transaction to sign              |
| testnet (optional)  | bool   | Whether we're on testnet or mainnet. Default is `false` |
| webview (optional)  | bool   | Whether we're using webview. Default is `false`         |

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
        accessLevel: 4,
        accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
        btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
        encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
        hasExtraText: false,
        network: "mainnet",
      },
      BC1...
    },
    signedTransactionHex: "0196be9786f1634b2734196bad0798..."
  }
}
```

### derive

The derive endpoint is used to generate a derived key for a user. When you hold a derived key, you can sign transactions for a user without having to interact with the DeSo Identity Service. Derived keys are intended to be used primarily in mobile applications and with [#callbacks](window-api.md#callbacks "mention"). If no callback is specified, you will receive the derived key through [#messages](window-api.md#messages "mention"). In such case, you will receive the payload with `method: "derive"` (opposed to `"login"`).  More information on derived keys can be found in [#derived-keys](mobile-integration.md#derived-keys "mention").

#### Request

```javascript
const derive = window.open('https://identity.deso.org/derive');
```

#### URL Parameters

| Name                | Type   | Description                                                                                  |
| ------------------- | ------ | -------------------------------------------------------------------------------------------- |
| callback (optional) | string | Callback URL for the payload as explained in [#callbacks](window-api.md#callbacks "mention") |
| testnet (optional)  | bool   | Whether we're on testnet or mainnet. Default is `false`                                      |
| webview (optional)  | bool   | Whether we're using webview. Default is `false`                                              |

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "derive",
  payload: {
    derivedSeedHex: "40538066039f21f42f0247f49fe4e7d63a9d80528486b02fea37ce3c57886546",
    derivedPublicKey: "BC1YLjGYUcpF7HMqmUYNoDaV7Wxc8TYoGhGwmigyEVSCKNAxU9GmikD",
    publicKey: "BC1YLj8iwsicimv8ttrPg6rBtizvM7X3KCsiVQwYZKqj6Wj3rT8TD3D",
    btcDepositAddress: "Not implemented yet",
    ethDepositAddress: "Not implemented yet",
    expirationBlock: 91587,
    network: "mainnet",
    accessSignature: "304502206a612483e970e3494191e5242683b2be8d22ca6e20473e990f50098021099d8b022100f5b011ea683499dee50079ed2497bfbd16461e802e53f956d4c9c6cdaa423f47",
    jwt: "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2Mzc5OTQ3NDMsImV4cCI6MTY0MDU4Njc0M30.o74505dYS0qZ5EqSxeYiuSzMCTFRLEToLXmudTtbdk3oYhu80M95qbHX5BjHQw4Bvuh7yz8Hv1u2-leReAaf9Q",
    derivedJwt: "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2Mzc5OTQ3NDMsImV4cCI6MTY0MDU4Njc0M30.Nb1IuWgOxrG21NG6zyHMA6M-Mlq2kVrIPVNWAhj49jlaDCqlGQAAEBOl4jlVBC3vNU0S9yyq_8QI5Gmz4Qk1lA"
  }
}
```

The meaning of each of these fields is explained in [#derived-keys](mobile-integration.md#derived-keys "mention"). In the case of the callback, all these fields will be passed as URL parameters. Assuming, we've passed `callback=auth://derive`, the payload will be sent as a GET request like this:

```javascript
GET auth://derive?derivedSeedHex=...&derivedPublicKey=...&publicKey=...&
btcDepositAddress=...&ethDepositAddress=...&expirationBlock=...&network=...&
accessSignature=...&jwt=...&derivedJwt=...
```

### get-shared-secrets

This endpoint is intended to be used in combination with derived keys. It allows you to get message encryption/decryption keys, or shared secrets, so that you can read messages without querying Identity (as is traditionally done via [iframe-api.md](iframe-api.md "mention")). The `get-shared-secrets` endpoint requires a callback as well as other URL parameters that are used to verify ownership of the derived key. Another required URL parameter is `messagePublicKeys` which are the public keys of messaging parties in [CSV format](https://en.wikipedia.org/wiki/Comma-separated\_values#Basic\_rules) for which we want to fetch shared secrets. The `get-shared-secrets` endpoint allows to fetch multiple shared secrets at once. For example, if the owner of the derived key was public key `A`, and we wanted to read conversations between parties `B`,`C`,`D` (or more) we would set `ownerPublicKey=A` and `messagePublicKeys=B,C,D` .

#### Request

```javascript
const secrets = window.open('https://identity.deso.org/get-shared-secrets');
```

#### URL Parameters

| Name               | Type                                                                                      | Description                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| callback           | string                                                                                    | Callback URL for the payload as explained in [#callbacks](window-api.md#callbacks "mention") |
| ownerPublicKey     | string                                                                                    | Master public key that was used to get the derived key                                       |
| derivedPublicKey   | string                                                                                    | Derived public key                                                                           |
| JWT                | string                                                                                    | JWT signed by the derived key, it's passed as `derivedJwt` in `/derive`                      |
| messagePublicKeys  | strings, [CSV format](https://en.wikipedia.org/wiki/Comma-separated\_values#Basic\_rules) | Public keys of users for which we want to fetch shared secrets.                              |
| testnet (optional) | bool                                                                                      | Whether we're on testnet or mainnet. Default is `false`                                      |

#### Response

The response is a list of shared secrets in CSV format corresponding to `messagePublicKeys` . The payload will always be sent as a [#callbacks](window-api.md#callbacks "mention").

```javascript
GET callback?sharedSecrets=ccf9474d7578658e0c77adb7aea5cc9b61d3bccad5bddddd11e1850dfa4db059,
    db7aea5cc9b61d3b..., a11e1850dfa4db059...
```

### get-free-deso

The `get-free-deso` endpoint allows for launching the Jumio KYC flow, and if completed successfully, it will end up sending your users free starter DeSo or their referral bonus.

#### Request

```javascript
const free = window.open('https://identity.deso.org/get-free-deso');
```

#### URL Parameters

| Name                    | Type   | Description                                             |
| ----------------------- | ------ | ------------------------------------------------------- |
| public\_key             | string | Public key of the user to be KYC verified               |
| referralCode (optional) | string | Referral code to be used in Jumio                       |
| testnet (optional)      | bool   | Whether we're on testnet or mainnet. Default is `false` |

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
        accessLevel: 4,
        accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
        btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
        encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
        hasExtraText: false,
        network: "mainnet",
      },
      BC1...
    },
    publicKeyAdded: "BC1...",
    signedUp: true,
    jumioSuccess: true
  }
}
```

The `signedUp` boolean variable determines if the user has logged in or signed up, and `jumioSuccess` indicates if the Jumio KYC was successful.

### verify-phone-number

The `verify-phone-number` endpoint will give the user some starter DeSo after a successful phone verification.

#### Request

```javascript
const phone = window.open('https://identity.deso.org/verify-phone-number');
```

#### URL Parameters

| Name               | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| public\_key        | string | Public key of the user to be phone verified             |
| testnet (optional) | bool   | Whether we're on testnet or mainnet. Default is `false` |

#### Response

```javascript
{
  id: null,
  service: "identity",
  method: "login",
  payload: {
    users: {
      BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
        accessLevel: 4,
        accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
        btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
        encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
        hasExtraText: false,
        network: "mainnet",
      },
      BC1...
    },
    publicKeyAdded: "BC1...",
    signedUp: true,
    phoneNumberSuccess: true
  }
}
```

The `signedUp` boolean variable determines if the user has logged in or signed up, and `phoneNumberSuccess` indicates if the phone verification was successful.

## Conclusion

Window API is an essential component of integrating with the DeSo Identity Service. If you're building a desktop application, you should now take a look at the [iframe-api.md](iframe-api.md "mention"). And if you're creating a mobile app, check out our [mobile-integration.md](mobile-integration.md "mention") guide next.
