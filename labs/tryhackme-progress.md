# TryHackMe Progress & Learning Notes
DFIR + Forensics Summary (Short & Simple)
DAY 1
DFIR & Windows Forensics —
Introduction on DFIR
🔹 Overview
This document provides a concise summary of the key concepts learned in DFIR (Digital Forensics and Incident Response), essential forensic tools, the PICERL incident response model, and an introduction to Windows forensic artifacts.
It is formatted for technical documentation and GitHub usage.

## Digital Forensics & Incident Response (DFIR)
What is DFIR?

DFIR combines Digital Forensics and Incident Response to:

Collect and preserve digital evidence

Analyze attacker activity

Respond to and recover from security incidents

Forensic Artifacts

Artifacts are traces left behind by user or attacker activity, collected from:

File system

RAM

Network traffic

System logs

## Evidence Preservation Principles
1. Write Protection

Original evidence must never be modified. A write-protected version is kept intact while analysis is performed on a copy.

2. Chain of Custody

A documented record of who handled the evidence at every stage. Prevents contamination and ensures legal validity.

3. Order of Volatility

Collect evidence from most volatile → least volatile sources:

RAM

Running processes

Network connections

Disk storage

4. Timeline Creation

Events are arranged chronologically to reconstruct the attacker’s actions and understand the full scope of the incident.

## Essential DFIR Tools
Eric Zimmerman Tools

A suite of Windows forensic tools for registry analysis, timelines, file metadata, prefetch files, event logs, etc.

KAPE (Kroll Artifact Parser & Extractor)

Automates artifact collection

Parses Windows evidence

Quickly builds forensic timelines

Autopsy

An open-source forensic suite used for:

Disk analysis

File recovery

Timeline generation

Mobile & external media investigations

Volatility

Industry-standard framework for RAM/memory analysis:

Detects malware

Extracts processes

Analyzes system memory artifacts

Redline

FireEye’s incident response tool for:

Collecting system forensic data

Analyzing malicious activity

Velociraptor

Powerful open-source platform for:

Endpoint monitoring

Artifact collection at scale

Enterprise forensics and threat hunting

## Incident Response (IR) Process — PICERL

The IR process is commonly represented by the PICERL acronym (SANS).
NIST’s IR model is equivalent, with slight naming differences.

1. Preparation

Tools, teams, policies, and monitoring systems must be ready before an incident occurs.

2. Identification

Detect the incident using alerts, logs, and indicators.
Verify true vs false positives.

3. Containment

Limit the spread and impact of the threat.
Includes short-term isolation and long-term strategic fixes.

4. Eradication

Remove the threat entirely from the network:

Delete malware

Disable compromised accounts

Close attacker entry points

5. Recovery

Restore services, rebuild systems, and return to normal operations.

6. Lessons Learned (NIST: Post-Incident Activity)

Review the incident, document findings, and update defenses to prevent future recurrence.

## Introduction to Windows Forensics
Why Windows Forensics Matters

Windows holds over 80% of the desktop OS market

Crucial for corporate investigations and incident response

Windows creates rich user activity artifacts, excellent for forensic analysis

Windows Artifact Sources

Windows stores forensic artifacts in:

Registry

User profile directories

Application data folders

Browser databases

System logs

Is Windows “spying” on users?

Not intentionally.
Windows stores user preferences and activity to enhance the user experience, but forensic analysts use these same artifacts as evidence during investigations.

##  Summary of What You Learned

Fundamentals of DFIR

Importance of proper evidence handling

Why timelines matter in investigations

Key forensic and IR tools (Eric Zimmerman, KAPE, Autopsy, Volatility, Redline, Velociraptor)

PICERL incident response process

Windows forensic artifacts and their role in tracking system activity

DAY 2

Windows Forensics 1 – DFIR Notes & Answers

(TryHackMe Learning Summary)

📌 Overview

This repository documents my hands-on learning from the Windows Forensics 1 room on TryHackMe, focusing on registry-based forensic artifacts, user activity, program execution, and USB forensics using real triage data.

🔍 Topics Covered
1️⃣ System Information (Registry)

OS Version & Build Number

