---
description: >-
  Description of endpoints used to get data related to transaction spending
  limits on the DeSo blockchain
---

# Transaction Spending Limits Endpoints

{% swagger method="get" path="" baseUrl="/api/v0/get-transaction-spending-limit-response-from-hex/{transactionSpendingLimitHex}" summary="Get Transaction Spending Limit Response From Hex" %}
{% swagger-description %}
This endpoint converts a hex string representing a TransactionSpendingLimit object into a client-friendly 

[#transactionspendinglimitresponse](../basics/data-types.md#transactionspendinglimitresponse "mention")

 object. This is mainly used by identity to parse the transaction spending limit from extra data of an 

[#authorize-derived-key](../../transactions/construct-transactions/derived-keys-transaction-api.md#authorize-derived-key "mention")

 transaction to show the user the permissions they are granting to a derived key.
{% endswagger-description %}

{% swagger-parameter in="path" name="transactionSpendingLimitHex" type="String" required="true" %}
Hex string representing a TransactionSpendingLimit
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieved a TransactionSpendingLimitResponse from the provided hex" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  <TransactionSpendingLimitResponse> // See the TransactionSpendingLimitResponse description in DataTypes for more details 
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

{% swagger method="post" path="" baseUrl="/api/v0/get-transaction-spending-limit-hex-string" summary="Get Transaction Spending Limit Hex String" %}
{% swagger-description %}
This endpoint converts a 

[#transactionspendinglimitresponse](../basics/data-types.md#transactionspendinglimitresponse "mention")

 into a hex string. This is mainly used by identity to convert the object into the form needed to generate an access signature at the 

[#derive](../../../identity/window-api/endpoints.md#derive "mention")

 endpoint.
{% endswagger-description %}

{% swagger-parameter in="body" name="TransactionSpendingLimit" type="TransactionSpendingLimitResponse" required="true" %}
The 

[#transactionspendinglimitresponse](../basics/data-types.md#transactionspendinglimitresponse "mention")

for which you wish to retrieve the hex string.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Successfully retrieve the hex string for the provided TransactionSpendingLimitResponse object" %}
{% tabs %}
{% tab title="Sample Response" %}
```json5
{
  "HexString": "80d0dbc3f40202020a16010b210000000000000000000000000000000000000000000000000000000000000000000005210210ec74e153aa5c18167dc089030e922cbbfa439acb2051e3f1d4222a33ca417701858af9c3012102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52010a2102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52030221029380f3d890348085a22e07d8daad9f1f3706767bfdd59337641c4d3231046509010421029380f3d890348085a22e07d8daad9f1f3706767bfdd59337641c4d3231046509037b2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4500032103f016bef71b72d07ececebc62af189da31c3fa0359d142d732c54c2e2c466915a00e4b0022103f016bef71b72d07ececebc62af189da31c3fa0359d142d732c54c2e2c466915a0198ce0b2103f016bef71b72d07ececebc62af189da31c3fa0359d142d732c54c2e2c466915a028ef0072103f016bef71b72d07ececebc62af189da31c3fa0359d142d732c54c2e2c466915a03904e042102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce5200022102397b1a80eba0a60644650af13c2a6ffdfbbf38830cafc34937a75ddd44b8ce52050a2102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c4500022102aa3dc8d299ea1e4914de66494ed3e16eda9a0d65719d523c1a9a03cbf9f60c45050a0a200000000000000000000000000000000000000000000000000000000000000000000202200000000000000000000000000000000000000000000000000000000000000000000305203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc000302203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc010004203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc020002203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc03020a203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc040102203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc050003203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc060402203e42215a120a6e9d4848117f5829a2c4d9f692360fd14b78daea483a72d142dc070603"
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
