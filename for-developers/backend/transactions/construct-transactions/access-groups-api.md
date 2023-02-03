# Access Groups API

{% swagger method="post" path="" baseUrl="/api/v0/create-access-group" summary="Create Access Group" %}
{% swagger-description %}
Prepare an access group transaction to create a new access group. Transaction needs to be signed and submitted through 

`api/v0/submit-transaction`

 before changes come into effect. Endpoint implementation in 

[backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access_group.go#L176)

.
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the user creating the access group. This user will be the owner of the access group.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group that is being created.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Name of the access group that is being created.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TotalInputNanos": 99967828,
  "ChangeAmountNanos": 99967562,
  "FeeNanos": 266,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          200,
          13,
          13,
          151,
          191,
          238,
          44,
          73,
          203,
          166,
          3,
          131,
          33,
          228,
          244,
          13,
          50,
          169,
          228,
          12,
          44,
          74,
          52,
          254,
          114,
          112,
          84,
          50,
          182,
          57,
          30,
          62
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99967562
      }
    ],
    "TxnMeta": {
      "AccessGroupOwnerPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
      "AccessGroupPublicKey": "AxJsKZRZjnXENR0XCY1fL3kTHwLUu7OLfpJhcxraza2+",
      "AccessGroupKeyName": "ZGVtb2NoYXQ=",
      "AccessGroupOperationType": 2
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 31
  },
  "TransactionHex": "01c80d0d97bfee2c49cba6038321e4f40d32a9e40c2c4a34fe72705432b6391e3e000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45cac4d52f1f4e2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452103126c2994598e75c4351d17098d5f2f79131f02d4bbb38b7e9261731adacdadbe0864656d6f63686174022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/update-access-group" summary="Update Access Group" %}
{% swagger-description %}
Prepare an access group transaction to update an existing access group. Transaction needs to be signed and submitted through 

`api/v0/submit-transaction`

 before changes come into effect. Endpoint implementation in 

[backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access_group.go#L183)

.
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the user updating their access group. This must be the access group owner.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group that is being updated.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Name of the access group that is being updated.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Coming soon!" %}
```javascript
{
  "TotalInputNanos": 99967828,
  "ChangeAmountNanos": 99967562,
  "FeeNanos": 266,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          200,
          13,
          13,
          151,
          191,
          238,
          44,
          73,
          203,
          166,
          3,
          131,
          33,
          228,
          244,
          13,
          50,
          169,
          228,
          12,
          44,
          74,
          52,
          254,
          114,
          112,
          84,
          50,
          182,
          57,
          30,
          62
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99967562
      }
    ],
    "TxnMeta": {
      "AccessGroupOwnerPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
      "AccessGroupPublicKey": "AxJsKZRZjnXENR0XCY1fL3kTHwLUu7OLfpJhcxraza2+",
      "AccessGroupKeyName": "ZGVtb2NoYXQ=",
      "AccessGroupOperationType": 3
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 31
  },
  "TransactionHex": "01c80d0d97bfee2c49cba6038321e4f40d32a9e40c2c4a34fe72705432b6391e3e000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45cac4d52f1f4e2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452103126c2994598e75c4351d17098d5f2f79131f02d4bbb38b7e9261731adacdadbe0864656d6f63686174022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/add-access-group-members" summary="Add Access Group Members" %}
{% swagger-description %}
Prepare an access group member transaction to add new members to an access group. Transaction needs to be signed and submitted through 

`api/v0/submit-transaction`

 before changes come into effect. Endpoint implementation in 

[backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access_group.go#L435)

.
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group owner. Must be the public key signing this transaction.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Name of the access group to which the members will be added.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupMemberList" type="AccessGroupMember[]" required="true" %}
Array of 

[#accessgroupmember](../basics/data-types.md#accessgroupmember "mention")

 objects representing the users to be added to the access group.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TotalInputNanos": 99967562,
  "ChangeAmountNanos": 99966522,
  "FeeNanos": 1040,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          128,
          156,
          232,
          236,
          161,
          203,
          117,
          179,
          124,
          43,
          53,
          142,
          175,
          117,
          106,
          235,
          241,
          60,
          176,
          22,
          120,
          33,
          81,
          146,
          181,
          10,
          226,
          106,
          73,
          216,
          212,
          79
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99966522
      }
    ],
    "TxnMeta": {
      "AccessGroupOwnerPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
      "AccessGroupKeyName": "ZGVtb2NoYXQ=",
      "AccessGroupMembersList": [
        {
          "AccessGroupMemberPublicKey": "ApOA89iQNICFoi4H2Nqtnx83BnZ7/dWTN2QcTTIxBGUJ",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "MDRjZDUyN2RmZGZmMzVmODY0NDkwOTY4MTY1ZWY3ZTM0YTNjYjAwOWFiY2JkZTg2MDE2OTgzNGNhYzg4Yzk4Y2VjMTkyZjY2MWMyNjdkYzY2YmE2ZGVkNDZiOWVmMDczNTllMTc0ZDRmZWE4NzA1MmU4M2YxNzhjYjU0OTE0NWY4NTlhOTMzOGZhOGY0NjU3ZTc0ZmYyMjYxNDJmNDdmYmM0YzRhYWQ2MWNhOThmOGUyZjkwZWY0NTliYWFiYjg2ZWIyYjFhMTM3OGNhMmE2ODA0MWU5OTYwM2NhYzMzMDlhNzQ3ZDMwYzQzYmZiN2E1ODRhNzY4MjY1MGJlOGFhNDliNTU0NzBmNDJmOTQwN2FmMmVlMWNmY2ZkMDY5NzQ1MDQ2OTVkZmQzODNjYmU3ZDhlZmZkYTNmZTgxNWI3Njc1NzQzZjdkZTMyYmIzYTc1MzE4NzRmNjkwNGFlYmIxZjgw",
          "ExtraData": {}
        },
        {
          "AccessGroupMemberPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "MDRhYjUzNmFiNWMwOWEyOWExYWE1MzkzNmQ2ZmUzNGY2M2Y1YTgwNTVmODE3YjYzMThhOGM5NzNkOTM2NTM1ZjMxM2FiNzE5N2Q3OWUwNTE3Yjk0OWE1NTc0ZDUwZjk1Mzk1MDk1NTYwNTM4ZTgwMDdjZDM5YTgzZjE0MTYyODU1NGY4Mjk5OTdiNWMzYTYyNjNhN2Q3M2RhZjE3MWZjMmQwNThiZDZkZjUxMmNiN2NhN2ZmMGNlY2E5MzM1N2IyNjhlNTFiY2MxZWRhNTFjNzUwYTY2NTA1YmYzY2Q3NWFhMjgxZGU2MDM3ZTExNjY2ZGI5YzJlNzAwMTA3YTE3NmFmMzVmYTM2OGE3ZGQ2ODNiMGQyYjJlZGM0OWRkMmJlMmRkYjk0YTkzMDdkYWNhZGMxZWM3NTJkN2I2OTM2M2Y5MThiMzAwMmU2MTEzMGY5ODRiY2I5ODM4Y2IxZTM3ODg3",
          "ExtraData": {}
        }
      ],
      "AccessGroupMemberOperationType": 2
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 32
  },
  "TransactionHex": "01809ce8eca1cb75b37c2b358eaf756aebf13cb01678215192b50ae26a49d8d44f000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45babcd52f20d3062102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450864656d6f636861740221029380f3d890348085a22e07d8daad9f1f3706767bfdd59337641c4d32310465090b64656661756c742d6b6579e202303463643532376466646666333566383634343930393638313635656637653334613363623030396162636264653836303136393833346361633838633938636563313932663636316332363764633636626136646564343662396566303733353965313734643466656138373035326538336631373863623534393134356638353961393333386661386634363537653734666632323631343266343766626334633461616436316361393866386532663930656634353962616162623836656232623161313337386361326136383034316539393630336361633333303961373437643330633433626662376135383461373638323635306265386161343962353534373066343266393430376166326565316366636664303639373435303436393564666433383363626537643865666664613366653831356237363735373433663764653332626233613735333138373466363930346165626231663830002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450b64656661756c742d6b6579e20230346162353336616235633039613239613161613533393336643666653334663633663561383035356638313762363331386138633937336439333635333566333133616237313937643739653035313762393439613535373464353066393533393530393535363035333865383030376364333961383366313431363238353534663832393939376235633361363236336137643733646166313731666332643035386264366466353132636237636137666630636563613933333537623236386535316263633165646135316337353061363635303562663363643735616132383164653630333765313136363664623963326537303031303761313736616633356661333638613764643638336230643262326564633439646432626532646462393461393330376461636164633165633735326437623639333633663931386233303032653631313330663938346263623938333863623165333738383700022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/remove-access-group-members" summary="Remove Access Group Members" %}
{% swagger-description %}
Prepare an access group member transaction to remove members from an access group. Transaction needs to be signed and submitted through 

`api/v0/submit-transaction`

 before changes come into effect. Endpoint implementation in 

[backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access_group.go#L442)

.
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group owner. Must be the public key signing this transaction.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Name of the access group from which the members will be removed.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupMemberList" type="AccessGroupMember[]" required="true" %}
Array of 

[#accessgroupmember](../basics/data-types.md#accessgroupmember "mention")

 objects representing the users to be removed from the access group. Please note that EncryptedKey and ExtraData must be excluded from these objects when removing members.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TotalInputNanos": 99967562,
  "ChangeAmountNanos": 99966522,
  "FeeNanos": 1040,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          128,
          156,
          232,
          236,
          161,
          203,
          117,
          179,
          124,
          43,
          53,
          142,
          175,
          117,
          106,
          235,
          241,
          60,
          176,
          22,
          120,
          33,
          81,
          146,
          181,
          10,
          226,
          106,
          73,
          216,
          212,
          79
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99966522
      }
    ],
    "TxnMeta": {
      "AccessGroupOwnerPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
      "AccessGroupKeyName": "ZGVtb2NoYXQ=",
      "AccessGroupMembersList": [
        {
          "AccessGroupMemberPublicKey": "ApOA89iQNICFoi4H2Nqtnx83BnZ7/dWTN2QcTTIxBGUJ",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "",
          "ExtraData": {}
        },
        {
          "AccessGroupMemberPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "",
          "ExtraData": {}
        }
      ],
      "AccessGroupMemberOperationType": 3
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 32
  },
  "TransactionHex": "01809ce8eca1cb75b37c2b358eaf756aebf13cb01678215192b50ae26a49d8d44f000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45babcd52f20d3062102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450864656d6f636861740221029380f3d890348085a22e07d8daad9f1f3706767bfdd59337641c4d32310465090b64656661756c742d6b6579e202303463643532376466646666333566383634343930393638313635656637653334613363623030396162636264653836303136393833346361633838633938636563313932663636316332363764633636626136646564343662396566303733353965313734643466656138373035326538336631373863623534393134356638353961393333386661386634363537653734666632323631343266343766626334633461616436316361393866386532663930656634353962616162623836656232623161313337386361326136383034316539393630336361633333303961373437643330633433626662376135383461373638323635306265386161343962353534373066343266393430376166326565316366636664303639373435303436393564666433383363626537643865666664613366653831356237363735373433663764653332626233613735333138373466363930346165626231663830002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450b64656661756c742d6b6579e20230346162353336616235633039613239613161613533393336643666653334663633663561383035356638313762363331386138633937336439333635333566333133616237313937643739653035313762393439613535373464353066393533393530393535363035333865383030376364333961383366313431363238353534663832393939376235633361363236336137643733646166313731666332643035386264366466353132636237636137666630636563613933333537623236386535316263633165646135316337353061363635303562663363643735616132383164653630333765313136363664623963326537303031303761313736616633356661333638613764643638336230643262326564633439646432626532646462393461393330376461636164633165633735326437623639333633663931386233303032653631313330663938346263623938333863623165333738383700022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/update-access-group-members" summary="Update Access Group Members" %}
{% swagger-description %}
Prepare an access group member transaction to update a member in an access group. Note that you can only update the EncryptedKey and ExtraData attributes of an AccessGroupMember's entry. If you need to change the AccessGroupMemberKeyName, you'll need to remove and re-add them. Transaction needs to be signed and submitted through 

`api/v0/submit-transaction`

 before changes come into effect. Endpoint implementation in 

[backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/access_group.go#L449)

.
{% endswagger-description %}

{% swagger-parameter in="body" name="AccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group owner. Must be the public key signing this transaction.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupKeyName" type="String" required="true" %}
Name of the access group in which the members will be updated.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="AccessGroupMemberList" type="AccessGroupMember[]" required="true" %}
Array of 

[#accessgroupmember](../basics/data-types.md#accessgroupmember "mention")

 objects representing the users to be updated in this access group.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" %}
Array of 

[#transactionfee](../basics/data-types.md#transactionfee "mention")

 objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TotalInputNanos": 99967562,
  "ChangeAmountNanos": 99966522,
  "FeeNanos": 1040,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          128,
          156,
          232,
          236,
          161,
          203,
          117,
          179,
          124,
          43,
          53,
          142,
          175,
          117,
          106,
          235,
          241,
          60,
          176,
          22,
          120,
          33,
          81,
          146,
          181,
          10,
          226,
          106,
          73,
          216,
          212,
          79
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99966522
      }
    ],
    "TxnMeta": {
      "AccessGroupOwnerPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
      "AccessGroupKeyName": "ZGVtb2NoYXQ=",
      "AccessGroupMembersList": [
        {
          "AccessGroupMemberPublicKey": "ApOA89iQNICFoi4H2Nqtnx83BnZ7/dWTN2QcTTIxBGUJ",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "MDRjZDUyN2RmZGZmMzVmODY0NDkwOTY4MTY1ZWY3ZTM0YTNjYjAwOWFiY2JkZTg2MDE2OTgzNGNhYzg4Yzk4Y2VjMTkyZjY2MWMyNjdkYzY2YmE2ZGVkNDZiOWVmMDczNTllMTc0ZDRmZWE4NzA1MmU4M2YxNzhjYjU0OTE0NWY4NTlhOTMzOGZhOGY0NjU3ZTc0ZmYyMjYxNDJmNDdmYmM0YzRhYWQ2MWNhOThmOGUyZjkwZWY0NTliYWFiYjg2ZWIyYjFhMTM3OGNhMmE2ODA0MWU5OTYwM2NhYzMzMDlhNzQ3ZDMwYzQzYmZiN2E1ODRhNzY4MjY1MGJlOGFhNDliNTU0NzBmNDJmOTQwN2FmMmVlMWNmY2ZkMDY5NzQ1MDQ2OTVkZmQzODNjYmU3ZDhlZmZkYTNmZTgxNWI3Njc1NzQzZjdkZTMyYmIzYTc1MzE4NzRmNjkwNGFlYmIxZjgw",
          "ExtraData": {}
        },
        {
          "AccessGroupMemberPublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
          "AccessGroupMemberKeyName": "ZGVmYXVsdC1rZXk=",
          "EncryptedKey": "MDRhYjUzNmFiNWMwOWEyOWExYWE1MzkzNmQ2ZmUzNGY2M2Y1YTgwNTVmODE3YjYzMThhOGM5NzNkOTM2NTM1ZjMxM2FiNzE5N2Q3OWUwNTE3Yjk0OWE1NTc0ZDUwZjk1Mzk1MDk1NTYwNTM4ZTgwMDdjZDM5YTgzZjE0MTYyODU1NGY4Mjk5OTdiNWMzYTYyNjNhN2Q3M2RhZjE3MWZjMmQwNThiZDZkZjUxMmNiN2NhN2ZmMGNlY2E5MzM1N2IyNjhlNTFiY2MxZWRhNTFjNzUwYTY2NTA1YmYzY2Q3NWFhMjgxZGU2MDM3ZTExNjY2ZGI5YzJlNzAwMTA3YTE3NmFmMzVmYTM2OGE3ZGQ2ODNiMGQyYjJlZGM0OWRkMmJlMmRkYjk0YTkzMDdkYWNhZGMxZWM3NTJkN2I2OTM2M2Y5MThiMzAwMmU2MTEzMGY5ODRiY2I5ODM4Y2IxZTM3ODg3",
          "ExtraData": {}
        }
      ],
      "AccessGroupMemberOperationType": 4
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 32
  },
  "TransactionHex": "01809ce8eca1cb75b37c2b358eaf756aebf13cb01678215192b50ae26a49d8d44f000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45babcd52f20d3062102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450864656d6f636861740221029380f3d890348085a22e07d8daad9f1f3706767bfdd59337641c4d32310465090b64656661756c742d6b6579e202303463643532376466646666333566383634343930393638313635656637653334613363623030396162636264653836303136393833346361633838633938636563313932663636316332363764633636626136646564343662396566303733353965313734643466656138373035326538336631373863623534393134356638353961393333386661386634363537653734666632323631343266343766626334633461616436316361393866386532663930656634353962616162623836656232623161313337386361326136383034316539393630336361633333303961373437643330633433626662376135383461373638323635306265386161343962353534373066343266393430376166326565316366636664303639373435303436393564666433383363626537643865666664613366653831356237363735373433663764653332626233613735333138373466363930346165626231663830002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450b64656661756c742d6b6579e20230346162353336616235633039613239613161613533393336643666653334663633663561383035356638313762363331386138633937336439333635333566333133616237313937643739653035313762393439613535373464353066393533393530393535363035333865383030376364333961383366313431363238353534663832393939376235633361363236336137643733646166313731666332643035386264366466353132636237636137666630636563613933333537623236386535316263633165646135316337353061363635303562663363643735616132383164653630333765313136363664623963326537303031303761313736616633356661333638613764643638336230643262326564633439646432626532646462393461393330376461636164633165633735326437623639333633663931386233303032653631313330663938346263623938333863623165333738383700022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}
