# Social Endpoints

## Get Hodlers For Public Key

```
POST /api/v0/get-hodlers-for-public-key
```

Get BalanceEntryResponses of users who are holding a certain public key's creator coin.\
\
Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1168).

Example usages in frontend:

* Make request to [Get Hodlers For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1208)
* Use GetHodlersForPublicKey to [show all the users who are holding a creator's coin](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-hodlers/creator-profile-hodlers.component.ts#L36)

**Parameters**

| Name                     | Type   | Description                                                                                                                   | Required                                         | Restrictions |
| ------------------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------ |
| PublicKeyBase58Check     | string | Public key for which you want to fetch hodlings or hodlers                                                                    | required if Username is not provided             |              |
| Username                 | string | Username for which you want to fetch hodlings or hodlers                                                                      | required if PublicKeyBase58Check is not provided |              |
| LastPublicKeyBase58Check | string | Public key of the last hodler/hodlee from the previous page. Indicates the point at which we want to start returning results. | n                                                |              |
| NumToFetch               | uint64 | number of records to fetch                                                                                                    | n                                                |              |
| FetchHodlings            | bool   | If true, fetch balance entries for hodlings of the user instead of balance entries for hodlers of the user's coin             | n                                                |              |
| FetchAll                 | bool   | if true, fetch all results. Supercedes NumToFetch.                                                                            | n                                                |              |

**Response**

```json5
{
    Hodlers: [<BalanceEntryResponse>, <BalanceEntryResponse>],
    LastPublicKeyBase58Check: "BC1YLianxEsskKYNyL959k6b6UPYtRXfZs4MF3GkbWofdoFQzZCkJRB" 
}
```

## Get Diamonds for Public Key

```
POST /api/v0/get-diamonds-for-public-key
```

Get a list of objects representing all the diamonds a user has given or received.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L1304).

Example usages in frontend:

* Make request to [Get Diamonds For Public Key](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1414)
* Use GetDiamondsForPublicKey to [show all users who have given a creator a diamond, how many diamonds they've given to that creator, and the highest level of diamond](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-diamonds/creator-diamonds.component.ts#L41)

**Parameters**

| Name                 | Type   | Description                                                                      | Required | Restrictions |
| -------------------- | ------ | -------------------------------------------------------------------------------- | -------- | ------------ |
| PublicKeyBase58Check | string | Public key of the user for whom we want to fetch diamonds                        | y        |              |
| FetchYouDiamonded    | bool   | If true, fetch diamonds this user gave out instead of diamond this user received | n        |              |

**Response**

```json5
{
    DiamondSenderSummaryResponses: [
        {
            SenderPublicKeyBase58Check: "BC1YLhtBTFXAsKZgoaoYNW8mWAJWdfQjycheAeYjaX46azVrnZfJ94s", // Public Key of the user who sent the diamond
            ReceiverPublicKeyBase58Check: "BC1YLgxLrxvq5mgZUUhJc1gkG6pwrRCTbdT6snwcrsEampjqnSD1vck", // Public key of the user who received the diamond
            TotalDiamonds: 8, // The number of diamonds the sender has sent to the receiver.
            HighestDiamondLevel: 2, // The highest level of diamond the sender has sent to the receiver.
            DiamondLevelMap: { 1: 4, 2: 2 }, // Map of diamond level to the number of times the sender gave that level of diamond to the receiver.
            ProfileEntryResponse: <ProfileEntryResponse>, // If FetchYouDiamonded is true, this will be the profile of the receiver. If false, this will be the profile of the sender.
        },
    ],
    TotalDiamonds: 555 // Total number of diamonds received or given by the profile that matches PublicKeyBase58Check
}
```

## Get Follows Stateless

```
POST /api/v0/get-follows-stateless
```

Get followers of a certain user/public key or get users followed by a certain user/public key and the total number of followers/followees.&#x20;

Endpoint implementation in backend.

Example usages in frontend:

* Make request to [Get Follows Stateless](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/backend-api.service.ts#L1274)
* Use GetFollows to show [the total number of users following a creator and the total number of users followed by a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/creator-profile-page/creator-profile-top-card/creator-profile-top-card.component.ts#L161)
* Use GetFollows to [see all users who follow (or alternatively are followed by) a creator](https://github.com/deso-protocol/frontend/blob/e006beb72867f6d48a78adb1d126c66144a4298c/src/app/manage-follows-page/manage-follows/manage-follows.component.ts#L54)

**Parameters**

| Name                        | Type   | Description                                                                                                                                          | Required                                         | Restrictions |
| --------------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------ | ------------ |
| PublicKeyBase58Check        | string | Public key for which we want to fetch followers or followees                                                                                         | required if Username is not provided             |              |
| Username                    | string | username for which we want to fetch followers or following                                                                                           | required if PublicKeyBase58Check is not provided |              |
| GetEntriesFollowingUsername | bool   | <ul><li>If true, get entries that are following the specified user.</li><li>If false, get entries that are followed by the specified user.</li></ul> | n                                                |              |
| LastPublicKeyBase58Check    | string | Public key of the last follower/followee from the previous page. Indicates the point at which we want to start returning results.                    | n                                                |              |
| NumToFetch                  | uint64 | number of records to fetch                                                                                                                           | n                                                |              |

**Response**

```json5
{
  PublicKeyToProfileEntry: { // Map of public key to ProfileEntryResponse representing followers or followees
    "BC1YLfuD5AGm2guj3q5wF7WGi3jTUzNhHUHc84GtVsk9kHyxbnk5V1H" : <ProfileEntryResponse>
  }, 
  NumFollowers: 17707 // Total number of followers or followees
}
```

## Is Following Public Key

```
POST /api/v0/is-following-public-key
```

Check if the user is following a public key.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2669).

**Parameters**

| Name                            | Type   | Description                                                         | Required | Restrictions |
| ------------------------------- | ------ | ------------------------------------------------------------------- | -------- | ------------ |
| PublicKeyBase58Check            | string | Public key of the user that may be following                        | y        |              |
| IsFollowingPublicKeyBase58Check | string | Public key of the creator we want to check if the user is following | y        |              |

**Response**

```json5
{
  IsFollowing: true // true if the user is following the IsFollowingPublicKeyBase58Check. Otherwise, false.
}
```

## Is Hodling Public Key

```
POST /api/v0/is-hodling-public-key
```

Check if the user holds the creator coin of a public key. If user is holding some amount of creator coin, we return the BalanceEntryResponse representing how much the user holds.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/709cbfbc62cf3a0e6d56c393e555fc277c93fb76/routes/user.go#L2725).

**Parameters**

| Name                          | Type   | Description                                                                                  | Required | Restrictions |
| ----------------------------- | ------ | -------------------------------------------------------------------------------------------- | -------- | ------------ |
| PublicKeyBase58Check          | string | Public key of the user that may be holding the creator coin of IsHodlingPublicKeyBase58Check | y        |              |
| IsHodlingPublicKeyBase58Check | string | Public key of the creator we want to check that the user is holding                          | y        |              |

**Response**

```json5
{
  IsHodling: true, // true if the user holds the creator coin of IsHodlingPublicKeyBase58Check, Otherwise, false.
  BalanceEntry: <BalanceEntryResponse> // Balance entry that shows the amount of creator coins the user holds.
}
```
