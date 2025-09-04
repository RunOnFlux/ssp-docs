---
description: Post action using data specified in the request body
---

# POST /v1/action

<mark style="color:green;">`POST`</mark> **/v1/action**

**Headers**

| Name         | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

**Body**

| Parameter  | Description                      |
| ---------- | -------------------------------- |
| chain      | chain where the id is associated |
| path       | path value                       |
| wkIdentity | wallet key value                 |
| action     | action value                     |
| payload    | payload value                    |
| utxos      | utxos values                     |

```javascript
{
  "action": "tx",
  "payload": "0400008085202f89016bf2b6449710be3300c3cc4a9ad2d4db7e88cea56168c46a16278b496e3415eb0000000092000047304402206802b39e235731e1798c226b5230b6730596309a91d8293b804817e0d8e25718022038fa7d3ded97d25be0303755040a1ccf9ec04bf7081f8bfb94588bc5b11a380101475221022a316c22acf16a9108b57f48802143cc0c0ac4b8fc360a87568e1794e51558752103749c957461154dfca921d0872ba3c9ac85d98c92e4a34fdac32bd03597fbd2f252aeffffffff0310270000000000001976a91473562bc6a1db9dc6effebc1ef4379942feb3cf2c88ac305e01000000000017a9147a29650fb76a20926553c0ab0e60a509919d5698870000000000000000066a047465737400000000121f1c000000000000000000000000",
  "chain": "flux",
  "path": "0-0",
  "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
  "utxos": [
    {
      "txid": "eb15346e498b27166ac46861a5ce887edbd4d29a4accc30033be109744b6f26b",
      "vout": 0,
      "scriptPubKey": "a9147a29650fb76a20926553c0ab0e60a509919d569887",
      "satoshis": "100000",
      "confirmations": 136988,
      "coinbase": false
    }
  ]
}
```

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/action' \
--header 'Content-Type: application/json' \
--data '{
  "action": "tx",
  "payload": "0400008085202f89016bf2b6449710be3300c3cc4a9ad2d4db7e88cea56168c46a16278b496e3415eb0000000092000047304402206802b39e235731e1798c226b5230b6730596309a91d8293b804817e0d8e25718022038fa7d3ded97d25be0303755040a1ccf9ec04bf7081f8bfb94588bc5b11a380101475221022a316c22acf16a9108b57f48802143cc0c0ac4b8fc360a87568e1794e51558752103749c957461154dfca921d0872ba3c9ac85d98c92e4a34fdac32bd03597fbd2f252aeffffffff0310270000000000001976a91473562bc6a1db9dc6effebc1ef4379942feb3cf2c88ac305e01000000000017a9147a29650fb76a20926553c0ab0e60a509919d5698870000000000000000066a047465737400000000121f1c000000000000000000000000",
  "chain": "flux",
  "path": "0-0",
  "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
  "utxos": [
    {
      "txid": "eb15346e498b27166ac46861a5ce887edbd4d29a4accc30033be109744b6f26b",
      "vout": 0,
      "scriptPubKey": "a9147a29650fb76a20926553c0ab0e60a509919d569887",
      "satoshis": "100000",
      "confirmations": 136988,
      "coinbase": false
    }
  ]
}'
```
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
    "status": "success",
    "data": {
        "chain": "flux",
        "path": "0-0",
        "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
        "action": "tx",
        "payload": "0400008085202f89016bf2b6449710be3300c3cc4a9ad2d4db7e88cea56168c46a16278b496e3415eb0000000092000047304402206802b39e235731e1798c226b5230b6730596309a91d8293b804817e0d8e25718022038fa7d3ded97d25be0303755040a1ccf9ec04bf7081f8bfb94588bc5b11a380101475221022a316c22acf16a9108b57f48802143cc0c0ac4b8fc360a87568e1794e51558752103749c957461154dfca921d0872ba3c9ac85d98c92e4a34fdac32bd03597fbd2f252aeffffffff0310270000000000001976a91473562bc6a1db9dc6effebc1ef4379942feb3cf2c88ac305e01000000000017a9147a29650fb76a20926553c0ab0e60a509919d5698870000000000000000066a047465737400000000121f1c000000000000000000000000",
        "utxos": [
            {
                "txid": "eb15346e498b27166ac46861a5ce887edbd4d29a4accc30033be109744b6f26b",
                "vout": 0,
                "scriptPubKey": "a9147a29650fb76a20926553c0ab0e60a509919d569887",
                "satoshis": "100000",
                "confirmations": 136988,
                "coinbase": false
            }
        ],
        "createdAt": "2025-02-19T11:42:40.922Z",
        "expireAt": "2025-02-19T11:57:40.922Z"
    }
}
```
{% endtab %}

{% tab title="400 - Failed Response" %}
```
Bad Request
```
{% endtab %}
{% endtabs %}
