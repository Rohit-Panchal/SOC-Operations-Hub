# 📁 01 — SharePoint Site Structure

> Full guide to setting up the SOC Operations Hub SharePoint site — folder hierarchy, naming conventions, permissions, and best practices.

---

## Site Creation

1. Go to **SharePoint Online** → **+ Create site**
2. Choose **Team site** (not Communication site)
3. Name: `SOC Operations Hub`
4. Privacy: **Private** (invite only)
5. Add all SOC analysts as **Members**
6. Add the SOC Team Lead as **Owner**

---

## Full Folder Hierarchy

```
SOC Operations Hub (SharePoint Site Root)
│
├── 📂 Clients/
│   │
│   ├── 📂 Client-A/                          ← Use agreed short name, no spaces
│   │   ├── 📄 Contact Details & Links.docx   ← Primary & escalation contacts, portal URLs
│   │   ├── 📄 Escalation Contacts.xlsx       ← Quick-reference escalation matrix
│   │   ├── 📂 Monthly Reports/
│   │   │   ├── 📂 2025-01/
│   │   │   │   ├── SOC-Report-Client-A-2025-01-DRAFT.docx
│   │   │   │   └── SOC-Report-Client-A-2025-01-FINAL.docx
│   │   │   ├── 📂 2025-02/
│   │   │   └── 📂 ...
│   │   └── 📂 Documents/
│   │       ├── 📂 SOW & Contracts/
│   │       ├── 📂 Architecture Diagrams/
│   │       ├── 📂 Onboarding Notes/
│   │       └── 📂 Correspondence/
│   │
│   ├── 📂 Client-B/                          ← Same structure as Client-A
│   ├── 📂 Client-C/
│   └── 📂 Client-D/
│
├── 📂 Templates/
│   ├── 📂 Escalation Emails/
│   │   ├── 📧 P1-Critical-Incident.oft
│   │   ├── 📧 P2-High-Severity.oft
│   │   ├── 📧 P3-Medium-Escalation.oft
│   │   └── 📧 General-Update.oft
│   ├── 📂 Monthly SOC Reports/
│   │   ├── 📄 Monthly-Report-Template.docx   ← Master template — do not edit directly
│   │   └── 📂 Archive/                       ← Old template versions
│   └── 📂 Other Templates/
│       ├── 📄 Incident-Postmortem-Template.docx
│       ├── 📄 Tabletop-Exercise-Template.docx
│       └── 📄 Analyst-Handover-Template.docx
│
├── 📂 SOC Schedule & Runbooks/
│   ├── 📄 Weekly-Analyst-Role-Review.docx
│   ├── 📄 Monthly-Threat-Hunting-Checklist.docx
│   ├── 📄 Analyst-On-Call-Roster.xlsx
│   └── 📂 Runbooks/
│       ├── 📄 P1-Incident-Response-Runbook.docx
│       ├── 📄 Phishing-Response-Runbook.docx
│       ├── 📄 Ransomware-Response-Runbook.docx
│       ├── 📄 BEC-Business-Email-Compromise-Runbook.docx
│       └── 📄 Data-Exfiltration-Runbook.docx
│
└── 📂 Resources/
    ├── 📄 Tool-Links-Quick-Reference.docx    ← All portal URLs, tool logins, dashboards
    ├── 📄 Client-Quick-Reference.xlsx        ← One-page summary of all clients
    └── 📄 SOC-Team-Directory.docx            ← Analyst roster, roles, contact details
```

---

## Naming Conventions

### Folders
- Use hyphens, not spaces: `Client-A` not `Client A`
- Use consistent short codes agreed by the team for client names
- Monthly report folders use ISO date format: `YYYY-MM`

### Files
- Reports: `SOC-Report-[ClientCode]-[YYYY-MM]-[STATUS].docx`
  - Statuses: DRAFT, REVIEW, FINAL
  - Example: `SOC-Report-ClientA-2025-03-FINAL.docx`
- Templates: prefix with descriptive name, no date (they are evergreen)
- Runbooks: `[IncidentType]-Response-Runbook.docx`

---

## Permissions Model

| Role | SharePoint Permission | Access Scope |
|---|---|---|
| SOC Team Lead | Owner | Full site — read, write, manage |
| SOC Analyst | Member | Full site — read, write (no delete) |
| Client Stakeholder | Site visitor (specific library only) | Read-only to their own Client folder |
| Management | Member | Full site — read-only recommended |

### To Share a Client Folder with a Client Stakeholder
1. Navigate to `Clients/[ClientName]/`
2. Right-click → **Manage access**
3. Add the client contact as **Can view** (read only)
4. Do **not** add them to the full site

---

## Quick Setup Checklist

- [ ] Create SharePoint Team site named "SOC Operations Hub"
- [ ] Add all analysts as Members, Team Lead as Owner
- [ ] Create top-level folders: Clients, Templates, SOC Schedule & Runbooks, Resources
- [ ] Create a subfolder for each client under Clients/
- [ ] Upload the contact details template to each client folder
- [ ] Upload master report template to Templates/Monthly SOC Reports/
- [ ] Upload .OFT email templates to Templates/Escalation Emails/
- [ ] Upload runbooks to SOC Schedule & Runbooks/Runbooks/
- [ ] Pin the SharePoint document library as a tab in Teams

---

*Next: [02 — Escalation Email Templates](./02-escalation-email-templates.md)*
