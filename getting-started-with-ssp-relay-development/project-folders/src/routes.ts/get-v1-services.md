---
description: Retrieve services status
---

# GET /v1/services

<mark style="color:green;">`GET`</mark> **/v1/services**

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/services'
```
{% endtab %}
{% endtabs %}

**Response**

<table><thead><tr><th width="191">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>onramp</td><td>is onramp enabled</td></tr><tr><td>offramp</td><td>is offramp enabled</td></tr><tr><td>swap</td><td>is swap enabled</td></tr></tbody></table>

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "onramp": true,
    "offramp": true,
    "swap": true
}
```
{% endtab %}
{% endtabs %}
