---
description: Retrieve rates of fiat and crypto
---

# GET  /v1/rates

<mark style="color:green;">`GET`</mark> **/v1/rates**

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/rates'
```
{% endtab %}
{% endtabs %}

**Response**

<table><thead><tr><th width="220">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>fiat</td><td>rates of fiat currencies</td></tr><tr><td>crypto</td><td>rates of crypto currencies</td></tr></tbody></table>

{% tabs %}
{% tab title="200 - Success" %}
<pre class="language-json"><code class="lang-json"><strong>{
</strong>    "fiat": {
        "MXN": 20.25709,
        "EUR": 0.9591057,
        "JPY": 151.8131346,
        "PKR": 279.8079328,
        "SEK": 10.7237533,
        "BHD": 0.3768549,
        "CNY": 7.2825489,
        "XAU": 0.0003396,
        "TWD": 32.77147,
        "DKK": 7.1525034,
        "XDR": 0.7642685,
        "XAG": 0.0302938,
        "KRW": 1441.7946281,
        "AED": 3.6712849,
        "ZAR": 18.4364528,
        "ARS": 1058.1003351,
        "BMD": 1,
        "BDT": 121.6825889,
        "RUB": 90.7017456,
        "ILS": 3.5412394,
        "HUF": 385.3248637,
        "HKD": 7.7727205,
        "KWD": 0.3088301,
        "USD": 1,
        "THB": 33.7017956,
        "NGN": 1507.4275997,
        "NZD": 1.7506127,
        "SGD": 1.3404692,
        "VEF": 0.10013,
        "MYR": 4.4415545,
        "BRL": 5.6919922,
        "IDR": 16369.0291602,
        "CHF": 0.9047837,
        "PLN": 3.991397,
        "LKR": 296.5838043,
        "GBP": 0.7937198,
        "INR": 86.8604011,
        "UAH": 41.6645826,
        "VND": 25519.9319762,
        "CLP": 952.3833342,
        "CAD": 1.4204972,
        "AUD": 1.5705497,
        "MMK": 2098.0043015,
        "NOK": 11.1439004,
        "SAR": 3.751313,
        "TRY": 36.3027059,
        "PHP": 58.1003351,
        "CZK": 24.088431,
        "BTC": 1,
        "ETH": 1
    },
    "crypto": {
        "btc": 1,
        "eth": 1
    }
}
</code></pre>
{% endtab %}

{% tab title="404 - Rate unavailable" %}
```javascript
Not Found
```
{% endtab %}
{% endtabs %}
