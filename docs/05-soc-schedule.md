# 📅 05 — SOC Schedule & Recurring Tasks

> Guide to implementing a consistent SOC operating rhythm — weekly analyst roles, monthly threat hunting, quarterly reviews, and team scheduling using Microsoft Planner and a shared calendar.

---

## Overview

A well-run SOC operates on a predictable rhythm. Without a structured schedule:
- Threat hunting gets skipped when things are busy
- Analyst role reviews drift and become reactive
- Monthly reports are late
- New analysts don't know what "normal" looks like

The solution is two complementary tools:
- **Microsoft Planner** — task management with rotation, runbook links, and checklists
- **Shared Outlook/Teams Calendar** — team-wide visibility of recurring events

---

## Microsoft Planner — SOC Task Board

### Board Structure

The SOC Planner board uses columns (buckets) based on cadence:

```
📋 SOC Task Board
│
├── 🔁 Weekly
├── 📅 Monthly
├── 📆 Quarterly
├── 🎯 Ad Hoc
└── ✅ Completed
```

### Weekly Tasks

| Task | Assigned To | Due | Linked Runbook/Checklist |
|---|---|---|---|
| Analyst Role Review | Rotating | Monday | Weekly-Analyst-Role-Review.docx |
| Sentinel Health Check | Rotating | Monday | N/A — Sentinel workbook |
| Threat Feed Review | Rotating | Wednesday | N/A — threat intel sources |
| Vulnerability Report Review | Rotating | Friday | N/A — Tenable dashboard |
| Analyst Queue Audit | Team Lead | Friday | N/A |

### Monthly Tasks

| Task | Assigned To | Due | Linked Runbook/Checklist |
|---|---|---|---|
| Threat Hunting Session | All analysts | 1st Thursday | Monthly-Threat-Hunting-Checklist.docx |
| SOC Report — Client A | Assigned analyst | 5th | Monthly-Report-Template.docx |
| SOC Report — Client B | Assigned analyst | 5th | Monthly-Report-Template.docx |
| SOC Report — Client C | Assigned analyst | 5th | Monthly-Report-Template.docx |
| SOC Report — Client D | Assigned analyst | 5th | Monthly-Report-Template.docx |
| Detection Rule Audit | Senior Analyst | 15th | N/A — Sentinel analytics rules |
| Playbook Review | Senior Analyst | 15th | N/A — Sentinel playbooks |

### Quarterly Tasks

| Task | Assigned To | Due | Linked Resource |
|---|---|---|---|
| Tabletop Exercise | Team Lead + All | Q end (last week) | Tabletop-Exercise-Template.docx |
| SLA Review — All Clients | Team Lead | Q end | Client Tracker SharePoint List |
| Detection Coverage Review | Senior Analyst | Q end | MITRE ATT&CK coverage map |
| Analyst Performance Review | Team Lead | Q end | Internal HR process |
| Onboarding Review | Team Lead | Q end | Check for outdated contacts |

### Ad Hoc Tasks
Used for: client onboardings, new detection rule deployments, unplanned projects, and audit requests. Create tasks here with clear owners and due dates.

---

## Setting Up the Planner Board

1. Open **Microsoft Teams** → your SOC channel
2. Click **+** to add a tab → search for **Tasks by Planner**
3. Create a **New plan** named: `SOC Task Board`
4. Create the buckets: Weekly, Monthly, Quarterly, Ad Hoc, Completed
5. Add the recurring tasks listed above
6. For each task:
   - Set **Due date** (use recurrence workaround — see note below)
   - Assign to an analyst
   - Add **Checklist items** within the task card
   - Attach the relevant SharePoint link in the task notes

> ⚠️ **Planner does not natively support recurring tasks.** Workaround: Create the task, complete it at end of period, and manually re-create it. Alternatively, use Power Automate to auto-create recurring tasks on a schedule.

### Power Automate Recurring Task Creation (Recommended)

```
Trigger: Recurrence (weekly — Monday 7am)
Action: Create a task in Planner
    - Bucket: Weekly
    - Title: "Analyst Role Review — Week of [Date]"
    - Due: Monday 5pm
    - Assign to: [Rotating analyst — use a list variable]
```

Repeat for each recurring task type.

---

## Shared Calendar — "SOC Schedule"

### Create the Shared Calendar

**In Outlook:**
1. Calendar view → **Add Calendar → Create new blank calendar**
2. Name: `SOC Schedule`
3. Right-click the new calendar → **Sharing permissions**
4. Add all SOC analysts with **Can view all details** permission
5. Each analyst subscribes by accepting the share invite