Control Sets & Last Known Good

Computer Name

Time Zone Information

Important Registry Paths

SOFTWARE\Microsoft\Windows NT\CurrentVersion
SYSTEM\Select
SYSTEM\CurrentControlSet

2️⃣ User & Account Information

User-created accounts

Last logon time

Password hint

RID identification

Registry Path

SAM\Domains\Account\Users
SAM\Domains\Account\Users\Names

3️⃣ User Activity Artifacts

Recently opened files

File extensions (.txt, .pdf, etc.)

Explorer interaction

Open/Save dialogs

Registry Paths

NTUSER.DAT\...\Explorer\RecentDocs
NTUSER.DAT\...\ComDlg32
NTUSER.DAT\...\TypedPaths

4️⃣ Program Execution Evidence

GUI-based execution

Background execution

Installed & executed binaries

Artifact	Purpose
UserAssist	GUI app execution
ShimCache	Executed binaries
AmCache	Full path + SHA1
BAM/DAM	Background execution

Key Locations

NTUSER.DAT\...\UserAssist
SYSTEM\...\AppCompatCache
C:\Windows\AppCompat\Programs\Amcache.hve

5️⃣ USB Forensics

Connected USB devices

Manufacturer & serial number

Friendly name

First & last connection time

Registry Paths

SYSTEM\CurrentControlSet\Enum\USBSTOR
SOFTWARE\Microsoft\Windows Portable Devices\Devices

✅ Questions & Final Answers
🔹 System & Registry

Current Build Number: 19044

Last Known Good ControlSet: ControlSet001

Computer Name: THM-4n6

TimeZoneKeyName: Pakistan Standard Time

DHCP IP Address: 192.168.100.58

🔹 Accounts

RID of Guest Account: 501

User-created accounts: 3

User never logged in: thm-user2

Password hint (THM-4n6): 123

🔹 User Activity

EZtools opened: 2021-12-01 13:00:34

My Computer last interacted: 2021-12-01 13:06:47

Notepad opened file path:

C:\Program Files\Amazon\Ec2ConfigService\settings


File opened time: 2021-11-30 10:56:19

Changelog.txt accessed: 2021-11-25 03:42:53

🔹 Program Execution

File Explorer launched: 26 times

Another name of ShimCache: Application Compatibility Cache

Artifact storing SHA1 hashes: AmCache

Artifact storing full execution path: AmCache

Python 3.8.2 installer run from:

C:\Users\THM-4n6\Downloads\python-3.8.2.exe

🔹 USB Forensics

USB Manufacturer: Kingston

Serial Number: 1C6f654E59A3B0C179D366AE&0

Device Name: Kingston DataTraveler 2.0 USB Device

Friendly Name: USB

Last connected time: 2021-11-24 18:18:48

🎯 Skills Gained

Registry-based forensic analysis

User activity reconstruction

Program execution tracking

USB device investigation

DFIR triage methodology

🚀 Tools Used

Registry Explorer

AmCache Parser

ShimCache Parser

ShellBags Explorer

EZTools (Eric Zimmerman)

🏁 Status

✅ Windows Forensics 1 – COMPLETED

 
 Day 3
Windows Forensics 2

(File System, Execution Evidence, USB & File Recovery)

📌 Overview

In Windows Forensics 2, we move beyond the Windows Registry and analyze file system–based forensic artifacts.
This day focuses on identifying:

Evidence of program execution

File & folder access (knowledge)

External device / USB usage

Deleted file recovery

NTFS & FAT forensic artifacts using Eric Zimmerman tools and Autopsy

🗂️ File Systems in Windows
🔹 FAT File System

Variants: FAT12, FAT16, FAT32

Commonly used in:

USB drives

SD cards

Digital cameras

Type	Addressable Bits	Max File Size
FAT32	28 bits	4 GB – 1 byte

📌 Limitation: No journaling, weak forensic reliability

🔹 exFAT

Designed for large removable media

Used by digital cameras & SD cards

Supports:

Huge file sizes

Large volumes

Lightweight compared to NTFS

🔹 NTFS (Most Important)

