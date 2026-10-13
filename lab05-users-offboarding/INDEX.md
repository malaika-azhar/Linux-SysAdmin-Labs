<a id="top"></a>
<div align="center">

# 👤 Lab 05 — Index
### Linux User Management & Offboarding
**Lab 05 — Linux SysAdmin Labs**

![Linux](https://img.shields.io/badge/Linux-Ubuntu_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Cloud](https://img.shields.io/badge/Cloud-Azure_VM-0078D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Access](https://img.shields.io/badge/Access-SSH_Key-76448A?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2EA043?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this lab.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🖼️ Screenshots | 🛠️ Tools Used | 🔒 Offboarding Actions |
|:---:|:---:|:---:|:---:|
| **5** | **18** | **10** | **4** |

</div>

<p align="center">🧩 <b>Case:</b> Test user <code>intern1</code> (UID 1001, a made-up account, not a real person) · Created, given sudo, then offboarded on <code>lab-server</code>, 13 Oct 2026</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the VM in Azure and how it was created | 🟣 Module 1 | `lab-server` is `Running`, Ubuntu 24.04, B4as v2; created in `sysadmin-labs` | [Exhibit 1](#ex1), [Exhibit 2](#ex2), [Exhibit 3](#ex3) |
| 2 | Connect to the server | 🟣 Module 1 | SSH works, logged in as `azureuser` | [Exhibit 4](#ex4) |
| 3 | Record the starting state | 🟣 Module 1 | Only `azureuser` exists and is in `sudo` | [Exhibit 5](#ex5) |
| 4 | Create the user `intern1` | 🟣 Module 1 | UID/GID 1001 and `/home/intern1` created | [Exhibit 6](#ex6), [Exhibit 7](#ex7) |
| 5 | Give `intern1` sudo access | 🟣 Module 1 | `sudo` group is `azureuser,intern1` | [Exhibit 8](#ex8) |
| 6 | Create some user data | 🟣 Module 1 | `notes.txt` created, home folder is private | [Exhibit 9](#ex9), [Exhibit 10](#ex10) |
| 7 | Lock and expire the account | 🔵 Module 2 | Status `L`, account expires Jan 02, 1970 | [Exhibit 11](#ex11) |
| 8 | Remove sudo access | 🔵 Module 2 | `sudo` group back to `azureuser` only | [Exhibit 12](#ex12) |
| 9 | Archive the home folder and check it | 🟠 Module 3 | 2,351-byte archive holds `notes.txt` | [Exhibit 13](#ex13), [Exhibit 14](#ex14) |
| 10 | Delete the user | 🟢 Module 4 | `no such user`, archive still there | [Exhibit 15](#ex15) |
| 11 | Stop (deallocate) the VM | 🟡 Module 5 | Status `Stopped (deallocated)` | [Exhibit 16](#ex16) |
| 12 | Resize the VM | 🟡 Module 5 | Standard B4as v2 → Standard B2ls_v2, `Running` | [Exhibit 17](#ex17) |
| 13 | Reconnect to the resized VM | 🟡 Module 5 | SSH login works at 13:37:37 UTC | [Exhibit 18](#ex18) |

---

## 🟣 Module 1 — Set Up

Exhibits 1 to 10.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="screenshots/00-azure-vm-overview.png"><img src="screenshots/00-azure-vm-overview.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Azure VM overview</b>
<br><sub>VM <code>lab-server</code> is <code>Running</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="screenshots/00a-resource-group.png"><img src="screenshots/00a-resource-group.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — Resource group</b>
<br><sub><code>lab-server</code>, its public IP and its NSG in <code>sysadmin-labs</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="screenshots/00b-vm-deployment.png"><img src="screenshots/00b-vm-deployment.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — VM deployment</b>
<br><sub><code>CreateVirtualMachine-lab-server</code> started 13/10/2026 16:47:48</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="screenshots/01-ssh-connected.png"><img src="screenshots/01-ssh-connected.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — SSH connected</b>
<br><sub>Logged in as <code>azureuser</code> on <code>lab-server</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="screenshots/02-baseline-users.png"><img src="screenshots/02-baseline-users.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Starting state</b>
<br><sub>Only <code>azureuser</code> in <code>/home</code> and in <code>sudo</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="screenshots/03-create-user.png"><img src="screenshots/03-create-user.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — User created</b>
<br><sub><code>adduser</code> builds <code>intern1</code> and its home folder</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="screenshots/04-verify-user.png"><img src="screenshots/04-verify-user.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — User checked</b>
<br><sub><code>id intern1</code> returns UID/GID 1001</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="screenshots/05-grant-sudo.png"><img src="screenshots/05-grant-sudo.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Sudo granted</b>
<br><sub><code>sudo</code> group shows <code>azureuser,intern1</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="screenshots/06-home-permission-denied.png"><img src="screenshots/06-home-permission-denied.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Home folder is private</b>
<br><sub><code>Permission denied</code> without <code>sudo</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="screenshots/07-intern-file.png"><img src="screenshots/07-intern-file.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — User data created</b>
<br><sub><code>notes.txt</code> owned by <code>intern1</code>, 22 bytes</sub>
</td>
</tr>
</table>

---

## 🔵 Module 2 — Secure

Exhibits 11 to 12.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="screenshots/08-lock-account.png"><img src="screenshots/08-lock-account.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — Account locked and expired</b>
<br><sub><code>passwd -S</code> shows <code>L</code>, <code>chage -l</code> shows Jan 02, 1970</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="screenshots/09-remove-sudo.png"><img src="screenshots/09-remove-sudo.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Sudo removed</b>
<br><sub><code>sudo</code> group is back to <code>azureuser</code></sub>
</td>
</tr>
</table>

---

## 🟠 Module 3 — Preserve

Exhibits 13 to 14.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="screenshots/10-archive-home.png"><img src="screenshots/10-archive-home.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Home folder archived</b>
<br><sub><code>intern1_2026-10-13.tar.gz</code>, 2,351 bytes</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="screenshots/11-verify-archive.png"><img src="screenshots/11-verify-archive.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Archive checked</b>
<br><sub><code>tar -tzf</code> lists <code>notes.txt</code> and the dotfiles</sub>
</td>
</tr>
</table>

---

## 🟢 Module 4 — Remove

Exhibit 15.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="screenshots/12-delete-user.png"><img src="screenshots/12-delete-user.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — User deleted</b>
<br><sub><code>no such user</code>, archive still in <code>/var/backups/offboarded</code></sub>
</td>
</tr>
</table>

---

## 🟡 Module 5 — Wrap Up

Exhibits 16 to 18.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="screenshots/13-vm-deallocated.png"><img src="screenshots/13-vm-deallocated.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — VM stopped</b>
<br><sub>Status <code>Stopped (deallocated)</code>, size B4as v2</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="screenshots/14-vm-resized.png"><img src="screenshots/14-vm-resized.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — VM resized</b>
<br><sub>Size Standard B2ls_v2, status <code>Running</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="screenshots/15-ssh-reconnected.png"><img src="screenshots/15-ssh-reconnected.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — SSH reconnected</b>
<br><sub>Login to the resized VM at 13:37:37 UTC</sub>
</td>
</tr>
</table>

---

## 📝 Verdict

**Complete** — the test user was created, given sudo, then offboarded in a safe order: locked, sudo removed, home folder archived and checked, account deleted. The VM was then stopped (deallocated), resized to B2ls_v2 and reconnected.

| Element | Finding |
|---|---|
| Test user | `intern1`, UID/GID 1001 |
| Admin rights | Granted, then removed (`sudo` group: `azureuser,intern1` → `azureuser`) |
| Account | Locked (`L`), expires Jan 02, 1970 |
| Data | Archived in `intern1_2026-10-13.tar.gz`, 2,351 bytes, holds `notes.txt` |
| Final state | `id intern1` returns `no such user`, archive kept |
| VM | `Stopped (deallocated)`, then resized to Standard B2ls_v2 and reconnected over SSH |

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Starting state recorded | `ls /home`, `getent group sudo` | Module 1 | ✅ Confirmed |
| User created | `id intern1` | Module 1 | ✅ Confirmed |
| Sudo granted | `getent group sudo` shows `azureuser,intern1` | Module 1 | ✅ Confirmed |
| Account locked and expired | `passwd -S` (`L`), `chage -l` (Jan 02, 1970) | Module 2 | ✅ Confirmed |
| Sudo removed | `getent group sudo` shows `azureuser` | Module 2 | ✅ Confirmed |
| Archive holds the user's data | `tar -tzf` lists `notes.txt` | Module 3 | ✅ Confirmed |
| User deleted, archive kept | `id intern1`, `ls /home`, `ls -l /var/backups/offboarded` | Module 4 | ✅ Confirmed |
| VM deallocated | Azure Portal status `Stopped (deallocated)` | Module 5 | ✅ Confirmed |
| VM resized | Azure Portal size Standard B2ls_v2 | Module 5 | ✅ Confirmed |
| VM reachable after resize | SSH login banner at 13:37:37 UTC | Module 5 | ✅ Confirmed |

> [!NOTE]
> Public IP, subscription ID, account email, directory name and my own IP address are hidden in the Azure and SSH exhibits.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

☁️ **[Azure Portal](https://portal.azure.com/)** · 🐧 **[Ubuntu Server](https://ubuntu.com/server)**

</div>
