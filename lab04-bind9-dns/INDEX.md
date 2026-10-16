<a id="top"></a>
<div align="center">

# 🌐 Lab 04 — Index
### BIND9 DNS Server — Zone `lab.local`
**Lab 04 of the Linux SysAdmin Labs**

![BIND](https://img.shields.io/badge/DNS-BIND_9.18-1A5276?style=for-the-badge)
![Ubuntu](https://img.shields.io/badge/Linux-Ubuntu_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Azure](https://img.shields.io/badge/Cloud-Azure_VM-0078D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Testing](https://img.shields.io/badge/Testing-dig_%2B_nslookup-B9770E?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2EA043?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this lab.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Steps | 🖼️ Screenshots | 🛠️ Tools Used | 📜 Zone Records |
|:---:|:---:|:---:|:---:|
| **12** | **22** | **10** | **7** |

</div>

<p align="center">🧩 <b>Case:</b> Zone <code>lab.local</code> · VM private IP <code>172.16.0.4</code> · BIND 9.18.39 on Ubuntu 24.04</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the VM in Azure | 🟣 Module 1 | `lab-server` is `Running` | [Exhibit 00](#ex00) |
| 2 | Check what uses port 53 | 🟣 Module 2 | Only `systemd-resolved` listens, no clash | [Exhibit 01](#ex01) |
| 3 | Install BIND9 | 🟣 Module 3 | `bind9` and `dnsutils` installed | [Exhibit 02](#ex02) |
| 4 | Declare the zone | 🔵 Module 4 | One `zone "lab.local"` block added | [Exhibit 03](#ex03) |
| 5 | Create the zone file | 🔵 Module 5 | SOA, NS, MX, TXT and 3 A records written | [Exhibit 04](#ex04) |
| 6 | Set the listen address | 🔵 Module 6 | `listen-on` set to `127.0.0.1` and `172.16.0.4` | [Exhibit 05](#ex05) |
| 7 | Check the configuration | 🟠 Module 7 | `loaded serial 2`, `OK` | [Exhibit 06](#ex06) |
| 8 | Restart BIND and check the service | 🟠 Module 8 | `active (running)`, `enabled` | [Exhibit 07](#ex07) |
| 9 | Check port 53 again | 🟠 Module 9 | `named` listens on both addresses | [Exhibit 08](#ex08) |
| 10 | Query the records | 🟢 Module 10 | A, MX, TXT, NS host and NXDOMAIN answered correctly | [Exhibits 09–13](#module-10) |
| 11 | Check the firewall | 🔴 Module 11 | `ufw` active, no rule for port 53 | [Exhibit 14](#ex14) |
| 12 | Extra checks | 🔴 Module 12 | Version, private IP query, SOA, `nslookup`, boot setting, logs | [Exhibits 15–21](#module-12) |

---

## 🟣 Module 1–3 — Prepare

Exhibits 00 to 02.

<table>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex00"></a>
<a href="screenshots/lab04-00-vm-overview.png"><img src="screenshots/lab04-00-vm-overview.png" width="250" alt="Exhibit 00"></a>
<br><b>Exhibit 00 — Azure VM</b>
<br><sub><code>lab-server</code> running, subscription ID, email and public IP blurred</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex01"></a>
<a href="screenshots/lab04-01-port53.png"><img src="screenshots/lab04-01-port53.png" width="250" alt="Exhibit 01"></a>
<br><b>Exhibit 01 — Port 53 before BIND</b>
<br><sub>Only <code>systemd-resolved</code> is listening</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex02"></a>
<a href="screenshots/lab04-02-install.png"><img src="screenshots/lab04-02-install.png" width="250" alt="Exhibit 02"></a>
<br><b>Exhibit 02 — BIND9 installed</b>
<br><sub>Install finished, prompt returned</sub>
</td>
</tr>
</table>

---

## 🔵 Module 4–6 — Configure

Exhibits 03 to 05.

<table>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex03"></a>
<a href="screenshots/lab04-03-zone-declared.png"><img src="screenshots/lab04-03-zone-declared.png" width="250" alt="Exhibit 03"></a>
<br><b>Exhibit 03 — Zone declared</b>
<br><sub><code>named.conf.local</code> with the <code>lab.local</code> block</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex04"></a>
<a href="screenshots/lab04-04-zone-file.png"><img src="screenshots/lab04-04-zone-file.png" width="250" alt="Exhibit 04"></a>
<br><b>Exhibit 04 — Zone file</b>
<br><sub><code>db.lab.local</code> with 7 records</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex05"></a>
<a href="screenshots/lab04-05-listen-on.png"><img src="screenshots/lab04-05-listen-on.png" width="250" alt="Exhibit 05"></a>
<br><b>Exhibit 05 — Listen address</b>
<br><sub><code>listen-on</code> line added once</sub>
</td>
</tr>
</table>

---

## 🟠 Module 7–9 — Validate

Exhibits 06 to 08.

<table>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex06"></a>
<a href="screenshots/lab04-06-checks.png"><img src="screenshots/lab04-06-checks.png" width="250" alt="Exhibit 06"></a>
<br><b>Exhibit 06 — Config checks</b>
<br><sub><code>named-checkzone</code> shows serial 2 and <code>OK</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex07"></a>
<a href="screenshots/lab04-07-status.png"><img src="screenshots/lab04-07-status.png" width="250" alt="Exhibit 07"></a>
<br><b>Exhibit 07 — Service status</b>
<br><sub><code>active (running)</code>, PID 2547</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex08"></a>
<a href="screenshots/lab04-08-port53-after.png"><img src="screenshots/lab04-08-port53-after.png" width="250" alt="Exhibit 08"></a>
<br><b>Exhibit 08 — Port 53 after BIND</b>
<br><sub><code>named</code> on <code>172.16.0.4</code> and <code>127.0.0.1</code>, IPv6 address blurred</sub>
</td>
</tr>
</table>

---

<a id="module-10"></a>
## 🟢 Module 10 — Query the Records

Exhibits 09 to 13.

<table>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex09"></a>
<a href="screenshots/lab04-09-dig-a.png"><img src="screenshots/lab04-09-dig-a.png" width="250" alt="Exhibit 09"></a>
<br><b>Exhibit 09 — A record</b>
<br><sub><code>www.lab.local</code> → <code>172.16.0.4</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex10"></a>
<a href="screenshots/lab04-10-dig-mx.png"><img src="screenshots/lab04-10-dig-mx.png" width="250" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — MX record</b>
<br><sub><code>10 mail.lab.local.</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex11"></a>
<a href="screenshots/lab04-11-dig-txt.png"><img src="screenshots/lab04-11-dig-txt.png" width="250" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — TXT record</b>
<br><sub>SPF policy <code>v=spf1 mx -all</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex12"></a>
<a href="screenshots/lab04-12-dig-ns1.png"><img src="screenshots/lab04-12-dig-ns1.png" width="250" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Name server host</b>
<br><sub><code>ns1.lab.local</code> → <code>172.16.0.4</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex13"></a>
<a href="screenshots/lab04-13-nxdomain.png"><img src="screenshots/lab04-13-nxdomain.png" width="250" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Missing name</b>
<br><sub><code>NXDOMAIN</code> for <code>nothere.lab.local</code></sub>
</td>
<td></td>
</tr>
</table>

---

## 🔴 Module 11 — Firewall Check

Exhibit 14.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="screenshots/lab04-14-ufw.png"><img src="screenshots/lab04-14-ufw.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — ufw status</b>
<br><sub>Active, ports 22, 80 and 443 allowed, no rule for 53</sub>
</td>
</tr>
</table>

---

<a id="module-12"></a>
## 🔴 Module 12 — Extra Checks

Exhibits 15 to 21.

<table>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex15"></a>
<a href="screenshots/lab04-15-named-version.png"><img src="screenshots/lab04-15-named-version.png" width="250" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — BIND version</b>
<br><sub>BIND 9.18.39, Ubuntu package</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex16"></a>
<a href="screenshots/lab04-16-dig-private-ip.png"><img src="screenshots/lab04-16-dig-private-ip.png" width="250" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Query through private IP</b>
<br><sub>Answered by <code>172.16.0.4#53</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex17"></a>
<a href="screenshots/lab04-17-dig-short.png"><img src="screenshots/lab04-17-dig-short.png" width="250" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Short answer</b>
<br><sub><code>+short</code> prints only <code>172.16.0.4</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex18"></a>
<a href="screenshots/lab04-18-dig-soa.png"><img src="screenshots/lab04-18-dig-soa.png" width="250" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — SOA record</b>
<br><sub>Serial number <code>2</code></sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex19"></a>
<a href="screenshots/lab04-19-nslookup.png"><img src="screenshots/lab04-19-nslookup.png" width="250" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — nslookup</b>
<br><sub>Second tool, same answer</sub>
</td>
<td align="center" valign="top" width="33%">
<a id="ex20"></a>
<a href="screenshots/lab04-20-enabled.png"><img src="screenshots/lab04-20-enabled.png" width="250" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Boot setting</b>
<br><sub><code>bind9</code> is an alias of <code>named.service</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="33%">
<a id="ex21"></a>
<a href="screenshots/lab04-21-logs.png"><img src="screenshots/lab04-21-logs.png" width="250" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — Service logs</b>
<br><sub>Last 20 <code>named</code> log lines</sub>
</td>
<td></td>
<td></td>
</tr>
</table>

---

## 📝 Result

**Complete** — BIND9 is installed, the `lab.local` zone passes the config checks, and every record answers correctly when queried from the VM itself.

| Element | Finding |
|---|---|
| Zone | `lab.local`, serial `2` |
| Records | SOA, NS, MX, TXT and 3 × A (`ns1`, `www`, `mail`) |
| Service | `named` `active (running)`, `enabled` |
| Listening on | `127.0.0.1` and `172.16.0.4`, port 53 (UDP and TCP) |
| Missing name | `NXDOMAIN` (correct reply) |
| Firewall | `ufw` active, no rule for port 53 |

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Port 53 free for BIND | `ss -tulpn` before install | Module 2 | ✅ Confirmed |
| Config and zone file valid | `named-checkconf`, `named-checkzone` | Module 7 | ✅ Confirmed |
| Service running | `systemctl status bind9` | Module 8 | ✅ Confirmed |
| BIND on its own addresses | `ss -tulpn` after install | Module 9 | ✅ Confirmed |
| A, MX, TXT, NS host and SOA answer | `dig` on `127.0.0.1` | Modules 10 + 12 | ✅ Confirmed |
| Missing name returns `NXDOMAIN` | `dig` for `nothere.lab.local` | Module 10 | ✅ Confirmed |
| Private IP also answers | `dig @172.16.0.4` | Module 12 | ✅ Confirmed |
| Cross-tool corroboration | `dig` vs `nslookup` | Modules 10 + 12 | ✅ Confirmed (agree) |

> [!NOTE]
> `dig` prints a warning that `.local` is reserved for multicast DNS. The lab uses `lab.local` only as a private test name.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

🌐 **[ISC BIND](https://www.isc.org/bind/)** · 📚 **[BIND 9 documentation](https://bind9.readthedocs.io/)**

</div>
