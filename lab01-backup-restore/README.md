<div align="center">

# 💾 Lab 01 — Linux Backup & Restore

**Ubuntu 24.04 on Azure · Apache + MySQL · Backup Script · Checksums · Cron · Disaster Test · Restore**

A hands-on Linux sysadmin lab. I backed up a web page and a database with a script, checked the backup with checksums, scheduled it with cron, deleted the live data on purpose, and restored everything from the backup.

![Linux](https://img.shields.io/badge/Linux-Ubuntu_24.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Cloud](https://img.shields.io/badge/Cloud-Azure_VM-0078D6?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Web](https://img.shields.io/badge/Web-Apache_2.4-D22128?style=for-the-badge&logo=apache&logoColor=white)
![Database](https://img.shields.io/badge/Database-MySQL_8.0-005EB8?style=for-the-badge&logo=mysql&logoColor=white)
![Automation](https://img.shields.io/badge/Automation-Bash_%2B_Cron-B9770E?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Azure_Student_Credit-2ea44f?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

The method: set up test data, back it up, verify the backup, break the system, restore it, and keep the evidence.

### [📸 Jump to the steps](#steps)

</div>

---

## 📑 Table of Contents

1. [At a Glance](#at-a-glance)
2. [Objective](#objective)
3. [Environment](#environment)
4. [Tools & Technologies](#tools)
5. [The Lab Flow](#flow)
6. [Which Step Do I Need?](#which-step)
7. [Steps](#steps)
8. [Results](#results)
9. [Where a Backup Can Break](#backup-breaks)
10. [Backup Method Pipeline](#pipeline)
11. [Command Cheat Sheet](#cheat-sheet)
12. [Challenges & Fixes](#problems-fixes)
13. [Scope & Limitations](#scope-limitations)
14. [What I Learned](#what-i-learned)
15. [Skills Demonstrated](#skills-demonstrated)
16. [Folder Structure](#folder-structure)

---

<a id="at-a-glance"></a>
## 📊 At a Glance

| 🖥 Server | 🧩 Backed Up | 🔐 Integrity Check | ⏱ Restore Time |
|:---:|:---:|:---:|:---:|
| **1 Ubuntu 24.04 VM** | **Web files + 1 database** | **SHA-256, both files OK** | **2 min 47 s** |

---

<a id="objective"></a>
## 🎯 Objective

Show that I can protect a small web server and recover it after data loss.

- Back up the website files and a MySQL database with one script.
- Prove the backup files are not damaged by checking SHA-256 checksums.
- Schedule the script with cron.
- Delete the live data, then restore it from the backup and confirm it is back.

<div align="center">

### 🧩 Lab Workflow at a Glance

<table>
<tr>
<td align="center" valign="top" width="18%">

**🧪 Prepare**<br>
<sub>Test web page<br>and database</sub>

</td>
<td align="center" valign="middle" width="2.5%">

**➜**

</td>
<td align="center" valign="top" width="18%">

**💾 Back up**<br>
<sub>One script,<br>3 files</sub>

</td>
<td align="center" valign="middle" width="2.5%">

**➜**

</td>
<td align="center" valign="top" width="18%">

**🔐 Verify**<br>
<sub>Check the<br>checksums</sub>

</td>
<td align="center" valign="middle" width="2.5%">

**➜**

</td>
<td align="center" valign="top" width="18%">

**💥 Destroy**<br>
<sub>Delete the<br>live data</sub>

</td>
<td align="center" valign="middle" width="2.5%">

**➜**

</td>
<td align="center" valign="top" width="18%">

**♻️ Restore**<br>
<sub>Bring the data<br>back and confirm</sub>

</td>
</tr>
<tr>
<td colspan="9" align="center">

![tar](https://img.shields.io/badge/tar-1A5276?style=for-the-badge)
![mysqldump](https://img.shields.io/badge/mysqldump-117864?style=for-the-badge)
![sha256sum](https://img.shields.io/badge/sha256sum-76448A?style=for-the-badge)
![crontab](https://img.shields.io/badge/crontab-B9770E?style=for-the-badge)<br>
<sub>Built-in Linux tools, no paid software</sub>

</td>
</tr>
</table>

</div>

---

<a id="environment"></a>
## 🧱 Environment

| Item | Value |
|---|---|
| Cloud | Azure virtual machine, India South Central |
| VM name / size | `lab-server` · Standard B4as v2 (4 vCPU, 16 GiB) |
| Operating system | Ubuntu 24.04.4 LTS |
| Web server | Apache 2.4.58 |
| Database | MySQL 8.0.46 |
| Access | SSH with a key file, user `azureuser` |
| Firewall (`ufw`) | Inactive during this lab |
| Date of lab | 13 Oct 2026 (VM clock is UTC) |

> [!NOTE]
> Public IP, subscription details and key fingerprints are blurred in the screenshots. The VM details are shown in Step 1.

---

<a id="tools"></a>
## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| `ssh` | Connect to the VM from my laptop |
| `systemctl status` | Check that Apache and MySQL are running |
| `ss -tlnp`, `curl -I` | Check that Apache listens on port 80 and answers |
| `mysql`, `mysqldump` | Create test data and export the database |
| `tar` | Pack and unpack the website folder |
| `sha256sum` | Create and verify checksums for the backup files |
| `crontab` | Schedule the backup |
| `nano`, `chmod` | Write the script and make it executable |

---

<a id="flow"></a>
## ⏱️ The Lab Flow

```mermaid
flowchart LR
    subgraph G1["🟣 PREPARE"]
        direction TB
        A1["Test data<br/>web page + database"]
        A2["Backup script<br/>backup.sh"]
    end
    subgraph G2["🔵 PROTECT"]
        direction TB
        B1["Run backup<br/>3 files created"]
        B2["Verify<br/>sha256sum -c"]
        B3["Schedule<br/>cron at 02:00"]
    end
    subgraph G3["🟠 DISASTER"]
        direction TB
        C1["Destroy<br/>delete page + database"]
    end
    subgraph G4["🟢 RECOVER"]
        direction TB
        D1["Restore<br/>tar + mysql"]
        D2["Verify<br/>data is back"]
    end
    G1 ==> G2 ==> G3 ==> G4

    classDef pr fill:#f0eaf8,stroke:#6f42c1,stroke-width:2px,color:#000
    classDef po fill:#e8f1fb,stroke:#005EB8,stroke-width:2px,color:#000
    classDef di fill:#fff4e5,stroke:#e08a00,stroke-width:2px,color:#000
    classDef re fill:#eef7ee,stroke:#2ea44f,stroke-width:2px,color:#000
    class A1,A2 pr
    class B1,B2,B3 po
    class C1 di
    class D1,D2 re
```

<p align="center"><em>The groups show the four stages of the lab, from preparing the data to confirming the restore.</em></p>

---

<a id="which-step"></a>
## 🧭 Which Step Do I Need?

```mermaid
flowchart TD
    Start(["🎯 What do you need to do?"]):::start
    Start --> A["Keep a copy of the<br/>web page and database"]:::q
    Start --> B["Check the backup<br/>is not damaged"]:::q
    Start --> C["Run the backup every day<br/>without doing it by hand"]:::q
    Start --> D["Data was deleted<br/>or lost"]:::q
    A --> S1(["Backup script<br/>Step 5"]):::s
    B --> S2(["Checksum check<br/>Step 6"]):::s
    C --> S3(["Cron job<br/>Step 7"]):::s
    D --> S4(["Restore<br/>Steps 8 and 9"]):::s

    classDef start fill:#943126,stroke:#571C16,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef q fill:#1A5276,stroke:#0B2E43,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef s fill:#1E8449,stroke:#0E4A28,stroke-width:3px,color:#FFFFFF,font-weight:bold
```

---

<a id="steps"></a>
## 📸 Steps

### Step 1 — Check the VM in Azure

The VM `lab-server` in the `sysadmin-labs` resource group: Linux (Ubuntu 24.04), Standard B4as v2, India South Central, created 13/10/2026 11:48 UTC. Status was `Running` when this screenshot was taken.

![Azure VM overview](screenshots/00-azure-vm-overview.png)

### Step 2 — Connect to the server

Connected to `lab-server` over SSH with the key file.

![SSH connected](screenshots/01-ssh-connected.png)

### Step 3 — Check the services

Apache is `active (running)`.

![Apache active](screenshots/02-apache-active.png)

`curl -I http://localhost` returns `200 OK`.

![Apache curl 200](screenshots/02a-apache-curl-200.png)

`ufw` shows `Status: inactive`.

![ufw inactive](screenshots/02b-ufw-inactive.png)

Apache is listening on port 80.

![Apache listening on 80](screenshots/02c-apache-listening-80.png)

MySQL is `active (running)`.

![MySQL running](screenshots/02d-mysql-running.png)

### Step 4 — Create the test data

A simple web page:

```bash
echo "<h1>Backup Lab</h1>" | sudo tee /var/www/html/index.html
cat /var/www/html/index.html
```

![Web page created](screenshots/03a-web-page-created.png)

A database `labdb` with a `users` table and 3 rows:

```sql
CREATE DATABASE labdb;
USE labdb;
CREATE TABLE users (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(50));
INSERT INTO users (name) VALUES ('Ali'),('Sara'),('Omar');
SELECT * FROM users;
```

![Test data](screenshots/03-test-data-select.png)

### Step 5 — Write the backup script

`/usr/local/bin/backup.sh`, made executable with `chmod +x`:

```bash
#!/bin/bash
STAMP=$(date +%F_%H%M)
DEST=/var/backups/lab
mkdir -p $DEST
tar -czf $DEST/web_$STAMP.tar.gz /var/www/html
mysqldump labdb > $DEST/db_$STAMP.sql
sha256sum $DEST/web_$STAMP.tar.gz $DEST/db_$STAMP.sql > $DEST/checksums_$STAMP.txt
find $DEST -type f -mtime +7 -delete
```

What each line does:

- `STAMP` puts the date and time in every file name.
- `tar` packs the website folder. `mysqldump` exports the database.
- `sha256sum` saves a checksum for each backup file.
- `find ... -mtime +7 -delete` removes backup files older than 7 days.

![Backup script](screenshots/04-backup-script.png)

### Step 6 — Run the backup and verify it

```bash
sudo /usr/local/bin/backup.sh
ls -l /var/backups/lab
cd /var/backups/lab && sudo sha256sum -c checksums_*.txt
```

Three files were created with the same timestamp (`2026-10-13_1235`), and both checksums returned `OK`.

![Backup and checksums](screenshots/05-backup-files-checksum-ok.png)

### Step 7 — Schedule it with cron

```bash
sudo crontab -e
sudo crontab -l
```

Entry added: `0 2 * * * /usr/local/bin/backup.sh` (every day at 02:00 server time).

![Cron job](screenshots/06-cron-job.png)

### Step 8 — Destroy the data

Noted the time (**12:43:42 UTC**), then deleted the page and the database:

```bash
sudo rm /var/www/html/index.html
sudo mysql -e "DROP DATABASE labdb;"
ls /var/www/html
sudo mysql -e "SHOW DATABASES;"
```

`/var/www/html` is empty and `labdb` is gone from the database list. The backup folder was not touched.

![After destroy](screenshots/07-after-destroy.png)

### Step 9 — Restore from the backup

```bash
sudo tar -xzf /var/backups/lab/web_2026-10-13_1235.tar.gz -C /
sudo mysql -e "CREATE DATABASE labdb;"
sudo mysql labdb < /var/backups/lab/db_2026-10-13_1235.sql
```

![Restore commands](screenshots/08-restore-commands.png)

Then checked the result:

```bash
cat /var/www/html/index.html
sudo mysql -e "SELECT * FROM labdb.users;"
date
```

The page shows `<h1>Backup Lab</h1>` and the table has all 3 rows again. End time: **12:46:29 UTC**.

![Restore verified](screenshots/09-restore-verified.png)

---

<a id="results"></a>
## ✅ Results

| Check | Result |
|---|---|
| Backup files created | 3 files: web archive, database dump, checksum list |
| Checksum verification | Both files `OK` |
| Cron job | Added and listed with `crontab -l` |
| Data destroyed | Web page deleted, `labdb` dropped |
| Web page restored | `<h1>Backup Lab</h1>` is back |
| Database restored | `users` table has 3 rows (Ali, Sara, Omar) |
| Restore time | 12:43:42 → 12:46:29 UTC = **2 min 47 s** |

> [!NOTE]
> The 2 min 47 s includes my own typing time and a very small data set (web archive 186 bytes, SQL dump 1,944 bytes). It is not a measure of how long a real restore would take.

---

<a id="backup-breaks"></a>
## 🔍 Where a Backup Can Break

```mermaid
flowchart LR
    A["💾 Live data<br/>web page + database"]:::a --> B["📜 Backup script<br/>tar + mysqldump"]:::b --> C["📁 Backup files<br/>stored on the same VM"]:::c --> D["🔐 Checksum<br/>files are intact"]:::d --> E["♻️ Restore test<br/>data comes back"]:::e

    classDef a fill:#1A5276,stroke:#0B2E43,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef b fill:#76448A,stroke:#432752,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef c fill:#943126,stroke:#571C16,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef d fill:#B9770E,stroke:#6E4409,stroke-width:3px,color:#FFFFFF,font-weight:bold
    classDef e fill:#1E8449,stroke:#0E4A28,stroke-width:3px,color:#FFFFFF,font-weight:bold
```

<p align="center"><em>The red box is the weak point in this lab: the backup sits on the same VM as the data it protects.</em></p>

### 🔍 Analyst Note — Why the Restore Test Comes Last

A checksum only proves the files were not changed. Only a restore proves the data comes back.

```mermaid
flowchart TD
    A["🎯 Backup is made"]:::start --> B["🔐 Checksum check<br/>are the files intact"]:::work
    B -->|OK| C["♻️ Restore test<br/>does the data come back"]:::work
    B -->|Not OK| X["🔁 Make the backup again"]:::bad
    C -->|Data is back| D["✅ Backup is proven"]:::good
    C -->|Data is missing| E["🔎 Check the script and the files"]:::bad

    classDef start fill:#e8f1fb,stroke:#005EB8,stroke-width:2px,color:#000
    classDef work fill:#fff4e5,stroke:#e08a00,stroke-width:2px,color:#000
    classDef good fill:#eef7ee,stroke:#2ea44f,stroke-width:2px,color:#000
    classDef bad fill:#fdeaea,stroke:#C8102E,stroke-width:2px,color:#000
```

---

<a id="pipeline"></a>
## 🧭 Backup Method Pipeline

How the lab turns a plain server into a backup that has been proven to work

```mermaid
flowchart TB
    Pre["🧪 PREPARE<br/>Create the test data"]:::symClass
    Scr["📜 SCRIPT<br/>Write backup.sh"]:::isoClass
    Bak["💾 BACK UP<br/>Run it, get 3 files"]:::decClass
    Ver["🔐 VERIFY<br/>Check the checksums"]:::fixClass
    Sch["⏰ SCHEDULE<br/>Add the cron job"]:::verClass
    Res["♻️ RESTORE<br/>Destroy, then recover"]:::repClass
    Doc["📝 DOCUMENT<br/>Commands, times, screenshots"]:::docClass

    Pre --> Scr --> Bak --> Ver --> Sch --> Res --> Doc

    classDef symClass fill:#2C3E70,stroke:#131B3A,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef isoClass fill:#1A5276,stroke:#0B2E43,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef decClass fill:#76448A,stroke:#432752,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef fixClass fill:#B9770E,stroke:#6E4409,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef verClass fill:#1E8449,stroke:#0E4A28,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef repClass fill:#117864,stroke:#083D33,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px
    classDef docClass fill:#943126,stroke:#571C16,stroke-width:5px,color:#FFFFFF,font-weight:bold,font-size:16px

    linkStyle default stroke:#2C3E70,stroke-width:4px
```

---

<a id="cheat-sheet"></a>
## 🧾 Command Cheat Sheet

| Command | Purpose |
|---------|---------|
| `sudo systemctl status apache2 --no-pager` | Check that Apache is running |
| `sudo ss -tlnp \| grep ':80'` | See what is listening on port 80 |
| `curl -I http://localhost` | Check that the web server answers |
| `tar -czf <file>.tar.gz <folder>` | Create a compressed archive |
| `tar -xzf <file>.tar.gz -C /` | Extract an archive back to its original path |
| `mysqldump <db> > <file>.sql` | Export a database |
| `mysql <db> < <file>.sql` | Load an export into a database |
| `sha256sum <files> > <list>.txt` | Save checksums |
| `sha256sum -c <list>.txt` | Check files against saved checksums |
| `sudo crontab -e` · `sudo crontab -l` | Edit and list scheduled jobs |
| `find <dir> -type f -mtime +7 -delete` | Delete files older than 7 days |

---

<a id="problems-fixes"></a>
## ⚠️ Challenges & Fixes

| ❌ Challenge | ✅ Solution |
|---|---|
| `Test-NetConnection` gave `command not found` | It is a Windows PowerShell command, and I ran it inside the Linux SSH session. Used `curl -I http://localhost` and `ss -tlnp` on the VM instead |
| A pasted `cat` command was cut in the middle and ran `p.sh` as a command | The terminal paste glitched. Typed the command by hand and it showed the full script |
| Pasted the cron line into the editor choice menu instead of choosing an editor | The menu rejected it and nothing broke. Chose `1` (nano), then added the line inside nano |
| The page was not reachable from my laptop browser on port 80 | **Cause not confirmed.** The VM side checked out locally (`ss` and `curl`), so I used `curl` on the VM as proof |

---

<a id="scope-limitations"></a>
## 🚧 Scope & Limitations

- **Backup is on the same VM.** If the VM is lost, the backup is lost too. A real setup copies it to Azure Blob Storage or another server.
- **Cron has not run on its own yet.** I only checked that the job is listed. I did not wait for 02:00 to see it run.
- **Cron uses the server clock, which is UTC.** 02:00 there is 07:00 in Pakistan.
- **The cron line sits at the top of the crontab file,** not the bottom. Cron does not care about position.
- **No alert if the backup fails.** The script does not check whether each step worked.
- **Checksums prove the files were not changed, not that they restore well.** The restore test covers that part, but only once.
- **MySQL root login uses the local socket,** so `sudo mysql` needs no password and none is stored in the script.
- **Browser access to port 80 from my laptop was not confirmed** (see Challenges). The page was checked with `curl` on the VM.
- **Tiny data set.** Restore time will be much longer with real data.

---

<a id="what-i-learned"></a>
## 🧠 What I Learned

- **A backup is not proven until it is restored.** The checksum says the files are intact; only the restore shows the data comes back.
- **Time-stamped file names** keep each backup separate and make the restore command easy to build.
- **Checksums** are a quick way to catch a damaged backup file.
- **`tar` and `mysqldump` solve two different problems:** files and database content need separate handling.
- **Keep the backup away from the data it protects.** Same-VM storage is a weak point, noted in the limitations.
- **Keep the evidence.** Screenshots with times let me show how long the recovery took.

---

<a id="skills-demonstrated"></a>
## 🛠️ Skills Demonstrated

- Connecting to a Linux server over SSH with a key
- Checking service and port status on Ubuntu
- Writing and running a Bash backup script
- Backing up files with `tar` and a database with `mysqldump`
- Verifying backup integrity with SHA-256 checksums
- Scheduling jobs with cron
- Restoring a website and a database after data loss
- Documenting a lab with commands, times, screenshots and honest limits

---

<a id="folder-structure"></a>
## 📁 Folder Structure

```text
lab01-backup-restore/
|-- README.md
`-- screenshots/
    |-- 00-azure-vm-overview.png
    |-- 01-ssh-connected.png
    |-- 02-apache-active.png
    |-- 02a-apache-curl-200.png
    |-- 02b-ufw-inactive.png
    |-- 02c-apache-listening-80.png
    |-- 02d-mysql-running.png
    |-- 03a-web-page-created.png
    |-- 03-test-data-select.png
    |-- 04-backup-script.png
    |-- 05-backup-files-checksum-ok.png
    |-- 06-cron-job.png
    |-- 07-after-destroy.png
    |-- 08-restore-commands.png
    `-- 09-restore-verified.png
```
