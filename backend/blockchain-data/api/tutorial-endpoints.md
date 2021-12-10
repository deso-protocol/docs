# Tutorial Endpoints

{% swagger method="post" path="" baseUrl="POST /api/v0/get-tutorial-creators" %}
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
```
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

## Get Tutorial Creators

```
POST /api/v0/get-tutorial-creators
```

Get the well-known and up-and-coming creators featured in the Buy a Creator step of the tutorial. Creators who have a founder's reward greater than 10% are excluded here.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L32).

Example usage in frontend:

* Make request to [Get Tutorial Creators](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2107)
* Use GetTutorialCreators to [display creators to the user when they are prompted to buy well-known or up-and-coming creators in the tutorial](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/tutorial/buy-creator-coins-tutorial-page/buy-creator-coins-tutorial/buy-creator-coins-tutorial.component.ts#L44)

**Parameters**

| Name          | Type | Description                                    | Required |   |
| ------------- | ---- | ---------------------------------------------- | -------- | - |
| ResponseLimit | int  | Number of creators to return for each category | y        |   |

**Response**

```
{
  "WellKnownProfileEntryResponses": [<ProfileEntryResponse>, <ProfileEntryResponse>...], // ProfileEntryResponses of creators randomly selected from the Well-Known category
  "UpAndComingProfileEntryResponses": [<ProfileEntryResponse>, <ProfileEntryResponse>...], // ProfileEntryResponses of creators randomly selected from Up-And-Coming category
}
```

{% swagger method="post" path="" baseUrl="/api/v0/start-or-skip-tutorial" summary="" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

## Start Or Skip Tutorial

```
POST /api/v0/start-or-skip-tutorial
```

Begin or skip the tutorial.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L191).

Example usages in frontend:

* Make request to [Start Or Skip Tutorial](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2215)
* Use StartOrSkipTutorial to [send user into tutorial or skip](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/left-bar/left-bar.component.ts#L109)

**Parameters**

| Name                 | Type    | Description                                                                                          | Required |   |
| -------------------- | ------- | ---------------------------------------------------------------------------------------------------- | -------- | - |
| PublicKeyBase58Check | string  | Public key of user starting or skipping tutorial                                                     | y        |   |
| JWT                  | string  | JSON web token authenticating user                                                                   | y        |   |
| IsSkip               | boolean | if true, update the user's tutorial status to skipped. Otherwise, set the tutorial status to started | n        |   |

**Response**

No response body. 200 status code

## Update Tutorial Status

```
POST /api/v0/update-tutorial-status
```

Override endpoint to automatically update a user's tutorial status

Valid values for tutorial status are `TutorialStarted`, `TutorialSkipped`, `InvestInOthersBuyComplete`, `InvestInOthersSellComplete`, `TutorialCreateProfileComplete`, `InvestInYourselfComplete`, `FollowCreatorsComplete`, `GiveADiamondComplete`, `TutorialComplete`

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/tutorial.go#L36).

Example usages in [diamondapp.com](https://diamondapp.com)'s frontend:

* Make request to [Update Tutorial Status](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/backend-api.service.ts#L2275)
* Use UpdateTutorialStatus to [set the user's tutorial status to the current step in your tutorial](https://github.com/diamond-app/frontend/blob/735634e38dfa0605035ded19b46b92766ec856c4/src/app/trade-creator-page/trade-creator/trade-creator.component.ts#L399)

**Parameters**

| Name                                | Type    | Description                                                                              | Required |   |
| ----------------------------------- | ------- | ---------------------------------------------------------------------------------------- | -------- | - |
| PublicKeyBase58Check                | string  | Public key of user whose tutorial status is being updated                                | y        |   |
| JWT                                 | string  | JSON web token authenticating user                                                       | y        |   |
| TutorialStatus                      | string  | Value to be set for user's Tutorial status.                                              | y        |   |
| CreatorPurchasedInTutorialPublicKey | string  | Public key of creator the well-known or up-and-coming the user purchased in the tutorial | n        |   |
| ClearCreatorCoinPurchasedInTutorial | boolean | If true, sets the user's CreatorCoinsPurchasedInTutorial to 0                            | n        |   |

**Response**

No response body. 200 status code