Introduced with Windows NT, NTFS provides:

Journaling

Access controls

File recovery support

Advanced forensic artifacts

Key NTFS Artifacts:
File	Purpose
$MFT	Master File Table – tracks all files
$LogFile	NTFS transaction log
$UsnJrnl	File change journal
$Boot	Volume boot information
🧰 Tools Used (Eric Zimmerman Suite)
Tool	Purpose
MFTECmd	Parse $MFT
PECmd	Parse Prefetch files
WxTCmd	Parse Windows 10 Timeline
JLECmd	Parse Jump Lists
LECmd	Parse Shortcut (.lnk) files
EZViewer	View CSV outputs
Autopsy	Deleted file recovery & browser artifacts
🧠 Evidence of Execution (Task 5)
🔹 Windows Prefetch

Location:

C:\Windows\Prefetch


Contains:

Last execution time

Run count

Executed program name

Files used by program

Command:

PECmd.exe -d "C:\Windows\Prefetch" --csv output_folder


📌 Prefetch = Strong evidence of execution

🔹 Windows 10 Timeline

Location:

C:\Users\<user>\AppData\Local\ConnectedDevicesPlatform\*\ActivitiesCache.db


Parsed using:

WxTCmd.exe -f ActivitiesCache.db --csv output_folder


📌 Shows:

Application execution

Focus time

File interaction

🔹 Jump Lists

Location:

C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations


Parsed using:

JLECmd.exe -d <path> --csv output_folder


📌 Shows:

Programs executed

Recently opened files

First & last execution time

📁 File / Folder Knowledge (Task 6)
🔹 Shortcut Files (.lnk)

Locations:

Recent\
Office\Recent\


Parsed using:

LECmd.exe -d <path> --csv output_folder


📌 Evidence:

First opened time → .lnk created

Last accessed time → .lnk modified

Original file path

USB volume details (if removable)

🔹 IE / Edge WebCache

Location:

WebCacheV*.dat


Includes local file access

Files appear as:

file:///C:/...


Parsed using Autopsy with:

Logical Files

Recent Activity module

🔌 External Devices / USB Forensics (Task 7)
🔹 setupapi.dev.log (MOST IMPORTANT)

Location:

C:\Windows\inf\setupapi.dev.log


📌 Contains:

USB device serial number

First connection time

Last connection time

Search keyword:

USBSTOR

🔹 Shortcut Files (USB Evidence)

Shortcut files can reveal:

USB volume name

Volume serial number

Drive type (Removable)

📌 Correlates USB usage with file access

🗑️ Deleted Files & Recovery
🔹 Disk Image Concept

Bit-by-bit copy of storage

Preserves metadata

Enables forensic analysis without altering evidence

🔹 Recovering Deleted Files using Autopsy

Steps:

Create new case

Add data source → Disk Image

Select image (usb.001)

Disable ingest modules (for speed)

Navigate:

File Views → Deleted Files


Right-click → Extract File(s)

📌 Deleted files are marked with ❌

🧪 Key Forensic Takeaways
Artifact	Evidence
Prefetch	Program execution
Timeline	App usage history
Jump Lists	File & app access
Shortcut (.lnk)	File knowledge
setupapi.dev.log	USB connection
$MFT	File existence & metadata
🏁 Conclusion

Windows Forensics 2 demonstrates how file system artifacts provide powerful forensic evidence even without registry access.
By correlating Prefetch, Timeline, Jump Lists, Shortcut files, and setupapi logs, an investigator can reliably reconstruct user activity, execution history, file access, and USB usage.
Windows Forensics 2 – COMPLETED

 Day 4
 🐧 Linux Forensics 

(System Configuration, Persistence, Execution & Logs)

📌 Overview

Day 3 of Linux Forensics focuses on identifying system configuration, persistence mechanisms, evidence of execution, and log file analysis on a Linux host.
Unlike Windows, Linux stores forensic artifacts primarily in files and logs, making file system knowledge critical for investigations.

🖥️ System Configuration & Host Information
🔹 Hostname

Location

/etc/hostname


Command

cat /etc/hostname


Answer

Linux4n6

🔹 Timezone

