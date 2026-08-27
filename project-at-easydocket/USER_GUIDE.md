# DocketFlow — User Guide

This guide explains how to set up and use **DocketFlow** for everyday construction work tracking.

Field staff record completed work and collect a client signature on site. Office staff set up contracts, work items, and staff, then review dockets and progress.

---

## Who should read which section?

| Your role | Start here |
|-----------|------------|
| **Administrator** | [Getting started](#getting-started-everyone), then [Part A — Administrator](#part-a--administrator) |
| **Supervisor** | [Getting started](#getting-started-everyone), then [Part B — Supervisor](#part-b--supervisor) |
| **Field Technician** | [Getting started](#getting-started-everyone), then [Part C — Field Technician](#part-c--field-technician) |

Everyone should also skim [Common tasks](#common-tasks-all-roles) and [Tips & troubleshooting](#tips--troubleshooting).

---

## Getting started (everyone)

### Sign in

1. Open the DocketFlow web app.
2. Enter your **Email** and **Password**.
3. Select **Sign in**.

If you received an invite or need to set a password for the first time, follow the link in your email and choose a password when prompted.

### First-time prompts

You may be asked to:

- **Change your password** — required before you can use the rest of the app.
- **Complete your profile** — enter at least your **full name** under **Profile**.

### Online / Offline

In the top bar you will see **Online** or **Offline**.

- **Online** — you are connected; work saves to the server.
- **Offline** — Field Technicians can still work on contracts they downloaded earlier; dockets queue until you are back online.

### Sign out

Select **Sign out** in the top bar when you finish.

---

## Roles at a glance

| Role | Main job |
|------|----------|
| **Administrator** | Set up the firm, users, clients, contracts, rates, and work items. Full view including money fields and progress. |
| **Supervisor** | Manage contracts (including rates), allocate Field Technicians, create dockets, and work offline. Sees financial fields; views Active clients (no create/edit Clients). |
| **Field Technician** | See assigned contracts, create and submit your own dockets (including offline), and check your Overview. |

---

## Part A — Administrator

You manage the organisation’s setup and commercial data. You see **Overview**, **Contracts**, **Dockets**, and under **Settings**: **Users**, **Clients**, **Organizational Settings**, **Email Settings**, and **Audit Log**.

### First-time setup checklist

Do these in order the first time you set up DocketFlow:

1. **Organizational Settings** — firm name, address, logo (optional), payment terms, GST treatment, retention defaults, numbering prefixes, plus your **Time Zone & Locale** preferences.
2. **Email Settings** — choose default email recipients and review/edit the **Docket Sharing Template** (subject + body HTML).
3. **Users** — create **Supervisors** and **Field Technicians**.
4. **Clients** — add the customers you work for (and their contacts).
5. **Contracts** — create a **New Contract** for each job (status **Active** when people should work on it).
6. **Rate Card** — set variation minimums and staff rates for that contract (before or while adding work items).
7. **Work Items** — add the schedule of work (item numbers, descriptions, quantities, rates).
8. **Staff Allocation** — assign Field Technicians so they can create dockets on that contract.
9. Field staff create and submit **Dockets** on site.

### Organizational Settings

1. Open **Organizational Settings** under **Settings**.
2. Fill in firm details used on documents and in the header welcome text.
3. Set **contract** and **docket** number prefixes / next numbers if you want branded numbering.
4. Review retention defaults used for claims.
5. Set **Time Zone & Locale** (timezone, date format, time format, and number/currency formatting).
5. Save your changes.

#### Email Settings

1. Open **Email Settings** under **Settings** (visible only when email is enabled for your organisation by the platform super admin).
2. Configure the default recipients:
   - **Client representative (To)**
   - **Contract administrator (CC)**
   - **Site manager (CC)**
3. Optionally add additional CC recipients (users or other emails) via **Add Recipient**. These recipients are included as CC on sent docket emails.
4. Edit the **Docket Sharing Template** (subject + HTML body). Placeholders are resolved automatically when an email is sent.
5. Save.

### Users

1. Open **Users** under **Settings**.
2. Create a user with **Create User** (set a password) or tick **Send email invitation**.
3. Choose a role: **Supervisor**, **Field Technician**, or **View Only** (you cannot create another Administrator from this screen).
4. Optionally add an employee number.
5. **Status badges** next to each team member:
   - **Pending** (amber) — created but has never signed in. Use **Delete** to remove a typo account so you can recreate the same email. You cannot Deactivate Pending users.
   - **Active** (green) — has signed in at least once and is allowed to use the app. Use **Deactivate** to revoke access (no Delete).
   - **Inactive** (red) — deactivated. Use **Activate** to restore access. If they never signed in, **Delete** remains available.
6. Before **Delete** / **Cancel invite**, remove the person from **Staff Allocation** on any contracts if they were assigned.
7. **Pending invites** (email invites not finished): **Set password**, **Resend invite**, or **Cancel invite** (permanently removes the unused account). **Expired** appears about 24 hours after the invite was sent — resend to refresh.
8. Email invitees appear only under **Pending invites** until they complete setup / first sign-in (not also under Team members).
9. For members who have signed in (not yourself, not the Administrator): **Set password** or **Send reset email** as needed.

### Clients

1. Open **Clients**.
2. Add a client with name and optional code, ABN, address, and status (**Active** / inactive).
3. Add contacts (name, phone, emails). Mark one contact as primary when you have several.

### Contracts

1. Open **Contracts**.
2. Select **New Contract** and complete required fields (contract name, client name, billing type, dates as needed, retention if shown, status). Optionally fill Contract Administrator and Site Manager (name, phone, email) for later PDF email sharing.
3. Save. **Contract No** is saved from the form (prefilled from Next Contract No; you may edit it if it stays unique).
4. Contracts are grouped as **Active**, **Completed**, and **Inactive**. Click a row to open **Contract Details**.

On **Contract Details** you can:

- **Edit** contract fields and status.
- Open **Work Items**, **Rate Card**, **Staff Allocation**, **View Progress**, or **Create Docket** (when the contract is **Active**).

### Rate Card

1. From **Contract Details**, open **Rate Card**.
2. Set variation minimum quantity and variation item rate if you use variation-category work items.
3. Add rate rows (staff category × billing method → rate).
4. **Save**.

Rates on a work item are fixed when that work item is saved; changing the Rate Card later does not rewrite older work items.

### Work Items

1. From **Contract Details**, open **Work Items**.
2. Select **New Work Item**.
3. Enter item number, description, category, billing method, agreed quantity, rates (as applicable), and status.
4. **Save**.

After Field Technicians **submit** dockets, **Completed** and **Remaining** quantities on this list update from submitted work only.

### Staff Allocation

1. From **Contract Details**, open **Staff Allocation**.
2. Tick Field Technicians who should work on this contract.
3. **Save**.

Only users with the Field Technician role appear in this list. They must be assigned before they can create dockets for that contract (from their side of the app).

### Dockets (office view)

1. Open **Dockets**.
2. Submitted dockets default to the **last month** (shown as a **Last month** chip). Open **Filters** to narrow further (client, contract, staff, dates, search, variation status). Clear a chip with × or use **Reset** to restore defaults.
3. **Draft** dockets can be edited or deleted by their creator.
4. **Submitted** dockets are final — open with **View** to see lines, plans, and signature.
5. You can also **Create Docket** from the Dockets page or from an Active contract’s **Contract Details**.

### Overview

Your **Overview** shows organisation-wide tiles (last 30 days where noted):

- Active contracts  
- Dockets submitted  
- Work items completed  
- Dockets with variation items  

Plus **Recent Dockets** and **Recent Variations**. Click a row to open that docket.

### Progress Claims

Administrators can prepare progress claims, but this area is **not** listed in the main menu. If your organisation uses claims, your implementation team can point you to the Progress Claims screens. Day-to-day setup and docketing do not require claims.

### What you can / cannot do (Administrator)

**You can:** manage users, clients, contracts, rates, work items, staff allocation, dockets, overview, audit log, and financial fields; **Download offline** from Contracts / Contract Details for offline docketing.

**You cannot:** create a second Administrator from **Users** (there is only one tenant Administrator).

---

## Part B — Supervisor

You run contracts day to day: create and edit contracts and work items (including rates), maintain Rate Cards, allocate Field Technicians, use View Progress, and create dockets online or offline. You can also manage **Users** (except the Administrator), **Clients** (all statuses), and **Organizational Settings** (including Email Settings and Time Zone & Locale). Audit Log and Progress Claims stay with the Administrator.

### What you see in the menu

- **Overview**
- **Contracts** (Active, Completed, and Inactive)
- **Dockets**
- Under **Settings**: **Users**, **Clients**, **Organizational Settings**, **Email Settings**

### Day-to-day workflow

1. Open **Contracts** — create a **New Contract** when needed, or open an existing one.
2. On **Contract Details**, set up **Rate Card**, **Work Items**, and **Staff Allocation** as required.
3. Use **View Progress** to check quantities and valuations.
4. Use **Download offline** on the Contracts list or Contract Details before site work without reliable network.
5. Use **Create Docket** when you need to raise a docket yourself, or open **Dockets** to review drafts and submitted work across the team.
6. Use **Overview** for a quick picture of active contracts and recent dockets / variations.

### Working offline (Supervisor)

1. On **Contracts** or **Contract Details**, use **Download offline** (or **Re-download offline**) for each contract you need on site.
2. When Offline, create or edit drafts and submit as usual; items may show **Queued** until sync finishes.
3. When you are **Online** again, the app syncs. Caches you downloaded are kept for your browser profile (not trimmed by Field Technician assignment rules).

### What you can / cannot do (Supervisor)

**You can:** create/edit contracts (all statuses); create/edit work items and rate cards; see financial values; open View Progress and Work Item Details; allocate Field Technicians; create and view dockets; download contracts for offline use; manage Users (non-Administrators), Clients (all statuses), and Organizational Settings; use Overview.

**You cannot:** act on the Administrator user or assign the Administrator role; change your own role or deactivate yourself; view Audit Log; or manage Progress Claims.

---

## Part C — Field Technician

You work on contracts assigned to you. You create and submit **your own** dockets, including when the network is poor.

### What you see in the menu

- **Overview** (personal — about you)
- **My Contracts**
- **My Dockets**

### Day-to-day workflow

1. Open **My Contracts** (or **Overview** → **Contracts Assigned to Me**).
2. Select a contract (row or eye icon) to open **Contract Details**.
3. Optionally open **Work Items** to see agreed / completed / remaining quantities (no rates).
4. Select **Create Docket**.
5. Choose the work items you completed, then **Continue**.
6. Enter worked quantities, notes, **Docket Comments**, and plans as needed. Use **+ Add Variation Item** if required (the link hides while you add or edit an item; it comes back after Save or Discard). **Total Hours Today** shows **N/A** when there are no hours.
7. Enter the **client representative name**, capture their **signature**, then:
   - **Save as Draft** if you are not finished, or  
   - **Sign Off** / **Submit** to finalise.

Submitted dockets cannot be changed. Use **My Dockets** to find drafts (edit/delete) and submitted records (view only).

### Overview (personal)

Your tiles focus on **you** (often for the last 30 days):

- Contracts recently assigned to you  
- Dockets you submitted  
- Work items completed from your dockets  
- Your dockets that include variation items  

You also see **Contracts Assigned to Me** and **My Recent Dockets** (with a Variation badge when relevant).

### Working offline

1. On **My Contracts**, use **Download offline** (or **Re-download offline**) for each Active contract you need on site.
2. When Offline, create or edit drafts and submit as usual; items may show **Queued** until sync finishes.
3. When you are **Online** again, the app syncs. If you see **Sync Error**, open the docket again when connected or contact your Administrator.
4. You only see **your** dockets — not other Field Technicians’ work on the same contract.

### What you can / cannot do (Field Technician)

**You can:** view assigned Active contracts; view Work Items (quantities); create/submit your dockets; download contracts for offline use; use your personal Overview and My Dockets.

**You cannot:** see Rate Card, Staff Allocation, View Progress, or financial values; open other people’s dockets; manage contracts, clients, or users.

---

## Common tasks (all roles)

### Create and submit a docket

1. Start from **Create Docket** on a contract, from **Dockets** / **My Dockets**, or from Overview (Field Technicians).
2. Confirm client and contract (sometimes locked if you started from a contract).
3. Select the work items done on this visit → **Continue**.
4. Enter **Worked** quantities (hours for hourly items; some billing types fill a fixed quantity for you).
5. Optionally **+ Add Variation Item** (the link hides while the editor is open; it returns after Save or Discard). Add line notes, **Docket Comments**, and **+ Add Plan** if needed. **Total Hours Today** is **N/A** when there are no hours.
6. Enter client representative name, email (required), phone (optional), and signature.
7. **Save as Draft** (returns to where you started Create Docket) or **Sign Off** / **Submit**.
8. After **Submit**, you land on that docket’s **detail** screen. Use **View PDF** when it is enabled (after sync if you were offline). **Back** returns to where you started Create Docket.

### View PDF

- Open a submitted docket from **Dockets** / **My Dockets** (eye), or land there after Submit.
- On detail, click **View PDF** to open and download an A4 PDF of the docket (firm details from Organizational Settings when set, plus all docket detail fields — including **Total Hours Today**, shown as **N/A** when there are no hours, and **Docket Comments** — and the client signature).
- The PDF is **generated when you click** — it is **not saved** on the server.
- While the docket is still syncing (Docket No shows **Pending sync**, or you are offline), View PDF stays **disabled** with **Offline** next to it. On the list, the PDF icon appears only when the docket is fully ready.
- Draft dockets have no View PDF.

### Share docket via Email (tenant admins/supervisors)

Available only when email is enabled for your organisation (controlled by the platform super admin).

- Open the docket **detail** page.
- Click **Share** (mail icon). The app generates the docket PDF attachment and sends the email in the background.
- Recipients are taken from your organisation’s **Email Settings** defaults (client representative, contract contacts, organisational users, and other recipients you configured).
- If you are **Offline**, the email is queued and sent when you return **Online**.
- Check success/failure for that docket in **Email log** (mail icon on the docket page).

### Variation warnings

If the work you enter goes **past** the agreed quantity, the app may show a **variation** caution (red banner). Completing an item exactly up to the agreed quantity is **At Limit**, not variation, and does not show that banner. If you select an item that is already at or over its agreed quantity from earlier submitted dockets, the app warns on the select step and asks you to confirm before Continue — adding more quantity on this docket is variation work. Your Administrator can explain how your firm handles variations commercially.

### Drafts vs submitted

| Status | Meaning |
|--------|---------|
| **Draft** | Still editable by the person who created it; can be deleted. |
| **Submitted** | Final. Number assigned. Cannot be edited. Counts toward completed quantities. |

---

## Tips & troubleshooting

| Situation | What to try |
|-----------|-------------|
| I cannot see a contract | Administrators/Supervisors: confirm status filters (Active / Completed / Inactive). Field Technicians: ask to be added under **Staff Allocation**, and ensure the contract is Active. |
| I cannot see rates or $ values | Expected for Field Technician. Administrators and Supervisors see financial fields. |
| I cannot create a docket on a job | Contract must be **Active**. Field Technicians must be **allocated** to that contract. |
| “Account deactivated” | Your Administrator must reactivate your user. |
| Organisation is **read-only** | You can view data but cannot create or edit. Contact your Administrator. |
| Docket stuck **Queued** / **Pending sync** | Connect to the internet and keep the app open briefly so sync can finish. View PDF enables after the real Docket No and signature are available. |
| Email queued (offline) | Go **Online** and keep the app open briefly so the queued email can send. |
| Email failed | Open **Notifications** (bell icon) and/or **Email log** for the docket. If it keeps failing after retries, contact your platform administrator (super admin) to review email provider settings for your organisation. |
| Email features missing | Email may be disabled for your organisation by the platform super admin. Contact them to enable email; contract/client contact fields remain available for data entry. |
| Offline download missing | Administrators/Supervisors: download from **Contracts** or **Contract Details** while Online. Field Technicians: download on **My Contracts**. Inactive or unassigned field caches are not kept offline. |
| Odd dockets from another company | Should not appear. If you still see empty leftover rows after an update, clear site data for this app in the browser, or sign out and back in. Offline data is scoped to your current organisation. |
| Password or profile blocked | Complete **Change password** and **Profile** (full name) when prompted. |

---

## Known gaps (email sharing)

- None for this release: all configured CC recipients (including “Other Recipients”) are included in the sent docket email CC list.

## Need more help?

Ask your organisation’s **Administrator** for account access, contract setup, and staff allocation. This guide describes the tenant app only.
