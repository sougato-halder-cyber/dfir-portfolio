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

 Day 3
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
