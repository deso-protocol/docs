---
description: >-
  Description of endpoints used to construct Social Transactions on the DeSo
  blockchain
---

# Social Transactions API



{% swagger method="post" path="" baseUrl="/api/v0/update-profile" summary="Update Profile" %}
{% swagger-description %}
Create an Update profile transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Update profile transactions creates a profile update such as changing a username, description, profile picture, or founder reward.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L263).

Example usages in frontend:\
\- Make request to [Update Profile](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L263)\
\- Using Update Profile on the [Profile Update page](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/update-profile-page/update-profile/update-profile.component.ts#L170)
{% endswagger-description %}

{% swagger-parameter in="body" name="UpdaterPublicKeyBase58Chck" type="String" required="true" %}
Public key of updater
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ProfilePublicKeyBase58Check" type="String" required="true" %}
Public key of the profile if different from updater
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NewUsername" required="true" type="String" %}
Username
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NewDescription" required="true" type="String" %}
Description
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NewProfilePic" required="true" type="String" %}
Base64 encoded profile picture
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NewCreatorBasisPoints" type="uint64" required="true" %}
Founder reward in basis points - e.g. 1000 basis points = 10% founder reward
{% endswagger-parameter %}

{% swagger-parameter in="body" name="NewStakeMultipleBasisPoints" type="uint64" required="false" %}
Deprecated

~~Staking Reward~~
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsHidden" type="boolean" required="false" %}
Deprecated
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[string]string" required="false" %}
extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a profile
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFees]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successful construction of an update profile transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TotalInputNanos": 933084474,
  "ChangeAmountNanos": 933082094,
  "FeeNanos": 2380,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 933082094
      }
    ],
    "TxnMeta": {
      "ProfilePublicKey": null, // Public key of the profile being updated if different from transactor
      "NewUsername": "dGVzdGFz", // New username for this public key
      "NewDescription": "", // New descrpition for this public key
      "NewProfilePic": "ZGF0YTppbWFnZS93ZWJwO2Jhc2U2NCxVa2xHUmtZR0FBQlhSVU..." // New profile picture for this public key
      "NewCreatorBasisPoints": 10000, // New founder reward percentage in basis points 
      "NewStakeMultipleBasisPoints": 12500, // Deprecated
      "IsHidden": false // Is this profile hidden
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null, // Any additional keys in the ExtraData field of the request body will be included here
    "Signature": null,
    "TxnTypeJSON": 6
  },
  "TransactionHex": "015959c32e6e39a99ce8ca05835a291e4e0ba6b203138aecfc4a527e6d992dd2f9000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45eee7f6bc03068f11000674657374617300ff10646174613a696d6167652f776562703b6261736536342c556b6c47526b59474141425852554a51566c413457416f4141414149414141415977414154674141566c41344947594641414477467743644153706b41453841506e30326c30656b6f7949684e525a4f714a415069574d417667733550456455534b616f6f39454c4f733131766647725871792f7453494d683646666e67684c6f4d55724c6674536b6c6b4b676555556842646d584d6f4e6f516a53654b772b504e3166356f6764576154694a4a715975427a4c6c304e625359312b2b6a4f3434584e347149326c6c69485768484e55433677467a506e56796664492b446c55667a3053594c306a4b364139417074414850333367766d41344247354f75776c7443736f37494a5a34585861703975524c6f6f5541433573395736325a4f69427238723941584d462f4b4d6f586e463270634e423853506e786b4c364941442b377074595a70714b746f65494f4a64375877724c7844437a43784b7162644b5877526d73456553582b6e4f6f6536616f355837727a62352b4b7133524b4275336c6b4c34595944586f464d52435444514a6f36686a7a436b7572524c62765a4844494b776765534f4d4252507347776e42614f65723934534b692f397379397a624a674b665947686d5764366b4a2b5534476e51654e67663468746370593975513552782b7776596533422f5766346c57796a494150596d3934634c41524e427258384935354149565064396a3773776d59764f706b7738674c6a6d796c573351786c35594e52723550675767477934574f49625665797635645334793851473852616c664b4c555575355533533773746d582b486f7968575a6e6b6b51586b534235304a584a2f5541686f42785977614d683746537a515158705a72487735436a7935474f4b3344536449412b4861746d702b6d516e666859417276644d5a636565506453516b6438496c757851736b6b4a6c46774176787374664331526d3054616c4d383831714a727473394947316b625431652f55686e626e56634f386845346e454364484777764d505862706e6a57696142624a574b513263533444514b2b474f454e35322f495073582f444d716c57794f756f5168383751757933794b736a6d30594133692f4c64502b7a575a2b4d564461315932544b6b64517558546d4254646f42763135473459356d545158644a417733585a64514e2b67766675314134564a38663731712f304c6c362f614e5034557a6d6c76642f6f702b47394d664f6d50725651324945512b6c314b6c64784f4c64452b665877314e386c6f696f5478467a45673757787163384844727672345247674532675743485343792f56676b6342524a4a4b7452414c564b486e766e763379417a3838637a35494e517868624e6354472b326a78627a4a746831762b3845562b59663578685a762b4b4770796d65503558656847346435664f4a6848386a58613277493138754736364b4d4376676a6d354d6541322f3654764961736546774d31794f367a624a624d4c75776547513739792f3167415a45367844617272784c426a7459727975464930506d4e356e4f58482f412f4b472b4f6964304173316d7776697a47434e77346e41674d496d614c30765653665669317a6a524e5a4e496c6d362f446d6e434550376639594c4d746139304f595437474c6e417170656d63326d547a6d4c6a4c65547855646e684b56503532382b733946513944724a6476336151303741554655724b5436466c434a69784c6b474765393943523565483952372b586f4e6a6e6f374c556d47774e74525443345557384e4263464144303677784d5a6c4d57474c435a57304c6b6833446e6572675a644a766e714a44465a69714969776b6d2f6c6b574e2f53442f2f6e3659504e335635745663654735654970626f2b6d74323856634871767568567a4a6639657471767650324b36594a6943576e507175316f4e3461422b50633535492b3157707753464b4f654831326566472b4b76303274443879594a7a52413554627064575877324f716d7a4f415a4f453370337a636e305a314952327079535a6563704757666b6d7243666d414f2b426a705353426974546e6a6843374375364755772f657a75504b5463302f3446784c75695a5630464472596b50726c6d484a784973785232335936724451583630486763456b724f485957774c6a765632594474303541724e382b4d4159565939546e7851555a35744f537577314d6f41764e5a61306a474731366e554d6d696d5832514c6843732f485a587968345a6b704e4756637333746955556165704878304445767a656f62386d4375315a6d5073646b4173463741786b2f6373716a6f6d713430446c7453614b54622f732f7461756c6c4a476a425230466967497953634c577251624545574c4862533957553771536378583346304a4a363633664778736c7851524a386769696359336759346a6a4b7a65517547625a7469444339593471444a673067336e624d417377442b304d7735557a78446a4e44543670355a735034374f734d78365330536f4c37486f333165412b766d35616f4a745931456e6a4e45566f383750706a4f424376554b6e397078555037334b6b4f787333775355557030414556595355613641414141525868705a67414153556b7141416741414141474142494241774142414141414151414141426f42425141424141414156674141414273424251414241414141586741414143674241774142414141414167414141424d4341774142414141414151414141476d4842414142414141415a67414141414141414141345977414136414d414144686a4141446f41774141426741416b41634142414141414441794d5441426b5163414241414141414543417741416f41634142414141414441784d4441426f414d414151414141502f2f414141436f4151414151414141475141414141446f41514141514141414538414141414141414141904ed461002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000",
  "TxnHashHex": "0f042cc71b1489a81d4342e61b7f4bc6d60cf81579fe629dd94a68211730a027"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/submit-post" summary="Submit Post" %}
{% swagger-description %}
Create a submit post transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect. \\

