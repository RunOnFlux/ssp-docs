---
description: Retrieve action data using id
---

# GET  /v1/action

<mark style="color:green;">`GET`</mark> **/v1/action/{id}**

**Path Params**

| Name | Value                    |
| ---- | ------------------------ |
| id   | action id to be retrieve |

**Request**

{% tabs %}
{% tab title="Curl" %}
<pre class="language-url"><code class="lang-url"><strong>curl --location 'https://relay.sspwallet.io/v1/action/bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c'
</strong></code></pre>
{% endtab %}
{% endtabs %}

**Response**

| Parameter  | Description                      |
| ---------- | -------------------------------- |
| chain      | chain where the id is associated |
| path       | path value                       |
| wkIdentity | wallet key value                 |
| action     | action value                     |
| payload    | payload value                    |
| utxos      | utxos values                     |
| expireAt   | expiration timestamp             |
| createdAt  | created timestamp                |

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
    "action": "txid",
    "chain": "flux",
    "createdAt": "2025-02-19T12:01:44.162Z",
    "expireAt": "2025-02-19T12:16:44.162Z",
    "path": "0-0",
    "payload": "df37d443a8685cc67c9d518eb85cde3010565e7fc2f28501c4eea6afb3533226",
    "utxos": [
        {
            "txid": "eb15346e498b27166ac46861a5ce887edbd4d29a4accc30033be109744b6f26b",
            "vout": 0,
            "scriptPubKey": "a9147a29650fb76a20926553c0ab0e60a509919d569887",
            "satoshis": "100000",
            "confirmations": 136996,
            "coinbase": false
        }
    ]
}
```
{% endtab %}

{% tab title="404 - Action Id does not exist" %}
```
Not Found
```
{% endtab %}

{% tab title="400 - Invalid Id" %}
```
Bad Request
```
{% endtab %}
{% endtabs %}
