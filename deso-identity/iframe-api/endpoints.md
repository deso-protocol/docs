---
description: List of DeSo Identity iframe API endpoints
---

# Endpoints

#### Note:

The iframe API supports functionality for both public keys and derived keys. You can determine key type from the login response by checking for `derivedPublicKeyBase58Check`.

## sign

[**AccessLevel**](broken-reference)**: 3, 4 (depends on** [**transaction**](https://github.com/deso-protocol/identity/blob/9dad527dc46498b9aaa0344abd70dc8895acf246/src/app/identity.service.ts#L288)**)**

The sign message is responsible for signing transaction hexes. If approval is required an application must call the [#approve](../window-api/#approve "mention") endpoint in the Window API to sign the transaction.

#### Payload for public keys

<table><thead><tr><th width="295.3333333333333">Name</th><th width="141">Type</th><th>Description</th></tr></thead><tbody><tr><td>transactionHex</td><td>string</td><td>Hex of the transaction you want to sign.</td></tr></tbody></table>

#### Payload for derived keys

| Name                        | Type   | Description                                                                                      |
| --------------------------- | ------ | ------------------------------------------------------------------------------------------------ |
| transactionHex              | string | Hex of the transaction you want to sign.                                                         |
| derivedPublicKeyBase58Check | string | Only required if logged in user is using a derived key to sign on behalf of an owner public key. |

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
    derivedPublicKeyBase58Check: "0fab13f4...",

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

[**AccessLevel**](broken-reference)**: 2**

The encrypt API is responsible for encrypting messages. For more details check out [#messages](../identity/concepts.md#messages "mention")

#### Payload for public keys

<table><thead><tr><th width="316.3333333333333">Name</th><th width="127">Type</th><th>Description</th></tr></thead><tbody><tr><td>recipientPublicKey</td><td>string</td><td>Public key of the recipient in base58check format.</td></tr><tr><td>message</td><td>string</td><td>Message text that you want to encrypt.</td></tr></tbody></table>

#### Payload for derived keys

Only required if logged in user is using a derived key to sign on behalf of an owner public key.

<table><thead><tr><th width="322"></th><th width="120"></th><th></th></tr></thead><tbody><tr><td>recipientPublicKey</td><td>string</td><td>Public key of the recipient in base58check format.</td></tr><tr><td>message</td><td>string</td><td>Message text that you want to encrypt.</td></tr><tr><td>encryptedMessagingKeyRandomness</td><td>string</td><td>This value is used in place of the <code>encryptedSeedHex</code> when encrypting the message.</td></tr><tr><td>derivedPublicKeyBase58Check</td><td>string</td><td>Public key requesting encryption in base58check format.</td></tr><tr><td>ownerPublicKeyBase58Check</td><td>string</td><td>Public key used  only for validation. </td></tr></tbody></table>

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
    derivedPublicKeyBase58Check: "BC1YLsond7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ",
    ownerPublicKeyBase58Check: "BC1YLdadd7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ",
    encryptedMessagingKeyRandomness: "837fab39...",
    
  },
}
```

#### Response for Derived keys (Encrypted Messaging Key Randomness Required)

You will get this response if the request includes a `derivedPublicKeyBase58Check` and does not include both `ownerPublicKeyBase58Check` and `encryptedMessagingKeyRandomness`.

```javascript
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    encryptedMessage: "",
    requiresEncryptedMessagingKeyRandomness: true,
  },
}
```

You can request Encrypted MessagingKeyRandomness by calling the messaging-group in the Window API.

#### Response (Approval Required)

You will get this response if the `accessLevel` your user has authorized doesn't match the access level required to sign a transaction.

To fix, the user needs to allow at least access level 2.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    approvalRequired: true,
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

[**AccessLevel**](broken-reference)**: 2**

The decrypt API is responsible for decrypting messages.

As we mentioned in the [#messages](../identity/concepts.md#messages "mention") section, the current messaging protocol is `V2`; however, it is still possible to decrypt messages from the `V1` scheme.

The decrypt API allows you to decrypt multiple messages at once by passing an array of `encryptedMessage` objects.

The `decrypt` API is intended to be constructed right after calling the `/api/v0/get-messages-stateless` backend API endpoint, and so the structure of `encryptedMessage` matches the structure of the response from backend.

We recommend tracing through [`GetMessages()` ](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/backend-api.service.ts#L1293)function in the DeSo Protocol frontend's `src/app/backend-api.service.ts`.

Assuming `message` is a taken from `OrderedContactsWithMessages.Messages` from the backend API response, `encryptedMessage` can be constructed as follows:

```javascript
encryptedMessage : {
    EncryptedHex: message.EncryptedText,
    PublicKey: message.IsSender ? message.RecipientPublicKeyBase58Check : message.SenderPublicKeyBase58Check,
    IsSender: message.IsSender,
    Legacy: !message.V2,
}
```

Another use-case for the `decrypt` API is decrypting unlock-able text in NFTs.

To see how this can be done, we recommend tracing through the [`DecryptUnlockableTexts()`](https://github.com/deso-protocol/frontend/blob/6d6225a8425f2fe7ad84a222027159333b2c754f/src/app/backend-api.service.ts#L945) in the DeSo Protocol repository.

#### Payload for public keys

| Name              | Type                | Description                         |
| ----------------- | ------------------- | ----------------------------------- |
| encryptedMessages | \[]encryptedMessage | List of encrypted messages objects. |

#### Payload for derived keys

<table><thead><tr><th></th><th width="201"></th><th></th></tr></thead><tbody><tr><td>derivedPublicKeyBase58Check</td><td>string</td><td>Public key requesting decryption in base58check format.</td></tr><tr><td>ownerPublicKeyBase58Check</td><td>string</td><td>Used to identify which messaging group member entry is used to decrypt group messages.</td></tr><tr><td>encryptedMessagingKeyRandomness</td><td>string</td><td>Required to decrypt the request.</td></tr><tr><td>encryptedMessages</td><td>[]encryptedMessage</td><td>List of encrypted messages objects.</td></tr></tbody></table>

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
    derivedPublicKeyBase58Check: "BC1YLsond7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ",
    ownerPublicKeyBase58Check: "BC1YLdadd7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ",
    encryptedMessagingKeyRandomness: "837fab39...",
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

#### Response (Encrypted Messaging Key Randomness Required)

You will get this response if the request includes a `derivedPublicKeyBase58Check` and does not include both `ownerPublicKeyBase58Check` and `encryptedMessagingKeyRandomness`.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    decryptedHexes: {}
    requiresEncryptedMessagingKeyRandomness: true,
  },
}
```