Submit post transactions handle all manipulation of posts. It can be used to create a post, update a post, hide a post, or repost a post.\
\
Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L1200).\
\
Example usages in frontend:\
\- Make Request to [Submit Post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1035)\
\- Using SubmitPost function to [create a new post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-create-post/feed-create-post.component.ts#L186)
{% endswagger-description %}

{% swagger-parameter in="body" name="UpdaterPulicKeyBase58Check" type="String" required="true" %}
Public key of the user who is making or updating the post
{% endswagger-parameter %}

{% swagger-parameter in="body" name="PostHashHexToModify" type="String" required="false" %}
When provided, update the post that matches this hash instead of creating a new one
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ParentStakeID" type="String" required="false" %}
PostHashHex of the parent of this post if applicable. When set, it creates a comment on the parent
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="BodyObj" type="{ Body: String, ImageURLs: String[], VideoURLs: String[]" %}
The body of the post
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RepostedPostHashHex" type="String" required="false" %}
Hash of post that this post is reposting
{% endswagger-parameter %}

{% swagger-parameter in="body" name="PostExtraData" type="Map[String]String" required="false" %}
extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a post
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsHidden" type="Boolean" required="false" %}
When true, this post will be hidden. You can't hide a post when creating it, you must submit a second transaction to hide the post.
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="MinFeeRateNanosPerKB" type="uint64" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="InTutorial" type="Boolean" required="false" %}
When true, perform additional checks to ensure user is at the correct point in the tutorial to execute a submit post transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successful construction of a submit post transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TstampNanos": 1637774290019245600,
  "PostHashHex": "ef782e8a43e8ebcadbb869fc8c74ebaa263dad2eb79c4c39906ac201d7863e1e",
  "TotalInputNanos": 933082094,
  "ChangeAmountNanos": 933081867,
  "FeeNanos": 227,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 933081867
      }
    ],
    "TxnMeta": {
      "PostHashToModify": "ef782e8a43e8ebcadbb869fc8c74ebaa263dad2eb79c4c39906ac201d7863e1e", // Hex of Post Hash created or modified by this transaction
      "ParentStakeID": "5959c32e6e39a99ce8ca05835a291e4e0ba6b203138aecfc4a527e6d992dd2f9", // Hex of Parent Post hash created or modified by this transaction
      "Body": "eyJCb2R5IjoidGVzdCJ9", // Body of the post
      "CreatorBasisPoints": 1000, // deprecated 
      "StakeMultipleBasisPoints": 12500, // deprecated
      "TimestampNanos": 1637774290019245600, // timestamp of post creation
      "IsHidden": false // If true, this post is not included in responses from any endpoitns
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {
      "Node": "MQ=="
    },
    "Signature": null,
    "TxnTypeJSON": 5
  },
  "TransactionHex": "01e206ccc2406e80b6544af58e8c0e7415bfc064c80fb0d57855299b25c8adfb52000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c458be6f6bc03052000000f7b22426f6479223a2274657374227de807d461d9bcf5d6a1e1a2dd16002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4501044e6f6465013100"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/create-follow-txn-stateless" summary="Follow" %}
{% swagger-description %}
Create a follow/unfollow transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Follow transactions adds the creator to the list of creators followed by the follower. This creator will now appear on the creator's following feed.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L1463).

