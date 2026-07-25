# SOC Home Lab — Threat Detection Environment (`securecorp.local`)

A simulated enterprise network built in VMware to practice end-to-end SOC workflows: domain infrastructure, endpoint logging, SIEM monitoring, and attack detection.

## Goal

Build a small but realistic enterprise environment — domain controller, workstation, and SIEM — to practice how a SOC analyst actually sees and investigates activity, rather than just studying concepts in isolation.

## Architecture

| Component | Role | Details |
|---|---|---|
| **DC01** | Domain Controller | Windows Server 2022 — AD DS, DNS, DHCP — `192.168.1.20` |
| **PC01** | Workstation | Windows 10 Pro, domain-joined — `192.168.1.101` |
| **SIEM** | Splunk Enterprise | Ubuntu VM — `192.168.1.160` |
| **Kali Linux** | Attacker box | Used to simulate attacks against the domain |
| **Metasploitable2** | Vulnerable target | Practice target for scanning/exploitation |

**Domain:** `securecorp.local`
**Log pipeline:** Sysmon + Splunk Universal Forwarders on DC01 and PC01 → Splunk Enterprise (SIEM)

> Diagram placeholder — [add a simple network diagram here, e.g. draw.io export]

## What I Built

- Stood up Active Directory Domain Services, DNS, and DHCP on DC01, and joined PC01 to the `securecorp.local` domain
- Deployed Sysmon on endpoints for detailed process, network, and file-event logging
- Installed Splunk Enterprise as the SIEM and configured Universal Forwarders on DC01 and PC01 to ship logs
- Built an 8-panel SOC monitoring dashboard in Splunk
- Wrote detection alert rules for brute-force login attempts
- Simulated brute-force attacks from the Kali VM and validated detection of failed-logon events (**Windows EventCode 4625**) in Splunk

## Log Analysis Practice

Worked through Splunk log analysis (TryHackMe SIEM room, Task 4) querying:
- `EventCode=3` (Sysmon — network connections)
- `EventCode=1` (Sysmon — process creation, including MD5 hashes)
- `EventCode=4698` (Windows Security — scheduled task creation)
- Scoped to `index=task4`

> Screenshot placeholder — [Splunk dashboard, alert firing, search results]

## Issues Encountered & Fixes

| Issue | Fix |
|-------|-----|
| Network adapter conflicts with Jio 5G hotspot | Switched from Bridged to NAT mode for internet access and used Internal Network for inter-VM communication |
| ARM64 vs x64 ISO mismatch | Downloaded incorrect ARM64 Windows ISO. Fixed by downloading correct x64 64-bit ISO from Microsoft official website |
| AD DS vs AD CS confusion | Accidentally installed Certificate Services instead of Domain Services. Removed AD CS completely then correctly installed AD DS and promoted to Domain Controller |
| Windows 10 Home cannot join domain | Rebuilt PC01 using Windows 10 Pro ISO as Home edition does not support Active Directory domain joining |
| Splunk Universal Forwarder not sending logs | Manually created inputs.conf file with correct WinEventLog paths, verified port 9997 open on Splunk server, confirmed correct Ubuntu IP address and restarted forwarder service |

---

## Lessons Learned

Building this SOC home lab from scratch 
taught me that real cybersecurity work 
involves much more troubleshooting than 
expected. Understanding WHY something 
fails is more valuable than just fixing 
it — the ARM64 vs x64 issue taught me 
to always verify hardware compatibility 
before downloading software.

The biggest surprise was how powerful 
Splunk is even in a home environment. 
I expected basic logs but saw detailed 
attack evidence including exact 
timestamps, source IPs, targeted 
accounts and failure reasons all 
captured automatically through 
EventCode 4625.

If starting over I would take VM 
snapshots before every major 
configuration change. Several times 
I rebuilt machines from scratch due 
to misconfigurations. This taught me 
the critical importance of change 
management in real SOC environments.

Most importantly this project changed 
how I think about security. When I 
simulated a brute force attack from 
Kali Linux and immediately saw failed 
login events in Splunk it clicked — 
this is exactly what SOC analysts do 
every day to protect organizations.
## Next Steps

- [ ] Add more detection rules (e.g. suspicious PowerShell execution, lateral movement)
- [ ] Document a full incident walkthrough (attack → detection → investigation → response)
- [ ] Add network diagram

---
*Credentials, IPs, and internal hostnames in this repo are for a local, non-internet-facing lab environment only.*
