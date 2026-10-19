<a id="top"></a>
<div align="center">

# 🧭 Executive Summary
### Linux SysAdmin Labs

![Ubuntu](https://img.shields.io/badge/Ubuntu_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![BIND9](https://img.shields.io/badge/BIND9-1A5276?style=for-the-badge)
![Security](https://img.shields.io/badge/ufw_·_SSH_·_fail2ban-943126?style=for-the-badge)
![Monitoring](https://img.shields.io/badge/Uptime_Kuma-5CDD8B?style=for-the-badge)

**A one-page read of what this portfolio proves — protect, publish, harden, name, manage, monitor.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Labs | 🖼️ Screenshots | 🖥 Servers | 📦 Updates Installed | 🌐 DNS Records | ⏱ Restore Time |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **6** | **146** | **1 Ubuntu VM** | **84** | **7** | **2 min 47 s** |

</div>

---

## 🧩 The Six Labs, One Thread

```mermaid
flowchart LR
    A["💾 Protect<br/>Backup"] --> B["🔒 Publish<br/>Nginx + TLS"]
    B --> C["🛡️ Harden<br/>ufw, SSH"]
    C --> D["🌐 Name<br/>DNS"]
    D --> E["👤 Manage<br/>Users"]
    E --> F["📡 Monitor<br/>Disk, uptime"]
    style A fill:#1A5276,color:#fff,stroke:#0B2E43
    style B fill:#117864,color:#fff,stroke:#083D33
    style C fill:#943126,color:#fff,stroke:#561C16
    style D fill:#B9770E,color:#fff,stroke:#6E4409
    style E fill:#76448A,color:#fff,stroke:#432752
    style F fill:#1E8449,color:#fff,stroke:#0E4A28
```

---

## 📂 Lab Summary

<table>
<tr>
<td width="50%" valign="top">

**💾 01 — Linux Backup & Restore**
`Bash` `tar` `mysqldump` `SHA-256` `cron`

Backed up a web page and a database, checked the backup, deleted the live data on purpose and restored it.

**Status:** ✅ Complete
[README](./lab01-backup-restore/README.md) · [Index](./lab01-backup-restore/INDEX.md)

</td>
<td width="50%" valign="top">

**🔒 02 — Nginx Reverse Proxy & TLS**
`Nginx` `Apache` `OpenSSL` `Azure NSG`

Put Nginx with a self-signed certificate in front of Apache. HTTP now redirects to HTTPS.

**Status:** ✅ Complete
[README](./lab02-nginx-tls/README.md) · [Index](./lab02-nginx-tls/INDEX.md)

</td>
</tr>
<tr>
<td width="50%" valign="top">

**🛡️ 03 — SSH Hardening, Firewall & Patching**
`ufw` `OpenSSH` `apt` `fail2ban`

Snapshot first, then firewall, SSH settings, 84 updates with a reboot, automatic updates and fail2ban.

**Status:** ✅ Complete
[README](./lab03-ssh-firewall-patching/README.md) · [Index](./lab03-ssh-firewall-patching/INDEX.md)

</td>
<td width="50%" valign="top">

**🌐 04 — BIND9 DNS Server**
`BIND9` `dig` `nslookup`

Built a DNS server for `lab.local` and tested all 7 records plus a missing name.

**Status:** ✅ Complete
[README](./lab04-bind9-dns/README.md) · [Index](./lab04-bind9-dns/INDEX.md)

</td>
</tr>
<tr>
<td width="50%" valign="top">

**👤 05 — User Management & Offboarding**
`adduser` `sudo` `chage` `tar`

Created a test user with sudo, then locked, archived and deleted it in a safe order.

**Status:** ✅ Complete
[README](./lab05-users-offboarding/README.md) · [Index](./lab05-users-offboarding/INDEX.md)

</td>
<td width="50%" valign="top">

**📡 06 — Disk & Uptime Monitoring**
`Bash` `cron` `syslog` `Uptime Kuma` `pm2`

Wrote a disk alert, then stopped Nginx on purpose and watched the monitor go DOWN and recover.

**Status:** ✅ Complete
[README](./lab06-monitoring/README.md) · [Index](./lab06-monitoring/INDEX.md)

</td>
</tr>
</table>

---

## 🔎 How a Claim Gets Verified

```mermaid
flowchart LR
    C["📝 Claim made<br/>in a lab"] --> E{"Evidence<br/>exists?"}
    E -->|"Screenshot"| S["🖼️ Backed by<br/>a screenshot"]
    E -->|"Command output"| L["🔍 Backed by a live<br/>command check"]
    E -->|"Neither"| N["📝 Marked as a note,<br/>not a proven result"]
    S --> V["✅ Verified"]
    L --> V
    N --> G["⚠️ Stated as a gap<br/>in Scope & Limitations"]

    style C fill:#1A5276,color:#fff,stroke:#0B2E43
    style E fill:#B7950B,color:#fff,stroke:#6B5807
    style S fill:#117864,color:#fff,stroke:#083D33
    style L fill:#117864,color:#fff,stroke:#083D33
    style N fill:#943126,color:#fff,stroke:#571C16
    style V fill:#1E8449,color:#fff,stroke:#0E4A28
    style G fill:#B9770E,color:#fff,stroke:#6E4409
```

<p align="center"><em>This is the rule every lab follows, applied to the checks below.</em></p>

---

## 🎯 Verification Snapshot

| Check | Status |
|---|:---:|
| Backup restored and the data seen again (page and 3 table rows) | ✅ |
| Proxy and redirect tested from the VM and from my laptop | ✅ |
| SSH changes proven with a fresh login after restart | ✅ |
| New kernel confirmed running after the reboot | ✅ |
| DNS answers checked with `dig` and `nslookup` | ✅ |
| Test user confirmed deleted, archive kept | ✅ |
| Monitor shown going DOWN, then UP | ✅ |
| Backup copy stored off the VM | ⚠️ Kept on the same VM |

> [!NOTE]
> This is a summary only — full methodology, screenshots and limitations for each finding live in that lab's own README.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

</div>
