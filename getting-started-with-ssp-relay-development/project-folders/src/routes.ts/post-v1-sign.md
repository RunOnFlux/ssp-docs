---
description: Retrieve services status
---

# POST /v1/sign

<mark style="color:green;">`POST`</mark> **/v1/sign**

**Headers**

| Name         | Value                             |
| ------------ | --------------------------------- |
| Content-Type | application/x-www-form-urlencoded |

Form Data

```javascript
networkWallets=bitcoin:bc1qa4w830n2c4v0j0usehg38xlr58xltxml3cyphrgz0kp4292yw7aqrsqhcr
```

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/sign' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--form 'networkWallets="bitcoin:bc1qa4w830n2c4v0j0usehg38xlr58xltxml3cyphrgz0kp4292yw7aqrsqhcr"'
```
{% endtab %}
{% endtabs %}

**Response**

| Parameter | Description                     |
| --------- | ------------------------------- |
| signature | onramper signature using sha256 |

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "signature":"8c01d648b453fc5397cb93c5e89194f294f4b361c39480127a5077968b0ef352"
}
```
{% endtab %}

{% tab title="400 - Invalid data to sign" %}
```
Bad Request
```
{% endtab %}

{% tab title="400 - Data is too short" %}
```
Bad Request
```
{% endtab %}

{% tab title="400 - Data is too long" %}
```
Bad Request
```
{% endtab %}
{% endtabs %}
