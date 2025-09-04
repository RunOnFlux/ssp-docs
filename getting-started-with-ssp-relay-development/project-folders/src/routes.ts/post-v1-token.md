---
description: Post token using data specified in the request body
---

# POST /v1/token

<mark style="color:green;">`POST`</mark> **/v1/token**

**Headers**

| Name         | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

**Body**



| Parameter  | Description      |
| ---------- | ---------------- |
| wkIdentity | wallet key value |
| keyToken   | SSP key token    |

```javascript
{
    "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
    "keyToken": "1", 
}
```

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/token' \
--header 'Content-Type: application/json' \
--data '{
    "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
    "keyToken": "1"

}'
```
{% endtab %}
{% endtabs %}

**Response**

| Parameter  | Description      |
| ---------- | ---------------- |
| wkIdentity | wallet key value |
| keyToken   | SSP key token    |

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "status": "success",
    "data": {
        "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
        "keyToken": "1"
    }
}
```
{% endtab %}

{% tab title="400 - Failed Response" %}
```javascript
// List of messages:
// Invalid SSP identity specified
// Invalid SSP Key Token specified
// Failed to update synced data

{
    "status": "error",
    "data": {
        "name": "Error",
        "message": "<message>"
    }
}
```
{% endtab %}
{% endtabs %}
