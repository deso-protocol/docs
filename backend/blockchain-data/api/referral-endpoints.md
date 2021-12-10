# Referral Endpoints

## Get Referral Info For User

```
POST /api/v0/get-referral-info-for-user
```

Gets all data about all referral codes for this owner, including users referred by this code.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/referrals.go#L23).

Example usages in frontend:

* Make request to [Get Referral Info For User](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2073)
* Use GetReferralInfoForUser to [fetch the current state of all referral links for a user upon login](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/global-vars.service.ts#L328)

**Parameters**

| Name                 | Type   | Description                        | Required | Restrictions |
| -------------------- | ------ | ---------------------------------- | -------- | ------------ |
| PublicKeyBase58Check | string | user public key                    | y        |              |
| JWT                  | string | JSON web token authenticating user | y        |              |

&#x20;**Response**

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

## Get Referral Info For Referral Hash

```
POST /api/v0/get-referral-info-for-referral-hash
```

Gets a summary of the current state of a single referral code. This is useful when a user arrives at your site with a referral code. It allows you to tell if the referral code is still valid and how much the user would receive if they signed up.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/referrals.go#L77).

Example usages in frontend:

* Make request to [Get Referral Info For Referral Hash](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L2079)
* Use GetReferralInfoForReferralHash to [show a new user the amount of money they would receive as a sign-up bonus with this referral code](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/global-vars.service.ts#L1184)

**Parameters**

| Name         | Type   | Description   | Required | Restrictions |
| ------------ | ------ | ------------- | -------- | ------------ |
| ReferralHash | string | referral code | y        |              |

**Response**

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
  }
}
```
