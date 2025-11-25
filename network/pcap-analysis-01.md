# PCAP Analysis 01 – Suspicious HTTP Traffic

## Objective
Analyze a PCAP file to identify suspicious network activity.

## Tools Used
- Wireshark

## Steps Performed
1. Loaded sample `traffic.pcap` file in Wireshark.
2. Applied filter: `http.request`
3. Observed repeated GET requests from the same IP address.
4. Checked packet details → suspicious User-Agent string.
5. Noted abnormal frequency of requests within short time.

## Findings
- Source IP: 192.168.1.45 (example)
- Repeated GET requests = possible enumeration / scanning.
- User-Agent looked like an automated script.
- Time gap between requests very low.

## Conclusion
This traffic behavior suggests:
- Possible brute-force
- Automated scanning tool  
Further investigation should check:
- Destination server logs  
- Authentication failures  
