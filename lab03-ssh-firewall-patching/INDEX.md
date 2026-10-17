<a id="top"></a>
<div align="center">

# 🛡️ Lab 03 — Index
### SSH Hardening, Firewall & Patching
**Lab 03 — Linux SysAdmin Labs**

![ufw](https://img.shields.io/badge/Firewall-ufw-E95420?style=for-the-badge)
![OpenSSH](https://img.shields.io/badge/SSH-OpenSSH-2C3E70?style=for-the-badge)
![apt](https://img.shields.io/badge/Updates-apt-1A5276?style=for-the-badge)
![fail2ban](https://img.shields.io/badge/Protection-fail2ban-943126?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2EA043?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this lab.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Steps | 🖼️ Screenshots | 🛠️ Tools Used | 📦 Updates Left |
|:---:|:---:|:---:|:---:|
| **12** | **30** | **5** | **0** |

</div>

<p align="center">🧩 <b>Case:</b> Snapshot <code>lab-server-before-lab3</code> · ufw ports 22, 80, 443 · kernel <code>6.17.0-1022-azure</code> → <code>7.0.0-1017-azure</code></p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the VM in Azure | 🟣 Module 1 | `lab-server` running, B2ls_v2 | [Exhibit 1](#ex1) |
| 2 | Take a snapshot before changing anything | 🟣 Module 1 | Snapshot `lab-server-before-lab3` created | [Exhibits 2–5](#ex2) |
| 3 | Record the SSH settings before | 🟣 Module 1 | Root login `without-password`, password login `no` | [Exhibit 6](#ex6) |
| 4 | Open a second SSH window | 🟣 Module 1 | Safety net open, 84 updates pending | [Exhibit 7](#ex7) |
| 5 | Turn on the firewall (ufw) | 🔵 Module 2 | Active, ports 22, 80, 443 allowed | [Exhibits 8–12](#ex8) |
| 6 | Check that the website still works | 🔵 Module 2 | `Backup Lab` opens over HTTPS | [Exhibits 13–14](#ex13) |
| 7 | Harden SSH | 🔵 Module 2 | Root login `no`, 3 tries, 30 s, new login works | [Exhibits 15–19](#ex15) |
| 8 | Install the updates | 🟠 Module 3 | 73 upgradable after refresh, none left after upgrade | [Exhibits 20–23](#ex20) |
| 9 | Reboot and check the kernel | 🟠 Module 3 | Kernel `7.0.0-1017-azure`, 0 updates | [Exhibit 24](#ex24) |
| 10 | Check the firewall and services after the reboot | 🟠 Module 3 | ufw, Nginx, Apache, MySQL active, site opens | [Exhibits 25–26](#ex25) |
| 11 | Turn on automatic updates | 🟢 Module 4 | `unattended-upgrades` enabled and active | [Exhibit 27](#ex27) |
| 12 | Install and check fail2ban | 🟢 Module 4 | `sshd` jail active, 1 IP banned | [Exhibits 28–30](#ex28) |

---

## 🟣 Module 1 — Safety Net

Exhibits 1 to 7.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="screenshots/01-vm-running.png"><img src="screenshots/01-vm-running.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — VM running</b>
<br><sub><code>lab-server</code>, Standard B2ls_v2, status Running</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="screenshots/02-vm-disks.png"><img src="screenshots/02-vm-disks.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — OS disk</b>
<br><sub>30 GiB Premium SSD, no data disks</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="screenshots/03-os-disk-page.png"><img src="screenshots/03-os-disk-page.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Create snapshot</b>
<br><sub>OS disk page with the snapshot option</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="screenshots/04-snapshot-form.png"><img src="screenshots/04-snapshot-form.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Snapshot form</b>
<br><sub><code>lab-server-before-lab3</code>, Incremental, validation passed</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="screenshots/05-snapshot-created.png"><img src="screenshots/05-snapshot-created.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Snapshot created</b>
<br><sub>Deployment complete</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="screenshots/06-ssh-settings-before.png"><img src="screenshots/06-ssh-settings-before.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — SSH settings before</b>
<br><sub>Root login <code>without-password</code>, password login <code>no</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="screenshots/07-second-ssh-window.png"><img src="screenshots/07-second-ssh-window.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Second SSH window</b>
<br><sub>Safety net open, banner shows 84 updates (60 security)</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🔵 Module 2 — Firewall and SSH

Exhibits 8 to 19.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="screenshots/08-ufw-allow-ssh.png"><img src="screenshots/08-ufw-allow-ssh.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Allow SSH in ufw</b>
<br><sub>Rule added before ufw is turned on</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="screenshots/09-ufw-allow-80.png"><img src="screenshots/09-ufw-allow-80.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Allow port 80</b>
<br><sub>HTTP rule added</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="screenshots/10-ufw-allow-443.png"><img src="screenshots/10-ufw-allow-443.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Allow port 443</b>
<br><sub>HTTPS rule added</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="screenshots/11-ufw-enabled.png"><img src="screenshots/11-ufw-enabled.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — ufw enabled</b>
<br><sub>Firewall active and enabled on system startup</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="screenshots/12-ufw-status.png"><img src="screenshots/12-ufw-status.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — ufw status</b>
<br><sub>22, 80, 443 allowed for IPv4 and IPv6</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="screenshots/13-browser-warning-after-ufw.png"><img src="screenshots/13-browser-warning-after-ufw.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Browser warning after ufw</b>
<br><sub>Self-signed certificate warning, Nginx reached on 443</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="screenshots/14-website-after-ufw.png"><img src="screenshots/14-website-after-ufw.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Website after ufw</b>
<br><sub><code>Backup Lab</code> page opens over HTTPS</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="screenshots/15-ssh-hardening-file.png"><img src="screenshots/15-ssh-hardening-file.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — SSH hardening file</b>
<br><sub><code>PermitRootLogin no</code>, <code>MaxAuthTries 3</code>, <code>LoginGraceTime 30</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="screenshots/16-sshd-configtest.png"><img src="screenshots/16-sshd-configtest.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — SSH config test</b>
<br><sub><code>CONFIG OK</code> before the restart</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="screenshots/17-ssh-restarted.png"><img src="screenshots/17-ssh-restarted.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — SSH restarted</b>
<br><sub><code>SSH RESTARTED</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="screenshots/18-new-login-after-restart.png"><img src="screenshots/18-new-login-after-restart.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — New login</b>
<br><sub>Fresh login from the laptop</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="screenshots/19-ssh-settings-after.png"><img src="screenshots/19-ssh-settings-after.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — SSH settings after</b>
<br><sub>Root login <code>no</code>, 3 tries, 30 s grace time</sub>
</td>
</tr>
</table>

---

## 🟠 Module 3 — Patching

Exhibits 20 to 26.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="screenshots/20-updates-pending-list.png"><img src="screenshots/20-updates-pending-list.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Pending updates</b>
<br><sub>First 20 packages from the upgradable list</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="screenshots/21-apt-update.png"><img src="screenshots/21-apt-update.png" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — Package lists refreshed</b>
<br><sub>73 packages can be upgraded</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="screenshots/22-apt-upgrade-kernel-pending.png"><img src="screenshots/22-apt-upgrade-kernel-pending.png" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Upgrade output</b>
<br><sub>Pending kernel upgrade, services restarted</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="screenshots/23-apt-history-log.png"><img src="screenshots/23-apt-history-log.png" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — apt history</b>
<br><sub>Upgrade list and run time, no package left</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="screenshots/24-reboot-and-relogin.png"><img src="screenshots/24-reboot-and-relogin.png" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — Reboot and re-login</b>
<br><sub>Kernel <code>7.0.0-1017-azure</code>, 0 updates</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex25"></a>
<a href="screenshots/25-ufw-and-services-after-reboot.png"><img src="screenshots/25-ufw-and-services-after-reboot.png" width="380" alt="Exhibit 25"></a>
<br><b>Exhibit 25 — Firewall and services after reboot</b>
<br><sub>ufw active, Nginx, Apache and MySQL active</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex26"></a>
<a href="screenshots/26-website-after-reboot.png"><img src="screenshots/26-website-after-reboot.png" width="380" alt="Exhibit 26"></a>
<br><b>Exhibit 26 — Website after reboot</b>
<br><sub><code>Backup Lab</code> still opens</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟢 Module 4 — Automation and Protection

Exhibits 27 to 30.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex27"></a>
<a href="screenshots/27-unattended-upgrades-status.png"><img src="screenshots/27-unattended-upgrades-status.png" width="380" alt="Exhibit 27"></a>
<br><b>Exhibit 27 — Automatic updates</b>
<br><sub>Service enabled and active, both config lines set to 1</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex28"></a>
<a href="screenshots/28-fail2ban-installed.png"><img src="screenshots/28-fail2ban-installed.png" width="380" alt="Exhibit 28"></a>
<br><b>Exhibit 28 — fail2ban installed</b>
<br><sub>Install output ends cleanly</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex29"></a>
<a href="screenshots/29-fail2ban-enabled-and-status.png"><img src="screenshots/29-fail2ban-enabled-and-status.png" width="380" alt="Exhibit 29"></a>
<br><b>Exhibit 29 — fail2ban enabled</b>
<br><sub>Service active, 1 jail: <code>sshd</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex30"></a>
<a href="screenshots/30-fail2ban-sshd-jail.png"><img src="screenshots/30-fail2ban-sshd-jail.png" width="380" alt="Exhibit 30"></a>
<br><b>Exhibit 30 — sshd jail status</b>
<br><sub>6 failed logins, 1 IP banned</sub>
</td>
</tr>
</table>

---

## 📝 Verdict

**Complete** — firewall on, SSH tightened, server patched and rebooted into the new kernel, automatic updates and fail2ban running, and the website still works.

| Element | Finding |
|---|---|
| Firewall | `ufw` active, default deny incoming, 22, 80, 443 allowed |
| SSH | `PermitRootLogin no`, `MaxAuthTries 3`, `LoginGraceTime 30`, password login already `no` |
| Updates | 84 in the login banner → 0 after upgrade and reboot |
| Kernel | `6.17.0-1022-azure` → `7.0.0-1017-azure` |
| Automatic updates | `unattended-upgrades` enabled and active |
| fail2ban | Active, 1 jail (`sshd`), 1 IP banned |

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Snapshot created | Azure deployment complete | Module 1 | ✅ Confirmed |
| Only ports 22, 80, 443 allowed | `ufw status verbose` | Module 2 | ✅ Confirmed |
| Website opens after ufw | Browser, HTTPS page | Module 2 | ✅ Confirmed |
| SSH settings applied | `sshd -T` before and after | Module 2 | ✅ Confirmed |
| SSH config valid | `sshd -t` returned `CONFIG OK` | Module 2 | ✅ Confirmed |
| SSH login works after the changes | Fresh login from the laptop, also after the reboot | Modules 2 + 3 | ✅ Confirmed |
| New kernel running | `uname -r` after reboot, 0 updates in the banner | Module 3 | ✅ Confirmed |
| Services and firewall after reboot | `systemctl is-active`, `ufw status`, browser | Module 3 | ✅ Confirmed |
| Automatic updates on | `systemctl is-enabled` and `is-active` | Module 4 | ✅ Confirmed |
| fail2ban jail running | `fail2ban-client status sshd` | Module 4 | ✅ Confirmed |

> [!NOTE]
> The order matters: ufw allows SSH before it is turned on, and the SSH config is tested with `sshd -t` before the restart. A second SSH window and the snapshot were ready as a way back, and neither was needed.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Azure portal](https://portal.azure.com/)** · 🔐 **[OpenSSH](https://www.openssh.com/)** · 🚫 **[fail2ban](https://github.com/fail2ban/fail2ban)** · 🐧 **[Ubuntu](https://ubuntu.com/)**

</div>
