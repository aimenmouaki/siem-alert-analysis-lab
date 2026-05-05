# 🚨 ALERT-001 — Multiple Failed Login Attempts

## Alert Information

| Field | Details |
|-------|---------|
| **Alert ID** | ALERT-001 |
| **Date** | 2026-05-01 |
| **Time** | 08:42 AM |
| **Severity** | 🟡 Medium |
| **Source** | Windows Event Log / SIEM |
| **Triage Result** | ✅ True Positive |
| **Status** | Resolved |

---

## 📋 Alert Description

The SIEM triggered an alert for **multiple failed login attempts** on a user account within a short time window. This pattern is commonly associated with brute-force attacks or unauthorized access attempts.

---

## 📄 Log Sample

```
Event ID: 4625 - An account failed to log on
Timestamp: 2026-05-01 08:42:11
Account Name: j.martin
Workstation: DESKTOP-HR04
IP Address: 192.168.1.45
Failure Reason: Unknown username or bad password
Attempt Count: 17 failed attempts in 4 minutes
```

---

## 🔍 Analysis

**Step 1 — Review the alert details**
- 17 failed login attempts in 4 minutes on account `j.martin`
- All attempts came from internal IP `192.168.1.45`
- Event ID 4625 confirms repeated authentication failures

**Step 2 — Check if the account was eventually locked**
- Searched for Event ID 4740 (Account Lockout) — confirmed account was locked after 10 attempts
- This is expected behavior based on the company's password policy

**Step 3 — Identify the source**
- IP `192.168.1.45` belongs to workstation `DESKTOP-HR04` in the HR department
- Checked if the actual user `j.martin` was at work — confirmed she was present and had forgotten her password after returning from vacation

**Step 4 — Rule out malicious activity**
- No login attempts from external or unknown IPs
- No successful login from a different location before or after the attempts
- No lateral movement or suspicious processes detected on the workstation

---

## 🏷️ Triage Decision

**True Positive** — The alert correctly flagged repeated failed logins. However, after investigation, the root cause was a legitimate user forgetting their password, not a malicious actor.

---

## ✅ Response Actions Taken

- Unlocked the user account via Active Directory
- Reset the password and provided it securely to the user
- Reminded the user of the company's password policy
- Closed the alert as **Resolved — Legitimate User Activity**

---

## 📝 Recommendations

- Consider implementing a self-service password reset tool to reduce help desk load
- Review the account lockout threshold — 10 attempts may be too high; 5 is more secure
- Add MFA (Multi-Factor Authentication) to reduce risk from brute-force attacks
