# 🗂️ 03 — Client Tracker

> Guide to building and maintaining the SOC client registry using a SharePoint List — centralising all client contacts, portal links, and SLA details in one filterable, team-accessible location.

---

## Why a SharePoint List (Not a Spreadsheet)

| Feature | SharePoint List | Excel / Word File |
|---|---|---|
| Simultaneous editing | ✅ Yes — no version conflicts | ❌ No — one user at a time |
| Filterable views | ✅ Built-in column filters | ⚠️ Manual |
| Pin in Teams | ✅ Native tab | ❌ File download only |
| Hyperlinks per row | ✅ Native field type | ⚠️ Manual formatting |
| Access control | ✅ Inherits SharePoint permissions | ❌ File-level only |
| Audit trail | ✅ Automatic version history | ❌ Manual |

---

## SharePoint List Setup

### Step 1: Create the List

1. Go to your **SOC Operations Hub** SharePoint site
2. Click **+ New → List**
3. Choose **Blank list**
4. Name: `Client Tracker`
5. Description: `Central registry of all SOC clients — contacts, links, SLAs, and document references`
6. Click **Create**

### Step 2: Configure Columns

Delete the default "Title" column label — rename it to **Client Name**.

Then add the following columns:

| Column Name | Type | Configuration |
|---|---|---|
| Client Name | Single line of text | Required. This is the renamed Title column. |
| Short Code | Single line of text | e.g. CLT-A — used in folder names and report filenames |
| Primary Contact | Person or Group | The main SOC point of contact at the client |
| Primary Contact Email | Single line of text | Email address (hyperlink not needed, type is fine) |
| Primary Contact Phone | Single line of text | Direct phone number |
| Escalation Contact Name | Single line of text | After-hours escalation contact name |
| Escalation Contact Phone | Single line of text | After-hours direct number |
| Client Portal URL | Hyperlink | Link to the client's SIEM/management portal |
| SharePoint Folder | Hyperlink | Direct link to the client's folder in SharePoint |
| SLA Tier | Choice | Options: P1 (≤15 min), P2 (≤1 hr), P3 (≤4 hr) |
| Sentinel Workspace | Single line of text | Workspace name or resource ID |
| Defender Tenant | Single line of text | Tenant name or ID |
| Assigned Analyst | Person or Group | Primary analyst responsible for this client |
| Contract Review Date | Date and time | Date only — next SLA/contract review |
| Onboarding Date | Date and time | Date only — when client was onboarded |
| Status | Choice | Options: Active, Offboarding, On Hold |
| Notes | Multiple lines of text | Freeform operational notes, caveats, special instructions |

---

## Filtered Views

Create saved views so analysts can quickly see what they need:

### View 1: All Clients (default)
- Show all columns
- Sort by: Client Name (A → Z)

### View 2: My Clients
- Filter: Assigned Analyst = [Me]
- Good for individual analyst daily reference

### View 3: Active Clients Only
- Filter: Status = Active
- Hides offboarding/on-hold clients from busy view

### View 4: P1 SLA Clients
- Filter: SLA Tier = P1 (≤15 min)
- Quick reference during incidents — who needs the fastest response

### How to Create a View
1. In the SharePoint List → **+ Add view** (top right)
2. Name the view
3. Set filters and column visibility
4. Save

---

## Pinning in Microsoft Teams

1. Open your **SOC Teams channel**
2. Click **+** to add a tab
3. Search for **Lists**
4. Select the **SOC Operations Hub** site → choose **Client Tracker**
5. Name the tab: `📋 Client Tracker`
6. Click **Save**

Analysts can now open, filter, and update the client tracker directly inside Teams without opening a browser.

---

## Maintaining the Tracker

### When a New Client is Onboarded
- Add a new row immediately during onboarding
- Fill all required fields before the first security event
- Create the client folder in SharePoint and paste the link into the SharePoint Folder column
- Assign the primary analyst

### When Contact Details Change
- Update the row directly — SharePoint automatically saves version history
- No need to notify anyone; the change is live immediately for the whole team

### When a Client Offboards
- Change Status to **Offboarding**
- Update to **Inactive** once service ends
- Do not delete — retain for historical reference and any post-contract queries

### Regular Review
- Review the tracker during the monthly SLA review meeting
- Confirm contact details are current (ask clients to verify annually)
- Check Contract Review Date column for upcoming renewals

---

## Example Row (Anonymised)

| Field | Example Value |
|---|---|
| Client Name | Northgate Financial Group |
| Short Code | NFG |
| Primary Contact | Jane Smith |
| Primary Contact Email | j.smith@northgatefin.com.au |
| Primary Contact Phone | 0412 XXX XXX |
| Escalation Contact Name | IT Director — Mark Jones |
| Escalation Contact Phone | 0421 XXX XXX |
| Client Portal URL | https://portal.northgatefin.com |
| SharePoint Folder | [Link to NFG folder] |
| SLA Tier | P1 (≤15 min) |
| Sentinel Workspace | nfg-sentinel-prod |
| Assigned Analyst | Rohit Panchal |
| Contract Review Date | 01/07/2025 |
| Status | Active |

---

*Previous: [02 — Escalation Email Templates](./02-escalation-email-templates.md) | Next: [04 — Monthly Reports](./04-monthly-reports.md)*
