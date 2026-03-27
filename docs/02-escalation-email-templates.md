# ✉️ 02 — Escalation Email Templates

> Guide to setting up and using centralised escalation email templates for SOC analysts — stored in SharePoint, accessible in under 10 minutes.

---

## Overview

The goal is for any analyst to send a professional, complete escalation email to the right client contacts within **10 minutes of incident triage**.

Two approaches are documented here:
1. **OFT Files in SharePoint** — simple, works today, no extra setup
2. **Power Automate Flow** — advanced, one-click with auto-filled contacts

---

## Approach 1: OFT Files in SharePoint

### What is an OFT file?
An `.oft` file is an Outlook email template. When opened, it creates a new email draft pre-filled with subject, body text, To/CC fields, and formatting. It does not send automatically — the analyst reviews and sends.

### How to Create an OFT Template

1. In Outlook, compose a new email
2. Fill in the **Subject**, **Body**, and **To/CC** fields with the template content (use placeholders like `[CLIENT NAME]`, `[INCIDENT ID]`)
3. Go to **File → Save As**
4. Change file type to **Outlook Template (.oft)**
5. Save the file
6. Upload the `.oft` file to SharePoint: `Templates/Escalation Emails/`

### How Analysts Use OFT Templates

1. Navigate to `Templates/Escalation Emails/` in SharePoint (or open via the Teams Files tab)
2. Click the `.oft` file → browser prompts to open
3. It launches in Outlook as a new draft with all pre-filled content
4. Analyst fills in: incident-specific details, client name, any relevant IOCs
5. Confirms To/CC contacts match the current client
6. Sends — **total time: under 5 minutes**

> ⚠️ OFT files require Outlook desktop client (not Outlook web). Ensure all analysts have Outlook desktop installed.

---

## Template Library

### P1 — Critical Incident

**Subject:** `[P1 CRITICAL] Security Incident Notification — [CLIENT NAME] — [DATE]`

**To:** Client Primary Security Contact  
**CC:** Client Technical Lead, SOC Team Lead

```
Dear [CLIENT NAME] Security Team,

We are writing to notify you of a critical (P1) security incident identified in your environment.

Incident Reference: [INCIDENT ID]
Detection Time: [TIME] AEST
Severity: P1 — Critical
Status: Active Investigation

Summary:
[2-3 sentence summary of what was detected, e.g. "We have detected indicators of ransomware activity on host [HOSTNAME]. Lateral movement has been observed and we are currently scoping the extent of compromise."]

Immediate Actions Taken:
- [Action 1, e.g. Host isolated via Defender for Endpoint]
- [Action 2, e.g. Affected account credentials reset]
- [Action 3]

Current Status:
[Brief status, e.g. "Investigation is ongoing. We are actively reviewing logs in Microsoft Sentinel and coordinating containment."]

Required Actions from Your Team:
- [Any client actions needed, e.g. "Please confirm whether [USER] should be locked out at directory level"]
- [Contact your incident response retainer if data exfiltration is confirmed]

We will provide an update within [30 minutes / 1 hour]. Please contact us immediately on [SOC PHONE] if you have additional information.

Regards,
[ANALYST NAME]
SOC Analyst — [COMPANY NAME]
[EMAIL] | [PHONE]
```

---

### P2 — High Severity

**Subject:** `[P2 HIGH] Security Alert Notification — [CLIENT NAME] — [DATE]`

**To:** Client Primary Security Contact  
**CC:** SOC Team Lead

```
Dear [CLIENT NAME] Security Team,

We are writing to notify you of a high-severity (P2) security alert detected in your environment.

Incident Reference: [INCIDENT ID]
Detection Time: [TIME] AEST
Severity: P2 — High
Status: Under Investigation

Summary:
[Brief description of alert, e.g. "We have detected suspicious login activity from an unusual geographic location for account [USERNAME]. Multiple failed authentication attempts were followed by a successful login from [COUNTRY]."]

Actions Taken:
- [Action 1]
- [Action 2]

Next Steps:
[What the SOC is doing next, e.g. "We are reviewing sign-in logs and MFA activity. If malicious intent is confirmed, we will escalate to P1 and initiate containment."]

We will provide a full update within [2 hours / next business hours].

Regards,
[ANALYST NAME]
SOC Analyst — [COMPANY NAME]
```