Location

/etc/timezone


Command

cat /etc/timezone


Answer

Asia/Karachi

🔹 Network Interfaces & IP Information

Static config

/etc/network/interfaces


Live system

ip address show


Artifacts obtained:

Interface name

IP address

MAC address

🔹 Active Network Connections

Command

netstat -natp


Question

What program is listening on 127.0.0.1:5901?

Answer

Xtigervnc

🔹 Running Process Path

Command

ps aux


Question

What is the full path of this program?

Answer

/usr/bin/Xtigervnc

🔐 Persistence Mechanisms

Persistence mechanisms allow programs or malware to survive system reboots.

🔹 Cron Jobs

Location

/etc/crontab


Command

cat /etc/crontab


Used for:

Scheduled execution

Malware persistence via hidden scripts

🔹 Startup Services

Location

/etc/init.d/


Command

ls /etc/init.d/


Services listed here automatically start during boot.

🔹 Bash Startup Files

User-level persistence

~/.bashrc


System-wide

/etc/bash.bashrc
/etc/profile


Attackers often insert malicious commands here.

❓ Question

In the bashrc file, the size of the history file is defined.
What is the size of the history file set for the user Ubuntu?

Answer

2000

▶️ Evidence of Execution
🔹 Sudo Execution History

Location

/var/log/auth.log


Command

cat /var/log/auth.log* | grep -i COMMAND


Records:

sudo commands

Working directory (PWD)

Executed binary

❓ Question 1

The user tryhackme used apt-get to install a package.
What command was issued?

Answer

sudo apt-get install apache2

❓ Question 2

What was the current working directory when the command to install net-tools was issued?

Answer

/home/ubuntu

🔹 Bash History

Location

~/.bash_history


Contains:

Non-sudo commands

File and directory operations

🔹 Vim File Access

Location

~/.viminfo


Provides:

Files opened in Vim

Command & search history

Timestamps

📜 Log Files Analysis

Log files are stored in:

/var/log/

🔹 Syslog

Location

/var/log/syslog*


Command

cat /var/log/syslog* | head


Contains:

System events

Cron jobs

Service activity

🔹 Authentication Logs

Location

/var/log/auth.log*


Contains:

User creation

Group assignments

sudo activity

Login attempts

🔹 Third-Party Application Logs

Examples:

/var/log/apache2/
/var/log/samba/
/var/log/openvpn/


Apache logs:

access.log

error.log

❓ Question

The machine previously had a different hostname.
What was the previous hostname?

Answer

tryhackme

🧠 Key Forensic Takeaways
Artifact	Evidence Provided
/etc/passwd	User accounts & UID
/etc/group	Group membership
auth.log	Sudo & authentication
syslog	System & service events
crontab	Scheduled persistence
.bashrc	User-level persistence
.bash_history	Command execution
.viminfo	File access evidence
🏁 Summary

Linux Forensics relies heavily on log analysis, configuration files, and user artifacts rather than a centralized registry.
By correlating data from system configuration files, persistence mechanisms, execution history, and logs, investigators can accurately reconstruct user activity and system behavior over time.

Day 5
🧪 Digital Forensics with Autopsy 
📌 Overview

Day 5 focuses on learning and applying Autopsy, an open-source digital forensics platform widely used in law enforcement, DFIR teams, and corporate investigations.
This day covers workflow, data sources, ingest modules, user interface, visualization tools, reporting, and real-world data analysis using a mini investigation scenario.

🧠 What is Autopsy?

Autopsy is a powerful, GUI-based digital forensics platform built on The Sleuth Kit.

Key Features

Disk image & VM analysis

Plug-in based architecture (Ingest Modules)

Timeline & visualization tools

Keyword search & artifact correlation

Report generation (HTML/CSV)

Used by:

Law enforcement

National security

Corporate incident response teams

🔁 Autopsy Workflow (Core Concept)

Every Autopsy investigation follows this workflow:

Create / Open Case

Add Data Source

Configure Ingest Modules

Review Extracted Artifacts

Generate Report

🔑 Always review Data Source Summary first before deep analysis.

📂 Case Management
🔹 Case File Extension
.aut

