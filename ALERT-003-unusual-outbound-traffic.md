# 🚨 ALERT-003 — Unusual Outbound Network Traffic

## Alert Information

| Field | Details |
|-------|---------|
| **Alert ID** | ALERT-003 |
| **Date** | 2026-05-03 |
| **Time** | 11:08 PM |
| **Severity** | 🟡 Medium |
| **Source** | Firewall Logs / SIEM |
| **Triage Result** | ✅ True Positive |
| **Status** | Resolved |

---

## 📋 Alert Description

The SIEM triggered an alert for **unusual outbound network traffic** detected late at night from a workstation that is normally inactive outside business hours. The volume and timing of the traffic were anomalous.

---

## 📄 Log Sample

```
Alert Type: Anomalous Outbound Traffic
Timestamp: 2026-05-03 23:08:44
Source IP: 192.168.1.72
Source Hostname: DESKTOP-ACC02
Destination IP: 91.108.56.130
Destination Port: 443
Data Transferred: 1.8 GB outbound
Duration: 47 minutes
User Logged In: None (no active session)
```

---

## 🔍 Analysis

**Step 1 — Review the alert**
- 1.8 GB of outbound data at 11 PM from a workstation with no active user session
- This is highly unusual and a strong indicator of malicious activity

**Step 2 — Identify the destination**
- Destination IP `91.108.56.130` — checked against threat intelligence feeds
- IP is associated with known data exfiltration infrastructure

**Step 3 — Investigate the source machine**
- Remotely accessed `DESKTOP-ACC02` logs
- Found a scheduled task created 3 days ago running a PowerShell script at 11 PM daily
- PowerShell script was compressing and uploading files from the Documents folder

**Step 4 — Trace the origin**
- Investigated how the scheduled task was created
- Found that a malicious macro in an Excel file (opened 3 days ago) had created the task
- The Excel file was received via email attachment

**Step 5 — Containment**
- Immediately isolated `DESKTOP-ACC02` from the network
- Deleted the malicious scheduled task
- Removed the PowerShell script
- Blocked the destination IP at the firewall

---

## 🏷️ Triage Decision

**True Positive.** A malicious scheduled task was silently exfiltrating data every night. The attack originated from a malicious macro in an email attachment opened 3 days prior.

---

## ✅ Response Actions Taken

- Endpoint isolated and cleaned
- Malicious scheduled task and script removed
- Destination IP blocked at firewall
- Affected files identified — management notified of potential data breach
- Email attachment traced and removed from mail server
- Full forensic scan performed on the machine

---

## 📝 Recommendations

- Disable macros by default in Microsoft Office across all endpoints
- Set up alerts for any new scheduled tasks created outside business hours
- Monitor large outbound data transfers — set a threshold alert (e.g. over 500 MB)
- Implement Data Loss Prevention (DLP) tools to detect and block sensitive data uploads
