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
