---
icon: compass
---

# Navigating SSP Wallet

SSP Wallet v2 uses two navigation systems that never overlap: the **identity pill** at the top for *what you're looking at* (wallet + chain), and the **bottom bar** for *where you are* (Home, Portfolio, Activity, Menu).

## The identity pill (top)

At the top of the wallet you'll see a pill showing your active wallet's **identicon**, **name**, and the active **chain**. The wallet's color dot and identicon make it recognizable at a glance — the same identicon appears on your SSP Key, which is one way to visually confirm the two devices are paired to each other.

**Tap the pill** to open the switcher — a single searchable sheet containing:

* **Your wallets** at the top, with balances — tap one to switch to it. From here you can also **Generate New Wallet**, name and color a wallet, or **Remove Last Wallet**.
* **Networks** below — tap a chain to switch to it. Chains you haven't activated yet can be activated from here (see [Switch Chain](switch-chain.md)).

Next to the pill sits the **Lock** button, which securely logs you out.

## The bottom bar

The bottom bar is a compact, icons-only bar (hover or long-press an icon for its name; in the browser side-panel it expands into a labeled rail):

| Tab | What it's for |
| --- | --- |
| 🏠 **Home** | The active wallet: your balance, the Send / Receive / Swap / Buy-Sell action row, and the Tokens, Activity, Contacts, and Nodes sections for the current chain. |
| 📊 **Portfolio** | Your total value across **all** chains, wallets, and tokens — with 24h change and allocation. See [Portfolio & Activity](portfolio-and-activity.md). |
| 🕘 **Activity** | Your transaction history. |
| ☰ **Menu** | Everything else — settings and tools (this replaces the old Settings menus). |

## The Menu tab

**Menu** is the full toolbox. It contains, in sections:

* **General** — Language, fiat currency, and theme (System / Light / Dark)
* **Security** — Change password, public nonces sync
* **Networks** — SSP Relay and per-chain node / API / explorer service configuration
* **WalletConnect** — connect to dApps (EVM chains)
* **Address Details** and **SSP Wallet Details** — your addresses, extended public keys, seed phrase, and sync QR codes
* **Sign Message**, **SSP Enterprise**, and the built-in **Tutorial**

## Privacy mode

**Tap your balance** on Home to blur every amount in the wallet. Tap again to show them. Useful when someone can see your screen.

## Backup health

Until your seed phrase backup is verified and your SSP Key is paired, an amber **backup health** card appears under your balance showing exactly what is at stake. Once everything is secured, it disappears.
