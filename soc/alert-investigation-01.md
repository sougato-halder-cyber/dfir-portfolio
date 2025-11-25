# SOC Alert Investigation – Multiple Failed Logins

## Alert Summary
SIEM detected 15 failed login attempts within 2 minutes.

## Tools Used
- Splunk (or any SIEM)
- Windows Event Logs

## Steps Taken
1. Checked event log IDs (4625 – failed login attempts)
2. Identified username targeted in the attack.
3. Noted the source IP used for login attempts.
4. Searched previous login history → no match (unusual IP).
5. Checked geo-location of IP (outside normal region).

## Findings
- Username targeted: `admin-test`
- Source IP: 103.88.xx.xx
- Attack type: Brute-force attempt

## Recommendation
- Immediately block the IP
- Reset targeted user password
- Enable MFA for all admin accounts
- Review firewall logs for related activity
