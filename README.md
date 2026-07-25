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
|---|---|
| Network adapter conflicts when using a Jio 5G hotspot for internet | *[fill in exact fix]* |
| ARM64 vs x64 ISO mismatch during VM setup | *[fill in exact fix]* |
| Confusion between AD DS and AD CS during role installation | *[fill in exact fix]* |
| Windows 10 **Home** can't join a domain | Rebuilt PC01 on Windows 10 **Pro** |
| Splunk Universal Forwarder configuration issues | *[fill in exact fix]* |

## Lessons Learned

*[2–4 sentences — what you'd do differently, what surprised you, what you understand now that you didn't at the start. This section is often what interviewers ask about directly, so it's worth writing carefully.]*

## Next Steps

- [ ] Add more detection rules (e.g. suspicious PowerShell execution, lateral movement)
- [ ] Document a full incident walkthrough (attack → detection → investigation → response)
- [ ] Add network diagram

---
*Credentials, IPs, and internal hostnames in this repo are for a local, non-internet-facing lab environment only.*
