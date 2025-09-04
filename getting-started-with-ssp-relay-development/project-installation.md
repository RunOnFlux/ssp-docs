---
icon: file-lines
---

# Project Installation

1. **Clone the SSP Relay Repository**

{% hint style="info" %}
Before cloning the repository, please make sure you have git installed in your local environment.

If you don't have git on your local machine please follow the link: [https://git-scm.com/](https://git-scm.com/)

Please use the installer and installation guide for your OS.
{% endhint %}

Open a terminal and run the following commands

<pre><code><strong>mkdir ssp-relay
</strong><strong>cd ssp-relay
</strong><strong>git clone https://github.com/RunOnFlux/ssp-relay.git
</strong></code></pre>

2. **Install SSP Relay Dependencies**

In installing the project dependencies you may use yarn

{% hint style="info" %}
For installing yarn you must follow this guide: [https://classic.yarnpkg.com/lang/en/docs/install](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

Open a terminal and run the following command to install SSP Relay dependencies

```
yarn install
```

3. Start the Service

Open a terminal and run the following command to start SSP Relay server

```
yarn start
```

{% hint style="info" %}
By default, the SSP Relay services runs at **http://127.0.0.1:9876**, alternatively **http://localhost:9876**
{% endhint %}

**Running Test Cases**

To run SSP Relay test cases, open a terminal and run the following command

```
yarn test
```
