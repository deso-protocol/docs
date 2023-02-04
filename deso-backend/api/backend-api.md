# Meta Data Endpoints

## General Endpoints

### Health Check

```
GET /api/v0/health-check
```

Check if your DeSo node is synced

**Parameters:**

None

**Response:**

If node is synced and received all transactions.

```
200
```

### Get Exchange Rate

```
GET /api/v0/get-exchange-rate
```

Get DeSo exchange rate, total amount of nanos sold, and Bitcoin exchange rate.

**Parameters:**

None

**Response:**

```
{
    "SatoshisPerDeSoExchangeRate":498484,
    "NanosSold":8491518125648433,
    "USDCentsPerBitcoinExchangeRate":3608200
}
```

### Get App State

```
POST /api/v0/get-app-state
```

Get state of DeSo App, such as cost of profile creation and diamond level map. Example use in the [frontend](https://github.com/deso-protocol/frontend/blob/96bdf0c/src/app/backend-api.service.ts#L1106) and endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/base.go#L86).

**Parameters**

None; however, you need to send an empty JSON `{ }`. Otherwise, you will get 400 - Bad Request. More info on the request [here](https://github.com/deso-protocol/backend/blob/47bcc8a/routes/base.go#L63).

| Name                 | Type   | Description                 |
| -------------------- | ------ | --------------------------- |
| PublicKeyBase58Check | string | (optional) check public key |

**Response**

```
{
    "AmplitudeKey": "",
    "AmplitudeDomain": "api.amplitude.com",
    "MinSatoshisBurnedForProfileCreation": 50000,
    "IsTestnet": false,
    "SupportEmail": "node.admin@protonmail.com",
    "ShowProcessingSpinners": true,
    "HasStarterDeSoSeed": false,
    "HasTwilioAPIKey": false,
    "CreateProfileFeeNanos": 10000000,
    "CompProfileCreation": false,
    "DiamondLevelMap": {
        "1": 50000,
        "2": 500000,
        "3": 5000000,
        "4": 50000000,
        "5": 500000000,
        "6": 5000000000,
        "7": 50000000000,
        "8": 500000000000
    },
    "HasWyreIntegration": false,
    "Password": ""
}
```

##
