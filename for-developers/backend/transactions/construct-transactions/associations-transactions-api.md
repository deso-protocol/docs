---
description: >-
  Description of endpoints to construct Associations Transactions on the DeSo
  blockchain
---

# Associations Transactions API

### User Associations

{% swagger method="post" path="create" baseUrl="/api/v0/user-associations/" summary="Create user association" %}
{% swagger-description %}
Creates a create user association transaction. The transaction needs to be signed and submitted through `/api/v0/submit-transaction` before changes come into effect.

Implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/associations.go#L152)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactorPublicKeyBase58Check" type="String" required="true" %}
The public key of the user creating the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TargetUserPublicKeyBase58Check" type="String" required="true" %}
The public key of the user to which the association is referencing
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AppPublicKeyBase58Check" type="String" %}
The public key of the application on which the association is being created
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationType" type="String" required="true" %}
The association type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationValue" type="String" required="true" %}
The association value
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
Any additional arbitrary key-value data to store with the association
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
The minimum fee rate (in nanos) per kb
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="[]TransactionFee" %}
An array of 

[TransactionFee](../basics/data-types.md)

 objects that define additional outputs to add to the transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a create user association transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "SpendAmountNanos": 0,
    "TotalInputNanos": 999999594,
    "ChangeAmountNanos": 999999032,
    "FeeNanos": 562,
    "Transaction": {
        "TxInputs": [
            {
                "TxID": [
                    53,
                    195,
                    47,
                    166,
                    22,
                    216,
                    106,
                    179,
                    186,
                    225,
                    130,
                    110,
                    104,
                    152,
                    74,
                    215,
                    74,
                    183,
                    183,
                    53,
                    148,
                    68,
                    5,
                    24,
                    140,
                    59,
                    26,
                    222,
                    129,
                    155,
                    71,
                    154
                ],
                "Index": 0
            }
        ],
        "TxOutputs": [
            {
                "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
                "AmountNanos": 999999032
            }
        ],
        "TxnMeta": {
            "TargetUserPublicKey": [
                3,
                95,
                255,
                160,
                105,
                4,
                12,
                4,
                60,
                209,
                243,
                177,
                97,
                96,
                27,
                254,
                116,
                12,
                121,
                196,
                61,
                110,
                188,
                220,
                197,
                18,
                109,
                28,
                59,
                216,
                172,
                68,
                224
            ],
            "AppPublicKey": [
                2,
                88,
                191,
                20,
                43,
                67,
                244,
                2,
                20,
                110,
                45,
                7,
                44,
                158,
                243,
                19,
                216,
                99,
                248,
                244,
                90,
                149,
                247,
                213,
                172,
                230,
                29,
                245,
                127,
                216,
                190,
                252,
                93
            ],
            "AssociationType": "RU5ET1JTRU1FTlQ=",
            "AssociationValue": "U1FM"
        },
        "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
        "ExtraData": {
            "PeerID": "QQ=="
        },
        "Signature": {
            "Sign": null,
            "RecoveryId": 0,
            "IsRecoverable": false
        },
        "TxnTypeJSON": 27
    },
    "TransactionHex": "0135c32fa616d86ab3bae1826e68984ad74ab7b735944405188c3b1ade819b479a00010342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b3b88cebdc031b5421035fffa069040c043cd1f3b161601bfe740c79c43d6ebcdcc5126d1c3bd8ac44e0210258bf142b43f402146e2d072c9ef313d863f8f45a95f7d5ace61df57fd8befc5d0b454e444f5253454d454e540353514c210342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b30106506565724944014100",
    "TxnHashHex": "7a8fec83970a0a564467c05ecf335b86d596ba090012584e34c691641ce70d3f"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="delete" baseUrl="/api/v0/user-associations/" summary="Delete user association" %}
{% swagger-description %}
Creates a delete user association transaction. The transaction needs to be signed and submitted through `/api/v0/submit-transaction` before changes come into effect.

Implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/associations.go#L265)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactorPublicKeyBase58Check" type="String" required="true" %}
The public key of the user creating the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationID" type="String" required="true" %}
The identifier of the association to delete
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
Any additional arbitrary key-value data to include with the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
The minimum fee rate (in nanos) per kb
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="[]TransactionFee" %}
An array of 

