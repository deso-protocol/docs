---
description: >-
  Description of endpoints used for creating and on-chain trading of DeSo
  Tokens.
---

# DeSo Tokens Endpoints

<mark style="color:red;">Note: "DAO Coins" are now referred to as "</mark><mark style="color:red;">**DeSo Tokens**</mark><mark style="color:red;">" in all public-facing documentation, but the code and API have not yet been updated to reflect this change.</mark>\
\
\
For endpoints to check ownership of DeSo Tokens, see [#get-hodlers-for-public-key](social-endpoints.md#get-hodlers-for-public-key "mention") and [#is-hodling-public-key](social-endpoints.md#is-hodling-public-key "mention").

## Gets All Open Orders on Order Book for a DeSo Token (DAO Coin) Market

<mark style="color:green;">`POST`</mark> `/api/v0/get-dao-coin-limit-orders`

There are two types of markets where DeSo Tokens can be traded on the on-chain order book exchange: 1) markets where a DeSo Token is traded for $DESO, and 2) markets where a DeSo Token is traded for another DeSo Token.

This endpoint returns all open orders given two coins that can be traded against each other. At least one of the two coins must be a DeSo Token.

See [#create-dao-coin-limit-order](../construct-transactions/dao-transactions-api.md#create-dao-coin-limit-order "mention") for how to create new limit orders to trade DeSo Tokens.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/dao\_coin\_exchange.go#L37).

#### Request Body

| Name                                                                             | Type   | Description                                                                                                                                                                                            |
| -------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| DAOCoin2CreatorPublicKeyBase58CheckOrUsername2<mark style="color:red;">\*</mark> | string | <p>Public key or username of the creator of the DAO, whose DeSo Token makes up the second side of the market.</p><p></p><p>An empty string here represents $DESO as the second side of the market.</p> |
| DAOCoin1CreatorPublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark>  | string | <p>Public key or username of the creator of the Token, whose DeSo Token makes up one side of a market.</p><p></p><p>An empty string here represents $DESO as one side of the market.</p>               |

{% tabs %}
{% tab title="200: OK Successfully retrieved all open orders for a coin pair" %}
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
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    "error": "..." // Error message
}
```
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "error": "..." // Error message
}
```
{% endtab %}
{% endtabs %}

## Gets All Open Limit Orders Created by a Transactor

<mark style="color:green;">`POST`</mark> `/api/v0/get-transactor-dao-coin-limit-orders`

This endpoint returns all open orders that were created by a given transactor on the DeSo Tokens on-chain order book exchange.

Endpoint implementation in [backend](https://github.com/deso-protocol/backend/blob/0af8093227b219de31487ac129e799fee61e39ef/routes/dao\_coin\_exchange.go#L136).

#### Request Body

| Name                                                                       | Type   | Description                                                               |
| -------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------- |
| TransactorPublicKeyBase58CheckOrUsername<mark style="color:red;">\*</mark> | string | Public key or username of the user whose open orders we want to retrieve. |

{% tabs %}
{% tab title="200: OK Successfully retrieved all open orders for the transactor" %}
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
{% endtab %}

{% tab title="400: Bad Request " %}
```javascript
{
    "error": "..." // Error message
}
```
{% endtab %}

{% tab title="500: Internal Server Error " %}
```javascript
{
    "error": "..." // Error message
}
```
{% endtab %}
{% endtabs %}