Example usages in frontend:\
\- Make request to [create follow txn stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1291).\
\- Use CreateFollowTxn to [follow a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/lib/services/follow/follow.service.ts#L39)
{% endswagger-description %}

{% swagger-parameter in="body" name="FollowerPublicKeyBase58Check" type="String" required="true" %}
Public key of the follower
{% endswagger-parameter %}

{% swagger-parameter in="body" name="FollowedPublicKeyBase58Check" type="String" required="true" %}
Public key of creator being followed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsUnfollow" type="Boolean" required="true" %}
false if follow. true if unfollow
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="Uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFees[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed Follow transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TotalInputNanos": 270953971,
  "ChangeAmountNanos": 270953749,
  "FeeNanos": 222,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 1
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 270953749
      }
    ],
    "TxnMeta": {
      "FollowedPublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S", // Public key of creator being followed by the transactor 
      "IsUnfollow": false // If true, this is an unfollow transaction
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 9
  },
  "TransactionHex": "01cf3f6ab70a2076dfcf1d3792f5c126947dd21b59e4bbc1b795f7924b34484be8010102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4595da998101092202397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}JSON
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/send-diamonds" summary="Send Diamonds" %}
{% swagger-description %}
Create a send diamond transaction. A send diamond transaction is a basic transfer transaction with some additional metadata. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Diamond transactions are a form of social tipping. A send diamond transaction will send DeSo from the sender the receiver as a reward for a certain post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L2007).

Example usages in frontend:\
\- Make request to [Send Diamonds](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1393)\
\- Use SendDiamonds to [tip a creator for a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-post-icon-row/feed-post-icon-row.component.ts#L430)
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderPublicKeyBase58Check" type="String" required="true" %}
Public key of user sending diamonds
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ReceiverPublicKeyBase58Check" type="String" required="true" %}
Public key of user receiving diamonds
{% endswagger-parameter %}

{% swagger-parameter in="body" name="DiamondPostHashHex" type="String" required="true" %}
Hash of post receiving diamond
{% endswagger-parameter %}

{% swagger-parameter in="body" name="DiamondLevel" type="int64" required="true" %}
Level of Diamond being given in this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="Uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a Send Diamond transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "SpendAmountNanos": 50000,
  "TotalInputNanos": 999997474,
  "ChangeAmountNanos": 999947186,
  "FeeNanos": 288,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S",
        "AmountNanos": 50000
      },
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 999947186
      }
    ],
    "TxnMeta": {},
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {
      "DiamondLevel": "Ag==", // Level of diamonds given. Note these are bytes
      "DiamondPostHash": "PkIhWhIKbp1ISBF/WCmixNn2kjYP0Ut42upIOnLRQtw=" // Bytes of Post Hash Hex of the post to which diamonds were given
    },
    "Signature": null,
    "TxnTypeJSON": 2
  },
  "TransactionHex": "013307a4f6ccc02e05077ce299d458e79dfb0fac7b24ad5849ab622c6726ad7176000202397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52d0860302aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45b2f7e7dc0302002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45020c4469616d6f6e644c6576656c01020f4469616d6f6e64506f737448617368203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc00",
  "TxnHashHex": "a78b07488f4c6d10183298d38bc14d2f8cb898e5e13c1a6a053c8678d01507c1"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/create-like-stateless" summary="Like" %}
{% swagger-description %}
Create a Like/Unlike transaction. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Like transactions increment the count of likes on a given post. Unlike transactions decrement the count of likes on a given post.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/transaction.go#L1090).