#### Response (Approval Required)

You will get this response if the `accessLevel` your user has authorized doesn't match the access level required to sign a transaction.

To fix, the user needs to allow at least access level 2.

```javascript
{
  id: '21e02080-0ef4-4056-a319-a66403f33768',
  service: 'identity',
  payload: {
    approvalRequired: true,
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

[**AccessLevel**](broken-reference)**: 2**

The `jwt` message creates signed JWT tokens that can be used to verify a user's ownership of a specific public key.

The JWT is only valid for 10 minutes.

JWTs are used in some Backend API endpoints such as `/api/v0/upload-image.` The best practice is to request the JWT right before calling these endpoints.

#### Payload for public keys

| Name | Type | Description |
| ---- | ---- | ----------- |
| N/A  | N/A  | No payload. |

#### payload for derived keys

<table><thead><tr><th width="348">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>derivedPublicKeyBase58Check</td><td>string </td><td>Informs Identity on how to sign the transaction.</td></tr></tbody></table>

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
    derivedPublicKeyBase58Check: "BC1YLsond7iADbrSgryTfXhMEcsF76cXDaWog4aDzoTunDb2DcZ3myZ",
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
func (fes *APIServer) ValidateJWT(publicKey string, jwtToken string) (bool, error) {
	pubKeyBytes, _, err := lib.Base58CheckDecode(publicKey)
	if err != nil {
		return false, errors.Wrapf(err, "Problem decoding public key")
	}

	pubKey, err := btcec.ParsePubKey(pubKeyBytes, btcec.S256())
	if err != nil {
		return false, errors.Wrapf(err, "Problem parsing public key")
	}

	token, err := jwt.Parse(jwtToken, func(token *jwt.Token) (interface{}, error) {
		// Do not check token issued at time. We still check expiration time.
		mapClaims := token.Claims.(jwt.MapClaims)
		delete(mapClaims, "iat")

		// We accept JWT signed by derived keys. To accommodate this, the JWT claims payload should contain the key
		// "derivedPublicKeyBase58Check" with the derived public key in base58 as value.
		if derivedPublicKeyBase58Check, isDerived := mapClaims[JwtDerivedPublicKeyClaim]; isDerived {
			// Parse the derived public key.
			derivedPublicKeyBytes, _, err := lib.Base58CheckDecode(derivedPublicKeyBase58Check.(string))
			if err != nil {
				return nil, errors.Wrapf(err, "Problem decoding derived public key")
			}
			derivedPublicKey, err := btcec.ParsePubKey(derivedPublicKeyBytes, btcec.S256())
			if err != nil {
				return nil, errors.Wrapf(err, "Problem parsing derived public key bytes")
			}
			// Validate the derived public key.
			utxoView, err := fes.mempool.GetAugmentedUniversalView()
			if err != nil {
				return nil, errors.Wrapf(err, "Problem getting utxoView")
			}
			blockHeight := uint64(fes.blockchain.BlockTip().Height)
			if err := utxoView.ValidateDerivedKey(pubKeyBytes, derivedPublicKeyBytes, blockHeight); err != nil {
				return nil, errors.Wrapf(err, "Derived key is not authorize")
			}

			return derivedPublicKey.ToECDSA(), nil
		}

		return pubKey.ToECDSA(), nil
	})

	if err != nil {
		return false, errors.Wrapf(err, "Problem verifying JWT token")
	}

	return token.Valid, nil
}
```
