---
icon: file-lines
---

# Project Installation

1. **Clone the SSP Key Repository**

{% hint style="info" %}
Before cloning the repository, please make sure you have git installed in your local environment.

If you don't have git on your local machine please follow the link: [https://git-scm.com/](https://git-scm.com/)

Please use the installer and installation guide for your OS.
{% endhint %}

Open a terminal and run the following commands

```
mkdir ssp-key
cd ssp-key
git clone git@github.com:RunOnFlux/ssp-key.git
```

2. **Install SSP Key Dependencies**

In installing the project dependencies you may use yarn

{% hint style="info" %}
For installing yarn you must follow this guide:&#x20;

[https://classic.yarnpkg.com/lang/en/docs/install](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

Open a terminal and run the following command to install SSP Key dependencies

```
yarn
bundle update --bundler
yarn bundleinstall
yarn podinstall
```

**Running Test Cases**

To run SSP Key test cases, open a terminal and run the following command

```
yarn test
```

**Running the Server**

To run SSP Key server, open a terminal and run the following command

```
yarn start
```

**Running on Android**

Open a terminal and run the following command

```
yarn android
```

**Running on iOS**

```
yarn ios
```
