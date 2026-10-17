<a id="top"></a>
<div align="center">

# 🔒 Lab 02 — Index
### Nginx Reverse Proxy & TLS — Apache Behind Nginx
**Lab 02 — Linux SysAdmin Labs**

![Nginx](https://img.shields.io/badge/Proxy-Nginx_1.24-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Apache](https://img.shields.io/badge/Backend-Apache_2.4-D22128?style=for-the-badge&logo=apache&logoColor=white)
![OpenSSL](https://img.shields.io/badge/TLS-OpenSSL-721412?style=for-the-badge&logo=openssl&logoColor=white)
![Azure](https://img.shields.io/badge/Cloud-Azure_VM_%2B_NSG-0078D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2EA043?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this lab.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Phases | 🖼️ Screenshots | 🛠️ Tools Used | 🔐 Certificate Validity |
|:---:|:---:|:---:|:---:|
| **6** | **26** | **7** | **365 days** |

</div>

<p align="center">🧩 <b>Setup:</b> Apache moved from port 80 to <code>8080</code> · Nginx 1.24.0 on <code>80</code> and <code>443</code> · self-signed certificate, 13 Oct 2026 to 13 Oct 2027</p>

---

## 📑 Step Index

| # | Step | Module | Result | Evidence |
|:---:|---|:---:|---|:---:|
| 1 | Check the VM in Azure | 🔵 Module 1 | Stopped on B4as v2, then Running on B2ls_v2 | [Exhibit 00](#ex00) |
| 2 | Connect to the server | 🔵 Module 1 | SSH login as `azureuser` works | [Exhibit 02](#ex02) |
| 3 | See what listens on port 80 | 🔵 Module 1 | Apache on `:80`, ports 443 and 8080 empty | [Exhibit 03](#ex03) |
| 4 | Find the Apache port settings | 🟣 Module 2 | `Listen 80` and `<VirtualHost *:80>` located | [Exhibit 04a](#ex04a) |
| 5 | Move Apache to 8080 | 🟣 Module 2 | `ports.conf` and `000-default.conf` both changed | [Exhibit 05](#ex05) |
| 6 | Check the config and restart Apache | 🟣 Module 2 | `Syntax OK`, Apache only on `:8080` | [Exhibit 07](#ex07) |
| 7 | Open port 443 in the Azure NSG | 🟠 Module 3 | `AllowHTTPS` rule added, 6 inbound rules | [Exhibit 09](#ex09) |
| 8 | Install Nginx | 🟡 Module 4 | Nginx 1.24.0 `active (running)` | [Exhibit 12](#ex12) |
| 9 | Set up the reverse proxy | 🟡 Module 4 | Proxy to `127.0.0.1:8080`, `200 OK` from Nginx | [Exhibit 13](#ex13) |
| 10 | Create the self-signed certificate | 🔴 Module 5 | RSA 2048 certificate, key file mode 600 | [Exhibit 17](#ex17) |
| 11 | Add TLS and the redirect | 🔴 Module 5 | Port 80 redirects, port 443 serves TLS, `nginx -t` OK | [Exhibit 18](#ex18) |
| 12 | Test the redirect and HTTPS | 🔴 Module 5 | `301` for HTTP, `200 OK` for HTTPS | [Exhibit 20](#ex20) |
| 13 | Check the certificate dates | 🔴 Module 5 | Valid 13 Oct 2026 to 13 Oct 2027 | [Exhibit 21](#ex21) |
| 14 | Test from the laptop browser | 🟢 Module 6 | Certificate warning, then the page opens over HTTPS | [Exhibit 22](#ex22) |
| 15 | Test the redirect from outside the VM | 🟢 Module 6 | `curl.exe` on the laptop gets `301` to `https://` | [Exhibit 24](#ex24) |

---

## 🔵 Module 1 — Setup and Baseline

Exhibits 00 to 03 (Steps 1 to 3).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex00"></a>
<a href="screenshots/00-vm-stopped-before-resize.png"><img src="screenshots/00-vm-stopped-before-resize.png" width="380" alt="Exhibit 00"></a>
<br><b>Exhibit 00 — VM stopped</b>
<br><sub><code>Stopped (deallocated)</code> on Standard B4as v2, before the resize</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex01"></a>
<a href="screenshots/01-vm-started-b2ls-v2.png"><img src="screenshots/01-vm-started-b2ls-v2.png" width="380" alt="Exhibit 01"></a>
<br><b>Exhibit 01 — VM running</b>
<br><sub><code>Running</code> on Standard B2ls_v2 (2 vCPU, 4 GiB)</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex02"></a>
<a href="screenshots/02-ssh-login.png"><img src="screenshots/02-ssh-login.png" width="380" alt="Exhibit 02"></a>
<br><b>Exhibit 02 — SSH login</b>
<br><sub><code>azureuser</code> on <code>lab-server</code>, Ubuntu 24.04.4 LTS</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex03"></a>
<a href="screenshots/03-ports-before.png"><img src="screenshots/03-ports-before.png" width="380" alt="Exhibit 03"></a>
<br><b>Exhibit 03 — Ports before</b>
<br><sub><code>ss</code> shows Apache only on <code>*:80</code>, 443 and 8080 empty</sub>
</td>
</tr>
</table>

---

## 🟣 Module 2 — Move Apache to Port 8080

Exhibits 04a to 08 (Steps 4 to 6).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex04a"></a>
<a href="screenshots/04a-grep-ports-conf-only.png"><img src="screenshots/04a-grep-ports-conf-only.png" width="380" alt="Exhibit 04a"></a>
<br><b>Exhibit 04a — Symlinks skipped</b>
<br><sub><code>grep -rn</code> found only <code>Listen 80</code> on line 5 of <code>ports.conf</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex04"></a>
<a href="screenshots/04-apache-config-before.png"><img src="screenshots/04-apache-config-before.png" width="380" alt="Exhibit 04"></a>
<br><b>Exhibit 04 — Site config found</b>
<br><sub><code>grep -Rn</code> found <code>&lt;VirtualHost *:80&gt;</code> on line 1 of <code>000-default.conf</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex05"></a>
<a href="screenshots/05-ports-conf-changed.png"><img src="screenshots/05-ports-conf-changed.png" width="380" alt="Exhibit 05"></a>
<br><b>Exhibit 05 — ports.conf changed</b>
<br><sub>Line 5 now reads <code>Listen 8080</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex06"></a>
<a href="screenshots/06-virtualhost-changed.png"><img src="screenshots/06-virtualhost-changed.png" width="380" alt="Exhibit 06"></a>
<br><b>Exhibit 06 — VirtualHost changed</b>
<br><sub>Line 1 now reads <code>&lt;VirtualHost *:8080&gt;</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex07"></a>
<a href="screenshots/07-apache-configtest.png"><img src="screenshots/07-apache-configtest.png" width="380" alt="Exhibit 07"></a>
<br><b>Exhibit 07 — Apache config test</b>
<br><sub><code>apache2ctl configtest</code> returned <code>Syntax OK</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex08"></a>
<a href="screenshots/08-apache-on-8080.png"><img src="screenshots/08-apache-on-8080.png" width="380" alt="Exhibit 08"></a>
<br><b>Exhibit 08 — Apache on 8080</b>
<br><sub>After the restart, <code>ss</code> shows Apache only on <code>*:8080</code></sub>
</td>
</tr>
</table>

---

## 🟠 Module 3 — Azure Firewall (NSG)

Exhibits 09 to 11 (Step 7).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex09"></a>
<a href="screenshots/09-nsg-before.png"><img src="screenshots/09-nsg-before.png" width="380" alt="Exhibit 09"></a>
<br><b>Exhibit 09 — NSG before</b>
<br><sub>Inbound port rules (5), no rule for 443</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="screenshots/10-nsg-rule-form.png"><img src="screenshots/10-nsg-rule-form.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — New rule form</b>
<br><sub>Port 443, TCP, Allow, priority 330, name <code>AllowHTTPS</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="screenshots/11-nsg-443-rule.png"><img src="screenshots/11-nsg-443-rule.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — NSG after</b>
<br><sub>Inbound port rules (6), <code>AllowHTTPS</code> 443 Allow in the list</sub>
</td>
<td></td>
</tr>
</table>

---

## 🟡 Module 4 — Nginx Reverse Proxy

Exhibits 12 to 16 (Steps 8 and 9).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="screenshots/12-nginx-installed.png"><img src="screenshots/12-nginx-installed.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Nginx installed</b>
<br><sub>Nginx 1.24.0, <code>active (running)</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="screenshots/13-nginx-lab-config.png"><img src="screenshots/13-nginx-lab-config.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Proxy config</b>
<br><sub><code>listen 80</code> and <code>proxy_pass http://127.0.0.1:8080</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="screenshots/14-nginx-site-enabled.png"><img src="screenshots/14-nginx-site-enabled.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Site enabled</b>
<br><sub>Default site removed, only <code>lab</code> left in <code>sites-enabled</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="screenshots/15-nginx-configtest.png"><img src="screenshots/15-nginx-configtest.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — Nginx config test</b>
<br><sub><code>nginx -t</code>: syntax is ok, test is successful</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="screenshots/16-curl-nginx-proxy.png"><img src="screenshots/16-curl-nginx-proxy.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Proxy answers</b>
<br><sub><code>200 OK</code>, <code>Server: nginx/1.24.0 (Ubuntu)</code>, before TLS</sub>
</td>
<td></td>
</tr>
</table>

---

## 🔴 Module 5 — TLS and HTTP-to-HTTPS Redirect

Exhibits 17 to 21 (Steps 10 to 13).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="screenshots/17-cert-created.png"><img src="screenshots/17-cert-created.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Certificate created</b>
<br><sub><code>lab.crt</code> (644) and <code>lab.key</code> (600, root only)</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="screenshots/18-nginx-tls-config.png"><img src="screenshots/18-nginx-tls-config.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — TLS config</b>
<br><sub>Port 80 <code>return 301</code>, port 443 <code>ssl</code> with the certificate</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="screenshots/19-nginx-tls-configtest.png"><img src="screenshots/19-nginx-tls-configtest.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — TLS config test</b>
<br><sub><code>nginx -t</code> OK, then <code>systemctl reload nginx</code></sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="screenshots/20-redirect-and-https.png"><img src="screenshots/20-redirect-and-https.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Redirect and HTTPS</b>
<br><sub><code>301 Moved Permanently</code> to <code>https://localhost/</code>, then <code>200 OK</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="screenshots/21-cert-expiry.png"><img src="screenshots/21-cert-expiry.png" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — Certificate dates</b>
<br><sub><code>notBefore</code> 13 Oct 2026, <code>notAfter</code> 13 Oct 2027</sub>
</td>
<td></td>
</tr>
</table>

---

## 🟢 Module 6 — Browser and Laptop Tests

Exhibits 22 to 24 (Steps 14 and 15).

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="screenshots/22-browser-warning.png"><img src="screenshots/22-browser-warning.png" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Certificate warning</b>
<br><sub>Chrome shows <code>NET::ERR_CERT_AUTHORITY_INVALID</code> (self-signed)</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="screenshots/23-browser-https-page.png"><img src="screenshots/23-browser-https-page.png" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — Page over HTTPS</b>
<br><sub>"Backup Lab" page opens, address bar shows <code>Not secure</code></sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="screenshots/24-redirect-from-laptop.png"><img src="screenshots/24-redirect-from-laptop.png" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — Redirect from the laptop</b>
<br><sub><code>curl.exe -I http://</code> from my laptop: <code>301 Moved Permanently</code> to <code>https://</code></sub>
</td>
<td></td>
</tr>
</table>

---

## 📝 Verdict

**Complete** — the site opens over HTTPS through Nginx, with Apache behind it on port 8080. The browser warning is expected because the certificate is self-signed.

| Element | Finding |
|---|---|
| Backend | Apache, listens only on `*:8080` |
| Proxy | Nginx 1.24.0 on ports 80 and 443 |
| Certificate | Self-signed, RSA 2048, 13 Oct 2026 14:02:01 GMT to 13 Oct 2027 14:02:01 GMT |
| HTTP | `301 Moved Permanently` to `https://localhost/` |
| HTTP from my laptop | `301 Moved Permanently` to `https://<VM-PUBLIC-IP>/` |
| HTTPS | `200 OK`, `Server: nginx/1.24.0 (Ubuntu)` |
| Browser | `NET::ERR_CERT_AUTHORITY_INVALID` warning, then the "Backup Lab" page |

---

## 🎯 Verification Checklist

| Check | Method | Module | Status |
|:---:|---|---|:---:|
| Apache moved off port 80 | `ss -tlnp` after restart | Module 2 | ✅ Confirmed |
| Azure allows port 443 | NSG rule list, 6 inbound rules | Module 3 | ✅ Confirmed |
| Nginx passes requests to Apache | `curl -I` shows `Server: nginx`, page from Apache | Module 4 | ✅ Confirmed |
| Certificate valid for 365 days | `openssl x509 -noout -dates` | Module 5 | ✅ Confirmed |
| HTTP redirects to HTTPS | `curl -I http://localhost` | Module 5 | ✅ Confirmed (on the VM) |
| HTTPS answers | `curl -kI https://localhost` | Module 5 | ✅ Confirmed |
| Page reachable from the laptop | Browser opened `https://` address | Module 6 | ✅ Confirmed |
| HTTP redirects to HTTPS from outside the VM | `curl.exe -I http://<VM-PUBLIC-IP>` from my laptop | Module 6 | ✅ Confirmed |

> [!NOTE]
> **Blurred in the screenshots:** Subscription ID, account email, portal address bar and link, the VM public IP, the client IP in the `Last login` line, and the Windows username in the laptop PowerShell screenshot (24). The private address `172.16.0.4` and the resource names (`lab-server`, `sysadmin-labs`) are left visible. In the README the VM public IP is written as `<VM-PUBLIC-IP>`.

<details>
<summary><b>🗂️ Screenshot file names and paths</b></summary>

<br>

File names are lowercase `.png`, so the README and index links match exactly on GitHub.

| Exhibit | File name | Path |
|:---:|---|---|
| 00 | `00-vm-stopped-before-resize.png` | `lab02-nginx-tls/screenshots/00-vm-stopped-before-resize.png` |
| 01 | `01-vm-started-b2ls-v2.png` | `lab02-nginx-tls/screenshots/01-vm-started-b2ls-v2.png` |
| 02 | `02-ssh-login.png` | `lab02-nginx-tls/screenshots/02-ssh-login.png` |
| 03 | `03-ports-before.png` | `lab02-nginx-tls/screenshots/03-ports-before.png` |
| 04a | `04a-grep-ports-conf-only.png` | `lab02-nginx-tls/screenshots/04a-grep-ports-conf-only.png` |
| 04 | `04-apache-config-before.png` | `lab02-nginx-tls/screenshots/04-apache-config-before.png` |
| 05 | `05-ports-conf-changed.png` | `lab02-nginx-tls/screenshots/05-ports-conf-changed.png` |
| 06 | `06-virtualhost-changed.png` | `lab02-nginx-tls/screenshots/06-virtualhost-changed.png` |
| 07 | `07-apache-configtest.png` | `lab02-nginx-tls/screenshots/07-apache-configtest.png` |
| 08 | `08-apache-on-8080.png` | `lab02-nginx-tls/screenshots/08-apache-on-8080.png` |
| 09 | `09-nsg-before.png` | `lab02-nginx-tls/screenshots/09-nsg-before.png` |
| 10 | `10-nsg-rule-form.png` | `lab02-nginx-tls/screenshots/10-nsg-rule-form.png` |
| 11 | `11-nsg-443-rule.png` | `lab02-nginx-tls/screenshots/11-nsg-443-rule.png` |
| 12 | `12-nginx-installed.png` | `lab02-nginx-tls/screenshots/12-nginx-installed.png` |
| 13 | `13-nginx-lab-config.png` | `lab02-nginx-tls/screenshots/13-nginx-lab-config.png` |
| 14 | `14-nginx-site-enabled.png` | `lab02-nginx-tls/screenshots/14-nginx-site-enabled.png` |
| 15 | `15-nginx-configtest.png` | `lab02-nginx-tls/screenshots/15-nginx-configtest.png` |
| 16 | `16-curl-nginx-proxy.png` | `lab02-nginx-tls/screenshots/16-curl-nginx-proxy.png` |
| 17 | `17-cert-created.png` | `lab02-nginx-tls/screenshots/17-cert-created.png` |
| 18 | `18-nginx-tls-config.png` | `lab02-nginx-tls/screenshots/18-nginx-tls-config.png` |
| 19 | `19-nginx-tls-configtest.png` | `lab02-nginx-tls/screenshots/19-nginx-tls-configtest.png` |
| 20 | `20-redirect-and-https.png` | `lab02-nginx-tls/screenshots/20-redirect-and-https.png` |
| 21 | `21-cert-expiry.png` | `lab02-nginx-tls/screenshots/21-cert-expiry.png` |
| 22 | `22-browser-warning.png` | `lab02-nginx-tls/screenshots/22-browser-warning.png` |
| 23 | `23-browser-https-page.png` | `lab02-nginx-tls/screenshots/23-browser-https-page.png` |
| 24 | `24-redirect-from-laptop.png` | `lab02-nginx-tls/screenshots/24-redirect-from-laptop.png` |

</details>

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

🔀 **[Nginx](https://nginx.org/)** · 🔐 **[OpenSSL](https://www.openssl.org/)** · 🌐 **[curl](https://curl.se/)**

</div>
