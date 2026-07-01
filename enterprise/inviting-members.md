---
icon: user-plus
---

# Inviting Team Members & Roles

This guide covers how to invite people to your organization, what each role can do, and how members accept or reject invitations.

## Roles overview

SSP Enterprise has four roles, in order of authority:

| Role | What they can do |
|---|---|
| **Owner** | Everything. Full control of the organization. Cannot be removed except via Transfer Ownership. Exactly one per organization. |
| **Admin** | Manage members (invite, remove, change roles), create vaults, configure policies. Cannot remove other Admins or the Owner. |
| **Member** | Be designated as a vault signer, propose transactions, sign transactions assigned to them. Read-only on members and policies. |
| **Viewer** | Read-only across the organization. Sees vaults, balances, transactions, audit logs but cannot sign or change anything. |

**Important:**
* Admins cannot demote or remove other Admins — only the Owner can.
* The Owner cannot be removed; ownership must be **transferred** first (see [Creating Your First Organization](creating-organization.md#transferring-ownership)).

## Inviting by SSP Identity vs Email

You have two ways to identify the person you're inviting:

* **SSP Identity** — their unique cryptographic identity (starts with `bc1q...`). The most secure option, since the invitation is bound directly to their wallet. They must already have SSP Wallet + SSP Key set up.
* **Email** — their email address. Useful if you don't have their SSP Identity yet. They'll log in via Google or email + verification code, and accept the invitation. Their SSP Identity is bound when they accept.

If you know the person already uses SSP Wallet, prefer SSP Identity. If they're new to the ecosystem, use Email.

## Step 1 — Open Members or Invitations

From your organization, navigate to either:

* **Members** — see current members and invite new ones
* **Invitations** — see pending invitations and invite new ones

Both pages show the **Invite Member** button in the top right.

<!-- SCREENSHOT: Members page with Invite Member button -->

## Step 2 — Click "Invite Member"

A modal opens titled **Invite Member**.

## Step 3 — Choose how to invite

At the top of the modal, toggle between **Email** and **SSP Identity**.

### If Email

Enter the recipient's email address.

> They can sign in with Google or email to accept the invitation.

### If SSP Identity

Enter the recipient's SSP Identity (starts with `bc1q...`, minimum 10 characters).

> Enter the SSP Identity (starts with bc1q...)

## Step 4 — Pick a role

Select **Member** or **Viewer** from the role dropdown.

> **Note:** You cannot invite someone directly as **Admin**. Invite them as **Member** first, then promote them from the Members page after they accept. This is intentional — it gives you a chance to confirm their identity before granting elevated access.

## Step 5 — Add an optional message

Up to 500 characters. This message appears in the invitation email (for Email invites) and in the recipient's in-app notification. Use it to introduce the organization or explain why you're inviting them.

## Step 6 — Send

Click **Send Invitation**.

You'll see **Invitation sent successfully**. The modal closes and the invitation appears in the **Invitations** tab under "Pending" with:

* The recipient (email or SSP Identity)
* Their assigned role
* Expiry time

You can revoke a pending invitation at any time from the Invitations list.

## What the invitee sees

### If invited by SSP Identity

The invitee logs in to SSP Enterprise with their WK Identity and goes to **Notifications**. They'll see your invitation under the **Invitations** tab, showing:

* Organization name
* Inviter name
* Role they've been invited as
* Personal message (if you included one)

They click **Accept** or **Reject**.

### If invited by Email

The invitee receives an email with a link to SSP Enterprise. They sign in (via Google or email + verification code), and the invitation appears the same way under Notifications → Invitations.

If they're new to SSP Enterprise, they may need to set up SSP Wallet + SSP Key first. The invitation stays pending until they accept.

## Accepting

When the invitee clicks **Accept**:

* The invitation is consumed
* They're added to the organization with the assigned role
* They see **Joined "[organization name]"**
* The org appears in their organization list and they can now access vaults and members per their role

## Rejecting

When the invitee clicks **Reject**:

* The invitation is removed
* They see **Invitation rejected**
* No record is created on your end (other than the audit log noting the rejection)

## Promoting and demoting

To change a member's role after they've joined:

1. Open **Members**
2. Find the member in the list
3. Click their role badge (or the **Edit** action)
4. Choose the new role

Constraints:
* Owners can promote anyone to Admin or demote Admins back to Member
* Admins can change Members and Viewers between those two roles (Member ↔ Viewer), but cannot promote anyone to Admin, nor modify other Admins or the Owner — only the Owner can grant or revoke Admin
* You can't demote yourself if doing so would leave the organization without an Admin

## Removing members

To remove a member:

1. Open **Members**
2. Find them in the list
3. Click **Remove**
4. Confirm — this is a **critical action** and requires WK re-signing (both SSP Wallet + SSP Key)

What happens on removal:
* The member loses access to the organization immediately
* They are removed as a designated signer from any vaults they were on
* Past signatures and audit log entries are preserved (members can leave, history cannot)
* If they were an active signer on a pending proposal, the proposal may need additional signers or to be cancelled and re-created

> **Important:** Removing a member who is a designated signer on existing vaults does **not** remove them from the on-chain multisig. The vault's signer set is fixed at creation. Removing them only revokes their access to the SSP Enterprise interface — they could in principle still sign proposals via direct relay if they had the technical means. For sensitive cases, plan to migrate funds to a new vault that excludes the removed member.

## Leaving an organization

A member who is not the Owner can leave at any time:

1. Open **Profile → Organizations**
2. Find the org and click **Leave**
3. Confirm

The Owner cannot leave — they must transfer ownership first.

## Next steps

* **[Create Multisig Vaults](creating-vaults.md)** — now that you have members, set up vaults
* **[Configuring Policy Controls](policies.md)** — apply spending limits and approval rules
