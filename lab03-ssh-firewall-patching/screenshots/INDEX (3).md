<div align="center">

# 📚 Linux SysAdmin Labs — Index

**Hands-on labs on an Ubuntu 24.04 Azure VM · one folder per lab · every lab has a README and screenshots**

![Linux](https://img.shields.io/badge/Linux-Ubuntu_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Cloud](https://img.shields.io/badge/Cloud-Azure_VM-0078D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Labs](https://img.shields.io/badge/Labs-4_finished-brightgreen?style=for-the-badge)

</div>

---

## 📑 Labs

A lab is listed here only when it is finished and has screenshots.

| # | Lab | What I did | README | Screenshots |
|:---:|---|---|:---:|:---:|
| 01 | **Backup & Restore** | Backed up a web page and a MySQL database with a script, checked SHA-256 checksums, scheduled it with cron, deleted the live data and restored it (2 min 47 s) | [README](lab01-backup-restore/README.md) | [screenshots](lab01-backup-restore/screenshots/) |
| 02 | **Nginx Reverse Proxy with TLS** | Moved Apache to port 8080, put Nginx in front on 80 and 443, made a self-signed certificate, redirected HTTP to HTTPS | [README](lab02-nginx-tls/README.md) | [screenshots](lab02-nginx-tls/screenshots/) |
| 03 | **SSH Hardening, Firewall & Patching** | Took a disk snapshot, turned on ufw, tightened SSH, installed updates and rebooted into a new kernel, turned on automatic updates and fail2ban | [README](lab03-ssh-firewall-patching/README.md) | [screenshots](lab03-ssh-firewall-patching/screenshots/) |
| 05 | **Users & Offboarding** | Created user accounts and offboarded a user on the VM | [README](lab5-users-offboarding/README.md) | [screenshots](lab5-users-offboarding/screenshots/) |

---

## 🧱 Shared Environment

| Item | Value |
|---|---|
| Cloud | Azure virtual machine `lab-server`, India South Central |
| Operating system | Ubuntu 24.04 LTS |
| VM size | Standard B4as v2 in Lab 01, resized to Standard B2ls_v2 for the later labs |
| Access | SSH with a key file, user `azureuser` |
| README layout | Objective · Environment · Steps · Results · Limitations |

> [!NOTE]
> Subscription details, account email and my own home IP address are blurred in the screenshots.

---

## 📁 Folder Structure

```text
Linux-SysAdmin-Labs/
|-- INDEX.md
|-- lab01-backup-restore/
|   |-- README.md
|   `-- screenshots/
|-- lab02-nginx-tls/
|   |-- README.md
|   `-- screenshots/
|-- lab03-ssh-firewall-patching/
|   |-- README.md
|   |-- screenshots/
|   `-- extra-not-for-readme/
`-- lab5-users-offboarding/
    |-- README.md
    `-- screenshots/
```
