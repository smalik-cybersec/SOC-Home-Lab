# SOC Home Lab — Complete Project Notes
### Analyst: Shahid | CS Graduate | Aspiring SOC Analyst
### Project: Cybersecurity Home Lab for SOC Analyst Portfolio
### Last Updated: July 2026

---

> **How To Use These Notes:**
> - Every `[Insert Screenshot: ...]` placeholder = add your screenshot there
> - Follow sections in order — they match your resume and GitHub
> - These notes are your interview preparation guide

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Lab Architecture](#2-lab-architecture)
3. [Tools and Technologies](#3-tools-and-technologies)
4. [Phase 1 — Virtualization Setup](#4-phase-1--virtualization-setup)
5. [Phase 2 — Domain Controller DC01](#5-phase-2--domain-controller-dc01)
6. [Phase 3 — Active Directory](#6-phase-3--active-directory)
7. [Phase 4 — DNS and DHCP](#7-phase-4--dns-and-dhcp)
8. [Phase 5 — Workstations PC01 and PC02](#8-phase-5--workstations-pc01-and-pc02)
9. [Phase 6 — Splunk SIEM](#9-phase-6--splunk-siem)
10. [Phase 7 — Sysmon Integration](#10-phase-7--sysmon-integration)
11. [Phase 8 — Attack Simulation](#11-phase-8--attack-simulation)
12. [Phase 9 — SOC Dashboard](#12-phase-9--soc-dashboard)
13. [Phase 10 — Incident Response](#13-phase-10--incident-response)
14. [Issues Encountered and Fixes](#14-issues-encountered-and-fixes)
15. [Lessons Learned](#15-lessons-learned)
16. [Interview Preparation](#16-interview-preparation)
17. [Current Status and Next Steps](#17-current-status-and-next-steps)

---

## 1. Project Overview

### What Is This Project?

This is a complete cybersecurity SOC (Security Operations Center)
home lab built from scratch on a personal laptop using VMware
Workstation. The lab simulates a real enterprise network environment
including a Windows domain, multiple workstations, a SIEM platform,
and attack simulation tools.

### Why I Built This

- To gain hands-on practical experience in SOC operations
- To demonstrate real skills to employers beyond certifications
- To practice threat detection, log analysis and incident response
- To build a strong portfolio for SOC Analyst job applications

### Host Machine Specifications

```
Operating System: Windows 10/11
RAM: 16GB
Storage: 474GB SSD (400GB+ free)
Processor: AMD Ryzen 5 5500U
Virtualization Software: VMware Workstation
```

---

## 2. Lab Architecture

### Network Overview

```
+------------------+     +------------------+
|   DC01           |     |   PC01           |
|   Windows        |     |   Windows        |
|   Server 2022    |<--->|   10 Pro         |
|   Domain         |     |   Domain         |
|   Controller     |     |   Workstation    |
|   192.168.160.10 |     |   192.168.160.20 |
+------------------+     +------------------+
         |                        |
         |                        |
+------------------+     +------------------+
|   Ubuntu         |     |   PC02           |
|   Splunk SIEM    |     |   Windows 11     |
|   192.168.160.x  |     |   Enterprise     |
|   Port 9997      |     |   Domain         |
+------------------+     |   Workstation    |
         |               |   192.168.160.30 |
         |               +------------------+
+------------------+     +------------------+
|   Kali Linux     |     |   Metasploitable2|
|   Attacker       |     |   Vulnerable     |
|   Machine        |     |   Target         |
+------------------+     +------------------+
```

### Domain Information

```
Domain Name: securecorp.local
Domain Controller: DC01
Admin Username: Administrator
Network Range: 192.168.160.0/24
Default Gateway: 192.168.160.2
DNS Server: 192.168.160.10 (DC01)
```

<img width="1668" height="876" alt="vmware_hP9pPVey6Y" src="https://github.com/user-attachments/assets/f295a60a-6ba2-4ba6-bb4e-d69fb43f1a37" />

---

## 3. Tools and Technologies

| Tool | Purpose | Version |
|------|---------|---------|
| VMware Workstation | Virtualization platform | Latest |
| Windows Server 2022 | Domain Controller OS | Evaluation |
| Windows 10 Pro | Workstation OS | x64 |
| Windows 11 Enterprise | Workstation OS | x64 Evaluation |
| Ubuntu Desktop | Splunk server OS | 24.04 LTS |
| Kali Linux | Attack simulation | 2026.1 |
| Metasploitable2 | Vulnerable target | Latest |
| Splunk Enterprise | SIEM platform | Latest |
| Sysmon | Advanced Windows logging | Latest |
| Nmap | Network scanning | Latest |
| Hydra | Password attack simulation | Latest |

---

## 4. Phase 1 — Virtualization Setup

### What Is Virtualization?

Virtualization allows you to run multiple computers inside one
physical computer. VMware Workstation is the software that makes
this possible. Each virtual machine (VM) behaves exactly like a
real physical computer but runs as software.

```
Think of it like this:
Your laptop = A building
VMware = Rooms inside the building
Each VM = A separate computer in each room
```

### VMware Workstation Installation

**Steps Completed:**

1. Downloaded VMware Workstation from official website
2. Installed with default settings
3. Accepted license agreement
4. Configured virtualization settings

<img width="1920" height="1020" alt="vmware_iR9x01c8R5" src="https://github.com/user-attachments/assets/6bc8495a-0266-4d92-a939-c63f3eb84d46" />

<img width="240" height="852" alt="vmware_BnTZhdI15O" src="https://github.com/user-attachments/assets/f8a4dc90-c7f7-443d-8bab-51d947daa310" />

### Virtual Machine List

```
VM 1: DC01          - Windows Server 2022
VM 2: PC01          - Windows 10 Pro
VM 3: PC02          - Windows 11 Enterprise
VM 4: Ubuntu        - Ubuntu Desktop 24.04
VM 5: Kali Linux    - Kali 2026.1
VM 6: Metasploitable2 - Linux vulnerable machine
```

### Network Configuration

All VMs use **NAT network** in VMware:

```
NAT Network Benefits:
- VMs share host laptop internet connection
- VMs can communicate with each other
- No dependency on external WiFi/hotspot
- Stable and reliable for lab environment

Gateway for NAT: 192.168.160.2
```

### VM Hardware Allocation

| VM | RAM | Storage | CPU |
|----|-----|---------|-----|
| DC01 | 4GB | 60GB | 2 |
| PC01 | 4GB | 60GB | 2 |
| PC02 | 4GB | 64GB | 2 |
| Ubuntu (Splunk) | 4GB | 45GB | 2 |
| Kali Linux | 2GB | 80GB | 2 |
| Metasploitable2 | 1GB | 20GB | 1 |

### VMware Tools

VMware Tools was installed on all Windows and Ubuntu VMs to enable:
- Copy paste between host and VM
- Auto screen resize
- Smooth mouse movement
- File drag and drop

**Installation Command for Ubuntu:**
```bash
sudo apt-get install open-vm-tools-desktop -y
```

<img width="391" height="320" alt="RmCld84d1k" src="https://github.com/user-attachments/assets/7805b0c3-c190-4a24-8d22-3b182ce98536" />

---

## 5. Phase 2 — Domain Controller DC01

### What Is A Domain Controller?

A Domain Controller is the most important server in a Windows
network. It controls who can log in, what they can access, and
manages all computers in the domain.

```
Real World Example:
Domain Controller = School Principal's office
- Controls who enters which rooms
- Manages all student and teacher accounts
- Sets rules for entire school network
```

### Windows Server 2022 Installation

**ISO Downloaded From:**
```
https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
```

**Important Note:** Downloaded x64 version (NOT ARM64)

**Installation Steps:**
1. Created new VM in VMware named DC01
2. Attached Windows Server 2022 ISO
3. Selected "Windows Server 2022 Standard Evaluation (Desktop Experience)"
4. Chose Custom installation
5. Set Administrator password: Admin@12345
6. Installed VMware Guest Additions

<img width="640" height="480" alt="image" src="https://github.com/user-attachments/assets/13abb4b2-33a7-4795-bd5b-fccc5f8971f4" />

[Insert Screenshot: DC01 Desktop After Installation]

### Rename Server to DC01

**Steps:**
1. Right click Start → System
2. Click "Rename this PC"
3. Changed name to: DC01
4. Restarted server

### Static IP Configuration

DC01 must have a permanent IP address that never changes.

**Network Settings Applied:**
```
IP Address:      192.168.160.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.160.2
Preferred DNS:   192.168.160.10 (itself)
Alternate DNS:   8.8.8.8
```

**Why Static IP?**
```
DHCP gives random IPs that change
Static IP stays the same forever
Servers always need static IPs so
other computers can always find them
```

**How To Set Static IP:**
1. Right click network icon → Open Network Settings
2. Change adapter options
3. Right click Ethernet → Properties
4. Double click IPv4
5. Select "Use the following IP address"
6. Enter values above
7. Click OK

[Insert Screenshot: DC01 Static IP Configuration]

**Verification Command:**
```cmd
ipconfig /all
```

[Insert Screenshot: DC01 ipconfig showing 192.168.160.10]

---

## 6. Phase 3 — Active Directory

### What Is Active Directory?

Active Directory Domain Services (AD DS) is Microsoft's directory
service that manages users, computers, and resources in a network.

```
Real World Example:
Active Directory = Company HR Department
- Creates and manages employee accounts
- Controls what each employee can access
- Organizes employees into departments (OUs)
- Applies company policies to all computers
```

### Installing Active Directory Domain Services

**Steps:**
1. Opened Server Manager on DC01
2. Clicked "Add Roles and Features"
3. Selected "Role-based or feature-based installation"
4. Selected DC01 as destination server
5. Checked "Active Directory Domain Services"
6. Clicked "Add Features" on popup
7. Clicked Next through remaining screens
8. Clicked Install

**Important Warning:**
```
⚠️ AD DS and AD CS are DIFFERENT:
AD DS = Active Directory Domain Services ✅ (what we need)
AD CS = Active Directory Certificate Services ❌ (not needed)
These have similar names but completely different purposes!
```

[Insert Screenshot: Server Manager Showing AD DS Installation]

### Promoting Server to Domain Controller

After AD DS installed, clicked yellow flag and selected
"Promote this server to a domain controller"

**Settings Used:**
```
Deployment: Add a new forest
Root Domain Name: securecorp.local
DSRM Password: Admin@12345
Forest/Domain Functional Level: Default
```

[Insert Screenshot: Domain Controller Promotion Wizard]

[Insert Screenshot: Login Screen Showing SECURECORP\Administrator]

### Verification

After restart, verified domain controller status:

```cmd
whoami
```
Result: `securecorp\administrator` ✅

```cmd
dcdiag /summary
```
Result: All tests passed ✅

[Insert Screenshot: whoami showing securecorp\administrator]

### Server Manager After AD DS

After successful installation Server Manager shows:
- AD DS ✅
- DNS ✅
- File and Storage Services ✅
- Local Server ✅
- All Servers ✅

[Insert Screenshot: Server Manager Left Panel with AD DS and DNS]

---

## 7. Phase 4 — DNS and DHCP

### DNS Server

#### What Is DNS?

```
DNS = Domain Name System

Think of it like a phone book:
Instead of remembering phone numbers
you look up names to find numbers

DNS translates:
"dc01.securecorp.local"
into
"192.168.160.10"
```

DNS was automatically installed when AD DS was installed. The DNS
server on DC01 handles all name resolution for securecorp.local domain.

**Verification:**
```cmd
nslookup securecorp.local
```
Result:
```
Server: dc01.securecorp.local
Address: 192.168.160.10
Name: securecorp.local
Address: 192.168.160.10 ✅
```

[Insert Screenshot: nslookup showing securecorp.local resolving correctly]

### DHCP Server

#### What Is DHCP?

```
DHCP = Dynamic Host Configuration Protocol

Think of it like a hotel receptionist:
- Guest arrives (computer joins network)
- Receptionist assigns room number (IP address)
- Guest leaves (IP address returned to pool)
- Room number given to next guest
```

#### Installing DHCP

**Steps:**
1. Server Manager → Add Roles and Features
2. Selected "DHCP Server"
3. Clicked Add Features → Next → Install
4. After install clicked yellow flag
5. Clicked "Complete DHCP Configuration"
6. Clicked Commit → Close

#### Configuring DHCP Scope

A scope defines the range of IP addresses DHCP can assign.

**Scope Settings:**
```
Scope Name:    SecHomeLab_Scope
Start IP:      192.168.160.100
End IP:        192.168.160.200
Subnet Mask:   255.255.255.0
Gateway:       192.168.160.2
DNS Server:    192.168.160.10
Lease Duration: 8 days
```

[Insert Screenshot: DHCP Manager Showing SecHomeLab_Scope]

[Insert Screenshot: DHCP and DNS Running in Server Manager]

**Verification:**
```
DHCP Panel → IPv4 → Green tick ✅
Scope [192.168.160.0] SecHomeLab_Scope ✅
```

---

## 8. Phase 5 — Workstations PC01 and PC02

### PC01 — Windows 10 Pro

#### Why Windows 10 Pro?

```
Windows 10 Home = Cannot join domain ❌
Windows 10 Pro = Can join domain ✅

Pro version supports:
- Active Directory domain joining
- Group Policy
- Remote Desktop
- BitLocker encryption
```

#### Installation Steps

1. Downloaded Windows 10 x64 ISO from Microsoft
2. Created new VM named PC01 in VMware
3. Set hardware: 4GB RAM, 60GB disk, 2 CPUs
4. Set network: NAT adapter
5. Installed Windows 10 Pro
6. During setup selected "Set up for an organization"
7. Selected "Domain join instead"
8. Created local account: localuser / Admin@12345

[Insert Screenshot: PC01 Windows 10 Pro Desktop]

#### Static IP for PC01

```
IP Address:      192.168.160.20
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.160.2
Preferred DNS:   192.168.160.10
Alternate DNS:   8.8.8.8
```

[Insert Screenshot: PC01 Network Configuration]

#### Joining PC01 to Domain

**Steps:**
1. Right click Start → System
2. Click "Rename this PC (Advanced)"
3. Click Change
4. Computer Name: PC01
5. Select Domain: securecorp.local
6. Credentials: Administrator / Admin@12345
7. Got "Welcome to securecorp.local domain!" message
8. Restarted PC01

**Verification:**
```cmd
whoami
```
Result: `securecorp\administrator` ✅

```cmd
systeminfo | findstr /B /C:"Domain"
```
Result: `Domain: securecorp.local` ✅

[Insert Screenshot: PC01 whoami showing securecorp\administrator]

[Insert Screenshot: PC01 systeminfo showing Domain: securecorp.local]

### PC02 — Windows 11 Enterprise

#### Installation Steps

1. Downloaded Windows 11 x64 Enterprise Evaluation ISO
2. Created new VM named PC02 in VMware
3. Set hardware: 4GB RAM, 64GB disk, 2 CPUs
4. Enabled UEFI and TPM 2.0 in VM settings
5. Set network: NAT adapter
6. Installed Windows 11 Enterprise

**Important Note:** Must download x64 ISO NOT ARM64!

```
ARM64 = For Apple Silicon / mobile processors ❌
x64   = For Intel/AMD processors (your laptop) ✅
```

[Insert Screenshot: PC02 Windows 11 Desktop]

#### Static IP for PC02

```
IP Address:      192.168.160.30
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.160.2
Preferred DNS:   192.168.160.10
Alternate DNS:   8.8.8.8
```

#### Joining PC02 to Domain

Same process as PC01:
1. Renamed computer to PC02
2. Joined securecorp.local domain
3. Used Administrator credentials
4. Restarted

[Insert Screenshot: PC02 whoami showing securecorp\administrator]

---

## 9. Phase 6 — Splunk SIEM

### What Is Splunk?

```
Splunk = Security Camera System for your network

Just like CCTV cameras record everything:
- Who entered the building
- What they did inside
- When they left
- Any suspicious activity

Splunk records everything on your network:
- Who logged in
- What programs ran
- What files were created
- Any failed login attempts
- Network connections made
```

### Why Splunk Is Important

```
85% of SOC Analyst job postings require SIEM experience
Splunk is the most widely used SIEM platform
Knowing Splunk = Major advantage in job market
```

### Ubuntu Desktop Installation (Splunk Server)

**ISO Downloaded From:**
```
https://ubuntu.com/download/desktop
Version: Ubuntu 24.04 LTS
```

**VM Settings:**
```
RAM:     4096 MB (4GB)
Storage: 45GB
CPU:     2 processors
Network: NAT
```

**Account Created:**
```
Name:     SOC Admin
Username: socadmin
Password: Admin@12345
```

[Insert Screenshot: Ubuntu Desktop After Installation]

### Splunk Enterprise Installation

**Downloaded From:**
```
https://www.splunk.com/en_us/download/splunk-enterprise.html
Package: Linux .deb file
```

**Installation Commands:**
```bash
# Navigate to downloads
cd ~/Downloads

# Install Splunk
sudo dpkg -i splunk-*.deb

# Start Splunk and accept license
sudo /opt/splunk/bin/splunk start --accept-license

# Set to start automatically on boot
sudo /opt/splunk/bin/splunk enable boot-start
```

**Admin Credentials Created:**
```
Username: admin
Password: Admin@12345
```

**Accessing Splunk Dashboard:**
```
Open Firefox on Ubuntu
Go to: http://localhost:8000
Login: admin / Admin@12345
```

[Insert Screenshot: Splunk Enterprise Login Page]

[Insert Screenshot: Splunk Enterprise Dashboard - Hello Administrator]

### Configure Splunk to Receive Logs

**Steps:**
1. Splunk Dashboard → Settings
2. Forwarding and Receiving
3. Configure Receiving
4. New Receiving Port: 9997
5. Save

```
Port 9997 = Standard Splunk receiving port
All forwarders send logs to this port
```

[Insert Screenshot: Splunk Receiving Port 9997 Configuration]

### Splunk Universal Forwarder on DC01

The Universal Forwarder is a lightweight agent installed on Windows
machines to send their logs to the Splunk server.

**Downloaded From:**
```
https://www.splunk.com/en_us/download/universal-forwarder.html
Package: Windows 64-bit MSI
```

**Installation Settings:**
```
Type: On-premises Splunk Enterprise instance
Username: admin
Password: Admin@12345
Receiving Indexer Host: 192.168.160.x (Ubuntu IP)
Receiving Indexer Port: 9997
```

**Created inputs.conf on DC01:**

File location:
```
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```

File contents:
```ini
[WinEventLog://Security]
disabled = 0
index = main
renderXml = false

[WinEventLog://System]
disabled = 0
index = main
renderXml = false

[WinEventLog://Application]
disabled = 0
index = main
renderXml = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main
renderXml = false
```

**Restarted Forwarder:**
```cmd
cd "C:\Program Files\SplunkUniversalForwarder\bin"
splunk restart
```

**Verified Forwarder Running:**
```cmd
sc query SplunkForwarder
```
Result: `STATE: RUNNING` ✅

[Insert Screenshot: SplunkForwarder Service Running on DC01]

### Splunk Universal Forwarder on PC01

Same installation process as DC01 with same inputs.conf file.

[Insert Screenshot: SplunkForwarder Service Running on PC01]

### Verifying Logs in Splunk

**Search Queries Used:**

All DC01 logs:
```
index=main host=DC01
```

All PC01 logs:
```
index=main host=PC01
```

Count by host:
```
index=main | stats count by host
```

[Insert Screenshot: Splunk Search Showing DC01 Logs]

[Insert Screenshot: Splunk Search Showing Both DC01 and PC01 Hosts]

---

## 10. Phase 7 — Sysmon Integration

### What Is Sysmon?

```
Default Windows Logging:
"A user logged in" (basic info only)

Sysmon Logging:
"User john logged in at 14:32:05
from IP 192.168.160.50
using program explorer.exe
which was started by winlogon.exe
and made network connection to 8.8.8.8
on port 443"

Sysmon = Much more detailed! ✅
```

### Sysmon Event IDs

| Event ID | What It Logs | Why Important |
|----------|-------------|--------------|
| 1 | Process created | Detect malware running |
| 2 | File creation time changed | Detect file tampering |
| 3 | Network connection | Detect C2 communication |
| 5 | Process terminated | Track execution |
| 7 | Image/DLL loaded | Detect DLL hijacking |
| 8 | Remote thread created | Detect injection |
| 10 | Process accessed | Detect credential theft |
| 11 | File created | Detect dropped files |
| 12 | Registry event | Detect persistence |
| 13 | Registry value set | Detect config changes |
| 22 | DNS query | Detect suspicious domains |

### Installing Sysmon on DC01 and PC01

**Downloaded From:**
```
https://download.sysinternals.com/files/Sysmon.zip
```

**Config File Downloaded From:**
```
https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml
```

**Installation Command (CMD as Administrator):**
```cmd
sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

**Verification:**
```cmd
sc query sysmon64
```
Result: `STATE: RUNNING` ✅

[Insert Screenshot: Sysmon Installed and Running on DC01]

[Insert Screenshot: Sysmon Events Appearing in Splunk]

### Updated inputs.conf with Sysmon

Added this section to inputs.conf on both DC01 and PC01:
```ini
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = main
renderXml = false
```

**Search for Sysmon events in Splunk:**
```
index=main source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

---

## 11. Phase 8 — Attack Simulation

### Lab Attack Scenario

```
🔴 Attacker: Kali Linux
🖥️ Victim:   PC01 (Windows 10 Pro)
🎥 Monitor:  Splunk on Ubuntu
🔍 Analyst:  Shahid (You!)
```

### Kali Linux Setup

Kali Linux was already pre-installed in VMware. Login credentials:
```
Username: kali
Password: kali
```

[Insert Screenshot: Kali Linux Desktop]

### Attack 1 — Network Scan with Nmap

**What Is Nmap?**
```
Nmap = Network Mapper
Used by both attackers and defenders
Attackers use it to find open ports
SOC analysts use it to understand network
```

**Command Used on Kali:**
```bash
nmap -sV -O (PC01 IP address)
```

**What This Does:**
```
-sV = Detect service versions on open ports
-O  = Detect operating system
Scans all common ports on PC01
Shows what services are running
```

[Insert Screenshot: Nmap Scan Running on Kali Linux]

### Attack 2 — Brute Force with Hydra

**What Is Hydra?**
```
Hydra = Password cracking tool
Tries many passwords automatically
Simulates real brute force attack
```

**Command Used:**
```bash
hydra -l administrator -P /usr/share/wordlists/rockyou.txt smb://(PC01 IP)
```

**What This Does:**
```
-l administrator = Target this username
-P rockyou.txt   = Use this password list
smb://           = Attack Windows file sharing
Tries thousands of passwords automatically
```

[Insert Screenshot: Hydra Brute Force Attack Running]

### Detecting Attack in Splunk

**Key Event Codes for Detection:**

| EventCode | Meaning | Detection |
|-----------|---------|-----------|
| 4625 | Failed login | Brute force attempt |
| 4624 | Successful login | Check after failed logins |
| 4648 | Login with credentials | Suspicious auth |
| 5156 | Network connection allowed | Port scan traffic |

**Search for Failed Logins (EventCode 4625):**
```
index=main host=PC01 EventCode=4625
```

**What Was Found:**
```
EventCode:      4625 ✅
Failure Reason: Unknown user name or bad password ✅
Account Name:   administrator ✅
Logon Type:     3 (Network logon) ✅
Logon Process:  NtLmSsp ✅
Workstation:    PC01 ✅
LogName:        Security ✅
Keywords:       Audit Failure ✅
```

[Insert Screenshot: EventCode 4625 Detected in Splunk]

[Insert Screenshot: Full Event Details Showing Attack Evidence]

**Count Failed Login Attempts:**
```
index=main EventCode=4625
| stats count by Account_Name, host
| sort -count
```

[Insert Screenshot: Failed Login Count by Account]

**Build Attack Timeline:**
```
index=main EventCode=4625
| table _time host Account_Name Failure_Reason
| sort _time
```

[Insert Screenshot: Attack Timeline in Splunk]

---

## 12. Phase 9 — SOC Dashboard

### What Is a SOC Dashboard?

```
SOC Dashboard = Single screen showing
all security status at a glance

Like a pilot's cockpit:
- All important information visible
- Alerts shown immediately
- Easy to monitor everything
- Quick to spot problems
```

### Dashboard Created: "SOC Monitoring Dashboard"

**8 Panels Created:**

#### Panel 1 — Failed Login Attempts
```
Title: 🔴 Failed Login Attempts
Type: Line Chart
Search:
index=main EventCode=4625
| timechart span=1h count as "Failed Logins" by host
```

[Insert Screenshot: Failed Login Attempts Panel]

#### Panel 2 — Successful Logins
```
Title: 🟢 Successful Logins
Type: Bar Chart
Search:
index=main EventCode=4624
| stats count as "Login Count" by Account_Name, host
| sort -"Login Count"
```

[Insert Screenshot: Successful Logins Panel]

#### Panel 3 — New Processes Created
```
Title: ⚡ New Processes Created
Type: Table
Search:
index=main EventCode=1
| table _time host User Image CommandLine
| sort -_time
```

[Insert Screenshot: New Processes Panel]

#### Panel 4 — Network Connections
```
Title: 🌐 Network Connections
Type: Table
Search:
index=main EventCode=3
| table _time host SourceIp DestinationIp DestinationPort Image
| sort -_time
```

[Insert Screenshot: Network Connections Panel]

#### Panel 5 — Top Targeted Accounts
```
Title: 🎯 Top Targeted Accounts
Type: Pie Chart
Search:
index=main EventCode=4625
| stats count as "Attack Count" by Account_Name
| sort -"Attack Count"
| head 10
```

[Insert Screenshot: Top Targeted Accounts Panel]

#### Panel 6 — DNS Queries Monitor
```
Title: 🔍 DNS Queries Monitor
Type: Table
Search:
index=main EventCode=22
| table _time host User QueryName QueryResults
| sort -_time
```

[Insert Screenshot: DNS Queries Panel]

#### Panel 7 — Security Events Timeline
```
Title: 📊 Security Events Timeline
Type: Area Chart
Search:
index=main
| eval EventType=case(
  EventCode=4625,"Failed Login",
  EventCode=4624,"Successful Login",
  EventCode=1,"New Process",
  EventCode=3,"Network Connection",
  EventCode=4720,"New User Created",
  true(),"Other")
| timechart span=1h count by EventType
```

[Insert Screenshot: Security Events Timeline Panel]

#### Panel 8 — Critical Security Events
```
Title: 🚨 Critical Security Events
Type: Table
Search:
index=main
(EventCode=4720 OR EventCode=4728
OR EventCode=4732 OR EventCode=1102)
| table _time host EventCode Message
| sort -_time
```

[Insert Screenshot: Critical Security Events Panel]

### Full Dashboard View

[Insert Screenshot: Complete SOC Dashboard All 8 Panels Visible]

### Alert Rules Created

#### Alert 1 — Brute Force Detection
```
Title:    🚨 Brute Force Attack Detected
Search:
  index=main EventCode=4625
  | stats count by Account_Name
  | where count > 5
Schedule: Every 5 minutes
Trigger:  Number of results > 0
Severity: High
```

#### Alert 2 — New Admin Account Created
```
Title:    🚨 New Admin Account Created
Search:
  index=main EventCode=4720 OR EventCode=4728
Schedule: Every 1 hour
Severity: Critical
```

#### Alert 3 — Suspicious PowerShell
```
Title:    🚨 Suspicious PowerShell Detected
Search:
  index=main EventCode=1
  Image="*powershell*"
Schedule: Real time
Severity: High
```

[Insert Screenshot: Alert Rules List in Splunk]

---

## 13. Phase 10 — Incident Response

### Incident Report #001

```
INCIDENT REPORT
================
Report Number:  IR-001
Date:           July 2026
Analyst:        Shahid
Severity:       High
Status:         Resolved
```

#### 1. Incident Summary

A brute force attack was detected against PC01 workstation
in the securecorp.local domain. Multiple failed authentication
attempts were identified targeting the Administrator account
via network logon. The attack originated from Kali Linux
machine in the lab environment.

#### 2. Detection Method

```
Tool:         Splunk Enterprise SIEM
Search Query: index=main host=PC01 EventCode=4625
Detection:    Multiple failed logins within short timeframe
Alert:        Brute Force Detection rule triggered
```

#### 3. Evidence Collected

```
EventCode:      4625 (Failed Login)
Account Name:   Administrator
Failure Reason: Unknown user name or bad password
Logon Type:     3 (Network)
Logon Process:  NtLmSsp
Workstation:    PC01
Source:         WinEventLog:Security
```

[Insert Screenshot: Splunk Evidence EventCode 4625]

#### 4. Attack Timeline

```
Time         Event
--------     -----
Start        Attacker begins nmap scan of PC01
+2 min       Port scan results obtained by attacker
+5 min       Hydra brute force attack begins
+5-10 min    Multiple failed logins (EventCode 4625)
Detected     Splunk alert fires after 5+ failures
Responded    Investigation begun by analyst
```

#### 5. Impact Assessment

```
Systems Affected: PC01 (192.168.160.20)
Data Compromised: None (attack failed)
Access Gained:    None
Services Disrupted: None
Severity:         High (attempted breach)
```

#### 6. MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|-----|-------------|
| Brute Force | T1110 | Password guessing attack |
| Network Service Scanning | T1046 | Nmap port scanning |
| Valid Accounts | T1078 | Targeting admin account |

#### 7. Recommendations

```
1. Implement account lockout policy
   (Lock after 5 failed attempts)

2. Enable multi-factor authentication
   for administrator accounts

3. Monitor EventCode 4625 continuously
   in Splunk with automated alerts

4. Block suspicious IP addresses
   at firewall level

5. Implement network segmentation
   to limit attacker movement

6. Use strong password policy
   (Minimum 14 characters, complexity required)
```

[Insert Screenshot: Full Incident Report Document]

### Snapshots Taken

After completing all configurations, snapshots were taken
of all VMs to preserve clean baseline state:

```
DC01:    DC01_Baseline_Clean
PC01:    PC01_Baseline_Clean
Ubuntu:  Ubuntu_Splunk_Baseline
Kali:    Kali_Baseline_Clean
```

**Why Snapshots Are Important:**
```
Before attack simulation: Take snapshot
Run attack and investigate
After done: Restore snapshot
Lab returns to clean state instantly!
```

[Insert Screenshot: VMware Snapshot Manager Showing All Snapshots]

---

## 14. Issues Encountered and Fixes

| Issue | Fix Applied |
|-------|------------|
| Network adapter conflicts with Jio 5G hotspot | Switched VMware network from Bridged to NAT mode. NAT allows VMs to share host laptop internet without mobile hotspot conflicts. For inter-VM communication used Internal Network mode for direct VM-to-VM connection |
| ARM64 vs x64 ISO mismatch | Downloaded incorrect Windows 11 ARM64 ISO designed for Apple Silicon. Host laptop uses Intel/AMD x64 architecture. Fixed by downloading correct x64 64-bit ISO from Microsoft official website. Always verify CPU architecture before downloading |
| AD DS vs AD CS confusion | Accidentally installed Active Directory Certificate Services instead of Domain Services. AD CS manages digital certificates while AD DS manages users and computers. Fixed by removing AD CS through Remove Roles wizard then correctly installing AD DS and promoting to Domain Controller |
| Windows 10 Home cannot join domain | Windows 10 Home edition does not support Active Directory domain joining. Rebuilt PC01 using Windows 10 Pro ISO which fully supports enterprise domain features |
| Splunk Universal Forwarder not sending logs | Manually created inputs.conf file in correct directory path. Verified port 9997 open on Splunk server. Confirmed correct Ubuntu IP in forwarder configuration. Restarted forwarder service after every configuration change |
| PC01 had no IP address initially | Set static IP manually through Network adapter IPv4 settings. Configured correct DNS pointing to DC01 at 192.168.160.10 |
| Domain join failing with credential error | Root cause was DNS not working on PC01. Fixed by ensuring preferred DNS server pointed to DC01 IP. Once DNS resolved correctly domain join succeeded immediately |
| VirtualBox boot failure (initial setup) | Initially used VirtualBox which had boot issues with Windows 11. Switched completely to VMware Workstation which handles Windows 11 requirements better |
| Black screen during Windows installation | Normal behavior during installation. Fixed by waiting and not interrupting. Also paused other VMs to free up RAM |

---

## 15. Lessons Learned

Building this SOC home lab from scratch taught me that real
cybersecurity work involves much more troubleshooting than
expected. Understanding WHY something fails is more valuable
than just fixing it. For example the ARM64 vs x64 ISO issue
taught me to always verify hardware architecture compatibility
before downloading software — a fundamental IT skill I will
carry throughout my career.

The biggest surprise was how powerful Splunk is even in a home
lab environment. I expected basic logs but was amazed to see
detailed attack evidence including exact timestamps, source IP
addresses, targeted accounts and failure reasons all captured
automatically through EventCode 4625. This showed me exactly
why SIEM tools are considered the heart of every Security
Operations Center.

If I were starting this project over I would take VM snapshots
before making any major configuration change. Several times I
had to rebuild machines from scratch because a misconfiguration
broke everything. Taking snapshots would have saved many hours
and taught me early the critical importance of change management
in real SOC environments — something every professional
environment requires.

Most importantly this project completely changed how I think
about cybersecurity. When I simulated a brute force attack from
Kali Linux and immediately saw the failed login events appear
in Splunk dashboard with full details of the attack it clicked
completely — this is exactly what SOC analysts do every single
day to protect organizations from real threats.

---

## 16. Interview Preparation

### Key Questions and Your Answers

#### Q1: Tell me about your home lab

```
"I built a complete SOC home lab using VMware Workstation
with 6 virtual machines including a Windows Server 2022
Domain Controller, two Windows workstations running Windows
10 Pro and Windows 11, an Ubuntu server running Splunk
Enterprise SIEM, Kali Linux for attack simulation and
Metasploitable2 as a vulnerable target. The entire lab
runs on my personal laptop."
```

#### Q2: What is Active Directory and how did you use it?

```
"Active Directory is Microsoft's directory service that
manages users, computers, and resources in a network.
I configured a full AD environment with a domain called
securecorp.local, set up DNS and DHCP services, and
joined both workstations to the domain. This gave me
hands-on experience with the same technology used in
most enterprise environments."
```

#### Q3: What is Splunk and how did you configure it?

```
"Splunk is a SIEM platform that collects and analyzes
security logs in real time. I installed Splunk Enterprise
on Ubuntu, configured Universal Forwarders on DC01 and
PC01 to send Windows Event Logs, integrated Sysmon for
detailed process and network monitoring, built an 8-panel
SOC dashboard covering failed logins, successful logins,
new processes, network connections and DNS queries, and
created alert rules for brute force detection and
suspicious activity."
```

#### Q4: How did you detect the brute force attack?

```
"I simulated a brute force attack using Hydra on Kali
Linux targeting PC01 with the administrator account.
I detected it in Splunk by searching for EventCode 4625
which represents failed Windows login attempts. I found
multiple failures in a short timeframe from the same
source targeting the administrator account via network
logon Type 3. I then created a Splunk alert rule that
triggers when more than 5 failed logins occur within
5 minutes, which is standard brute force detection
methodology in real SOC environments."
```

#### Q5: What is Sysmon and why did you use it?

```
"Sysmon is Microsoft's System Monitor tool that provides
much more detailed Windows event logging than default
Windows logging. I used it because default Windows logs
only show basic information while Sysmon provides detailed
data about process creation with full command lines,
network connections with source and destination IPs,
DNS queries, registry changes and file creation events.
I configured it with the SwiftOnSecurity Sysmon config
which is an industry-standard baseline configuration
used by many security teams."
```

#### Q6: What challenges did you face?

```
"The most significant challenge was getting the VMs to
communicate with each other while using a mobile hotspot
for internet. I solved this by using VMware's NAT network
mode which allows VMs to share the host internet connection
without conflicts. I also initially downloaded the wrong
Windows 11 ISO — the ARM64 version instead of x64 — which
caused boot failures. These troubleshooting experiences
actually taught me more than the successful configurations
because I had to understand the underlying technology to
fix the problems."
```

#### Q7: What Event Codes do you know?

```
EventCode 4624 = Successful login
EventCode 4625 = Failed login (brute force detection)
EventCode 4688 = New process created
EventCode 4720 = New user account created
EventCode 4728 = User added to security group
EventCode 4732 = User added to local group
EventCode 1102 = Audit log cleared (suspicious!)
Sysmon ID 1    = Process creation with full details
Sysmon ID 3    = Network connection with IPs and ports
Sysmon ID 22   = DNS query
```

#### Q8: What would you do differently?

```
"I would take VM snapshots before every major configuration
change to avoid rebuilding from scratch when something
goes wrong. I would also document every step as I go rather
than trying to remember everything later. These are actually
real SOC practices — change management and documentation
are critical in professional security operations."
```

### SPL (Splunk Search Language) Queries to Remember

```
# All events from DC01
index=main host=DC01

# Failed login events
index=main EventCode=4625

# Count failed logins by account
index=main EventCode=4625
| stats count by Account_Name
| sort -count

# Brute force detection
index=main EventCode=4625
| stats count by src_ip
| where count > 5

# Process creation with Sysmon
index=main EventCode=1
| table _time host User Image CommandLine

# Network connections
index=main EventCode=3
| table _time host SourceIp DestinationIp DestinationPort

# All events in last hour
index=main earliest=-1h

# Count events by type
index=main
| stats count by EventCode
| sort -count

# Timeline of events
index=main
| timechart span=1h count by host
```

---

## 17. Current Status and Next Steps

### What Is Completed ✅

```
✅ VMware Workstation installed and configured
✅ DC01 - Windows Server 2022 installed
✅ Active Directory Domain Services configured
✅ Domain securecorp.local created
✅ DNS Server configured and working
✅ DHCP Server configured with scope
✅ PC01 - Windows 10 Pro installed and domain joined
✅ PC02 - Windows 11 Enterprise installed
✅ Ubuntu Desktop installed with Splunk Enterprise
✅ Splunk receiving port 9997 configured
✅ Universal Forwarder installed on DC01
✅ Universal Forwarder installed on PC01
✅ inputs.conf configured for Security/System/Application logs
✅ Sysmon installed on DC01 and PC01
✅ Sysmon logs flowing into Splunk
✅ Attack simulation with Kali Linux (nmap + hydra)
✅ EventCode 4625 brute force detected in Splunk
✅ 8-panel SOC dashboard created
✅ Alert rules created (brute force, new admin, PowerShell)
✅ Incident Report IR-001 written
✅ VM snapshots taken for all machines
✅ Screenshots taken for portfolio
```

### What Is Pending ⏳

```
⏳ PC02 Windows 11 fully joined to domain
⏳ Splunk Forwarder installed on PC02
⏳ GitHub repository fully populated with screenshots
⏳ README files written for each project folder
⏳ LinkedIn profile updated with project
⏳ Log Analysis project documentation
⏳ Incident Response Playbook written
```

### Immediate Next Steps

```
Step 1: Complete PC02 domain join
        - Verify IP 192.168.160.30 working
        - Join securecorp.local
        - Verify whoami shows securecorp\administrator

Step 2: Install Splunk Forwarder on PC02
        - Same process as DC01 and PC01
        - Verify PC02 logs appearing in Splunk

Step 3: Upload everything to GitHub
        - Create SOC-Home-Lab-Portfolio repository
        - Upload all screenshots
        - Write README for each project

Step 4: Write Log Analysis project
        - Document 5 Splunk investigations
        - Show different search techniques
        - Add to GitHub portfolio

Step 5: Write Incident Response Playbook
        - Phishing incident playbook
        - Brute force incident playbook
        - Upload to GitHub
```

### GitHub Repository Structure

```
SOC-Home-Lab-Portfolio/
│
├── README.md (Main portfolio overview)
│
├── Project1_HomeLab/
│   ├── README.md
│   └── Screenshots/
│
├── Project2_Splunk_SIEM/
│   ├── README.md
│   ├── Screenshots/
│   │   ├── Dashboard/
│   │   └── Alerts/
│   └── configs/
│       ├── inputs.conf
│       └── sysmon-config.xml
│
├── Project3_Attack_Detection/
│   ├── README.md
│   ├── Screenshots/
│   └── Reports/
│       └── Incident_Report_IR001.md
│
└── Project4_Log_Analysis/
    ├── README.md
    ├── Screenshots/
    └── Investigations/
```

---

## Quick Reference — Important Values

```
DOMAIN:
Domain Name:     securecorp.local
Admin User:      Administrator
Admin Password:  Admin@12345
DSRM Password:   Admin@12345

DC01:
IP Address:      192.168.160.10
OS:              Windows Server 2022
Role:            Domain Controller + DNS + DHCP

PC01:
IP Address:      192.168.160.20
OS:              Windows 10 Pro
Role:            Domain workstation

PC02:
IP Address:      192.168.160.30
OS:              Windows 11 Enterprise
Role:            Domain workstation

UBUNTU (SPLUNK):
IP Address:      192.168.160.x (check with ip addr show)
OS:              Ubuntu Desktop 24.04 LTS
Splunk URL:      http://localhost:8000
Splunk User:     admin
Splunk Pass:     Admin@12345
Splunk Port:     9997

KALI LINUX:
User:            kali
Pass:            kali
Role:            Attack simulation

NETWORK:
Range:           192.168.160.0/24
Gateway:         192.168.160.2
DNS:             192.168.160.10
DHCP Range:      192.168.160.100-200
```

---

*Notes Created: July 2026*
*Project: SOC Home Lab Portfolio*
*Analyst: Shahid*
*Status: In Progress — 85% Complete*
