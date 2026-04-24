---
icon: building-circle-arrow-right
---

# Creating Your First Organization

Organizations are the top-level containers in SSP Enterprise. Every vault, member, invitation, policy, and audit log belongs to an organization. You're automatically the **Owner** of any organization you create.

This guide walks you through creating one.

## Before you begin

* You must be **logged in to SSP Enterprise** ([Getting Started](getting-started.md))
* If your plan limits the number of organizations you can own, make sure you're under the limit

## Step 1 — Open the Organizations page

From the main navigation, click **Organizations**.

If you don't belong to any organization yet, this page is your landing page after login. You'll see an empty state inviting you to **Create Organization** or accept a pending invitation.

<!-- SCREENSHOT: empty Organizations page with Create button -->

## Step 2 — Click "Create Organization"

The button is in the top right of the Organizations page. A modal opens titled **Create Organization**.

<!-- SCREENSHOT: Create Organization modal -->

## Step 3 — Fill in the form

**Organization Name** (required, 2–100 characters)
* Use the legal or operating name of your team.
* Example: `Acme Corporation`, `Defi Treasury Council`, `Smith & Jones Partnership`

**Description** (optional, up to 500 characters)
* Briefly describe what this organization is for.
* Example: `Operating treasury for Acme Corp. Vaults: ops, payroll, reserve.`
* The character count is shown as you type.

## Step 4 — Click "Create Organization"

The form validates, then submits to the backend.

You'll see a success message: **Organization "[name]" created successfully**. The modal closes and your new organization appears in the list. It also becomes your **current workspace** — meaning the rest of the app (vaults, members, policies) now operates in the context of this organization.

You're automatically assigned the **Owner** role.

## What gets created

When you create an organization, the system also assigns it a permanent **organization index** — a numeric identifier in the range 100–99999. You don't choose this number; the backend allocates the next available one.

The org index is used internally to derive the cryptographic paths for your vaults and signers, like this:

```
m/48'/<coin>'/<orgIndex>'/<scriptType>'/<vaultIndex>/<addressIndex>
```

**The org index is permanent.** Even if you delete the organization later, the index is never reused. This guarantees that vault addresses never collide across the lifetime of the system. You'll see the org index in your Organization Settings if you ever need it for advanced configuration or migration.

## Switching between organizations

If you belong to multiple organizations (your own + ones you've been invited to), use the organization switcher in the top nav to change context. All pages — vaults, members, policies, activity — update to show the selected organization.

## Updating organization settings

After creation, the **Owner** and **Admins** can update:

* **Name** — descriptive only, can be changed any time
* **Description**
* **Logo** — upload a small image that shows in the org switcher and on shared links

You **cannot** change:

* The organization index (permanent)
* The owner (only via Transfer Ownership, which is a critical action requiring WK re-signing — see below)

## Transferring ownership

Ownership is held by exactly one WK Identity. To move it to another member:

1. Open **Organization → Settings → Transfer Ownership**
2. Select the target member (must be an existing Admin)
3. The current owner signs the transfer with both SSP Wallet + SSP Key (a critical action)
4. Once confirmed, ownership transfers immediately and you become an Admin

Owners cannot leave an organization without first transferring ownership.

## Deleting an organization

Only Owners can delete an organization. Deletion is a critical action and requires WK re-signing.

What happens on delete:

* The org is **soft-deleted** — all data (members, invitations, vaults, audit logs) is preserved
* The org index is permanently retained (never reused)
* All pending invitations are auto-revoked
* Existing vaults remain on-chain — funds are not affected, but you lose the management interface

Deletion is reversible with administrative help (contact support), since data is retained. Vault funds are always recoverable using the underlying cryptographic seeds — the org is a coordination layer, not custody.

## Next steps

* **[Invite Team Members](inviting-members.md)** — bring your team into the org
* **[Create Multisig Vaults](creating-vaults.md)** — once members are in, set up vaults
