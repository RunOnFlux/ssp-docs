---
description: Retrieve network fees
---

# GET  /v1/networkfees

<mark style="color:green;">`GET`</mark> **/v1/networkfees**

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/networkfees'
```
{% endtab %}
{% endtabs %}

**Response**

<table><thead><tr><th width="207">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>array</td><td> network fee information per network</td></tr></tbody></table>

{% tabs %}
{% tab title="200 - Success" %}
```json
[
    {
        "coin": "btc",
        "economy": 3,
        "normal": 3,
        "fast": 5,
        "recommended": 5
    },
    {
        "coin": "ltc",
        "economy": 9,
        "normal": 11,
        "fast": 12,
        "recommended": 12
    }
]
```
{% endtab %}
{% endtabs %}
