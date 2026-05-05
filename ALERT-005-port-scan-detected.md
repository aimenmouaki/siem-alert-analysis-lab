# 🚨 ALERT-005 — Port Scan Detected

## Alert Information

| Field | Details |
|-------|---------|
| **Alert ID** | ALERT-005 |
| **Date** | 2026-05-05 |
| **Time** | 10:14 AM |
| **Severity** | 🟠 High |
| **Source** | Firewall / IDS / SIEM |
| **Triage Result** | ⚠️ Investigated — Monitoring |
| **Status** | Monitoring |

---

## 📋 Alert Description

The SIEM and IDS triggered an alert for a **port scan** originating from an external IP address targeting the company's public-facing infrastructure. Port scanning is often a precursor to an attack, used by attackers to discover open services and vulnerabilities.

---

## 📄 Log Sample

```
Alert Type: Port Scan Detected
Timestamp: 2026-05-05 10:14:02
Source IP: 185.156.73.201
Target IP Range: 203.0.113.0/24 (Company's public IP range)
Ports Scanned: 22, 23, 80, 443, 3389, 8080, 8443, 21, 25, 3306
Scan Type: SYN Scan (Stealth Scan)
Packets Sent: 1,247 in 3 minutes
IDS Signature: ET SCAN Nmap SYN Scan
```

---

## 🔍 Analysis

**Step 1 — Confirm the alert**
- IDS signature matched `Nmap SYN Scan` — a common reconnaissance tool used by attackers
- 1,247 packets across 10 common ports in just 3 minutes — clearly automated scanning
- Source IP `185.156.73.201` — checked against threat intelligence
- IP has been reported in multiple threat feeds for scanning and reconnaissance activity

**Step 2 — Assess what was discovered by the scan**
- Reviewed firewall rules to determine which ports are currently open to the public:
  - Port 80 (HTTP) — Open ✅
  - Port 443 (HTTPS) — Open ✅
  - Port 3389 (RDP) — ⚠️ Open — this should NOT be publicly accessible
  - All other scanned ports — Closed ✅

**Step 3 — Evaluate the threat level**
- Port scan alone is not an attack — it is reconnaissance
- However, the discovery that RDP (3389) is open externally is a serious finding
- Combined with the earlier ALERT-004 (unauthorized RDP login), this increases concern

**Step 4 — Check for follow-up activity**
- Monitored logs for the next 2 hours for any connection attempts from `185.156.73.201`
- No follow-up intrusion attempts detected from this IP
- Continued monitoring set up for 24 hours

---

## 🏷️ Triage Decision

**Investigated — Monitoring.** The port scan itself caused no damage, but it revealed that RDP port 3389 is publicly exposed — a serious vulnerability that needs immediate remediation. This finding is linked to ALERT-004.

---

## ✅ Response Actions Taken

- Source IP `185.156.73.201` blocked at the perimeter firewall
- **RDP port 3389 immediately closed to public internet** (critical finding)
- RDP access restricted to VPN-only as recommended in ALERT-004
- 24-hour enhanced monitoring set up on the public IP range
- Security team notified of the reconnaissance activity

---

## 📝 Recommendations

- Conduct a full **external attack surface review** — identify all publicly exposed ports and services
- Follow the principle of **least exposure** — only open ports that are absolutely necessary
- Deploy a **honeypot** to detect and study future scanning attempts
- Subscribe to threat intelligence feeds to proactively block known scanner IPs
- Schedule regular **vulnerability scans** on public-facing infrastructure