---

### P3 — Medium Escalation

**Subject:** `[P3 MEDIUM] Security Notification — [CLIENT NAME] — [DATE]`

```
Dear [CLIENT NAME] Team,

Please find below a security notification for your awareness and action.

Alert Reference: [INCIDENT ID]
Detected: [DATE / TIME]
Severity: P3 — Medium

Description:
[Description, e.g. "We have identified a policy violation involving a user attempting to send a document classified as Confidential to an external email address. The email was blocked by Microsoft Purview DLP."]

Recommended Action:
[e.g. "Please review with the user [USERNAME] whether this was intentional. If confirmed accidental, no further action required. If intentional, please initiate your internal HR process."]

No immediate containment action has been taken. Please advise if you require us to take any additional steps.

Regards,
[ANALYST NAME]
SOC Analyst — [COMPANY NAME]
```

---

### General Update / Status Email

**Subject:** `SOC Update — [CLIENT NAME] — [INCIDENT ID] — [DATE]`

```
Dear [CLIENT NAME] Team,

This is a status update regarding incident [INCIDENT ID].

Current Status: [In Progress / Contained / Closed]

Update:
[What has happened since the last communication. E.g. "Forensic analysis of the affected host is complete. No evidence of data exfiltration was found. The host has been re-imaged and returned to service."]

Actions Completed:
- [Action 1]
- [Action 2]

Remaining Actions:
- [If any]

Estimated Resolution: [Time/Date or "No further action required"]

Please do not hesitate to contact us if you have any questions.

Regards,
[ANALYST NAME]
SOC Analyst — [COMPANY NAME]
```

---

## Approach 2: Power Automate Email Flow (Advanced)

This approach enables analysts to trigger an email template directly from Teams with the client's escalation contacts **automatically populated**.

### Flow Design

```
Trigger: Manual (Teams Adaptive Card button or Power Apps form)
    ↓
Input: Select Client (dropdown from SharePoint Client List)
Input: Select Template Type (P1 / P2 / P3 / General)
Input: Brief incident description (text field)
    ↓
Action: Get client contact details from SharePoint List
    ↓
Action: Create draft email in Outlook with:
    - Subject line from selected template
    - Body pre-filled with selected template + incident description
    - To/CC populated from SharePoint Client List
    ↓
Output: Draft email opens in Outlook for analyst review and send
```

### Benefits
- No risk of sending to wrong contacts (pulled from single source)
- Incident description integrated into the template automatically
- Logs the escalation trigger in SharePoint for audit trail
- Works from Teams without opening a file browser

### Prerequisites
- Power Automate Premium licence (or M365 E3/E5)
- SharePoint Client List set up (see [03 — Client Tracker](./03-client-tracker.md))
- Analyst familiarity with triggering the flow

---

## Quick Reference Card

Print and pin at each analyst's desk or post in the SOC Teams channel:

| Severity | Template File | Use When |
|---|---|---|
| P1 Critical | `P1-Critical-Incident.oft` | Active breach, ransomware, confirmed compromise |
| P2 High | `P2-High-Severity.oft` | Suspicious activity, potential compromise, account takeover |
| P3 Medium | `P3-Medium-Escalation.oft` | Policy violations, anomalous behaviour, DLP triggers |
| General | `General-Update.oft` | Status updates, client questions, non-urgent comms |

**SharePoint path:** `SOC Operations Hub → Templates → Escalation Emails`

---

*Previous: [01 — SharePoint Structure](./01-sharepoint-structure.md) | Next: [03 — Client Tracker](./03-client-tracker.md)*
