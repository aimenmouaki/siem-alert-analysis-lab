# 🚨 ALERT-004 — Admin Login Outside Business Hours

## Alert Information

| Field | Details |
|-------|---------|
| **Alert ID** | ALERT-004 |
| **Date** | 2026-05-04 |
| **Time** | 03:27 AM |
| **Severity** | 🟠 High |
| **Source** | Windows Event Log / SIEM |
| **Triage Result** | ✅ True Positive |
| **Status** | Escalated & Resolved |

---

## 📋 Alert Description

The SIEM triggered a **High severity alert** for a successful login using an administrator account at 3:27 AM — well outside normal business hours (8 AM – 6 PM). Admin account logins outside business hours are considered high risk and require immediate investigation.

---

## 📄 Log Sample

```
Event ID: 4624 - An account was successfully logged on
Timestamp: 2026-05-04 03:27:19
Account Name: admin_sys
Account Type: Administrator
Logon Type: 10 (Remote Interactive / RDP)
Source IP: 41.102.88.214
Workstation: SERVER-MAIN
Country: Unknown (IP geolocation: Outside company's country)
```

---

## 🔍 Analysis

**Step 1 — Confirm the alert**
- Successful RDP login to `SERVER-MAIN` using `admin_sys` account at 3:27 AM
- Source IP `41.102.88.214` is external and geolocates outside the company's country
- No VPN connection was recorded for this session

**Step 2 — Contact the admin**
- Immediately contacted the system administrator on call
- Admin confirmed they did NOT log in at that time and were asleep
- This confirms the login was unauthorized

**Step 3 — Immediate containment**
- Disabled the `admin_sys` account immediately
- Terminated the active RDP session
- Blocked source IP `41.102.88.214` at the firewall

**Step 4 — Investigate what was accessed**
- Reviewed activity logs during the session (3:27 AM – 3:41 AM, 14 minutes)
- Attacker browsed file directories on the server
- No files were deleted or downloaded — likely reconnaissance activity
- No new accounts were created during the session

**Step 5 — Determine how access was gained**
- Investigated how the attacker obtained the admin credentials
- Found the `admin_sys` password had not been changed in over 2 years
- Password was likely obtained through a previous credential leak or brute-force

---

## 🏷️ Triage Decision

**True Positive — Unauthorized Admin Access.** An attacker gained RDP access to the main server using compromised admin credentials from an external IP. The session lasted 14 minutes and appeared to be reconnaissance. No data was confirmed stolen.

---

## ✅ Response Actions Taken

- Unauthorized RDP session terminated immediately
- Admin account disabled and password reset with a strong new password
- External IP blocked at the firewall
- All other admin account passwords reviewed and reset
- RDP access restricted to VPN-only connections going forward
- Incident escalated to management and security team
- Full server audit conducted — no backdoors or new accounts found

---

## 📝 Recommendations

- **Disable RDP from the public internet immediately** — only allow via VPN
- Enforce MFA on all administrator accounts
- Set up automatic alerts for any admin login outside business hours
- Implement a password rotation policy — admin passwords should change every 90 days
- Consider using a Privileged Access Management (PAM) solution for admin accounts
