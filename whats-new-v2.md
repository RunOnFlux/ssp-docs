---
icon: sparkles
---

# What's New in SSP v2.0.0

SSP Wallet v2.0.0 and SSP Key v2.0.0 are the biggest release in SSP's history — a complete redesign of both apps on the new SSP design system, with major security-UX additions. **Same keys, same 2-of-2 self-custody, zero migration**: updating in place keeps your wallet, pairing, settings, and history exactly as they were.

## Highlights

### 🔗 Multi-chain batch sync

Activate any set of chains with a **single approval** on your SSP Key (previously one approval per chain). Select the chains you want while pairing — or later — and watch live progress as your SSP Key prepares the keys for each chain. See [Switch Chain](getting-started-with-ssp-wallet/switch-chain.md).

### 🔑 Pairing verification code

After syncing, both devices independently derive the **same 6 words** from your keys. If they match, no one — not even the relay — could have tampered with your pairing. Scan-to-verify is supported. This is a security-critical check: read [Pairing Verification Code](getting-started-with-ssp-key/pairing-verification-code.md).

### 📊 Portfolio tab

Your total balance across every chain (tokens included), with 24-hour change and allocation at a glance. Chains you haven't activated yet can be activated right from the list. See [Portfolio & Activity](getting-started-with-ssp-wallet/portfolio-and-activity.md).

### 🧭 Redesigned navigation

Wallet and chain switching now live in one **identity pill** at the top — tap it to open a single searchable switcher for wallets and networks. Below, a compact bottom bar navigates between **Home · Portfolio · Activity · Menu** (Settings became **Menu** — everything from the old menus, one place). See [Navigating SSP Wallet](getting-started-with-ssp-wallet/navigating-ssp-wallet.md).

### 📤 Unified Send

One clear 3-step flow (**compose → review → approve**) for every chain, with the **full recipient address always shown** at review. Fees are automatic with a manual override. See [Send](getting-started-with-ssp-wallet/transaction/send.md).

### 🫱🏼‍🫲🏽 Slide to approve (SSP Key)

Every signature on SSP Key is now a deliberate gesture — slide to approve, release early and nothing happens. Biometric/PIN confirmation still follows the slide. See [Approving Requests with SSP Key](getting-started-with-ssp-key/approving-requests-with-ssp-key.md).

### 🔍 Clearer approvals (SSP Key)

Every request type shares one clean layout with **plain-language summaries decoded on your device**, never trusted from the network. Token approvals now warn loudly about **unlimited allowances** and **collection-wide NFT operator grants**, naming the spender.

### 🗒️ Signing history (SSP Key)

A **local, encrypted, biometric-gated** log of everything your Key has co-signed. It never leaves your device. See [Signing History & Privacy Mode](getting-started-with-ssp-key/signing-history-and-privacy-mode.md).

### 🙈 Privacy mode

In SSP Wallet, tap your balance to blur every amount. In SSP Key, mask wallet identities and references in your signing history at a tap.

### 🛡️ Real backup verification

Both apps now verify your seed backup with a **word challenge**: SSP Wallet asks you to confirm specific words of your new seed phrase, and SSP Key replaces the old "I backed it up" switch with a 3-word challenge when creating a new key.

### 💛 More v2 additions

* **Backup health** — the wallet reminds you what's at stake until your seed is verified and your Key is paired.
* **2-of-2 handshake screen** — watch your two devices co-sign in real time while a transaction awaits approval.
* **Complete visual refresh** — the new SSP brand (amber, Inter, Lucide icons, the pillar mark) across every screen, light and dark. Motion respects your system's reduce-motion setting.
* **Rebuilt onboarding** — name and color your wallet, verify your seed with a word challenge, and pair with live progress.
* **Smaller SSP Key app** — unused icon libraries and duplicate assets removed (~6 MB).

## Security

* **No changes to key derivation, signing, seed handling, or encryption** — verified by independent audit across every commit in this release.
* All dependencies updated and exactly pinned; every fixable vulnerability resolved.

## Compatibility & upgrading

* **Updating in place: nothing to re-do.** Your wallet, password, pairing, and settings carry over (covered by an automated release gate). You never need to re-pair or restore from seed because of this update.
* **Update in any order.** SSP Wallet v2 works with SSP Key v1, and SSP Key v2 works with SSP Wallet v1.
* **Batch chain sync activates when both apps are on v2.** Until then, chains sync one at a time exactly as before.
* The offline/manual pairing fallback (QR code and manual extended-public-key input) continues to work as it always has.
