# Identity API

The BitClout Identity service provides a convenient and secure way for users to login to many different BitClout nodes and applications. When using BitClout Identity, users' private key material never leaves the browser. All signing happens in a secured `iframe` and transaction approvals occur in a pop-up window.

The developer community highly recommends node operators and app developers integrate with [identity.bitclout.com](https://identity.bitclout.com) to provide users with consistent log in, sign up, and account management experiences. Identity currently integrates most smoothly with web-based applications. The developer community is working on creating libraries for integrating with iOS and Android.

## Message Protocol

Applications interact with Identity in two ways: embedding Identity in an `iframe` and opening Identity in a new window via `window.open`. The `iframe` context can handle transaction signing and message decryption. The `window.open` context can handle log in, sign up, log out, and account management. Both `iframe` and `window.open` contexts communicate with the parent via `window.postMessage` .

A few notes about message formats:

* Messages with an `id` and `method` are requests that expect a response.
* Messages with an `id` and no `method` are responses to requests.
* Messages without an `id` do not expect a response.
* UUID v4 is the recommended `id` format.

### `initialize`

The first message Identity sends to the parent when it loads in is `initialize`. This message is sent in both `iframe` and `window.open` contexts. A response is required so Identity knows the hostname of the parent window.

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'initialize',
}
```

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
}
```

## `window.open` context

Opening Identity using window.open allows an application to send and receive messages to the newly opened tab or pop-up. A new Identity window can be opened at many paths:

```javascript
const login   = window.open('https://identity.bitclout.com/log-in');
const signUp  = window.open('https://identity.bitclout.com/sign-up');
const logout  = window.open('https://identity.bitclout.com/logout?publicKey=BC123');
const approve = window.open('https://identity.bitclout.com/approve?tx=0abf35a');

// Can be added to any path for testnet bitclout and bitcoin addresses
const testnet = window.open('https://identity.bitclout.com/log-in?testnet=true');
```

Only one Identity window should be opened at a time.

### Access Levels

Users can control access level on a per-domain and per-account basis. The available access levels are:

```javascript
enum AccessLevel {
  // User revoked permissions
  None = 0,

  // Approval required for all transactions
  ApproveAll = 2,

  // Approval required for buys, sends, and sells
  ApproveLarge = 3,

  // Node can sign all transactions without approval
  Full = 4,
}
```

An application can specify which access level it would like to request by including `accessLevelRequest` as a query parameter when opening the Identity window. If no `accessLevelRequest` is specified then `ApproveAll` is used as the default.

### `login`

When a user finishes any action in an Identity window a `login` message is sent. The login message does not expect a response and means the Identity window can be closed by calling `window.close` on the stored reference to the window.open.

For a log in or sign up action the selected `publicKey` will be included in `publicKeyAdded`. When a user approves a transaction the signed transaction will be included in `signedTransactionHex`.

An application should store the current `publicKey` and `users` objects in its local storage. When an application wants to sign or decrypt something the `accessLevel`, `accessLevelHmac`, and `encryptedSeedHex` values will be required.

#### Request

```javascript
{
  users: {
    BC1YLfsWMfv8UdytwrWqWvqSP6M6eQJg7W5TWL1WNDYd7zxi6wEShQX: {
      accessLevel: 4,
      accessLevelHmac: "0d22e283751c904ab36dc3910afe1a981...",
      btcDepositAddress: "1PXhm3D6sgZtfGNe2mtP27NVBHEcNJX2AW",
      encryptedSeedHex: "bdad93a19eb3be8b4c2f63b5cefb82823...",
      hasExtraText: false,
      network: "mainnet",
    },
  },
  publicKeyAdded: '',
  signedUp: false,
  signedTransactionHex: '',
}
```

## `iframe` context

The iframe is responsible for signing and decryption. The iframe is usually entirely invisible to the user. However, the iframe does need to render when the user needs to grant storage access on Safari.

```markup
<iframe
  id="identity"
  frameborder="0"
  src="https://identity.bitclout.com/embed"
  style="height: 100vh; width: 100vw;"
  [style.display]="requestingStorageAccess ? 'block' : 'none'"
></iframe>
```

### `info`

The iframe responds to `info` messages which helps Identity support Safari and Chrome on iOS. Apple's Intelligent Tracking Prevention \(ITP\) places strict limitations on cross-domain data storage and access. This means the Identity `iframe` must request storage access every time the page reloads. When a user visits a BitClout application in Safari they will see a "Tap anywhere to unlock your wallet" prompt which is a giant button in the `iframe`. When the `info` message returns `hasStorageAccess: false`, an application should make the `iframe` take over the entire page. Above, this means setting `requestingStorageAccess = true`.

The `info` message also detects if a user has disabled third party cookies. Third party cookies are required for Identity to securely sign transactions. If `info` returns `browserSupported: false` an application should inform the user they will not be able to use Identity to sign or decrypt anything.

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'info',
}
```

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    hasStorageAccess: true,
    browserSupported: true,
  },
}
```

### `storageGranted`

The `iframe` sends a `storageGranted` message when a user clicks "Tap anywhere to unlock your wallet." It does not expect a response. When an application receives this message it can hide the `iframe` from view and the `iframe` is now ready to receive `sign` and `decrypt` messages.

#### Request

```javascript
{
  service: 'identity',
  method: 'storageGranted',
}
```

### `sign`

The sign message is responsible for signing transaction hexes. If approval is required an application must call `window.open` to acquire a `signedTransactionHex`.

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'sign',
  payload: {
    accessLevel: 3,
    accessLevelHmac: "0fab13f4...",
    encryptedSeedHex: "0fab13f4...",
    transactionHex: "0fab13f4...",
  },
}
```

#### Response \(Success\)

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    signedTransactionHex: "0fab13f4...",
  },
}
```

#### Response \(Approval Required\)

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    approvalRequired: true,
  },
}
```

### `decrypt`

The decrypt message is responsible for decrypting messages.

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'decrypt',
  payload: {
    accessLevel: 3,
    accessLevelHmac: "0fab13f4...",
    encryptedSeedHex: "0fab13f4...",
    encryptedHexes: [
      "0fab13f4...",
      "0fab13f4...",
    ]
  },
}
```

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    decryptedHexes: {
      "0fab13f4...": "hello world"
      "0fab13f4...": "in retrospect it was inevitable",
    }
  },
}
```

### `jwt`

The `jwt` message creates signed JWT tokens that can be used to verify a user's ownership of a specific public key.

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'jwt',
  payload: {
    accessLevel: 3,
    accessLevelHmac: "0fab13f4...",
    encryptedSeedHex: "0fab13f4...",
  },
}
```

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    jwt: "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE2MTk2NDk4MDcsImV4cCI6MTYxOTY0OTg2N30.FKZF8DSwwlnUaW_eRa7Wr1v2QcG7_iDN-NjdqXUcgrSAPg1EdSfpWsLL4GeUiD9zdLUgrNoKU7EsKkE-ZKMaVQ",
  },
}
```

#### Validation in Go

```javascript
func ValidateJWT(publicKey string, jwtToken string) (bool, error) {
    pubKeyBytes, _, err := Base58CheckDecode(publicKey)
    if err != nil {
        return false, err
    }

    pubKey, err := btcec.ParsePubKey(pubKeyBytes, btcec.S256())
    if err != nil {
        return false, err
    }

    token, err := jwt.Parse(jwtToken, func(token *jwt.Token) (interface{}, error) {
        return pubKey.ToECDSA(), nil
    })

    return token.Valid, err
}
```

