# 📊 04 — Monthly SOC Reports

> Guide to the monthly SOC report process — template structure, analyst workflow, distribution, and archiving.

---

## Overview

Monthly SOC reports serve two purposes:
1. **Client transparency** — demonstrating value, communicating security posture, and showing responsiveness to threats
2. **Internal accountability** — tracking metrics, identifying trends, and ensuring consistent service delivery

The master template lives in SharePoint. Analysts copy it each month, complete it, and store the final version in the client's folder.

---

## Template Location

```
SOC Operations Hub
└── Templates
    └── Monthly SOC Reports
        ├── Monthly-Report-Template.docx    ← Master — do not edit this directly
        └── Archive                         ← Old template versions
```

> ⚠️ **Never edit the master template directly.** Always copy it first.

---

## Monthly Report Template Structure

### Section 1 — Executive Summary
*Audience: Client management, non-technical stakeholders*

- Overall security posture for the month (Good / Elevated / Critical)
- Key highlights: what happened, what was resolved, what is outstanding
- 2–3 bullet points max — this is the section executives read

### Section 2 — Incident Summary

A table of all security incidents for the month:

| Incident ID | Date | Severity | Title | Status | MTTR |
|---|---|---|---|---|---|
| INC-2025-0042 | 12 Mar | P2 | Suspicious login from overseas IP | Closed | 45 min |
| INC-2025-0043 | 18 Mar | P1 | Ransomware indicators on WS-087 | Closed | 3h 10m |

Include brief notes on root cause and resolution for P1/P2 incidents.

### Section 3 — Detection Metrics

| Metric | This Month | Last Month | Trend |
|---|---|---|---|
| Total alerts generated | 142 | 168 | ↓ 15% |
| True positives | 12 | 9 | ↑ 33% |
| False positives | 130 | 159 | ↓ 18% |
| False positive rate | 91.5% | 94.6% | Improving |
| Mean Time to Detect (MTTD) | 8 min | 12 min | ↓ |
| Mean Time to Respond (MTTR) | 42 min | 67 min | ↓ |

### Section 4 — Threat Intelligence Highlights

- Summary of relevant threat actor activity this month
- Any industry-specific threats relevant to the client's sector
- Key IOCs distributed and applied to detection rules
- CVEs of significance (if applicable to client's tech stack)

### Section 5 — Vulnerability Summary

| Severity | Open (Start of Month) | New This Month | Remediated | Open (End of Month) |
|---|---|---|---|---|
| Critical | 3 | 1 | 2 | 2 |
| High | 14 | 3 | 8 | 9 |
| Medium | 47 | 12 | 15 | 44 |
| Low | 103 | 22 | 18 | 107 |

Include: oldest open critical/high CVE and estimated remediation date.

### Section 6 — Microsoft Sentinel Health

- Log ingestion volumes (GB/day average, any anomalies)
- Analytics rules: total active, any disabled or misconfigured
- Playbook execution summary (automated actions taken)
- Any gaps in log coverage identified and remediation status

### Section 7 — Recommendations

Numbered action items for the client:

1. **[Action]** — [Why it matters] — Priority: High / Medium / Low — Due: [Date]
2. **[Action]** — [Why it matters] — Priority: Medium — Due: [Date]

### Section 8 — Next Month Preview

- Upcoming scheduled maintenance windows
- Planned patching dates
- Any tabletop exercises or SLA reviews scheduled
- Items carried over from this month

---

## Monthly Workflow

### Week Before Month End
- Assigned analyst begins collecting data from Sentinel, Defender, and Tenable
- Reviews all incidents closed in the month
- Pulls metrics from Sentinel workbooks

### Day 1–3 of New Month
1. Navigate to `Templates/Monthly SOC Reports/` in SharePoint
2. **Copy** the `Monthly-Report-Template.docx` (right-click → Copy)
3. Navigate to `Clients/[ClientName]/Monthly Reports/`
4. Create a new folder named `YYYY-MM` (e.g. `2025-03`)
5. Paste the template copy into this folder
6. Rename it: `SOC-Report-[ClientCode]-[YYYY-MM]-DRAFT.docx`
7. Open and complete all sections
8. Save as: `SOC-Report-[ClientCode]-[YYYY-MM]-REVIEW.docx`
9. Tag the Team Lead in Teams for review

### Review & Approval
- Team Lead reviews within 1 business day
- Minor corrections made in place
- Final version renamed: `SOC-Report-[ClientCode]-[YYYY-MM]-FINAL.docx`

### Distribution
- Email the FINAL report to the client's Primary Contact
- Use the `General-Update.oft` template for the covering email
- CC the SOC Team Lead
- Save the sent email or log the date in the SharePoint List client row

### Archiving
- FINAL version remains in the `YYYY-MM` folder in SharePoint
- DRAFT and REVIEW versions can be deleted once FINAL is distributed
- All previous reports are accessible in the client's `Monthly Reports/` folder

---

## Power Automate Reminder (Optional)

Set up a monthly reminder flow:

```
Trigger: Recurrence — 1st of every month, 8:00am
Action: Post Teams message to SOC channel:
    "📊 Monthly SOC reports are due for all clients by [5th of month].
    Template: [SharePoint link]
    Clients due: Client-A, Client-B, Client-C, Client-D"
```

This removes the reliance on anyone remembering and ensures consistent delivery.

---

## Word Content Controls (Making the Template Form-Like)

To make the template easier and faster to complete:

1. Open the master template in Word
2. Enable the **Developer tab**: File → Options → Customize Ribbon → Check Developer
3. For each fillable field, insert a **Plain Text Content Control**
4. Label each control (e.g. "Executive Summary Text", "Report Month")
5. Analysts press **Tab** to move between fields — no hunting for placeholders

---

*Previous: [03 — Client Tracker](./03-client-tracker.md) | Next: [05 — SOC Schedule](./05-soc-schedule.md)*