Case Types

Single-User (used in this room)

Multi-User (server-based)

💽 Data Sources in Autopsy
Supported Disk Image Formats
Type	Examples
Raw Single	.img, .dd, .raw, .bin
Raw Split	.001, .002, .aa, .ab
EnCase	.e01, .e02, .e03
Virtual Machines	.vmdk, .vhd

📌 For split images, only the first file (e.g., .e01) is required.

❓ Q&A

What is the disk image name of the "e01" format?
✅ Answer: EnCase

⚙️ Ingest Modules

Ingest Modules are Autopsy plug-ins that extract specific forensic artifacts.

Examples

Interesting Files Identifier

Keyword Search

Recent Activity

Web Artifacts

Deleted Files

Execution Options

During data source addition

After data source is added (Right-click → Run Ingest Modules)

📌 Results appear under the Results node in Tree Viewer.

🖥️ User Interface – Part I
5 Main UI Areas
1️⃣ Tree Viewer

Top-level nodes:

Data Sources

Views

Results

Tags

Reports

2️⃣ Result Viewer

Tabs:

Table

Thumbnail

Summary

Allows file extraction and artifact review.

3️⃣ Views

Files categorized by:

Extension

MIME Type

Deleted Files

File Size

🔥 MIME Type is more reliable than file extension.

4️⃣ Contents Viewer

Shows detailed file metadata.

Columns:

S (Score) → Notable / Suspicious

C (Comment)

O (Occurrence) (Central Repository)

5️⃣ Status Area

Ingest progress

Alerts

Cancel option

❓ Q&A

Number of Data Sources: 4

Detected Removed Files: 10

Interesting File Name: googledrivesync.exe

🧾 User Interface – Part II (Summary & Reporting)
Data Source Summary

Provides high-level overview:

OS version

File types

Deleted files

User activity

Interesting files

Report Generation

Supported formats:

HTML

CSV

📌 Reports allow offline analysis without Autopsy GUI.

❓ Q&A

OS Version: Windows 7 Ultimate Service Pack 1

Documents Percentage: 40.8%

Interesting Files Identifier Job Number: 10

🕵️ Task 7 – Data Analysis (Mini Scenario)
Scenario

An employee is suspected of leaking company data.
Disk image metadata was analyzed using Autopsy.

Key Findings
Evidence	Finding
Installed Program	Eraser (Anti-forensics tool)
Password Hint	IAMAN
Network Access	SECRET files from 10.11.11.128
Web Searches	information leakage cases
Timed Search	anti-forensic tools
Suspicious Binary MD5	fe18b02e890f7a789c576be8abccdc99
Sticky Note	Tomorrow...Everything will be OK...
❓ Q&A (All Correct)

Installed Program → Eraser

Password Hint → IAMAN

Network IP → 10.11.11.128

Top Search Term → information leakage cases

Timed Search → anti-forensic tools

MD5 Hash → fe18b02e890f7a789c576be8abccdc99

Sticky Note Message → Tomorrow...Everything will be OK...

⏳ Visualization Tools – Timeline

Timeline is the most powerful visualization tool in Autopsy.

Components

Filters

Events

File / Content details

View Modes

Counts

Details

List

Supports:

Clustering

Pin / Unpin

Hide / Unhide events

❓ Q&A

Events on 2015-01-12: 46

Highest Activity Date: March 25, 2015

🧠 Forensic Correlation Summary

Strong indicators of insider data leakage:

Anti-forensic tools installed

Research on data leakage & anti-forensics

Access to SECRET files over network

Suspicious binaries

Psychological stress indicators

🔴 Confidence Level: HIGH

📝 One-Line GitHub Summary

Day 5 focused on mastering Autopsy for digital forensics, covering case workflow, data sources, ingest modules, UI navigation, visualization through timelines, reporting, and a real-world insider data leakage investigation.

🏁 Day 5 Status

✅ COMPLETED – Autopsy (TryHackMe)

 Day 6
Redline 
Task 6: IOC Search Collector Analysis (Threat Hunting)
📌 Scenario Overview

