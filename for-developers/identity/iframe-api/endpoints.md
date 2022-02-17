---
description: List of DeSo Identity iframe API endpoints
---

# Endpoints

## sign

[**AccessLevel**](../identity.md#access-levels)**: 3, 4 (depends on** [**transaction**](https://github.com/deso-protocol/identity/blob/9dad527dc46498b9aaa0344abd70dc8895acf246/src/app/identity.service.ts#L288)**)**

The sign message is responsible for signing transaction hexes. If approval is required an application must call the [#approve](../window-api/#approve "mention") endpoint in the Window API to sign the transaction.

#### Payload

| Name           | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| transactionHex | string | Hex of the transaction you want to sign. |

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'sign',
  payload: {
    accessLevel: 4,
    accessLevelHmac: "0fab13f4...",
    encryptedSeedHex: "0fab13f4...",
    transactionHex: "0fab13f4...",
  },
}
```

#### Response (Success)

You will get this response if the transaction was successful signed.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    signedTransactionHex: "0fab13f4...",
  },
}
```

#### Response (Approval Required)

You will get this response if the `accessLevel` your user has authorized doesn't match the access level required to sign a transaction.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    approvalRequired: true,
  },
}
```

## encrypt

[**AccessLevel**](../identity.md#access-levels)**: 2**

The encrypt API is responsible for encrypting messages. For more details check out [#messages](../identity.md#messages "mention")

#### Payload

| Name               | Type   | Description                                       |
| ------------------ | ------ | ------------------------------------------------- |
| recipientPublicKey | string | Public key of the recipient in base58check format |
| message            | string | Message text that you want to encrypt             |

#### Request

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  method: 'encrypt',
  payload: {
    accessLevel: 3,
    accessLevelHmac: "0fab13f4...",
    encryptedSeedHex: "0fab13f4...",
    recipientPublicKey: "BC1YLgwkd7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ"
    message: "This is a message",
  },
}
```

#### Response

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    encryptedMessage: "0fab13f4...",
  },
}
```

## decrypt

[**AccessLevel**](../identity.md#access-levels)**: 2**

The decrypt API is responsible for decrypting messages. As we mentioned in the [#messages](../identity.md#messages "mention") section, the current messaging protocol is `V2`; however, it is still possible to decrypt messages from the `V1` scheme. The decrypt API allows you to decrypt multiple messages at once by passing an array of `encryptedMessage` objects. The `decrypt` API is intended to be constructed right after calling the `/api/v0/get-messages-stateless` backend API endpoint, and so the structure of `encryptedMessage` matches the structure of the response from backend. We recommend tracing through [`GetMessages()` ](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/backend-api.service.ts#L1293)function in the DeSo Protocol frontend's `src/app/backend-api.service.ts`.

Assuming `message` is a taken from `OrderedContactsWithMessages.Messages` from the backend API response, `encryptedMessage` can be constructed as follows:

```
encryptedMessage : {
    EncryptedHex: message.EncryptedText,
    PublicKey: message.IsSender ? message.RecipientPublicKeyBase58Check : message.SenderPublicKeyBase58Check,
    IsSender: message.IsSender,
    Legacy: !message.V2,
}
```

Another use-case for the `decrypt` API is decrypting unlockable text in NFTs. To see how this can be done, we recommend tracing through the [`DecryptUnlockableTexts()`](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/backend-api.service.ts#L945) in the DeSo Protocol repository.

#### Payload

| Name              | Type                | Description                        |
| ----------------- | ------------------- | ---------------------------------- |
| encryptedMessages | \[]encryptedMessage | List of encrypted messages objects |

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
    encryptedMessage: [
      {
        EncryptedHex: "0f154bcad...",
        PublicKey: "BC1a4gbK1...",
        IsSender: true,
        Legacy: false
      },
      {
        EncryptedHex: "0afa44bcd...",
        PublicKey: "BC1asKl5j1...",
        IsSender: false,
        Legacy: true
      }, ...
    ]
  },
}
```

#### Response

Response contains a `decryptedHexes` field which is a map of decrypted messages, indexed by `EncryptedHex` from the request.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    decryptedHexes: {
      "0f154bcad...": "hello world"
      "0afa44bcd...": "in retrospect it was inevitable",
    }
  },
}
```

## jwt

[**AccessLevel**](../identity.md#access-levels)**: 2**

The `jwt` message creates signed JWT tokens that can be used to verify a user's ownership of a specific public key. The JWT is only valid for 10 minutes. JWTs are used in some Backend API endpoints such as `/api/v0/upload-image.` The best practice is to request the JWT right before calling these endpoints.

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

In case you want to validate the JWT token in Go, you could use the code below:

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