Example usages in frontend:\
\- Make request to [Create Like Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1376)\
\- Use CreateLike to [like a post](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/feed/feed-post-icon-row/feed-post-icon-row.component.ts#L332)
{% endswagger-description %}

{% swagger-parameter in="body" name="ReaderPublicKeyBase58Check" type="String" required="true" %}
Public key of the liker
{% endswagger-parameter %}

{% swagger-parameter in="body" name="LikedPostHashHex" type="String" required="true" %}
PostHashHex of the post being liked
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsUnlike" type="Boolean" required="true" %}
If true, remove the like from ReaderPublicKeyBase58Check on this post. If false, add a like from ReaderPublicKeyBase58Check on this post
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TrasactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed Like transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TotalInputNanos": 999947186,
  "ChangeAmountNanos": 999946965,
  "FeeNanos": 221,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 1
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 999946965
      }
    ],
    "TxnMeta": {
      "LikedPostHash": [103,248,14,166,144,139,147,204,169,33,162,164,158,242,104,173,55,55,86,181,186,69,175,244,224,107,247,163,31,127,32,192], // Bytes of the Post Hash that was liked by this transaction
      "IsUnlike": false // If true, this is an unlike transaction
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null,
    "Signature": null,
    "TxnTypeJSON": 10
  },
  "TransactionHex": "0126eb328dda3195e2eae7d74c79aa0d95711da4948c33fdfa6c9e3e9b78c29dad010102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45d5f5e7dc030a213e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/send-dm-message" summary="Send Direct Message" %}
{% swagger-description %}
Prepare a new message transaction to send a DM to another user. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.&#x20;

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L258).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderAccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the user sending message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group the sender is using for this message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupKeyName" type="String" required="true" %}
The name of the access group the sender is using for this message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the user who is the recipient of the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group of the recipient of the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupKeyNameName" type="String" required="true" %}
The name of the access group of the recipient of the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="EncryptedMessageText" type="String" required="true" %}
The content of the message to be sent. It is recommended that this is encrypted, but it can also be provided unencrypted
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" required="false" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TstampNanos": 0,
  "TotalInputNanos": 99965445,
  "ChangeAmountNanos": 99964924,
  "FeeNanos": 521,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          124,
          40,
          198,
          223,
          196,
          63,
          241,
          136,
          44,
          71,
          219,
          104,
          132,
          123,
          55,
          19,
          42,
          95,
          167,
          105,
          27,
          237,
          194,
          147,
          234,
          222,
          34,
          179,
          202,
          138,
          241,
          253
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99964924
      }
    ],
    "TxnMeta": {
      "SenderAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "SenderAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "SenderAccessGroupPublicKey": [
        2,
        76,
        64,
        36,
        39,
        178,
        223,
        208,
        159,
        28,
        11,
        190,
        92,
        27,
        153,
        251,
        147,
        126,
        52,
        183,
        95,
        105,
        4,
        169,
        85,
        96,
        149,
        15,
        225,
        158,
        141,
        195,
        5
      ],
      "RecipientAccessGroupOwnerPublicKey": [
        2,
        53,
        122,
        183,
        201,
        238,
        120,
        142,
        241,
        209,
        209,
        249,
        17,
        57,
        99,
        0,
        58,
        45,
        150,
        159,
        237,
        2,
        206,
        148,
        119,
        143,
        249,
        113,
        92,
        110,
        28,
        128,
        117
      ],
      "RecipientAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "RecipientAccessGroupPublicKey": [
        3,
        58,
        32,
        241,
        0,
        86,
        77,
        118,
        184,
        236,
        157,
        99,
        193,
        98,
        31,
        170,
        245,
        45,
        3,
        40,
        58,
        71,
        38,
        98,
        127,
        10,
        28,
        246,
        151,
        222,
        231,
        32,
        238
      ],
      "EncryptedText": "BJmb1jiZf6mpEnLxrRrDjS10p5O+OyrBvgKCNX6Zoakz6/mlSUbb66jBfriuJoRdLix5ybOXJJil6l0lTS870atH+xhVgBTFnugqBAjlHVRzsjWD6blgFmHOYnv1lTY0QNfrfuQmEfIK6yRGHhqi/Pno/BBpHQ==",
      "TimestampNanos": 1675454263701542000,
      "NewMessageType": 0,
      "NewMessageOperation": 0
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 33
  },
  "TransactionHex": "017c28c6dfc43ff1882c47db68847b37132a5fa7691bedc293eade22b3ca8af1fd000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45fcafd52f21cc022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452064656661756c742d6b657900000000000000000000000000000000000000000021024c402427b2dfd09f1c0bbe5c1b99fb937e34b75f6904a95560950fe19e8dc3052102357ab7c9ee788ef1d1d1f9113963003a2d969fed02ce94778ff9715c6e1c80752064656661756c742d6b657900000000000000000000000000000000000000000021033a20f100564d76b8ec9d63c1621faaf52d03283a4726627f0a1cf697dee720ee7604999bd638997fa9a91272f1ad1ac38d2d74a793be3b2ac1be0282357e99a1a933ebf9a54946dbeba8c17eb8ae26845d2e2c79c9b3972498a5ea5d254d2f3bd1ab47fb18558014c59ee82a0408e51d5473b23583e9b9601661ce627bf595363440d7eb7ee42611f20aeb24461e1aa2fcf9e8fc10691de9a8e08aea989aa01700002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/update-dm-message" summary="Update Direct Message" %}
{% swagger-description %}
Prepare a new message transaction to update an existing DM. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/transaction.go#L1331).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderAccessGroupOwnerPublicKey" type="String" required="true" %}
Public key of the user updating a DM
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupPublicKey" type="String" required="true" %}
Public key of the access group the sender used for the DM originally
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupKeyName" type="String" required="true" %}
The name of the access group the sender used to originally send the DM
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupOwnerPublicKey" type="String" required="true" %}
Public key of the user who received the DM
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupPublicKey" type="String" required="true" %}
Public key of the access group to which this DM was originally sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupKeyName" type="String" required="true" %}
The name of the access group to which this DM was originally sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="EncryptedMessageText" type="String" required="true" %}
The updated content of the message. It is recommended that this is encrypted, but it can also be provided unencrypted.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TimestampNanosString" type="String" required="true" %}
String version of the timestamp nanos field of the original DM. This is used to uniquely identify a message.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="false" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" required="false" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TstampNanos": 0,
  "TotalInputNanos": 99965445,
  "ChangeAmountNanos": 99964924,
  "FeeNanos": 521,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          124,
          40,
          198,
          223,
          196,
          63,
          241,
          136,
          44,
          71,
          219,
          104,
          132,
          123,
          55,
          19,
          42,
          95,
          167,
          105,
          27,
          237,
          194,
          147,
          234,
          222,
          34,
          179,
          202,
          138,
          241,
          253
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99964924
      }
    ],
    "TxnMeta": {
      "SenderAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "SenderAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "SenderAccessGroupPublicKey": [
        2,
        76,
        64,
        36,
        39,
        178,
        223,
        208,
        159,
        28,
        11,
        190,
        92,
        27,
        153,
        251,
        147,
        126,
        52,
        183,
        95,
        105,
        4,
        169,
        85,
        96,
        149,
        15,
        225,
        158,
        141,
        195,
        5
      ],
      "RecipientAccessGroupOwnerPublicKey": [
        2,
        53,
        122,
        183,
        201,
        238,
        120,
        142,
        241,
        209,
        209,
        249,
        17,
        57,
        99,
        0,
        58,
        45,
        150,
        159,
        237,
        2,
        206,
        148,
        119,
        143,
        249,
        113,
        92,
        110,
        28,
        128,
        117
      ],
      "RecipientAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "RecipientAccessGroupPublicKey": [
        3,
        58,
        32,
        241,
        0,
        86,
        77,
        118,
        184,
        236,
        157,
        99,
        193,
        98,
        31,
        170,
        245,
        45,
        3,
        40,
        58,
        71,
        38,
        98,
        127,
        10,
        28,
        246,
        151,
        222,
        231,
        32,
        238
      ],
      "EncryptedText": "BJmb1jiZf6mpEnLxrRrDjS10p5O+OyrBvgKCNX6Zoakz6/mlSUbb66jBfriuJoRdLix5ybOXJJil6l0lTS870atH+xhVgBTFnugqBAjlHVRzsjWD6blgFmHOYnv1lTY0QNfrfuQmEfIK6yRGHhqi/Pno/BBpHQ==",
      "TimestampNanos": 1675454263701542000,
      "NewMessageType": 0,
      "NewMessageOperation": 1
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 33
  },
  "TransactionHex": "017c28c6dfc43ff1882c47db68847b37132a5fa7691bedc293eade22b3ca8af1fd000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45fcafd52f21cc022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452064656661756c742d6b657900000000000000000000000000000000000000000021024c402427b2dfd09f1c0bbe5c1b99fb937e34b75f6904a95560950fe19e8dc3052102357ab7c9ee788ef1d1d1f9113963003a2d969fed02ce94778ff9715c6e1c80752064656661756c742d6b657900000000000000000000000000000000000000000021033a20f100564d76b8ec9d63c1621faaf52d03283a4726627f0a1cf697dee720ee7604999bd638997fa9a91272f1ad1ac38d2d74a793be3b2ac1be0282357e99a1a933ebf9a54946dbeba8c17eb8ae26845d2e2c79c9b3972498a5ea5d254d2f3bd1ab47fb18558014c59ee82a0408e51d5473b23583e9b9601661ce627bf595363440d7eb7ee42611f20aeb24461e1aa2fcf9e8fc10691de9a8e08aea989aa01700002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/send-group-chat-message" summary="Send Group Chat Message" %}
{% swagger-description %}
Prepare a new message transaction to send a new message to a group chat. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L279).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderAccessGroupOwnerPublicKey" type="String" required="true" %}
Public key of the user sending the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group the sender is using for this message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupKeyName" type="String" required="true" %}
The name of the access group the sender is using for this message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupOwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the owner of the group chat to which this message is being sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupPublicKeyBase58Check" type="String" required="true" %}
Public key of the access group of the group to which this message is being sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupKeyName" type="String" required="true" %}
The name of the access group of the group to which this message is being sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="EncryptedMessageText" type="String" required="true" %}
The content of the message to be sent. It is recommended that this is encrypted, but it can also be provided unencrypted
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="false" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" required="false" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TstampNanos": 0,
  "TotalInputNanos": 99964924,
  "ChangeAmountNanos": 99964404,
  "FeeNanos": 520,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          0,
          181,
          221,
          137,
          163,
          14,
          242,
          109,
          87,
          223,
          212,
          11,
          198,
          179,
          103,
          8,
          167,
          83,
          95,
          234,
          234,
          183,
          108,
          162,
          43,
          88,
          151,
          44,
          163,
          213,
          244,
          136
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99964404
      }
    ],
    "TxnMeta": {
      "SenderAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "SenderAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "SenderAccessGroupPublicKey": [
        2,
        76,
        64,
        36,
        39,
        178,
        223,
        208,
        159,
        28,
        11,
        190,
        92,
        27,
        153,
        251,
        147,
        126,
        52,
        183,
        95,
        105,
        4,
        169,
        85,
        96,
        149,
        15,
        225,
        158,
        141,
        195,
        5
      ],
      "RecipientAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "RecipientAccessGroupKeyName": [
        97,
        32,
        115,
        117,
        112,
        101,
        114,
        32,
        99,
        111,
        111,
        108,
        32,
        103,
        114,
        111,
        117,
        112,
        99,
        104,
        97,
        116,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "RecipientAccessGroupPublicKey": [
        3,
        3,
        94,
        26,
        126,
        158,
        42,
        200,
        157,
        157,
        221,
        254,
        78,
        232,
        48,
        73,
        48,
        155,
        57,
        206,
        208,
        107,
        131,
        252,
        109,
        168,
        55,
        150,
        14,
        66,
        225,
        84,
        213
      ],
      "EncryptedText": "BOjPxOvQ9V9hLjd54KIkxwKtiazzPESZw/glRPLA/yleJ90Aj2hUq+DGEJ7Caxu6rtweT6fH8dwN/wz8wCjd+fq15ABVnl8oCgRELUYWisZwYby1mKJ7qlC/ICEnOXBwy7rGhAGSF3ehYBcIm2t/d6AUR/+W",
      "TimestampNanos": 1675454352913804800,
      "NewMessageType": 1,
      "NewMessageOperation": 0
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 33
  },
  "TransactionHex": "0100b5dd89a30ef26d57dfd40bc6b36708a7535feaeab76ca22b58972ca3d5f488000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f4abd52f21cb022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452064656661756c742d6b657900000000000000000000000000000000000000000021024c402427b2dfd09f1c0bbe5c1b99fb937e34b75f6904a95560950fe19e8dc3052102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45206120737570657220636f6f6c2067726f757063686174000000000000000000002103035e1a7e9e2ac89d9dddfe4ee83049309b39ced06b83fc6da837960e42e154d57504e8cfc4ebd0f55f612e3779e0a224c702ad89acf33c4499c3f82544f2c0ff295e27dd008f6854abe0c6109ec26b1bbaaedc1e4fa7c7f1dc0dff0cfcc028ddf9fab5e400559e5f280a04442d46168ac67061bcb598a27baa50bf202127397070cbbac68401921777a16017089b6b7f77a01447ff96f5dbbcb6b69b9aa01701002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/update-group-chat-message" summary="Update Group Chat Message" %}
{% swagger-description %}
Prepare a new message transaction to update an existing message to a group chat. Transaction needs to be signed and submitted through `api/v0/submit-transaction` before changes come into effect.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/v3.1.1/routes/new\_message.go#L286).
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderAccessGroupOwnerPublicKey" type="String" required="true" %}
Public key of the user updating the message to a group chat
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupPublicKey" type="String" required="true" %}
Public key of the access group the sender originally used for the message to the group chat
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderAccessGroupKeyName" type="String" required="true" %}
The name of the access group the sender originally used to send the message to the group chat
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="RecipientAccessGroupOwnerPublicKey" type="String" %}
Public key of the owner of the group to which this message was originally sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupPublicKey" type="String" required="true" %}
The name of the access group of the group to which this message was originally sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientAccessGroupKeyName" type="String" required="true" %}
The name of the access group of the group to which this message was originally sent
{% endswagger-parameter %}