Organization: Osinski Inc.
Incident Type: Suspected intrusion
Attack Technique: Lateral Movement – Pass-the-Hash (PtH)

The security team suspects:

An attacker planted a malicious file

Tool used for lateral movement

File was masqueraded

Only partial indicators were initially known

🎯 Goal:
Identify the planted malicious file using IOC-based threat hunting with Redline.

🧠 Initial Intelligence (Known Artifacts)

Only the following IOCs were provided:

File Strings
20210513173819Z0w0=
<?<L<T<g=

File Size
834936 bytes


📌 No filename
📌 No hash
📌 No path

➡️ Classic threat hunting scenario with limited intel.

🛠️ Tools & Data Used

FireEye Redline

IOC Search Collector

Existing analysis session

📂 Session Location:

C:\Users\Administrator\Documents\Analysis\Sessions\AnalysisSession1

🔎 Investigation Approach (How the Hunt Was Done)

Created IOC using:

File strings

File size

Loaded existing .mans session in Redline

Executed IOC Search analysis

Filtered files matching all artifacts

Validated:

File path

Owner

Subsystem

Device path

Hash

Correlated hash with known attacker tools

🚨 Findings (Malicious File Identified)
✅ Full File Path (Including Filename)
C:\Users\Administrator\AppData\Local\Temp\8eJv8w2id6IqN85dfC.exe


📌 Temp directory → common attacker drop location
📌 Random filename → masquerading technique

✅ Directory Path Only
C:\Users\Administrator\AppData\Local\Temp\

✅ File Owner
BUILTIN\Administrators


⚠️ Indicates execution with administrative privileges

✅ Subsystem
Windows_CUI


📌 Command-line executable
📌 Common for lateral movement tools

✅ Device Path
\Device\HarddiskVolume2

✅ SHA-256 Hash
57492d33b7c0755bb411b22d2dfdfdf088cbbfcd010e30dd8d425d5fe66adff4

🎭 Masquerading Detection

Using hash correlation and known tool signatures:

🔴 Real Filename Identified
PsExec.exe

🧠 Why This Matters (Analyst Insight)

PsExec.exe is a legitimate Sysinternals tool

Frequently abused by attackers for:

Lateral movement

Pass-the-Hash attacks

Remote command execution

Masquerading helps evade:

AV detection

Casual inspection

📌 Context + behavior = malicious intent confirmed

❓ Questions & Answers
Question	Answer
Full file path	C:\Users\Administrator\AppData\Local\Temp\8eJv8w2id6IqN85dfC.exe
Directory path	C:\Users\Administrator\AppData\Local\Temp\
File owner	BUILTIN\Administrators
Subsystem	Windows_CUI
Device path	\Device\HarddiskVolume2
SHA-256 hash	57492d33b7c0755bb411b22d2dfdfdf088cbbfcd010e30dd8d425d5fe66adff4
Real filename	PsExec.exe
🧠 Key Takeaways (Day 6)

IOC hunting works even with partial indicators

File strings + file size are powerful together

Temp directories are attacker favorites

Masquerading is extremely common

PsExec is dual-use → intent matters

Contextual analysis is critical in DFIR

📝 GitHub Summary (Day 6)
### Day 6 – Redline IOC Search Collector Analysis

In this task, IOC-based threat hunting was conducted to identify a malicious executable used for lateral movement at Osinski Inc. Using limited indicators such as file strings and file size, a suspicious executable was discovered in the Temp directory. Further analysis revealed the file was masquerading under a random name but matched the SHA-256 hash of PsExec.exe, a tool commonly abused in Pass-the-Hash attacks. This task highlights the effectiveness of IOC-driven investigations and contextual analysis in incident response.

✅ Day 6 Status

✔ IOC-based Threat Hunting
✔ Masquerading Detection
✔ Lateral Movement Analysis
✔ Pass-the-Hash Context

Day 7 
KAPE (Kroll Artifact Parser and Extractor)

Platform: TryHackMe
Category: Windows Forensics | DFIR | Incident Response
Tool: KAPE
Author: Eric Zimmerman (Kroll)

🔍 Day 7 Overview

