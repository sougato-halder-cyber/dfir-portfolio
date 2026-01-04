# DFIR Portfolio

Hi 👋 I'm Sougato Halder.

This repository contains my Digital Forensics (DFIR) & Cybersecurity projects, case studies and learning notes.

## Folders
- forensics/ — forensic case studies  
- network/ — pcap & Wireshark analysis  
- soc/ — SIEM alert analysis  
- labs/ — TryHackMe & practice notes  

## Tools I use
Autopsy, FTK Imager, Wireshark, Splunk, Kali Linux

Case 01 
– Memory Forensics Investigation (Volatility)
📌 Case Information
Field	Details
Case ID	01
Case Type	Memory Forensics
Platform	TryHackMe
Tool Used	Volatility 3
Analyst	Sougato
Case Status	Completed
🎯 Objective

The objective of this investigation was to analyze volatile memory (RAM) dumps to identify malicious activity, suspicious processes, injected code, and ransomware artifacts using Volatility 3.

This case focuses on memory-only evidence, where malware may not exist on disk but is fully active in RAM.

🛠️ Tools & Environment

Volatility 3

Python 3

Windows Memory Dumps

TryHackMe Investigation Labs

📂 Evidence Overview

Two memory dumps were analyzed during this investigation:

Investigation	Memory File	Description
Case 001	Investigation-1.vmem	Banking trojan disguised as Adobe Reader
Case 002	Investigation-2.raw	Ransomware post-incident memory analysis
🔍 Investigation 1
🐎 “BOB! THIS ISN’T A HORSE!”
🧾 System Information

OS Build Version: 2600.xpsp.080413-2111

Memory Acquisition Time: 2012-07-22 02:45:08

⚠️ Suspicious Process Identified
Attribute	Value
Process Name	reader_sl.exe
PID	1640
Parent Process	explorer.exe
Parent PID	1484

📌 The process was masquerading as a legitimate Adobe Reader component.

🌐 Network Indicators

Suspicious IP Address: 41.168.5.140

User-Agent Used by Attacker:

Mozilla/5.0 (Windows; U; MSIE 7.0; Windows NT 6.0; en-US)

🏦 Threat Intelligence Findings

Multiple suspicious banking-related domains were found.

Chase Bank was confirmed as one of the targeted domains.

✅ Conclusion (Case 001):
A banking trojan was actively running in memory, disguised as an Adobe Reader process, communicating with a malicious external IP.

🔍 Investigation 2
😈 “That Kind of Hurt My Feelings”
🦠 Malware Identification
Attribute	Value
Malware Name	WannaCry
Suspicious Process	@WanaDecryptor@
PID	740
Parent Process	tasksche.exe
Parent PID	1940
📁 Malware File Path
C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe

🔑 Malware Artifacts
Artifact Type	Value
Socket DLL	Ws2_32.dll
Mutex	MsWinZonesCacheCounterMutexA
🔎 Useful Volatility Plugin

windows.filescan
Used to identify files loaded from the malware working directory.

✅ Conclusion (Case 002):
The system was infected with WannaCry ransomware, confirmed through memory-only artifacts without relying on disk evidence.

🧠 Key Findings

Malware can operate fully in memory without touching disk

Memory forensics is critical for:

Fileless malware

Banking trojans

Ransomware analysis

Volatility plugins like psscan, malfind, and filescan are essential

📌 Lessons Learned

pslist alone is not enough — hidden processes require psscan

malfind is powerful for detecting injected code

Memory analysis complements disk-based forensics

RAM often contains the most honest evidence

🧾 Final Verdict

System Compromised

✔ Banking Trojan Detected
✔ Ransomware (WannaCry) Confirmed
✔ Malicious Network Communication Observed
