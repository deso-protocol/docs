---
description: Description of endpoints used to get referral data
---

# Referral Endpoints

Please make sure you've read [.](./ "mention") so you are familiar with the following types referenced in this documentation:

* [#profileentryresponse](./#profileentryresponse "mention")
* [#postentryresponse](./#postentryresponse "mention")
* [#balanceentryresponse](./#balanceentryresponse "mention")
* [#nftentryresponse](./#nftentryresponse "mention")
* [#nftcollectionresponse](./#nftcollectionresponse "mention")

{% swagger method="post" path="" baseUrl="/api/v0/get-referral-info-for-user" summary="Get Referral Info For User" %}
{% swagger-description %}
Gets all data about all referral codes for this owner, including users referred by this code.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/referrals.go#L23).

Example usages in frontend:\
&#x20; \- Make request to [Get Referral Info For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2073)\
&#x20; \- Use GetReferralInfoForUser to [fetch the current state of all referral links for a user upon login](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/global-vars.service.ts#L328)
{% endswagger-description %}

{% swagger-parameter in="body" required="true" name="PublicKeyBase58Check" type="String" %}
user public key
{% endswagger-parameter %}

{% swagger-parameter in="body" required="true" name="JWT" type="String" %}
JSON web token authenticating user
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved information about all referral codes a given user has" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "ReferralInfoResponses": [{
      "IsActive": true, // If true, this referral code is still valid. If false, this referral code is not valid anymore.
      "ReferredUsers": [<ProfileEntryResponse>, <ProfileEntryResponse>...], // An array of ProfileEntryResponses representing the list of all users who successfully signed up with this referral code.
      "Info": {
        "ReferralHashBase58": "9diWfRVk", // Referral code
        "ReferrerPKID": [3, 35, 87, 246, 229, 114, 151, 131, 149, 22, 174, 63, 215, 30, 118, 206, 71, 228, 63, 136, 30, 139, 232, 104, 119, 240, 14, 196, 86, 89, 75, 16, 201], // PKID of the user who is the referrer for this code. PKID is a unique identity for a user - a user's PKID stays the same even if they have their identity swapped. This field is not particularly useful in this format for frontend consumption.
        "ReferrerAmountUSDCents": 500, // Amount referrer will receive in USD cents for a successful referral. Upon a successful referral, the referrer will receive the DeSo equivalent of $5 in this example. 
        "ReferreeAmountUSDCents": 2500, // Amount the user who was referred will receive in USD cents for a successful referral. Upon a successful referral, the user who signed up with this code will receive $25 in this example.
        "MaxReferrals": 10, // Maximum number of users that can be referred by this code. Note if this value is 0, there is no limit on the number of referrals
        "RequiresJumio": true, // Not implemented, but in a future state, there may be support for referral bonuses without going through the Jumio verification flow.
        "NumJumioAttempts": 100, // Number of times users have tried to sign up with this referral code and entered the Jumio verification flow
        "NumJumioSuccesses": 90, // Number of times users have successfully completed the Jumio verification flow with this referral code
        "TotalReferrals": 90, // Number of users who have been succesfully referred by this code
        "TotalRefererrerDeSoNanos": 900000, // Total amount of DeSo the referrer received from sign-ups with this referral code
        "TotalRefereeDeSoNanos": 180000, // Total amount of DeSo received across all users who have signed up with referral code.
        "DateCreatedTStampNanos": 9127398712983, // Timestamp of creation of this referral code
      }
    }]
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

{% swagger method="post" path="" baseUrl="/api/v0/get-referral-info-for-referral-hash" summary="Get Referral Info For Referral Hash" %}
{% swagger-description %}
Gets a summary of the current state of a single referral code. This is useful when a user arrives at your site with a referral code. It allows you to tell if the referral code is still valid and how much the user would receive if they signed up.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/referrals.go#L77).

Example usages in frontend:\
&#x20; \- Make request to [Get Referral Info For Referral Hash](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2079)\
&#x20; \- Use GetReferralInfoForReferralHash to [show a new user the amount of money they would receive as a sign-up bonus with this referral code](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/global-vars.service.ts#L1184)
{% endswagger-description %}

{% swagger-parameter in="body" name="ReferralHash" required="true" type="String" %}
referral code
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved referral info about the provided referral hash" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "ReferralInfoResponse": {
    "IsActive": true, // If true, this referral code is still valid
    "Info": {
      "ReferralHashBase58": "9diWfRVk", // Referral code
      "ReferreeAmountUSDCents": 2500, // Amount the user who was referred will receive in USD cents for a successful referral. Upon a successful referral, the user who signed up with this code will receive $25 in this example.
      "MaxReferrals": 10, // Maximum number of users that can be referred by this code. Note if this value is 0, there is no limit on the number of referrals
      "TotalReferrals": 90, // Number of users who have been succesfully referred by this code
    }
  },
  "CountrySignUpBonus": { // The CountrySignUpBonus object is based on the IP from which the request is made.
    "AllowCustomReferralAmount": true, // If true, referee amount specified in referral code will be paid to users who sign up with IDs from this country. If false, ReferralAmountOverrideUSDCents will be paid to users who sign up with IDs from this country.
    "ReferralAmountOverrideUSDCents": 100, // Amount all referees will be paid when signing up from this country if AllowCustomReferralAmount is false.
    "AllowCustomKickbackAmount": false, // If true, referrer amount specified in referral code will be paid as a kickback to users who gave out referral code that a user signed up with IDs from this country. If false, KickbackAmountOverrideUSDCents will be paid as a kickback to referrers when a user signs up with an ID from this country.
    "KickbackAmountOverrideUSDCents": 0, // Amount all referrers will be paid when a referee signs up from this country if AllowCustomKickbackAmount is false.
  }
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