[TransactionFee](../basics/data-types.md)

 objects that define additional outputs to add to the transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a delete user association transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "SpendAmountNanos": 0,
    "TotalInputNanos": 999999032,
    "ChangeAmountNanos": 999998590,
    "FeeNanos": 442,
    "Transaction": {
        "TxInputs": [
            {
                "TxID": [
                    22,
                    224,
                    4,
                    226,
                    226,
                    96,
                    82,
                    152,
                    255,
                    93,
                    21,
                    79,
                    44,
                    114,
                    203,
                    120,
                    59,
                    196,
                    155,
                    171,
                    147,
                    53,
                    123,
                    32,
                    255,
                    129,
                    135,
                    193,
                    129,
                    195,
                    178,
                    57
                ],
                "Index": 0
            }
        ],
        "TxOutputs": [
            {
                "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
                "AmountNanos": 999998590
            }
        ],
        "TxnMeta": {
            "AssociationID": [
                22,
                224,
                4,
                226,
                226,
                96,
                82,
                152,
                255,
                93,
                21,
                79,
                44,
                114,
                203,
                120,
                59,
                196,
                155,
                171,
                147,
                53,
                123,
                32,
                255,
                129,
                135,
                193,
                129,
                195,
                178,
                57
            ]
        },
        "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
        "ExtraData": {},
        "Signature": {
            "Sign": null,
            "RecoveryId": 0,
            "IsRecoverable": false
        },
        "TxnTypeJSON": 28
    },
    "TransactionHex": "0116e004e2e2605298ff5d154f2c72cb783bc49bab93357b20ff8187c181c3b23900010342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b3fe88ebdc031c212016e004e2e2605298ff5d154f2c72cb783bc49bab93357b20ff8187c181c3b239210342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b30000",
    "TxnHashHex": "ad731a94d56ead5850d595b45c86d18981e561a40036683ce9f40b9bc9f8ef93"
}