{% swagger-parameter in="body" name="EncryptedMessageText" type="String" required="true" %}
The updated content of the message. It is recommended that this is encrypted, but it can also be provided unencrypted.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TimestampNanosString" type="String" required="true" %}
String version of the timestamp nanos field of the original message to the group chat. This is used to uniquely identify a message.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="false" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[String]String" required="false" %}
arbitrary key value data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
  "TstampNanos": 0,
  "TotalInputNanos": 99964924,
  "ChangeAmountNanos": 99964404,
  "FeeNanos": 520,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [
          0,
          181,
          221,
          137,
          163,
          14,
          242,
          109,
          87,
          223,
          212,
          11,
          198,
          179,
          103,
          8,
          167,
          83,
          95,
          234,
          234,
          183,
          108,
          162,
          43,
          88,
          151,
          44,
          163,
          213,
          244,
          136
        ],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 99964404
      }
    ],
    "TxnMeta": {
      "SenderAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "SenderAccessGroupKeyName": [
        100,
        101,
        102,
        97,
        117,
        108,
        116,
        45,
        107,
        101,
        121,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "SenderAccessGroupPublicKey": [
        2,
        76,
        64,
        36,
        39,
        178,
        223,
        208,
        159,
        28,
        11,
        190,
        92,
        27,
        153,
        251,
        147,
        126,
        52,
        183,
        95,
        105,
        4,
        169,
        85,
        96,
        149,
        15,
        225,
        158,
        141,
        195,
        5
      ],
      "RecipientAccessGroupOwnerPublicKey": [
        2,
        170,
        61,
        200,
        210,
        153,
        234,
        30,
        73,
        20,
        222,
        102,
        73,
        78,
        211,
        225,
        110,
        218,
        154,
        13,
        101,
        113,
        157,
        82,
        60,
        26,
        154,
        3,
        203,
        249,
        246,
        12,
        69
      ],
      "RecipientAccessGroupKeyName": [
        97,
        32,
        115,
        117,
        112,
        101,
        114,
        32,
        99,
        111,
        111,
        108,
        32,
        103,
        114,
        111,
        117,
        112,
        99,
        104,
        97,
        116,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0,
        0
      ],
      "RecipientAccessGroupPublicKey": [
        3,
        3,
        94,
        26,
        126,
        158,
        42,
        200,
        157,
        157,
        221,
        254,
        78,
        232,
        48,
        73,
        48,
        155,
        57,
        206,
        208,
        107,
        131,
        252,
        109,
        168,
        55,
        150,
        14,
        66,
        225,
        84,
        213
      ],
      "EncryptedText": "BOjPxOvQ9V9hLjd54KIkxwKtiazzPESZw/glRPLA/yleJ90Aj2hUq+DGEJ7Caxu6rtweT6fH8dwN/wz8wCjd+fq15ABVnl8oCgRELUYWisZwYby1mKJ7qlC/ICEnOXBwy7rGhAGSF3ehYBcIm2t/d6AUR/+W",
      "TimestampNanos": 1675454352913804800,
      "NewMessageType": 1,
      "NewMessageOperation": 1
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": {},
    "Signature": {
      "Sign": null,
      "RecoveryId": 0,
      "IsRecoverable": false
    },
    "TxnTypeJSON": 33
  },
  "TransactionHex": "0100b5dd89a30ef26d57dfd40bc6b36708a7535feaeab76ca22b58972ca3d5f488000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f4abd52f21cb022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c452064656661756c742d6b657900000000000000000000000000000000000000000021024c402427b2dfd09f1c0bbe5c1b99fb937e34b75f6904a95560950fe19e8dc3052102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45206120737570657220636f6f6c2067726f757063686174000000000000000000002103035e1a7e9e2ac89d9dddfe4ee83049309b39ced06b83fc6da837960e42e154d57504e8cfc4ebd0f55f612e3779e0a224c702ad89acf33c4499c3f82544f2c0ff295e27dd008f6854abe0c6109ec26b1bbaaedc1e4fa7c7f1dc0dff0cfcc028ddf9fab5e400559e5f280a04442d46168ac67061bcb598a27baa50bf202127397070cbbac68401921777a16017089b6b7f77a01447ff96f5dbbcb6b69b9aa01701002102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c450000"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/send-message-stateless" summary="(DEPRECATED) Send Message" %}
{% swagger-description %}
Create a Send Message transaction.

