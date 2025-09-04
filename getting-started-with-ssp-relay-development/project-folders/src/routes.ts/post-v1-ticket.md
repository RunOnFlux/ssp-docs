---
description: Create ticket using data specified in the request body
---

# POST /v1/ticket

<mark style="color:green;">`POST`</mark> **/v1/ticket**

**Headers**

| Name            | Value                                |
| --------------- | ------------------------------------ |
| Content-Type    | application/json                     |
| X-Challenge     | Mandatory header for creating ticket |
| X-Forwarded-For | Host where the message came from     |

**Body**

<table><thead><tr><th width="203">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>description</td><td>ticket description</td></tr><tr><td>subject</td><td>ticket subject</td></tr><tr><td>type</td><td>ticket type</td></tr><tr><td>email</td><td>email address</td></tr></tbody></table>

```javascript
{
    "description": "sample description",
    "subject": "sample subject",
    "type": "inquiry",
    "email": "sample@email.com",
}
```

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/ticket' \
--header 'Content-Type: application/json' \
--header 'X-Challenge: sample' \
--header 'X-Forwarded-For: 103.0.113.165' \
--data-raw '{
    "description": "sample description", 
    "subject": "sample subject", 
    "type": "inquiry", 
    "email": "sample@email.com" 
}'
```
{% endtab %}
{% endtabs %}

**Response**

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "status": "success",
    "data": {
        "info": "Ticket created successfully."
    }
}
```
{% endtab %}

{% tab title="400 - Failed Response" %}
```json
// List of messages:
// No description specified
// No subject specified
// No type specified
// No email specified
// Invalid request
// Description is too long
// Subject is too long
// Email is invalid
// Type is too long
// Ticket already submitted

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