```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endswagger-response %}
{% endswagger %}

### Post Associations

{% swagger method="post" path="create" baseUrl="/api/v0/post-associations/" summary="Create post association" %}
{% swagger-description %}
Creates a create post association transaction. The transaction needs to be signed and submitted through `/api/v0/submit-transaction` before changes come into effect.

Implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/associations.go#L619)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactorPublicKeyBase58Check" type="String" required="true" %}
The public key of the user creating the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="PostHashHex" type="String" required="true" %}
The identifier of the post to which the association is referencing
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AppPublicKeyBase58Check" type="String" %}
The public key of the application on which the association is being created
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationType" type="String" required="true" %}
The association type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationValue" type="String" required="true" %}
The association value
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
Any additional arbitrary key-value data to store with the association
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
The minimum fee rate (in nanos) per kb
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="[]TransactionFee" %}
An array of 

[TransactionFee](../basics/data-types.md)

 objects that define additional outputs to add to the transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a create post association transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "SpendAmountNanos": 0,
    "TotalInputNanos": 999998132,
    "ChangeAmountNanos": 999997574,
    "FeeNanos": 558,
    "Transaction": {
        "TxInputs": [
            {
                "TxID": [
                    185,
                    36,
                    7,
                    40,
                    237,
                    109,
                    64,
                    8,
                    28,
                    148,
                    5,
                    239,
                    73,
                    214,
                    19,
                    60,
                    251,
                    248,
                    92,
                    246,
                    20,
                    89,
                    19,
                    199,
                    82,
                    232,
                    165,
                    207,
                    27,
                    140,
                    207,
                    63
                ],
                "Index": 0
            }
        ],
        "TxOutputs": [
            {
                "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
                "AmountNanos": 999997574
            }
        ],
        "TxnMeta": {
            "PostHash": [
                185,
                36,
                7,
                40,
                237,
                109,
                64,
                8,
                28,
                148,
                5,
                239,
                73,
                214,
                19,
                60,
                251,
                248,
                92,
                246,
                20,
                89,
                19,
                199,
                82,
                232,
                165,
                207,
                27,
                140,
                207,
                63
            ],
            "AppPublicKey": [
                2,
                88,
                191,
                20,
                43,
                67,
                244,
                2,
                20,
                110,
                45,
                7,
                44,
                158,
                243,
                19,
                216,
                99,
                248,
                244,
                90,
                149,
                247,
                213,
                172,
                230,
                29,
                245,
                127,
                216,
                190,
                252,
                93
            ],
            "AssociationType": "UkVBQ1RJT04=",
            "AssociationValue": "SEVBUlQ="
        },
        "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
        "ExtraData": {
            "PeerID": "Qg=="
        },
        "Signature": {
            "Sign": null,
            "RecoveryId": 0,
            "IsRecoverable": false
        },
        "TxnTypeJSON": 29
    },
    "TransactionHex": "01b9240728ed6d40081c9405ef49d6133cfbf85cf6145913c752e8a5cf1b8ccf3f00010342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b38681ebdc031d5220b9240728ed6d40081c9405ef49d6133cfbf85cf6145913c752e8a5cf1b8ccf3f210258bf142b43f402146e2d072c9ef313d863f8f45a95f7d5ace61df57fd8befc5d085245414354494f4e054845415254210342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b30106506565724944014200",
    "TxnHashHex": "d886a0fdcde9245f3799679b290f7c808df0f93396cada2996e13320b7688982"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="delete" baseUrl="/api/v0/post-associations/" summary="Delete post association" %}
{% swagger-description %}
Creates a delete post association transaction. The transaction needs to be signed and submitted through `/api/v0/submit-transaction` before changes come into effect.

Implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/associations.go#L731)
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactorPublicKeyBase58Check" type="String" required="true" %}
The public key of the user creating the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AssociationID" type="String" %}
The identifier of the association being deleted
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
Any additional arbitrary key-value data to include with the transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
The minimum fee rate (in nanos) per kb
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="[]TransactionFee" %}
An array of 

[TransactionFee](../basics/data-types.md)

 objects that define additional outputs to add to the transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a delete post association transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```javascript
{
    "SpendAmountNanos": 0,
    "TotalInputNanos": 999997574,
    "ChangeAmountNanos": 999997132,
    "FeeNanos": 442,
    "Transaction": {
        "TxInputs": [
            {
                "TxID": [
                    80,
                    45,
                    189,
                    249,
                    80,
                    253,
                    175,
                    41,
                    234,
                    210,
                    82,
                    131,
                    175,
                    1,
                    114,
                    246,
                    63,
                    83,
                    147,
                    58,
                    39,
                    158,
                    19,
                    145,
                    133,
                    89,
                    183,
                    250,
                    43,
                    243,
                    227,
                    0
                ],
                "Index": 0
            }
        ],
        "TxOutputs": [
            {
                "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
                "AmountNanos": 999997132
            }
        ],
        "TxnMeta": {
            "AssociationID": [
                80,
                45,
                189,
                249,
                80,
                253,
                175,
                41,
                234,
                210,
                82,
                131,
                175,
                1,
                114,
                246,
                63,
                83,
                147,
                58,
                39,
                158,
                19,
                145,
                133,
                89,
                183,
                250,
                43,
                243,
                227,
                0
            ]
        },
        "PublicKey": "A0LZQ7jbqTpM4puFhHnGfx5PERDuy+H4PcAbRV64sSOz",
        "ExtraData": {},
        "Signature": {
            "Sign": null,
            "RecoveryId": 0,
            "IsRecoverable": false
        },
        "TxnTypeJSON": 30
    },
    "TransactionHex": "01502dbdf950fdaf29ead25283af0172f63f53933a279e13918559b7fa2bf3e30000010342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b3ccfdeadc031e2120502dbdf950fdaf29ead25283af0172f63f53933a279e13918559b7fa2bf3e300210342d943b8dba93a4ce29b858479c67f1e4f1110eecbe1f83dc01b455eb8b123b30000",
    "TxnHashHex": "0009a31ccc2210a40cb3e44aa7df45fdc15f58b09b95d9349648fb578141ac88"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon!
{% endtab %}
{% endtabs %}


{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid parameters provided" %}
```javascript
{
    "error": "string"
}
```
{% endswagger-response %}
{% endswagger %}