Send Message Transactions sends an encrypted message from the sender to the receiver.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/message.go#L479).

Example usages in frontend:\
\- Make Request to [Send Message Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L691) including encrypting message with identity\
\- Use SendMessage to [send a private message](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/messages-page/messages-thread-view/messages-thread-view.component.ts#L117)
{% endswagger-description %}

{% swagger-parameter in="body" name="SenderPublicKeyBase58Check" type="String" required="true" %}
Public key of the user sending the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientPublicKeyBase58Check" type="String" required="true" %}
Public key of the recipient of the message
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MessageText" type="String" required="false" %}
Unencrypted text of the message. It is recommended to encrypt the text using a shared secret and supply it in the EncryptedMessageText field instead.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="EncryptedMessageText" type="String" required="true" %}
Text of message encrypted with a shared secret
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-parameter in="body" name="SenderMessagingGroupKeyName" type="String" required="false" %}
SenderMessagingGroupKeyName is the messaging group key name of the sender. If left empty, this endpoint will replace it with the base messaging key. If both SenderMessagingGroupKeyName and RecipientMessagingGroupKeyName are left empty, a V2 message will be constructed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="RecipientMessagingGroupKeyName" type="String" required="false" %}
RecipientMessagingGroupKeyName is the messaging group key name of the recipient. If left empty, this endpoint will replace it with the base messaging key. If both SenderMessagingGroupKeyName and RecipientMessagingGroupKeyName are left empty, a V2 message will be constructed
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[string]string" required="false" %}
extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a message
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a send message transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "TstampNanos": 1637775918769757700,
  "TotalInputNanos": 999946965,
  "ChangeAmountNanos": 999946613,
  "FeeNanos": 352,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 999946613
      }
    ],
    "TxnMeta": {
      "RecipientPublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S", // Public key (in bytes) of the recipients of the message.
      "EncryptedText": "BLBYlqu8Ns/woTNtXXi+XEbWvXYj6fe5U84VdRSPfd3gd86LENl/e03u5CChto5F+sWPRMm1/1+kuVSY0uOChLpa/9plPt/QYw0UtmTFLQTx5RWzGj5+ViE4/NB/fagQqjzusZx+gkgTzdMZU53C9jbpFEo=", // Encrypted message text bytes
      "TimestampNanos": 1637775918769757700 // Timestamp of message
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": { // Any additional keys in the ExtraData field of the request body will be included here as well.
      "V": "Ag==" // Version of message encryption used to encrypt this message
      "SenderMessagingPublicKey": "senderMessagingPublicKey", // SenderMesssagingPublicKey for v3 messages - Unintelligble bytes
      "SenderMessagingGroupKeyName": "senderMessagingGroupKeyName", // SenderGroupKeyName for v3 messages - Unintelligble bytes
      "RecipientMessagingPublicKey": "recipientMessagingPublicKey", // RecipientMesssagingPublicKey for v3 messages - Unintelligble bytes
      "RecipientMessagingGroupKeyName": "recipientMessagingGroupKeyName", // RecipientGroupKeyName for v3 messages - Unintelligble bytes
    },
    "Signature": null,
    "TxnTypeJSON": 4
  },
  "TransactionHex": "018c4bc1d70d40fa5b27d6299b9f1c6868552846bc7dbebb37c8f2c396ca3e16f6000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f5f2e7dc03049f0102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce527404b05896abbc36cff0a1336d5d78be5c46d6bd7623e9f7b953ce1575148f7ddde077ce8b10d97f7b4deee420a1b68e45fac58f44c9b5ff5fa4b95498d2e38284ba5affda653edfd0630d14b664c52d04f1e515b31a3e7e562138fcd07f7da810aa3ceeb19c7e824813cdd319539dc2f636e9144ababcd79fd590a3dd162102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45010156010200"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/register-messaging-group-key" summary="(DEPRECATED) Register Messaging Group Key" %}
{% swagger-description %}
Create a Group Messaging Key for V3 messages.

