---
description: Retrieve token info from alchemy
---

# GET /v1/tokeninfo

<mark style="color:green;">`GET`</mark> **/v1/tokeninfo/{network}/{address}**

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/tokeninfo/eth/0x514910771af9ca656af840dff83e8264ecf986ca'
```
{% endtab %}
{% endtabs %}

**Path Params**

| Name    | Value                  |
| ------- | ---------------------- |
| network | Chain being used       |
| address | Token contract address |

**Response**

| Parameter | Description                           |
| --------- | ------------------------------------- |
| name      | token name                            |
| symbol    | token symbol                          |
| decimals  | number of decimal places of the token |
| logo      | logo link provider by alchemy         |

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "name": "Chainlink",
    "symbol": "LINK",
    "decimals": 18,
    "logo": "https://samplelocation/logo"
}
```
{% endtab %}
{% endtabs %}