**In Teams:**
1. Pin the shared calendar as a tab using the **Channel calendar** app
2. Name the tab: `📅 SOC Schedule`

---

## Recurring Calendar Events

Set these up as recurring events in the SOC Schedule calendar:

### Weekly — Analyst Role Review
- **Title:** SOC Analyst Role Review
- **Day/Time:** Every Monday, 9:00am – 9:30am
- **Location:** SOC Teams channel (online)
- **Recurrence:** Weekly
- **Purpose:** Review queue assignments, check any weekend alerts, confirm analyst coverage for the week

### Monthly — Threat Hunting Session
- **Title:** Monthly Threat Hunt — [Month]
- **Day/Time:** First Thursday of each month, 9:00am – 11:00am
- **Recurrence:** Monthly (first Thursday)
- **Purpose:** Structured threat hunting using the monthly checklist. Rotate lead hunter role each month.

### Monthly — SOC Report Deadline
- **Title:** 🔴 SOC Reports Due — All Clients
- **Day/Time:** 5th of each month, all day (reminder)
- **Recurrence:** Monthly
- **Purpose:** Visual reminder that all client reports must be finalised and sent by end of day

### Quarterly — Tabletop Exercise
- **Title:** Quarterly Tabletop Exercise
- **Day/Time:** Last week of each quarter — half day
- **Recurrence:** Quarterly
- **Purpose:** Simulated threat scenario to test response procedures and playbook effectiveness

### Quarterly — Client SLA Review
- **Title:** Quarterly SLA Review — All Clients
- **Day/Time:** Last week of each quarter
- **Recurrence:** Quarterly
- **Purpose:** Review SLA performance, discuss service improvements, confirm escalation contacts

---

## Analyst Role Rotation

For the weekly role review, use a simple rotation roster:

| Role | Week 1 | Week 2 | Week 3 | Week 4 |
|---|---|---|---|---|
| Lead Analyst | Analyst A | Analyst B | Analyst C | Analyst A |
| Threat Feed Review | Analyst B | Analyst C | Analyst A | Analyst B |
| Vuln Report Review | Analyst C | Analyst A | Analyst B | Analyst C |
| On-Call (after hours) | Analyst A | Analyst B | Analyst C | Analyst A |

Store the current roster in `SOC Schedule & Runbooks/Analyst-On-Call-Roster.xlsx` in SharePoint, and post the current week's assignments in the Teams SOC channel every Monday.

---

## Monthly Threat Hunting Checklist

A starter checklist for the monthly threat hunt session (store in SharePoint as a .docx):

### Pre-Hunt (15 min)
- [ ] Review current threat intel feed for relevant TTPs
- [ ] Check MITRE ATT&CK for any new technique updates relevant to client sector
- [ ] Pull last month's hunt findings for context

### Hunt Activities (90 min)
- [ ] Hunt for living-off-the-land (LOLBin) abuse (e.g. PowerShell, WMI, certutil)
- [ ] Review privileged account activity for anomalies
- [ ] Check for lateral movement indicators (SMB, RDP, WMI across hosts)
- [ ] Review service account behaviour for deviations
- [ ] Hunt for persistence mechanisms (scheduled tasks, registry run keys, new services)
- [ ] Review cloud app OAuth grants for suspicious third-party access
- [ ] Check email forwarding rules for exfiltration indicators
- [ ] Review Defender for Cloud recommendations for critical findings

### Post-Hunt (15 min)
- [ ] Document findings (true positives, near-misses, confirmed clean)
- [ ] Create Planner tasks for any new detection rules to build
- [ ] Update monthly SOC report with threat hunt summary

---

## Quick Reference — SOC Operating Rhythm

| Cadence | Activity | Day/Time | Owner |
|---|---|---|---|
| Daily | Alert triage and response | Continuous | On-duty analyst |
| Weekly | Analyst role review | Monday 9am | Rotating lead |
| Weekly | Sentinel health check | Monday | Rotating |
| Weekly | Threat feed review | Wednesday | Rotating |
| Monthly | Threat hunting | 1st Thursday | Rotating lead |
| Monthly | SOC reports | Due 5th | Assigned analysts |
| Monthly | Detection rule review | 15th | Senior Analyst |
| Quarterly | Tabletop exercise | Last week of Q | Team Lead |
| Quarterly | SLA reviews | Last week of Q | Team Lead |
| Annually | Analyst skill reviews | Q4 | Team Lead |

---

*Previous: [04 — Monthly Reports](./04-monthly-reports.md) | Back to: [README](../README.md)*