Group Messaging Keys are used to power v3 messages. More info to come.
{% endswagger-description %}

{% swagger-parameter in="body" name="OwnerPublicKeyBase58Check" type="String" required="true" %}
Public key of the account for which we want to register a messaging key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MessagingPublicKeyBase58Check" type="String" required="true" %}
Public key of the messaging group we want to register
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MessagingGroupKeyName" type="String" required="true" %}
Name of the group key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MessagingKeySignatureHex" type="String" required="true" %}
Signature of sha256x2(MessagingPublicKeyBase58Check + MessagingGroupKeyName). Currently, the signature is only needed to register the default key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ExtraData" type="map[string]string" required="false" %}
extra data, values must be strings. This is an arbitrary json object that can be used to add extra metadata on a group messaging key
{% endswagger-parameter %}

{% swagger-parameter in="body" name="MinFeeRateNanosPerKB" type="uint64" required="true" %}
Rate per KB
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TransactionFees" type="TransactionFee[]" required="false" %}
Array of

[#transactionfee](./#transactionfee "mention")

objects that define additional outputs that need to be added to this transaction
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully constructed a register group messaging key transaction" %}
{% tabs %}
{% tab title="Sample Response" %}
```
{
  "TstampNanos": 1637775918769757700,
  "TotalInputNanos": 999946965,
  "ChangeAmountNanos": 999946613,
  "FeeNanos": 352,
  "Transaction": {
    "TxInputs": [
      {
        "TxID": [...],
        "Index": 0
      }
    ],
    "TxOutputs": [
      {
        "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
        "AmountNanos": 999946613
      }
    ],
    "TxnMeta": {
      "MessagingPublicKey": "Ajl7GoDroKYGRGUK8Twqb/37vziDDK/DSTenXd1EuM5S", // Public key (in bytes) of the messaging group
      "MessagingGroupKeyName": "somekeyname", // messaging group key name in bytes
      "GroupOwnerSignature": "somebytes" // group owner signature in bytes
      "MessagingGroupMembers": [ // list of group members
        "GroupMemberPublicKey":" "publickey", // the main public key of the group chat member.
        "GroupMemberKeyName": "keyaneminbytes", // GroupMemberKeyName determines the key of the recipient that the
	                                        // encrypted key is addressed to. We allow adding recipients by their
	                                        // messaging keys. It suffices to specify the recipient's main public key
	                                        // and recipient's messaging key name for the consensus to know how to
	                                        // index the recipient. That's why we don't actually store the messaging
	                                        // public key in the MessagingGroupMember entry.
	"EncryptedKey": "encryptedbytes", // Encrypted messaging key, addressed to the recipient
      }]
    },
    "PublicKey": "Aqo9yNKZ6h5JFN5mSU7T4W7amg1lcZ1SPBqaA8v59gxF",
    "ExtraData": null, // Any additional keys in the ExtraData field of the request body will be included here as well.
    "Signature": null,
    "TxnTypeJSON": 23
  },
  "TransactionHex": "018c4bc1d70d40fa5b27d6299b9f1c6868552846bc7dbebb37c8f2c396ca3e16f6000102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45f5f2e7dc03049f0102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce527404b05896abbc36cff0a1336d5d78be5c46d6bd7623e9f7b953ce1575148f7ddde077ce8b10d97f7b4deee420a1b68e45fac58f44c9b5ff5fa4b95498d2e38284ba5affda653edfd0630d14b664c52d04f1e515b31a3e7e562138fcd07f7da810aa3ceeb19c7e824813cdd319539dc2f636e9144ababcd79fd590a3dd162102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45010156010200"
}
```
{% endtab %}

{% tab title="Response Field Descriptions" %}
...coming soon! See comments in sample response for descriptions for now.
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}
