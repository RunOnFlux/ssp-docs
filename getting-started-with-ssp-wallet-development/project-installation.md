---
icon: file-lines
---

# Project Installation

1. **Clone the SSP Wallet Repository**

{% hint style="info" %}
Before cloning the repository, please make sure you have git installed in your local environment.

If you don't have git on your local machine please follow the link: [https://git-scm.com/](https://git-scm.com/)

Please use the installer and installation guide for your OS.
{% endhint %}

Open a terminal and run the following commands

```
mkdir ssp-wallet
cd ssp-wallet
git clone git@github.com:RunOnFlux/ssp-wallet.git
```

2. **Install SSP Wallet Dependencies**

In installing the project dependencies you may use yarn

{% hint style="info" %}
For installing yarn you must follow this guide:&#x20;

[https://classic.yarnpkg.com/lang/en/docs/install](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

Open a terminal and run the following command to install SSP Relay dependencies

```
yarn install
```

**Running Test Cases**

To run SSP Wallet test cases, open a terminal and run the following command

```
yarn test
```

**Running in Development Mode**

To run SSP Wallet in development mode, open a terminal and run the following command

```
yarn dev
```
