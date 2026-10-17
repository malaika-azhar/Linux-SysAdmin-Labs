<a id="top"></a>
<div align="center">

# 📡 Lab 06 — Index
### Disk & Uptime Monitoring
**Lab 06 of the Linux SysAdmin Labs**

![Bash](https://img.shields.io/badge/Script-Bash-1A5276?style=for-the-badge)
![Cron](https://img.shields.io/badge/Schedule-Cron-B9770E?style=for-the-badge)
![Uptime Kuma](https://img.shields.io/badge/Monitor-Uptime_Kuma_2.5.5-5CDD8B?style=for-the-badge)
![pm2](https://img.shields.io/badge/Process-pm2-2B037A?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2EA043?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

**Quick guide to every step and screenshot in this lab.**

### [📖 Open the full README](README.md)

</div>

---

<div align="center">

| 🧩 Modules | 🖼️ Screenshots | 💽 Disk Alert | ⏰ Monitor Interval | 🔻 DOWN → UP |
|:---:|:---:|:---:|:---:|:---:|
| **9** | **35** | **80% of `/`** | **20 s** | **22:05:52 → 22:07:32** |

</div>

<p align="center">🧩 <b>Case:</b> VM <code>lab-server</code> · Ubuntu 24.04 · Uptime Kuma 2.5.5 · Lab date 13 Oct 2026</p>

---

## 📑 Step Index

| # | Step | Result | Evidence |
|:---:|---|---|:---:|
| 1 | Check the VM in Azure | VM running | [Ex 1](#ex1) |
| 2 | Connect to the server | SSH login works | [Ex 2](#ex2) |
| 3 | Write the disk check script | Script created, executable, verified | [Ex 3](#ex3) [Ex 4](#ex4) [Ex 5](#ex5) |
| 4 | Run it at threshold 80 | No alert at about 10% | [Ex 6](#ex6) |
| 5 | Prove the alert works | Alert in log file and syslog | [Ex 7](#ex7) [Ex 8](#ex8) [Ex 9](#ex9) |
| 6 | Put the threshold back to 80 | Line 3 shows -ge 80 | [Ex 10](#ex10) [Ex 11](#ex11) |
| 7 | Schedule it with cron | */10 job listed | [Ex 12](#ex12) [Ex 13](#ex13) |
| 8 | Install Node.js 20 | Node v20.20.2, npm 10.8.2 | [Ex 14](#ex14) to [Ex 17](#ex17) |
| 9 | Download and set up Uptime Kuma | Version 2.5.5 set up | [Ex 18](#ex18) [Ex 19](#ex19) |
| 10 | Start Kuma and open the SSH tunnel | Dashboard opened through the tunnel | [Ex 20](#ex20) [Ex 21](#ex21) [Ex 22](#ex22) |
| 11 | Add a monitor for the Nginx site | UP after Ignore TLS/SSL errors | [Ex 23](#ex23) [Ex 24](#ex24) |
| 12 | Stop Nginx and watch it recover | DOWN 22:05:52, UP 22:07:32 | [Ex 25](#ex25) to [Ex 28](#ex28) |
| 13 | Keep Kuma running with pm2 | online, startup service enabled | [Ex 29](#ex29) to [Ex 34](#ex34) |
| 14 | Check that cron ran the check | Run seen at 17:10:01 UTC | [Ex 35](#ex35) |

---

## 🔵 Module 1 — Access

Exhibits 1 to 2.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex1"></a>
<a href="screenshots/lab06-01-azure-vm-running.png"><img src="screenshots/lab06-01-azure-vm-running.png" width="380" alt="Exhibit 1"></a>
<br><b>Exhibit 1 — Azure VM running</b>
<br><sub>lab-server Running, Standard B2ls v2</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex2"></a>
<a href="screenshots/lab06-02-ssh-login.png"><img src="screenshots/lab06-02-ssh-login.png" width="380" alt="Exhibit 2"></a>
<br><b>Exhibit 2 — SSH login</b>
<br><sub>Connected as azureuser, / at 10.3%</sub>
</td>
</tr>
</table>

---

## 🟣 Module 2 — Disk Check Script

Exhibits 3 to 5.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex3"></a>
<a href="screenshots/lab06-03-diskcheck-created.png"><img src="screenshots/lab06-03-diskcheck-created.png" width="380" alt="Exhibit 3"></a>
<br><b>Exhibit 3 — Script created</b>
<br><sub>diskcheck.sh written with tee</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex4"></a>
<a href="screenshots/lab06-04-chmod.png"><img src="screenshots/lab06-04-chmod.png" width="380" alt="Exhibit 4"></a>
<br><b>Exhibit 4 — Made executable</b>
<br><sub>chmod +x on the script</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex5"></a>
<a href="screenshots/lab06-05-verify-script.png"><img src="screenshots/lab06-05-verify-script.png" width="380" alt="Exhibit 5"></a>
<br><b>Exhibit 5 — Script verified</b>
<br><sub>-rwxr-xr-x, 209 bytes, 6 lines</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🟠 Module 3 — Alert Test

Exhibits 6 to 11.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex6"></a>
<a href="screenshots/lab06-06-run-normal-no-alert.png"><img src="screenshots/lab06-06-run-normal-no-alert.png" width="380" alt="Exhibit 6"></a>
<br><b>Exhibit 6 — Normal run</b>
<br><sub>Exit code 0, no alert at about 10%</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex7"></a>
<a href="screenshots/lab06-07-threshold-1-sed.png"><img src="screenshots/lab06-07-threshold-1-sed.png" width="380" alt="Exhibit 7"></a>
<br><b>Exhibit 7 — Threshold set to 1</b>
<br><sub>sed change for the test only</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex8"></a>
<a href="screenshots/lab06-08-alert-triggered.png"><img src="screenshots/lab06-08-alert-triggered.png" width="380" alt="Exhibit 8"></a>
<br><b>Exhibit 8 — Alert triggered</b>
<br><sub>disk_alert.log line at 11%</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex9"></a>
<a href="screenshots/lab06-09-syslog-warning.png"><img src="screenshots/lab06-09-syslog-warning.png" width="380" alt="Exhibit 9"></a>
<br><b>Exhibit 9 — Syslog warning</b>
<br><sub>DISK WARNING: / at 11% in syslog</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex10"></a>
<a href="screenshots/lab06-10-threshold-restored.png"><img src="screenshots/lab06-10-threshold-restored.png" width="380" alt="Exhibit 10"></a>
<br><b>Exhibit 10 — Threshold restored</b>
<br><sub>sed puts the limit back to 80</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex11"></a>
<a href="screenshots/lab06-11-verify-80.png"><img src="screenshots/lab06-11-verify-80.png" width="380" alt="Exhibit 11"></a>
<br><b>Exhibit 11 — Threshold 80 verified</b>
<br><sub>Line 3 shows -ge 80 again</sub>
</td>
</tr>
</table>

---

## 🟡 Module 4 — Cron Schedule

Exhibits 12 to 13.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex12"></a>
<a href="screenshots/lab06-12-cron-added.png"><img src="screenshots/lab06-12-cron-added.png" width="380" alt="Exhibit 12"></a>
<br><b>Exhibit 12 — Cron line added</b>
<br><sub>*/10 job added to root's crontab</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex13"></a>
<a href="screenshots/lab06-13-cron-verify.png"><img src="screenshots/lab06-13-cron-verify.png" width="380" alt="Exhibit 13"></a>
<br><b>Exhibit 13 — Cron verified</b>
<br><sub>Lab 1 backup and disk check both listed</sub>
</td>
</tr>
</table>

---

## 🔵 Module 5 — Node.js Install

Exhibits 14 to 17.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex14"></a>
<a href="screenshots/lab06-14-node-check-before.png"><img src="screenshots/lab06-14-node-check-before.png" width="380" alt="Exhibit 14"></a>
<br><b>Exhibit 14 — Node check</b>
<br><sub>node and npm missing, git present</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex15"></a>
<a href="screenshots/lab06-15-nodesource-repo.png"><img src="screenshots/lab06-15-nodesource-repo.png" width="380" alt="Exhibit 15"></a>
<br><b>Exhibit 15 — NodeSource repo</b>
<br><sub>node_20.x repository configured</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex16"></a>
<a href="screenshots/lab06-16-nodejs-install.png"><img src="screenshots/lab06-16-nodejs-install.png" width="380" alt="Exhibit 16"></a>
<br><b>Exhibit 16 — Node.js installed</b>
<br><sub>nodejs 20.20.2 installed</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex17"></a>
<a href="screenshots/lab06-17-node-npm-versions.png"><img src="screenshots/lab06-17-node-npm-versions.png" width="380" alt="Exhibit 17"></a>
<br><b>Exhibit 17 — Versions checked</b>
<br><sub>Node v20.20.2, npm 10.8.2</sub>
</td>
</tr>
</table>

---

## 🟢 Module 6 — Uptime Kuma Setup

Exhibits 18 to 22.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex18"></a>
<a href="screenshots/lab06-18-kuma-clone.png"><img src="screenshots/lab06-18-kuma-clone.png" width="380" alt="Exhibit 18"></a>
<br><b>Exhibit 18 — Uptime Kuma cloned</b>
<br><sub>git clone finished with no errors</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex19"></a>
<a href="screenshots/lab06-19-kuma-npm-setup.png"><img src="screenshots/lab06-19-kuma-npm-setup.png" width="380" alt="Exhibit 19"></a>
<br><b>Exhibit 19 — npm run setup</b>
<br><sub>649 packages added, release 2.5.5 extracted</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex20"></a>
<a href="screenshots/lab06-20-kuma-server-running.png"><img src="screenshots/lab06-20-kuma-server-running.png" width="380" alt="Exhibit 20"></a>
<br><b>Exhibit 20 — Kuma running</b>
<br><sub>Version 2.5.5 listening on port 3001</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex21"></a>
<a href="screenshots/lab06-21-ssh-tunnel.png"><img src="screenshots/lab06-21-ssh-tunnel.png" width="380" alt="Exhibit 21"></a>
<br><b>Exhibit 21 — SSH tunnel</b>
<br><sub>-L 3001:localhost:3001 opened</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex22"></a>
<a href="screenshots/lab06-22-kuma-setup-page.png"><img src="screenshots/lab06-22-kuma-setup-page.png" width="380" alt="Exhibit 22"></a>
<br><b>Exhibit 22 — Setup page</b>
<br><sub>Create your admin account</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 🔴 Module 7 — Monitor and DOWN Test

Exhibits 23 to 28.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex23"></a>
<a href="screenshots/lab06-23-monitor-down-self-signed.png"><img src="screenshots/lab06-23-monitor-down-self-signed.png" width="380" alt="Exhibit 23"></a>
<br><b>Exhibit 23 — Monitor DOWN, certificate</b>
<br><sub>self-signed certificate on first check</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex24"></a>
<a href="screenshots/lab06-24-monitor-up.png"><img src="screenshots/lab06-24-monitor-up.png" width="380" alt="Exhibit 24"></a>
<br><b>Exhibit 24 — Monitor UP</b>
<br><sub>200 - OK after Ignore TLS/SSL errors</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex25"></a>
<a href="screenshots/lab06-25-nginx-stop.png"><img src="screenshots/lab06-25-nginx-stop.png" width="380" alt="Exhibit 25"></a>
<br><b>Exhibit 25 — Nginx stopped</b>
<br><sub>systemctl stop nginx</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex26"></a>
<a href="screenshots/lab06-26-monitor-down-econnrefused.png"><img src="screenshots/lab06-26-monitor-down-econnrefused.png" width="380" alt="Exhibit 26"></a>
<br><b>Exhibit 26 — Monitor DOWN, refused</b>
<br><sub>ECONNREFUSED 127.0.0.1:443 at 22:05:52</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex27"></a>
<a href="screenshots/lab06-27-nginx-start.png"><img src="screenshots/lab06-27-nginx-start.png" width="380" alt="Exhibit 27"></a>
<br><b>Exhibit 27 — Nginx started</b>
<br><sub>systemctl start nginx</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex28"></a>
<a href="screenshots/lab06-28-monitor-up-again.png"><img src="screenshots/lab06-28-monitor-up-again.png" width="380" alt="Exhibit 28"></a>
<br><b>Exhibit 28 — Monitor UP again</b>
<br><sub>200 - OK at 22:07:32</sub>
</td>
</tr>
</table>

---

## 🟤 Module 8 — Keep It Running (pm2)

Exhibits 29 to 34.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex29"></a>
<a href="screenshots/lab06-29-kuma-foreground-stopped-log.png"><img src="screenshots/lab06-29-kuma-foreground-stopped-log.png" width="380" alt="Exhibit 29"></a>
<br><b>Exhibit 29 — Foreground server stopped</b>
<br><sub>SIGINT and the monitor history in the log</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex30"></a>
<a href="screenshots/lab06-30-pm2-start.png"><img src="screenshots/lab06-30-pm2-start.png" width="380" alt="Exhibit 30"></a>
<br><b>Exhibit 30 — pm2 start</b>
<br><sub>uptime-kuma online under pm2</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex31"></a>
<a href="screenshots/lab06-31-dashboard-after-pm2.png"><img src="screenshots/lab06-31-dashboard-after-pm2.png" width="380" alt="Exhibit 31"></a>
<br><b>Exhibit 31 — Dashboard after pm2</b>
<br><sub>Same monitor and events after restart</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex32"></a>
<a href="screenshots/lab06-32-pm2-startup-command.png"><img src="screenshots/lab06-32-pm2-startup-command.png" width="380" alt="Exhibit 32"></a>
<br><b>Exhibit 32 — pm2 startup command</b>
<br><sub>systemd found, command printed</sub>
</td>
</tr>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex33"></a>
<a href="screenshots/lab06-33-pm2-startup-enabled.png"><img src="screenshots/lab06-33-pm2-startup-enabled.png" width="380" alt="Exhibit 33"></a>
<br><b>Exhibit 33 — pm2 startup enabled</b>
<br><sub>pm2-azureuser.service enabled</sub>
</td>
<td align="center" valign="top" width="50%">
<a id="ex34"></a>
<a href="screenshots/lab06-34-pm2-save.png"><img src="screenshots/lab06-34-pm2-save.png" width="380" alt="Exhibit 34"></a>
<br><b>Exhibit 34 — pm2 save</b>
<br><sub>Process list saved to dump.pm2</sub>
</td>
</tr>
</table>

---

## ⚪ Module 9 — Cron Run Proof

Exhibit 35.

<table>
<tr>
<td align="center" valign="top" width="50%">
<a id="ex35"></a>
<a href="screenshots/lab06-35-cron-ran-in-syslog.png"><img src="screenshots/lab06-35-cron-ran-in-syslog.png" width="380" alt="Exhibit 35"></a>
<br><b>Exhibit 35 — Cron ran</b>
<br><sub>diskcheck.sh started by cron at 17:10:01 UTC</sub>
</td>
<td width="50%"></td>
</tr>
</table>

---

## 📝 Outcome

**Monitoring works** — the disk check fires when forced, cron runs it, and Uptime Kuma reports DOWN and UP when Nginx is stopped and started.

| Element | Finding |
|---|---|
| Disk check | Quiet at threshold 80, alert in log file and syslog at threshold 1 |
| Schedule | `*/10 * * * *` in root's crontab, run seen in syslog at 17:10:01 UTC |
| Web monitor | `Nginx Site`, HTTPS, every 20 s, Ignore TLS/SSL errors ticked |
| Stop test | DOWN `ECONNREFUSED` at 22:05:52, UP `200 - OK` at 22:07:32 |
| Background run | pm2 `online`, `pm2-azureuser.service` enabled, reboot not tested |

---

## 🎯 Verification Checklist

| Check | Method | Exhibit | Status |
|:---:|---|:---:|:---:|
| Script is executable | `ls -l` shows `-rwxr-xr-x` | [Ex 5](#ex5) | ✅ Confirmed |
| No false alert below threshold | Exit code 0, no `disk_alert.log` | [Ex 6](#ex6) | ✅ Confirmed |
| Alert fires above threshold | Log file line at 11% | [Ex 8](#ex8) | ✅ Confirmed |
| Alert reaches syslog | `grep "DISK WARNING"` | [Ex 9](#ex9) | ✅ Confirmed |
| Threshold restored | `grep -n "ge"` shows `-ge 80` | [Ex 11](#ex11) | ✅ Confirmed |
| Cron entry listed | `sudo crontab -l` | [Ex 13](#ex13) | ✅ Confirmed |
| Cron ran the script | Syslog CRON line, 17:10:01 UTC | [Ex 35](#ex35) | ✅ Confirmed |
| Monitor reports DOWN | Nginx stopped | [Ex 26](#ex26) | ✅ Confirmed |
| Monitor reports recovery | Nginx started | [Ex 28](#ex28) | ✅ Confirmed |
| Kuma survives a restart | Same data under pm2 | [Ex 31](#ex31) | ✅ Confirmed |
| Kuma starts after a VM reboot | Not rebooted | none | ⚠️ Not tested |

> [!NOTE]
> The Uptime Kuma dashboard shows Pakistan time (UTC+5) and syslog shows UTC. Public IPs, subscription details and account emails are blurred in the screenshots.

---

<div align="center">

[⬆️ Back to top](#top) &nbsp;·&nbsp; [📖 Full README](README.md)

💽 **Bash disk check** · ⏰ **Cron** · 🌐 **[Uptime Kuma](https://github.com/louislam/uptime-kuma)** · 🔁 **pm2**

</div>