আগের Windows Forensics rooms (WF1, WF2)-এ আমরা manualভাবে artifacts analyze করেছি।
কিন্তু বাস্তব incident response-এ সময় সবচেয়ে বড় factor।

👉 KAPE এই সমস্যা solve করে by:

দ্রুত forensic artifacts collect করা

automated parsing করা

minimal user interaction-এ triage output দেওয়া

🎯 Learning Objectives (Day 7)

KAPE কী এবং কেন ব্যবহার করা হয়

Targets vs Modules বোঝা

Compound Targets & Modules

KAPE GUI ব্যবহার

KAPE CLI ও Batch Mode

Policy Violation Investigation (Hands-on)

🧠 What is KAPE?

KAPE (Kroll Artifact Parser and Extractor) একটি portable DFIR triage tool, যা:

Live system / mounted image থেকে artifacts collect করে

OS-locked files bypass করে (raw disk read)

Original timestamps ও metadata preserve করে

Eric Zimmerman forensic tools দিয়ে artifacts parse করে

⚡ Goal: Speed + Accuracy in Incident Response

🧩 Core Architecture
🔹 Targets

Targets define করে → কি collect করা হবে

Extension: .tkape

Examples:

Prefetch

Registry Hives

Event Logs

USB Artifacts

Browser Artifacts

🔸 Compound Targets

একাধিক targets একসাথে collect করার জন্য

Examples:

KapeTriage

!BasicCollection

!SANS_Triage

📌 Used for rapid triage

🔹 Modules

Modules define করে → collected data কীভাবে parse হবে

Extension: .mkape

Output: CSV / TXT / JSON

Mostly Eric Zimmerman tools

Examples:

PECmd → Prefetch

LECmd → LNK files

EvtxECmd → Event logs

RECmd → Registry

📁 bin directory

External executables রাখা হয় (EZ tools)

🖥️ KAPE GUI (gkape.exe)
Important GUI Options

✔️ Use Target Options

✔️ Target Source → C:\

✔️ Target Destination → Output folder

✔️ %d → Append Date & Time

✔️ %m → Append Machine Name

✔️ Use Module Options

Lab Configuration

Target: KapeTriage

Module: !EZParser

⌨️ KAPE CLI Usage
Basic Command
kape.exe --tsource C: --target KapeTriage --tdest C:\Users\thm-4n6\Desktop\Target --mdest C:\Users\thm-4n6\Desktop\Module --module !EZParser

Useful Variables
Variable	Description
%d	Timestamp
%m	Machine Name
%s	System Drive
📦 Batch Mode (_kape.cli)

Used when:

Non-technical user runs KAPE

SOC automation

IR playbooks

Example:

--tsource C: --target KapeTriage --tdest C:\Target --mdest C:\Module --module !EZParser

🕵️ Hands-on Investigation (Acceptable Use Policy Violation)
Findings

USB mass storage connected

Software installed from Network drive

Browser search artifacts

Network profiles recorded

KAPE copied from removable media

❓ Questions & Answers
🔌 USB Devices

Q: Other USB Serial Number?
✅ 1C6F654E59A3B0C179D366AE

📦 Software Installation

Q: Network drive path used for installs?
✅ Z:\Setups

▶️ Execution Evidence

Q: CHROMESETUP.EXE execution time?
✅ 11/25/2021 03:33

🔍 User Activity

Q: Search query executed?
✅ RunWallpaperSetup.cmd

🌐 Network Evidence

Q: Network 3 first connection time?
✅ 11/30/2021 15:44

💾 Removable Media

Q: Drive letter KAPE copied from?
➡️ Identified via USB + LNK + ShellBags artifacts

🧠 Analyst Notes (Exam + Real World)

KAPE = Collection + Parsing, not full analysis

Always preserve timestamps

Compound Targets save huge time

Ideal combo:

KAPE + Autopsy

KAPE + Redline

USB + Network installs = policy breach indicator

🏁 Day 7 Takeaway

KAPE is a must-know DFIR tool

Use KAPE when:

Ransomware suspected

Insider threat

USB misuse

Rapid triage needed

👉 Collect fast → Analyze smart
