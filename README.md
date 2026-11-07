# lab2-Post-Compromise-SMB-Share-Discovery
This investigation follows a previously identified password spray attack that resulted in successful authentication to a Windows 11 endpoint.

### Lab Environment
- Kali Linux 10.161.170.22
- Windows 11 10.161.200.x 
- Security Onion SIEM
- Elastic Agent Installed

### Incident Summary
A successful authentication (Event ID 4624) was previously identified following multiple failed authentication attempts.
Further investigation identified Event ID 5140 immediately after the successful logon, confirming access to SMB network resources.
No evidence of privilege escalation, process execution, PowerShell activity, or lateral movement was observed during the investigation period.


### Attack Timeline

2026-11-07-- 09-04.57.412 (UTC-6) -- Multiple failed authentication attempts detected(4625)
2026-11-07-- 09-04.57.480 (UTC-6) -- Additional authentication failures observed (4625)
2026-11-07-- 09-04.57.533 (UTC-6) -- Successful network logon detected (4624)
2026-11-07-- 09-15.57.535 (UTC-6) -- SMB share access detected on multiple time (5140)

### Investigation findings

Source IP : 10.161.170.22
Target Accounts victim.user and finance.jane
Host Targeted : WLAB01
Logon Type : 3-Network Logon

Observations

- Multiple authentication failures detected
- Successful authentication observed
- Evidence consistent with Password Spraying
- Indicator of compromise (IOC) ( victim.user, Finance.jane, Event ID 4626/5140)

### Evidence

#### Attack from kali (Reconnaissance)
- <img width="1272" height="568" alt="Kali- Reconnaissance _spraying" src="https://github.com/user-attachments/assets/219a08a8-86ca-4e73-a7fc-a3b21cda64a9" />

#### Elastic Discovery
- <img width="1666" height="842" alt="4624-Failed Login" src="https://github.com/user-attachments/assets/44f1c575-b8f1-4938-80a1-64a08fd7b060" />

#### Attack from kali (Reconnaissance-Confirmed) --Account compromised

- <img width="1256" height="381" alt="Kali-Reconnaissance- Confirmed 2" src="https://github.com/user-attachments/assets/8e39b8d5-f91b-45a1-a1e4-5fb94f0f4819" />
#### Elastic Discovery
<img width="1911" height="856" alt="4625-Success Login-account compromise" src="https://github.com/user-attachments/assets/36804550-090f-4911-b0ff-366d2d1c2100" />

### Network Share Access

#### Discovery from kali
<img width="1254" height="481" alt="Kali- Discovery" src="https://github.com/user-attachments/assets/bbb46af6-2ab0-465b-97d1-1fc66f4b15bd" />
<img width="1265" height="509" alt="Kali-Discovery users account" src="https://github.com/user-attachments/assets/046cf9f3-a7d9-4acb-b764-1225dc77ce03" />

#### Elastic Discovery
<img width="1723" height="728" alt="5140-SMB enuration -ShareNetwork Object" src="https://github.com/user-attachments/assets/de692aba-e2a4-4ef5-9562-dfe6d53e17db" />


#### MITRE ATT&CK Mapping
- Tactic (Brute Force)- IDPassword Spraying :	T1110.003 
- Tactic (Initial Access) - Technique (Valid Accounts): T1078
- Tactic (Discovery)- Technique (Network Share Discovery): T1135


#### Impact Assessment

Successful authentication was observed after multiple failed attempts. Potential  access to shared resoucres with no evidence of modification activity or service disruption.This should be level as High 

#### Recommendations
- Reset affected user credential
- Implement Multi-Factor Authentication (MFA).
- Review SMB permissions.
- Enable account lockout policies.
- Monitor Event IDs 4624 and 5140.

#### Lessons Learned

This investigation demonstrated the importance of correlating Windows authentication events and SMB access events to understand attacker behavior following successful authentication.
Combining Event IDs 4624 and 5140 enabled identification of post-compromise discovery activity and improved visibility into attacker actions.

