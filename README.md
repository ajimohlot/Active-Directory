
# Windows Server 2022 Active Directory Home Lab


## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Lab Network Diagram](#lab-network-diagram)
- [Lab Environment](#lab-environment)
- [Project Objectives](#project-objectives)
- [Project Implementation](#project-implementation)
  - [1. Hyper-V and Virtual Machine Setup](#1-hyper-v-and-virtual-machine-setup)
  - [2. Network Configuration and Connectivity Troubleshooting](#2-network-configuration-and-connectivity-troubleshooting)
  - [3. Active Directory Domain Services Deployment](#3-active-directory-domain-services-deployment)
  - [4. DNS Troubleshooting and Resolution](#4-dns-troubleshooting-and-resolution)
- [Troubleshooting Summary](#troubleshooting-summary)
- [Skills Demonstrated](#skills-demonstrated)
- [Tools and Technologies](#tools-and-technologies)
- [Project Outcome](#project-outcome)
- [Known Limitations / Follow-Up Items](#known-limitations--follow-up-items)
- [Lessons Learned](#lessons-learned)
- [Project Demonstration](#project-demonstration)

## Lab Network Diagram


              ┌─────────────────────────────┐
              │       Windows 11 Host       │
              │  (Lab Management Workstation)│
              │                             │
              │  IP: 192.168.10.x           │
              │                             │
              └──────────────┬──────────────┘
                             │
                             │
              ┌──────────────┴──────────────┐
              │ Hyper-V Internal Switch     │
              │ (Isolated Lab Network)      │
              │ 192.168.10.0/24             │
              └──────────────┬──────────────┘
                             │
                             │
              ┌──────────────┴──────────────┐
              │       DC01 (VM)             │
              │ Windows Server 2022         │
              │ Domain Controller           │
              │ ajimohlab.local             │
              │ IP: 192.168.10.10 /24       │
              │ DNS: 192.168.10.10          │
              │ Roles: AD DS · DNS          │
              └─────────────────────────────┘




## Project Overview

This project demonstrates the deployment and configuration of a Windows Server 2022 Active Directory home lab using Hyper-V.

The lab was built to demonstrate hands-on skills in Windows Server administration, Active Directory Domain Services (AD DS), DNS, network configuration, and systematic troubleshooting.

During the project, I configured a Windows Server 2022 virtual machine as a domain controller, assigned a static IP address, installed Active Directory Domain Services, created a new Active Directory forest, and configured DNS services.

I also encountered and resolved network connectivity and DNS-related issues, including ICMP communication problems, DNS query timeouts, and missing reverse DNS resolution.

## Lab Environment

- **Hypervisor:** Microsoft Hyper-V
- **Server OS:** Windows Server 2022
- **Domain Controller:** DC01
- **Domain:** ajimohlab.local
- **Server IP Address:** 192.168.10.10
- **Network:** Hyper-V Internal Virtual Switch
- **Server Roles:** Active Directory Domain Services (AD DS) and DNS Server

## Project Objectives

- Configure a virtualized Windows Server environment using Hyper-V
- Configure a static IPv4 address for the domain controller
- Install Active Directory Domain Services
- Promote Windows Server to a domain controller
- Create the `ajimohlab.local` Active Directory forest
- Configure and verify DNS
- Test network communication between the host and domain controller
- Diagnose and resolve firewall and network profile issues
- Configure DNS reverse lookup and a PTR record
- Validate the final Active Directory and DNS configuration

## Project Implementation

The project was completed in four main stages:

1. Hyper-V and virtual machine setup
2. Network configuration and connectivity testing
3. Active Directory Domain Services installation and domain controller promotion
4. DNS configuration, testing, and troubleshooting


## 1. Hyper-V and Virtual Machine Setup

I first verified that hardware virtualization was enabled on the host computer and that Hyper-V was available.

I then created the lab's internal virtual switch and configured a Windows Server 2022 virtual machine named `DC01`. The virtual machine was allocated the required memory, processor, storage, and network resources before Windows Server 2022 was installed.

### Key tasks completed- Verified virtualization support
- Enabled and configured Hyper-V
- Created an internal Hyper-V virtual switch
- Created the `DC01` virtual machine
- Configured VM memory, processor, and virtual disk
- Installed Windows Server 2022


### Hyper-V Setup Evidence

#### 1. Virtualization Verification

I verified that hardware virtualization was enabled on the host computer before configuring Hyper-V.

![Virtualization Enabled](screenshots/01%20hyperv%20setup/01-Virtualization-Enabled.jpg)

#### 2. Hyper-V Enabled

Hyper-V was enabled on the Windows host to provide the virtualization platform for the lab environment.

![Hyper-V Enabled](screenshots/01%20hyperv%20setup/02-Hyper-V-Enabled.jpg)

#### 3. DC01 Virtual Machine Creation

I created a new virtual machine named `DC01` to host Windows Server 2022 and later function as the domain controller.

![DC01 VM Name](screenshots/01%20hyperv%20setup/04-DC01-VM-Name.jpg)

#### 4. Internal Virtual Switch

I created an internal Hyper-V virtual switch to provide network communication between the host computer and the lab virtual machine while keeping the lab network controlled.

![LAB Internal Virtual Switch](screenshots/01%20hyperv%20setup/07-LAB-Internal-Virtual-Switch.jpg)

#### 5. DC01 Virtual Machine Configuration

The completed virtual machine configuration was reviewed before installing the server operating system.

![DC01 VM Summary](screenshots/01%20hyperv%20setup/11-DC01-VM-Summary.jpg)

#### 6. Windows Server 2022 Installation

Windows Server 2022 was selected as the operating system for `DC01`, providing the platform for Active Directory Domain Services and DNS.

![Windows Server 2022 Edition Selection](screenshots/01%20hyperv%20setup/13-Windows-Server-2022-Edition-Selection.jpg)


## 2. Network Configuration and Connectivity Troubleshooting

After creating the Windows Server 2022 virtual machine, I configured the lab network and assigned static IP addresses to establish communication between the Windows host and the domain controller.

During connectivity testing, I encountered ICMP communication failures in both directions. I investigated the Windows Firewall configuration, identified the relevant ICMP rules and profile settings, enabled the required rules, and verified successful connectivity.

### 1. Server Renamed to DC01

I renamed the Windows Server virtual machine to `DC01` in preparation for its role as the domain controller.

![Server renamed to DC01](screenshots/02%20network%20configuration/16-Rename-Server-DC01.jpg)

### 2. Host Network Configuration

I configured the host-side adapter for the internal Hyper-V network to provide communication between the Windows host and DC01.

![LAB Internal Host IP Configuration](screenshots/02%20network%20configuration/18-LAB-Internal-Host-IP-Configuration.jpg)

### 3. DC01 Static IP Configuration

I assigned a static IPv4 configuration to DC01 to provide a consistent network address for Active Directory and DNS services.

![DC01 Static IP Configuration](screenshots/02%20network%20configuration/19-DC01-Static-IP-Configuration.jpg)

### 4. IP Configuration Verification

I verified the network configuration on DC01 after assigning the static IP settings.

![DC01 IP Configuration Verification](screenshots/02%20network%20configuration/20-DC01-IPConfig-Verification.jpg)

### 5. Initial Host-to-DC01 Connectivity Failure

An initial ping test from the host computer to DC01 failed, indicating that further network troubleshooting was required.

![Host to DC01 Ping Failed](screenshots/02%20network%20configuration/21-Host-to-DC01-Ping-Failed.jpg)

### 6. DC01 Firewall Investigation

I inspected the ICMP firewall rules on DC01 to determine whether Windows Firewall was preventing ping traffic.

![DC01 ICMP Firewall Rules Before](screenshots/02%20network%20configuration/28-DC01-ICMP-Firewall-Rules-Before.jpg)

#### 7. DC01 ICMP Firewall Rule Enabled

I enabled the required ICMP firewall rule on DC01 to allow inbound echo requests.

![DC01 ICMP Firewall Rule Enabled](screenshots/02%20network%20configuration/29-DC01-ICMP-Firewall-Rule-Enabled.jpg)

#### 8. Host-to-DC01 Connectivity Restored

After modifying the firewall configuration, I repeated the ping test from the host and confirmed successful communication with DC01.

![Host to DC01 Ping Success](screenshots/02%20network%20configuration/30-Host-to-DC01-Ping-Success.jpg)

### 9. Reverse Connectivity Test Failed

I then tested connectivity in the opposite direction. The ping from DC01 to the host initially failed, showing that communication was not yet working bidirectionally.

![DC01 to Host Ping Failed](screenshots/02%20network%20configuration/31-DC01-to-Host-Ping-Failed.jpg)

### 10. Host Firewall Profile Investigation

Further investigation identified a firewall rule/profile mismatch on the Windows host.

![Host ICMP Rule Profile Mismatch](screenshots/02%20network%20configuration/32-Host-ICMP-Rule-Profile-Mismatch.jpg)

### 11. Host ICMP Firewall Rule Enabled

I enabled the appropriate ICMP firewall rule on the host to permit inbound echo requests from DC01.

![Host ICMP Firewall Rule Enabled](screenshots/02%20network%20configuration/33-Host-ICMP-Firewall-Rule-Enabled.jpg)

### 12. Bidirectional Connectivity Confirmed

A final ping test from DC01 to the host completed successfully, confirming bidirectional communication across the internal Hyper-V network.

![DC01 to Host Ping Successful](screenshots/02%20network%20configuration/34-DC01-to-Host-Ping-Successful.jpg)

## 13. Network Category Investigation

Although ICMP connectivity was now working, I investigated why the internal lab network 
was still classified as a **Public** network profile rather than **Private** — the profile 
you'd expect for an isolated, trusted internal switch.

I attempted to change the profile directly:

```powershell
Set-NetConnectionProfile -InterfaceAlias "vEthernet (Home Lab-Internal)" -NetworkCategory Private
```

![Network Profile Change Failed](screenshots/02%20network%20configuration/34B-Network-Profile-Change-Failed.jpg))

This failed with a permission-denied error:

> Set-NetConnectionProfile : Unable to set the NetworkCategory due to one of the following 
> possible reasons: not running PowerShell elevated; the NetworkCategory cannot be changed 
> from 'DomainAuthenticated'; user initiated changes to NetworkCategory are being prevented 
> due to the Group Policy setting 'Network List Manager Policies'.

**Root Cause:** a local/effective Group Policy setting (`Network List Manager Policies`) 
explicitly blocks manual changes to network category classification, overriding even an 
elevated PowerShell session.

![HomeLab Network Profile Verification](screenshots/02%20network%20configuration/35-HomeLab-Network-Profile-Verification.jpg)

I confirmed via `Get-NetConnectionProfile` that the category remained `Public` after the 
failed change attempt.

## 14. Working Within the Public Profile

Rather than modify the Group Policy setting at this stage of the project, I adjusted my 
firewall approach to explicitly target the **Public** profile — the profile the interface 
was actually running under — instead of assuming it would be Private or Domain.

![Host Public ICMP Rule Disabled]( screenshots/02%20network%20configuration/36-Host-Public-ICMP-Rule-Disabled.jpg)

This is a known limitation of the current setup: the internal lab network functions 
correctly, but remains classified as Public rather than Private/Domain. Resolving the 
underlying Group Policy restriction is listed as a follow-up item below.



## Active Directory Domain Services Deployment

With network connectivity established, I installed Active Directory Domain Services (AD DS) on DC01 and promoted the server to a domain controller.

I created a new Active Directory forest for the `ajimohlab.local` domain, completed the prerequisite checks, promoted DC01, and verified that the domain and supporting DNS infrastructure were functioning correctly.

### 1. Active Directory Domain Services Installation

I selected and installed the Active Directory Domain Services role on DC01 using Server Manager.

![AD DS Installation Confirmation](screenshots/03%20active%20directory/ADDS_03_Installation_Confirmation.jpg)

### 2. AD DS Installation Completed

The Active Directory Domain Services role was installed successfully, allowing the server to proceed to domain controller promotion.

![AD DS Installation Successful](screenshots/03%20active%20directory/ADDS_04_ADDS_Installation_Successful.jpg)

### 3. New Active Directory Forest Configuration

I selected the option to create a new forest and configured `ajimohlab.local` as the root domain for the lab environment.

![New Forest Configuration](screenshots/03%20active%20directory/ADDS_05_New_Forest_Configuration.jpg)

### 4. Domain Controller Configuration Review

Before beginning the promotion, I reviewed the Active Directory Domain Services configuration to confirm the selected deployment settings.

![AD DS Configuration Review](screenshots/03%20active%20directory/ADDS_08_Configuration_Review.jpg)

### 5. Prerequisite Check Passed

The prerequisite check completed successfully, confirming that DC01 was ready to be promoted to a domain controller.

![AD DS Prerequisites Check Passed](screenshots/03%20active%20directory/ADDS_09_Prerequisites_Check_Passed.jpg)

### 6. Domain Login Verification

After the server restarted, I successfully logged in using the `AJIMOHLAB` domain context, confirming that the domain controller promotion had completed.

![AJIMOHLAB Domain Login](screenshots/03%20active%20directory/ADDS_10_AJIMOHLAB_Domain_Login.jpg)

### 7. Domain Controller Verification

I verified that DC01 was operating as a domain controller within the newly created Active Directory domain.

![DC01 Domain Verification](screenshots/03%20active%20directory/ADDS_11_DC01_Domain_Verification.jpg)

### 8. Active Directory Users and Computers Verification

I opened Active Directory Users and Computers (ADUC) and confirmed that the domain structure was available and being managed by DC01.

![DC01 ADUC Verification](screenshots/03%20active%20directory/ADDS_12_DC01_ADUC_Verification.jpg)

### 9. DNS Forward Lookup Zone Verification

I verified that the DNS forward lookup zone associated with the Active Directory domain had been created successfully.

![DNS Forward Lookup Zone](screenshots/03%20active%20directory/ADDS_13_DNS_Forward_Lookup_Zone.jpg)

## DNS Troubleshooting and Resolution

After deploying Active Directory Domain Services, I tested DNS functionality to verify that name resolution was operating correctly.

The troubleshooting process involved checking client DNS configuration, DNS server settings, port 53 connectivity, explicit DNS queries, IPv4 and IPv6 resolution, reverse DNS configuration, and final domain controller diagnostics.

#### 1. Initial DNS Lookup Test

I performed an initial `nslookup` test to check DNS name resolution and determine whether the DNS server was responding correctly.

![Initial DNS Lookup Test](screenshots/04%20dns%20troubleshooting/DNS_02_NSLookup_Initial_Test.jpg)
**Why this timeout appears:** `nslookup` attempts to resolve the *DNS server's own hostname* 
via a reverse (PTR) lookup before displaying results — that's what populates the "Server:" 
line at the top of its output. At this point in the project, no reverse lookup zone existed 
yet, so that reverse lookup timed out, producing `Server: UnKnown` / `Address: ::1`. This is 
purely cosmetic: the actual forward query for `dc01.ajimohlab.local` still completed 
successfully against the same DNS server, which is why a correct answer appears immediately 
below the timeout. This symptom disappeared on its own once the reverse lookup zone and PTR 
record were created later in this project (see step 9-10).

#### 2. Client DNS Configuration Diagnosis

I reviewed the client's network and DNS configuration to confirm that it was using the correct DNS server.

![Client DNS Configuration Diagnosis](screenshots/04%20dns%20troubleshooting/DNS_03_Client_Configuration_Diagnosis.jpg)

#### 3. IPv6 DNS Lookup Timeout

Further testing with `nslookup` produced an IPv6-related timeout, indicating that additional DNS investigation was required.

![NSLookup IPv6 Timeout](screenshots/04%20dns%20troubleshooting/DNS_05_NSLookup_IPv6_Timeout.jpg)

#### 4. DNS Server Listening Addresses

I checked the DNS server listening configuration to verify that the DNS service was listening on the required network interfaces and addresses.

![DNS Server Listening Addresses](screenshots/04%20dns%20troubleshooting/DNS_06_Server_Listening_Addresses.jpg)

#### 5. DNS Port 53 Connectivity Test

I tested connectivity to DNS port 53 to confirm that the DNS server was reachable and that the service port was accessible.

![DNS Port 53 Connectivity Test](screenshots/04%20dns%20troubleshooting/DNS_07_Port_53_Connectivity_Test.jpg)

#### 6. Explicit DNS Query

I performed an explicit DNS query against the DNS server to isolate the DNS service from the client's default resolver configuration and verify that the server could answer the query directly.

![Explicit DNS Query](screenshots/04%20dns%20troubleshooting/DNS_08_Explicit_DNS_Query.png.jpg)

#### 7. IPv6 DNS Resolution Test

I performed an additional DNS resolution test involving IPv6 as part of the troubleshooting process and observed the DNS response.

![IPv6 DNS Resolution Test](screenshots/04%20dns%20troubleshooting/DNS_09_IPv6_DNS_Resolution_Test.png.jpg)

#### 8. Reverse DNS Lookup Failure

I then tested reverse DNS resolution. The lookup failed because a reverse lookup zone and corresponding PTR record had not yet been configured.

![Reverse DNS Lookup Failed](screenshots/04%20dns%20troubleshooting/DNS_10_Reverse_Lookup_Failed.jpg)

#### 9. Reverse Lookup Zone Created

I created a reverse lookup zone in DNS Manager to support IP-address-to-hostname resolution.

![Reverse Lookup Zone Created](screenshots/04%20dns%20troubleshooting/DNS_11_Reverse_Lookup_Zone_Created.jpg)

#### 10. PTR Record Created

I created the required Pointer (PTR) record to map the domain controller's IP address back to its hostname.

![PTR Record Created](screenshots/04%20dns%20troubleshooting/DNS_12_PTR_Record_Created.jpg)

#### 11. IPv4 DNS Resolution Verified

I repeated the `nslookup` test using IPv4 and confirmed that DNS resolution was working successfully.

![IPv4 DNS Resolution Success](screenshots/04%20dns%20troubleshooting/DNS_13_NSLookup_IPv4_Success.jpg)

#### 12. TCP and UDP Port 53 Verification

I verified DNS communication over both TCP and UDP port 53 to confirm that the required DNS transport protocols were functioning correctly.

![TCP UDP Port 53 Verification](screenshots/04%20dns%20troubleshooting/DNS_14_TCP_UDP_Port_53_Verification.png.jpg)

#### 13. Forward DNS Resolution Verified

I performed a final forward resolution test and confirmed that the domain controller's hostname resolved successfully to the expected IPv4 address.

![Forward DNS Resolution Successful](screenshots/04%20dns%20troubleshooting/DNS_15_Forward_Resolution_Successful.jpg)

#### 14. Final DNS Health Verification

Finally, I used `dcdiag` to perform DNS diagnostic testing on the domain controller. The DNS tests passed, providing final verification that the DNS service was functioning correctly.

![DCDiag DNS Passed](screenshots/04%20dns%20troubleshooting/DNS_16_DCDiag_DNS_Passed.jpg)

## Troubleshooting Summary

This project included several real troubleshooting scenarios that required systematic diagnosis rather than simply following the initial configuration steps.

### ICMP Connectivity Issue

**Problem:**  
The Windows host and DC01 were initially unable to communicate successfully using ICMP ping.

**Investigation:**  
I verified the IP configuration, tested connectivity in both directions, and reviewed Windows Firewall ICMP rules and their associated network profiles.

**Resolution:**  
I enabled the appropriate inbound ICMP firewall rules on both DC01 and the Windows host.

**Result:**  
Bidirectional communication between the host and DC01 was successfully established.

### DNS Resolution Issue

**Problem:**
DNS queries initially returned `Server: UnKnown` / `Address: ::1` with a timeout message, 
even though the actual name resolution succeeded.

**Investigation:**
I reviewed the client's DNS configuration, checked the DNS server's listening addresses, 
tested connectivity to port 53, and performed explicit queries with `resolve-dnsname` to 
separate the DNS *service* from `nslookup`'s own behavior.

**Root Cause:**
`nslookup` performs a reverse lookup of the DNS server's own address before displaying its 
output. Since no reverse lookup zone existed yet at this stage, that reverse query timed out — 
independent of whether the actual forward query succeeded. This made the DNS server look 
broken when it was actually functioning correctly for forward resolution.

**Resolution:**
I confirmed forward resolution was working correctly throughout using `resolve-dnsname`, then 
created the reverse lookup zone and PTR record (see below), which resolved the cosmetic 
`nslookup` timeout as a side effect.

**Result:**
Both forward and reverse DNS resolution now complete cleanly with no timeout messages.

### Reverse DNS Resolution Issue

**Problem:**  
Forward DNS resolution worked, but reverse lookup of the domain controller's IP address failed.

**Investigation:**  
I identified that a reverse lookup zone and PTR record had not yet been configured.

**Resolution:**  
I created the reverse lookup zone and corresponding PTR record.

**Result:**  
Reverse DNS resolution succeeded, and final `dcdiag` DNS diagnostic testing passed.

## Skills Demonstrated

- Windows Server 2022 administration
- Hyper-V configuration and virtual machine deployment
- Active Directory Domain Services installation and configuration
- Domain controller deployment
- Active Directory forest creation
- Active Directory Users and Computers administration
- IPv4 and static IP configuration
- Windows Firewall troubleshooting
- ICMP connectivity troubleshooting
- DNS configuration and troubleshooting
- Forward and reverse DNS resolution
- DNS reverse lookup zone configuration
- PTR record creation
- `ping`, `ipconfig`, `nslookup`, and `dcdiag` diagnostic tools
- TCP/UDP port 53 connectivity testing
- Systematic troubleshooting and verification

- ## Tools and Technologies

| Technology | Purpose |
|---|---|
| Windows 11 Pro | Host operating system |
| Hyper-V | Virtualization platform |
| Windows Server 2022 | Server operating system |
| Active Directory Domain Services | Identity and domain services |
| DNS Server | Name resolution |
| Windows Firewall | Network traffic and ICMP rule management |
| PowerShell | Configuration and troubleshooting |
| Command Prompt | Network and DNS diagnostic testing |
| Server Manager | Windows Server role and feature management |
| DNS Manager | Forward and reverse DNS administration |
| Active Directory Users and Computers | Active Directory administration |
| GitHub | Project documentation and evidence |

## Project Outcome

The completed home lab provides a functional Windows Server 2022 Active Directory environment running on Hyper-V.

By the end of the project:

- DC01 was successfully deployed as a Windows Server 2022 virtual machine.
- A static IPv4 network configuration was established.
- Bidirectional connectivity between the host and domain controller was verified.
- Active Directory Domain Services was successfully installed.
- DC01 was promoted to a domain controller.
- A new Active Directory forest and domain were created.
- Forward DNS resolution was verified.
- Reverse DNS resolution was configured using a reverse lookup zone and PTR record.
- Final DNS diagnostic testing completed successfully.

The project strengthened my practical experience in Windows Server administration, Active Directory, networking, DNS, firewall configuration, and structured troubleshooting.

## Known Limitations / Follow-Up Items

- The internal lab network remains classified under the **Public** firewall profile rather 
  than Private, due to a Group Policy restriction I did not resolve in this phase of the 
  project. Locating and adjusting the specific `Network List Manager Policies` setting is a 
  planned next step.

## Lessons Learned

This project reinforced the importance of using a structured troubleshooting process rather than making configuration changes without first identifying the cause of a problem.

The ICMP connectivity issue demonstrated how Windows Firewall rules and network profiles can affect communication even when IP addressing is configured correctly.

The DNS troubleshooting process also showed the importance of testing each layer separately. Verifying the DNS server's listening configuration, port 53 connectivity, forward resolution, reverse resolution, and final domain controller diagnostics made it possible to isolate and resolve each issue systematically.

Most importantly, this project gave me practical experience deploying, configuring, testing, and troubleshooting an Active Directory environment rather than only studying the concepts theoretically.

## Project Demonstration

I recorded the complete implementation and troubleshooting process as a four-part video series. The videos demonstrate the configuration steps, issues encountered, troubleshooting process, and final verification of the Windows Server 2022 Active Directory home lab.

### Part 1 – Hyper-V and Windows Server Setup

Covers virtualization verification, Hyper-V configuration, virtual networking, DC01 virtual machine creation, and Windows Server 2022 installation.

▶️ [Watch Part 1 on YouTube](https://youtu.be/XZWxVUwlBLY)

### Part 2 – Network and Firewall Troubleshooting

Covers static IP configuration, connectivity testing, ICMP failures, Windows Firewall investigation, firewall rule and network profile troubleshooting, and successful bidirectional communication.

▶️ [Watch Part 2 on YouTube](https://youtu.be/MHAAaAqRegg)

### Part 3 – Active Directory Domain Services Deployment

Covers AD DS installation, creation of the `ajimohlab.local` forest, domain controller promotion, domain verification, ADUC verification, and DNS integration.

▶️ [Watch Part 3 on YouTube](https://youtu.be/-jAFozUGix0)

### Part 4 – DNS Troubleshooting and Verification

Covers DNS testing and troubleshooting, DNS port 53 connectivity, explicit DNS queries, IPv4 and IPv6 testing, reverse lookup zone and PTR record configuration, forward and reverse resolution, and final DNS health verification.

▶️ [Watch Part 4 on YouTube](https://youtu.be/aSFSaEkqp-U)
