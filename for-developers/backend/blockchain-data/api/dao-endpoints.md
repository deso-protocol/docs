---
description: Description of endpoints used to DAOs, and on-chain trading of DAO coins.
---

# DAO Endpoints

For endpoints to check ownership of DAO coins, see [#get-hodlers-for-public-key](social-endpoints.md#get-hodlers-for-public-key "mention") and [#is-hodling-public-key](social-endpoints.md#is-hodling-public-key "mention").

{% swagger method="post" path="" baseUrl="/api/v0/get-dao-coin-limit-orders" summary="Gets All Open Orders on Order Book for a DAO Coin Market" %}
{% swagger-description %}
There are two types of markets where DAO coins can be traded on the on-chain order book exchange: 1) markets where a DAO coin is traded for $DESO, and 2) markets where a DAO coin is traded for another DAO coin.

This endpoint returns all open orders given two coins that can be traded against each other. At least one of the two coins must be a DAO coin.

See [#create-dao-coin-limit-order](../../transactions/construct-transactions/dao-transactions-api.md#create-dao-coin-limit-order "mention") for how to create new limit orders to trade DAO coins.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/dao\_coin\_exchange.go#L37).
{% endswagger-description %}

{% swagger-parameter in="body" name="DAOCoin1CreatorPublicKeyBase58CheckOrUsername" type="string" required="true" %}
Public key or username of the creator of the DAO, whose DAO coin makes up one side of a market.



An empty string here represents $DESO as one side of the market.
{% endswagger-parameter %}

{% swagger-parameter in="body" name="DAOCoin2CreatorPublicKeyBase58CheckOrUsername2" type="string" required="true" %}
Public key or username of the creator of the DAO, whose DAO coin makes up the second side of the market.



An empty string here represents $DESO as the second side of the market.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved all open orders for a coin pair" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
   "Orders":[
      {
         "TransactorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // public key of the creator of this order
         "BuyingDAOCoinCreatorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // public key of the creator of a DAO coin
         "SellingDAOCoinCreatorPublicKeyBase58Check":"", // empty string represents $DESO
         "ExchangeRateCoinsToSellPerCoinToBuy":3.1,
         "QuantityToFill":5.2, // Denominated in number of coins (not nanos) and can have fractional values
         "OperationType":"BID",
         "OrderID":"4671f48ec7a0da7d769219efdf1689ed19cb3527ae3c1a9669dd5539c9674426" // unique identifier for this order, also equivalent to the transaction hex hash that created the order
      },
      {
         "TransactorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
         "BuyingDAOCoinCreatorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV",
         "SellingDAOCoinCreatorPublicKeyBase58Check":"",
         "ExchangeRateCoinsToSellPerCoinToBuy":0.333,
         "QuantityToFill":1.2,
         "OperationType":"BID",
         "OrderID":"a6c5ab24cde484d91a32b0f977eac2439614192311c7452d8477e4b9a821fc1c"
      },
      
   ]
}

```
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    "error": "..." // Error message
}
```
{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="" %}
```javascript
{
    "error": "..." // Error message
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="post" path="" baseUrl="/api/v0/get-transactor-dao-coin-limit-orders" summary="Gets All Open Limit Orders Created by a Transactor" %}
{% swagger-description %}
This endpoint returns all open orders that were created by a given transactor on the DAO coin on-chain order book exchange.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/dao\_coin\_exchange.go#L136).
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactorPublicKeyBase58CheckOrUsername" type="string" required="true" %}
Public key or username of the user whose open orders we want to retrieve.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved all open orders for the transactor" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
   "Orders":[
      {
         "TransactorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // public key of the creator of this order
         "BuyingDAOCoinCreatorPublicKeyBase58Check":"tBCKVERmG9nZpHTk2AVPqknWc1Mw9HHAnqrTpW1RnXpXMQ4PsQgnmV", // public key of the creator of a DAO coin
         "SellingDAOCoinCreatorPublicKeyBase58Check":"", // empty string represents $DESO
         "ExchangeRateCoinsToSellPerCoinToBuy":3.98734,
         "QuantityToFill":5.123457, // Denominated in number of coins (not nanos) and can have fractional values
         "OperationType":"BID",
         "OrderID":"4671f48ec7a0da7d769219efdf1689ed19cb3527ae3c1a9669dd5539c9674426" // unique identifier for this order, also equivalent to the transaction hex hash that created the order
      },
   ]
}

```
{% endtab %}
{% endtabs %}
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```javascript
{
    "error": "..." // Error message
}
```
{% endswagger-response %}

{% swagger-response status="500: Internal Server Error" description="" %}
```javascript
{
    "error": "..." // Error message
}
```
{% endswagger-response %}
{% endswagger %}
