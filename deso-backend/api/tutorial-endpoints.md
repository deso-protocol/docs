---
description: >-
  Description of endpoints used to get data related to tutorials on the DeSo
  blockchain
---

# Tutorial Endpoints

Please make sure you've read [.](./ "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](./#profileentryresponse "mention")
* [#postentryresponse](./#postentryresponse "mention")
* [#balanceentryresponse](./#balanceentryresponse "mention")
* [#nftentryresponse](./#nftentryresponse "mention")
* [#nftcollectionresponse](./#nftcollectionresponse "mention")

{% swagger method="post" path="" baseUrl="POST /api/v0/get-tutorial-creators" summary="Get Tutorial Creators" %}
{% swagger-description %}
Get the well-known and up-and-coming creators featured in the Buy a Creator step of the tutorial. Creators who have a founder's reward greater than 10% are excluded here.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L32).

Example usage in frontend:\
&#x20; \- Make request to [Get Tutorial Creators](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2107)\
&#x20; \- Use GetTutorialCreators to [display creators to the user when they are prompted to buy well-known or up-and-coming creators in the tutorial](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/tutorial/buy-creator-coins-tutorial-page/buy-creator-coins-tutorial/buy-creator-coins-tutorial.component.ts#L44)
{% endswagger-description %}

{% swagger-parameter in="body" name="ResponseLimit" type="int" required="true" %}
Number of creators to return for each category
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved tutorial creators" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "WellKnownProfileEntryResponses": [<ProfileEntryResponse>, <ProfileEntryResponse>...], // ProfileEntryResponses of creators randomly selected from the Well-Known category
  "UpAndComingProfileEntryResponses": [<ProfileEntryResponse>, <ProfileEntryResponse>...], // ProfileEntryResponses of creators randomly selected from Up-And-Coming category
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

{% swagger method="post" path="" baseUrl="/api/v0/start-or-skip-tutorial" summary="Start Or Skip Tutorial" %}
{% swagger-description %}
Begin or skip the tutorial.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L191).

Example usages in frontend:\
&#x20; \- Make request to [Start Or Skip Tutorial](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2215)\
&#x20; \- Use StartOrSkipTutorial to [send user into tutorial or skip](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/left-bar/left-bar.component.ts#L109)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="string" required="true" %}
Public key of user starting or skipping tutorial
{% endswagger-parameter %}

{% swagger-parameter in="body" name="JWT" type="string" required="true" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-parameter in="body" name="IsSkip" type="boolean" %}
if true, update the user's tutorial status to skipped. Otherwise, set the tutorial status to started
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully triggered start or skip of tutorial" %}
No response body
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/update-tutorial-status" summary="Update Tutorial Status" %}
{% swagger-description %}
Override endpoint to automatically update a user's tutorial status

Valid values for tutorial status are `TutorialStarted`, `TutorialSkipped`, `InvestInOthersBuyComplete`, `InvestInOthersSellComplete`, `TutorialCreateProfileComplete`, `InvestInYourselfComplete`, `FollowCreatorsComplete`, `GiveADiamondComplete`, `TutorialComplete`

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L36).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:\
&#x20; \- Make request to [Update Tutorial Status](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L2275)\
&#x20; \- Use UpdateTutorialStatus to [set the user's tutorial status to the current step in your tutorial](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/trade-creator-page/trade-creator/trade-creator.component.ts#L399)
{% endswagger-description %}

{% swagger-parameter in="body" name="PublicKeyBase58Check" type="string" required="true" %}
Public key of user whose tutorial status is being updated
{% endswagger-parameter %}

{% swagger-parameter in="body" name="JWT" type="string" required="true" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-parameter in="body" name="TutorialStatus" type="string" required="true" %}
Value to be set for user's Tutorial status.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="CreatorPurchasedInTutorialPublicKey" type="string" %}
Public key of creator the well-known or up-and-coming the user purchased in the tutorial
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ClearCreatorCoinPurchasedInTutorial" type="boolean" %}
If true, sets the user's CreatorCoinsPurchasedInTutorial to 0
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully updated tutorial" %}
No response body.
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

