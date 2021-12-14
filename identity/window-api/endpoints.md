---
description: List of DeSo Identity Window API endpoints
---

# Endpoints

This section describes all endpoints for the window API. Endpoints are called with URL parameters, some of which are optional. After user completes the flow within the window, Identity will send a `postMessage` response.

## log-in

This endpoint is used to handle user login or account creation.

#### Request

```javascript
const login = window.open('https://identity.deso.org/log-in');
```

#### URL Parameters

| Name                          | Type   | Description                                                                                                                                                                                               |
| ----------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessLevelRequest (optional) | int    | Requested access level, as in[#access-levels](../identity.md#access-levels "mention") Default is Access Level 2                                                                                           |
| testnet (optional)            | bool   | Whether we're on testnet or mainnet. Default is `false`                                                                                                                                                   |
| webview (optional)            | bool   | Whether we're using webview. Default is `false`                                                                                                                                                           |
| jumio (optional)              | bool   | Whether to show "get free deso" during signup. Default is `false`                                                                                                                                         |
| referralCode (optional)       | string | Referral Code through which the user accessed the site. The referral code allows the user to get a greater amount of money as a sign-up bonus. Also check out[#get-free-deso](./#get-free-deso "mention") |
| hideGoogle (optional)         | bool   | Hide Google login from the Identity UI. Default is `false`                                                                                                                                                |

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

An application should store the current `publicKeyAdded` and `users` objects in its local storage. You should always overwrite the existing, saved users. Check out the [#messages](./#messages "mention") section for more information. When an application wants to sign a transaction or decrypt a message, the `accessLevel`, `accessLevelHmac`, and `encryptedSeedHex` values will be required.

## logout

Logout is used to reset the `accessLevel` of an individual user. You should handle user logout by calling this endpoint. When you logout a user, you should delete the corresponding `userCredentials` entry form the local storage/database. Consult the [#messages](./#messages "mention") section for more information.

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

## approve

The approve endpoint is used for transaction signing. If you're unsure what this means, make sure to check out the [#transactions](../identity.md#transactions "mention") section to see how transactions work in the DeSo blockchain. Usually, the approve endpoint is called when you want to sign a transaction that's outside the scope of the [`accessLevel`](../identity.md#access-levels) you have requested during [#log-in](./#log-in "mention"). For example, if you requested `accessLevel=3` and want to sign a `BasicTransfer` transaction (constructed via `api/v0/send-deso`Backend API), you would need to use the approve endpoint because `BasicTransfer` requires `accessLevel=4`. If the transaction you want to sign is within the scope of the `accessLevel` you have, you should sign it through the [iframe-api](../iframe-api/ "mention").&#x20;

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

## derive

The derive endpoint is used to generate a derived key for a user. When you hold a derived key, you can sign transactions for a user without having to interact with the DeSo Identity Service. Derived keys are intended to be used primarily in mobile applications and with [#callbacks](./#callbacks "mention"). If no callback is specified, you will receive the derived key through [#messages](./#messages "mention"). In such case, you will receive the payload with `method: "derive"` (opposed to `"login"`).  More information on derived keys can be found in [#derived-keys](../mobile-integration.md#derived-keys "mention").

#### Request

```javascript
const derive = window.open('https://identity.deso.org/derive');
```

#### URL Parameters

| Name                | Type   | Description                                                                       |
| ------------------- | ------ | --------------------------------------------------------------------------------- |
| callback (optional) | string | Callback URL for the payload as explained in [#callbacks](./#callbacks "mention") |
| testnet (optional)  | bool   | Whether we're on testnet or mainnet. Default is `false`                           |
| webview (optional)  | bool   | Whether we're using webview. Default is `false`                                   |

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

The meaning of each of these fields is explained in [#derived-keys](../mobile-integration.md#derived-keys "mention"). In the case of the callback, all these fields will be passed as URL parameters. Assuming, we've passed `callback=auth://derive`, the payload will be sent as a GET request like this:

```javascript
GET auth://derive?derivedSeedHex=...&derivedPublicKey=...&publicKey=...&
btcDepositAddress=...&ethDepositAddress=...&expirationBlock=...&network=...&
accessSignature=...&jwt=...&derivedJwt=...
```

## get-shared-secrets

This endpoint is intended to be used in combination with derived keys. It allows you to get message encryption/decryption keys, or shared secrets, so that you can read messages without querying Identity (as is traditionally done via [iframe-api](../iframe-api/ "mention")). The `get-shared-secrets` endpoint requires a callback as well as other URL parameters that are used to verify ownership of the derived key. Another required URL parameter is `messagePublicKeys` which are the public keys of messaging parties in [CSV format](https://en.wikipedia.org/wiki/Comma-separated\_values#Basic\_rules) for which we want to fetch shared secrets. The `get-shared-secrets` endpoint allows to fetch multiple shared secrets at once. For example, if the owner of the derived key was public key `A`, and we wanted to read conversations between parties `B`,`C`,`D` (or more) we would set `ownerPublicKey=A` and `messagePublicKeys=B,C,D` .

#### Request

```javascript
const secrets = window.open('https://identity.deso.org/get-shared-secrets');
```

#### URL Parameters

| Name               | Type                                                                                      | Description                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| callback           | string                                                                                    | Callback URL for the payload as explained in [#callbacks](./#callbacks "mention") |
| ownerPublicKey     | string                                                                                    | Master public key that was used to get the derived key                            |
| derivedPublicKey   | string                                                                                    | Derived public key                                                                |
| JWT                | string                                                                                    | JWT signed by the derived key, it's passed as `derivedJwt` in `/derive`           |
| messagePublicKeys  | strings, [CSV format](https://en.wikipedia.org/wiki/Comma-separated\_values#Basic\_rules) | Public keys of users for which we want to fetch shared secrets.                   |
| testnet (optional) | bool                                                                                      | Whether we're on testnet or mainnet. Default is `false`                           |

#### Response

The response is a list of shared secrets in CSV format corresponding to `messagePublicKeys` . The payload will always be sent as a [#callbacks](./#callbacks "mention").

```javascript
GET callback?sharedSecrets=ccf9474d7578658e0c77adb7aea5cc9b61d3bccad5bddddd11e1850dfa4db059,
    db7aea5cc9b61d3b..., a11e1850dfa4db059...
```

## get-free-deso

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

## verify-phone-number

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
