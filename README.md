# 🏢 SOC Operations Hub

> A practical SOC knowledge base and operations framework built on Microsoft 365 — designed for small-to-medium SOC teams managing multiple enterprise clients.

This repository documents the architecture and implementation of a centralised SOC Operations Hub using SharePoint, Microsoft Teams, and Planner. It covers everything from escalation email workflows to monthly reporting, client tracking, and recurring analyst scheduling — all without relying on personal copies or split tooling.

---

## 📋 Table of Contents

- [Overview & Architecture](#-overview--architecture)
- [SharePoint Site Structure](#-sharepoint-site-structure)
- [Escalation Email Templates](#-escalation-email-templates)
- [Client Tracker](#-client-tracker)
- [Monthly SOC Reports](#-monthly-soc-reports)
- [SOC Schedule & Recurring Tasks](#-soc-schedule--recurring-tasks)
- [Microsoft Teams — The Front Door](#-microsoft-teams--the-front-door)
- [Detailed Guides](#-detailed-guides)

---

## 🎯 Overview & Architecture

Managing 3–4 enterprise clients from a Microsoft-focused SOC requires a system that is:

- **Centralised** — one source of truth, no personal copies
- **Fast** — escalation emails ready in under 10 minutes
- **Accessible** — any analyst can find anything immediately
- **Maintainable** — easy to update and keep current

### Core Principle

> **SharePoint owns the data. Teams surfaces it.**

| Layer | Tool | Purpose |
|---|---|---|
| Data & Documents | SharePoint Online | Single source of truth for all files, templates, client data |
| Collaboration | Microsoft Teams | Front door — all tabs pin back to SharePoint |
| Task Tracking | Microsoft Planner | Recurring analyst tasks, threat hunt schedule |
| Scheduling | Shared Outlook Calendar | Team-wide visibility of recurring events |
| Automation (optional) | Power Automate | Email template pre-population, report reminders |

---

## 📁 SharePoint Site Structure

The SharePoint site is named **"SOC Operations Hub"** and follows this folder structure:

```
SOC Operations Hub (SharePoint Site)
│
├── 📂 Clients/
│   ├── Client-A/
│   │   ├── Contact Details & Links.docx
│   │   ├── Escalation Contacts.xlsx
│   │   ├── Monthly Reports/
│   │   │   ├── 2025-01/
│   │   │   ├── 2025-02/
│   │   │   └── ...
│   │   └── Documents/
│   │       ├── SOW & Contracts/
│   │       ├── Architecture Diagrams/
│   │       └── Onboarding Notes/
│   ├── Client-B/
│   ├── Client-C/
│   └── Client-D/
│
├── 📂 Templates/
│   ├── Escalation Emails/
│   │   ├── P1-Critical-Incident.oft
│   │   ├── P2-High-Severity.oft
│   │   ├── P3-Medium-Escalation.oft
│   │   └── General-Update.oft
│   ├── Monthly SOC Reports/
│   │   ├── Monthly-Report-Template.docx
│   │   └── Archive/
│   └── Other Templates/
│       ├── Incident-Postmortem-Template.docx
│       └── Tabletop-Exercise-Template.docx
│
├── 📂 SOC Schedule & Runbooks/
│   ├── Weekly-Analyst-Role-Review.docx
│   ├── Monthly-Threat-Hunting-Checklist.docx
│   ├── Analyst-On-Call-Roster.xlsx
│   └── Runbooks/
│       ├── P1-Incident-Response-Runbook.docx
│       ├── Phishing-Response-Runbook.docx
│       └── Ransomware-Response-Runbook.docx
│
└── 📂 Resources/
    ├── Tool-Links-Quick-Reference.docx
    ├── Client-Quick-Reference.xlsx
    └── SOC-Team-Directory.docx
```

📄 See full details: [SharePoint Structure Guide](./docs/01-sharepoint-structure.md)

---

## ✉️ Escalation Email Templates

### Goal: Escalation email sent in under 10 minutes

**Approach: .OFT files stored in SharePoint**

`.oft` (Outlook Template) files are stored centrally in `Templates/Escalation Emails/`. Any analyst can open these directly from SharePoint and they launch as a pre-filled Outlook draft — no personal copies needed.

### Template Library

| Template | Use Case | Priority |
|---|---|---|
| `P1-Critical-Incident.oft` | Active breach, data exfiltration, ransomware | P1 |
| `P2-High-Severity.oft` | Suspicious activity, potential compromise | P2 |
| `P3-Medium-Escalation.oft` | Policy violations, anomalous behaviour | P3 |
| `General-Update.oft` | Status updates, non-urgent client comms | Info |

### How Analysts Use Them

1. Navigate to `Templates/Escalation Emails/` in SharePoint (or pin as a Teams tab)
2. Click the `.oft` file → opens as a new Outlook draft with subject, body, and To/CC pre-filled
3. Fill in the incident-specific details
4. Send — total time: **under 5 minutes**

### Advanced: Power Automate Email Flow

For one-click template selection with client contacts auto-filled:
- Analyst triggers a flow from Teams
- Selects client and incident type from a dropdown
- Flow pre-populates a draft with the correct escalation contacts from the Client Tracker SharePoint List
- Draft opens in Outlook ready to review and send

📄 See full details: [Escalation Email Guide](./docs/02-escalation-email-templates.md)

---

## 🗂️ Client Tracker

A **SharePoint List** (not a spreadsheet) is used as the central client registry. It is filterable, version-controlled, and accessible to the whole team with no conflicts.

### Client Tracker Columns

| Column | Type | Description |
|---|---|---|
| Client Name | Single line text | Full client name |
| Short Code | Single line text | e.g. CLT-A, used in folder names |
| Primary Contact | Person | Main point of contact |
| Escalation Contact | Single line text + phone | After-hours escalation |
| Client Portal URL | Hyperlink | Link to client's management portal |
| SharePoint Folder | Hyperlink | Direct link to client's folder |
| SLA Tier | Choice (P1 / P2 / P3) | Response SLA classification |
| Sentinel Workspace | Single line text | Workspace name or ID |
| Defender Tenant | Single line text | Tenant ID or name |
| Contract Review Date | Date | Next contract/SOW review |
| Notes | Multi-line text | Freeform operational notes |

### Access
- Pinned directly as a tab in the SOC Microsoft Teams channel
- Each row links to the client's SharePoint folder and portal
- Filtered views available (e.g., by SLA Tier, by assigned analyst)

📄 See full details: [Client Tracker Guide](./docs/03-client-tracker.md)

---

## 📊 Monthly SOC Reports

### Template Approach

The master report template is stored in `Templates/Monthly SOC Reports/Monthly-Report-Template.docx` and uses **Word Content Controls** to make it form-like — analysts tab through structured fields.

### Report Template Sections

1. **Executive Summary** — High-level posture summary for client stakeholders
2. **Incident Summary** — Table of all incidents (P1/P2/P3) for the month
3. **Detection Metrics** — Alerts triggered, false positive rate, MTTR
4. **Threat Intelligence Highlights** — Relevant threat actor activity, IOCs
5. **Vulnerability Summary** — Open vulnerabilities by severity, remediation progress
6. **Sentinel Health** — Log ingestion, analytics rule performance, playbook executions
7. **Recommendations** — Action items for client and SOC
8. **Next Month Preview** — Upcoming reviews, patching windows, scheduled exercises

### Monthly Workflow

1. Power Automate sends a reminder to the assigned analyst on the **1st of each month**
2. Analyst copies the template from SharePoint → saves to `Clients/[Client]/Monthly Reports/YYYY-MM/`
3. Completes the report using data from Sentinel, Defender, and Tenable
4. Sends to client and saves the finalised version to the same folder

📄 See full details: [Monthly Reports Guide](./docs/04-monthly-reports.md)

---

## 📅 SOC Schedule & Recurring Tasks

### Two-Tool Approach: Planner + Shared Calendar

#### Microsoft Planner Board

The SOC Planner board has columns representing cadence:

| Column | Tasks |
|---|---|
| 🔁 Weekly | Analyst role/queue review, Sentinel health check, Threat feed review |
| 📅 Monthly | Threat hunting session, SOC report generation, Vulnerability report review |
| 📆 Quarterly | Tabletop exercise, SLA review with clients, Detection rule audit |
| 🎯 Ad Hoc | Client requests, new onboardings, playbook updates |

Each task card includes:
- Assigned analyst (rotated)
- Due date
- Link to the relevant SharePoint runbook or checklist
- Checklist items within the card

#### Shared Outlook / Teams Calendar — "SOC Schedule"

A shared calendar visible to all analysts with recurring events:

| Event | Cadence | Duration |
|---|---|---|
| Analyst Role Review | Weekly — Monday 9:00am | 30 min |
| Monthly Threat Hunt | Monthly — 1st Thursday | 2 hours |
| SOC Report Due | Monthly — 5th of month | Reminder |
| Tabletop Exercise | Quarterly | Half day |
| SLA Review — All Clients | Quarterly | 1 hour each |

📄 See full details: [SOC Schedule Guide](./docs/05-soc-schedule.md)

---

## 🏠 Microsoft Teams — The Front Door

The SOC Teams channel is structured so analysts never need to go anywhere else. All SharePoint content is surfaced via pinned tabs.

### Channel Tab Setup

| Tab | What It Shows |
|---|---|
| 📋 Client Tracker | SharePoint List — all clients, contacts, links |
| 📁 Files | SharePoint document library (root) |
| ✅ Tasks | Microsoft Planner — SOC task board |
| 📅 SOC Schedule | Shared calendar for all recurring events |
| 🔗 Quick Links | SharePoint page with all key tool URLs |
| 📊 Reports | `Templates/Monthly SOC Reports/` folder |
| ✉️ Email Templates | `Templates/Escalation Emails/` folder |

### New Analyst Onboarding

When a new analyst joins:
1. Add them to the SOC Teams channel → they immediately get access to all tabs
2. Add them to the SharePoint site → file access granted
3. Walk them through the Planner board and shared calendar
4. No file sharing, no email chains, no "where is the template?" — everything is already there

---

## 📄 Detailed Guides

| Guide | Description |
|---|---|
| [01 — SharePoint Structure](./docs/01-sharepoint-structure.md) | Full folder hierarchy, naming conventions, permissions |
| [02 — Escalation Email Templates](./docs/02-escalation-email-templates.md) | OFT setup, Power Automate flow design, usage workflow |
| [03 — Client Tracker](./docs/03-client-tracker.md) | SharePoint List setup, column configuration, filtered views |
| [04 — Monthly Reports](./docs/04-monthly-reports.md) | Template structure, Word content controls, distribution workflow |
| [05 — SOC Schedule](./docs/05-soc-schedule.md) | Planner board setup, shared calendar, rotation management |

---

## 🛠️ Prerequisites

- Microsoft 365 Business Standard or above
- SharePoint Online access
- Microsoft Teams
- Microsoft Planner
- Power Automate (for automation flows — optional but recommended)
- Microsoft Outlook (desktop client for .OFT template support)

---

## 📌 Key Design Principles

1. **No personal copies** — everything lives in SharePoint, accessed by the team
2. **Teams as the single pane of glass** — analysts don't need to navigate SharePoint directly
3. **Templates are documents, not memory** — nothing depends on an analyst remembering a format
4. **New analyst = channel invite** — onboarding access is a single action
5. **Automation reduces admin** — Power Automate handles reminders and repetitive steps

---

*Built from real-world SOC operations experience managing enterprise Microsoft environments.*
