
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

### 1. Virtualization Verification

I verified that hardware virtualization was enabled on the host computer before configuring Hyper-V.

![Virtualization Enabled](screenshots/01%20hyperv%20setup/01-Virtualization-Enabled.jpg)

### 2. Hyper-V Enabled

Hyper-V was enabled on the Windows host to provide the virtualization platform for the lab environment.

![Hyper-V Enabled](screenshots/01%20hyperv%20setup/02-Hyper-V-Enabled.jpg)

### 3. DC01 Virtual Machine Creation

I created a new virtual machine named `DC01` to host Windows Server 2022 and later function as the domain controller.

![DC01 VM Name](screenshots/01%20hyperv%20setup/04-DC01-VM-Name.jpg)

### 4. Internal Virtual Switch

I created an internal Hyper-V virtual switch to provide network communication between the host computer and the lab virtual machine while keeping the lab network controlled.

![LAB Internal Virtual Switch](screenshots/01%20hyperv%20setup/07-LAB-Internal-Virtual-Switch.jpg)

### 5. DC01 Virtual Machine Configuration

The completed virtual machine configuration was reviewed before installing the server operating system.

![DC01 VM Summary](screenshots/01%20hyperv%20setup/11-DC01-VM-Summary.jpg)

### 6. Windows Server 2022 Installation

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

### 7. DC01 ICMP Firewall Rule Enabled

I enabled the required ICMP firewall rule on DC01 to allow inbound echo requests.

![DC01 ICMP Firewall Rule Enabled](screenshots/02%20network%20configuration/29-DC01-ICMP-Firewall-Rule-Enabled.jpg)

### 8. Host-to-DC01 Connectivity Restored

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

### 13. Network Category Investigation

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

### 14. Working Within the Public Profile

Rather than modify the Group Policy setting at this stage of the project, I adjusted my 
firewall approach to explicitly target the **Public** profile — the profile the interface 
was actually running under — instead of assuming it would be Private or Domain.

![Host Public ICMP Rule Disabled]( screenshots/02%20network%20configuration/36-Host-Public-ICMP-Rule-Disabled.jpg)

This is a known limitation of the current setup: the internal lab network functions 
correctly, but remains classified as Public rather than Private/Domain. Resolving the 
underlying Group Policy restriction is listed as a follow-up item below.



## 3. Active Directory Domain Services Deployment

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

## 4. DNS Troubleshooting and Resolution

After deploying Active Directory Domain Services, I tested DNS functionality to verify that name resolution was operating correctly.

The troubleshooting process involved checking client DNS configuration, DNS server settings, port 53 connectivity, explicit DNS queries, IPv4 and IPv6 resolution, reverse DNS configuration, and final domain controller diagnostics.

### 1. Initial DNS Lookup Test

I performed an initial `nslookup` test to verify DNS name resolution and determine whether the DNS server was responding correctly.

The lookup successfully resolved `dc01.ajimohlab.local` to `192.168.10.10`, confirming that forward DNS resolution was working.

However, the test also returned a DNS request timeout and displayed the DNS server as:

`Server: UnKnown`  
`Address: ::1`

This showed that although the forward lookup was successful, the DNS server itself was not being identified correctly. The `::1` address also indicated that the IPv6 loopback address was being used for the DNS query, so further investigation was required.

![Initial DNS Lookup Test](screenshots/04%20dns%20troubleshooting/DNS_02_NSLookup_Initial_Test.jpg)

### 2. Client DNS Configuration Diagnosis

I reviewed the network and DNS configuration to identify which DNS server addresses were configured and determine why `nslookup` was reporting the DNS server as `::1`.

![Client DNS Configuration Diagnosis](screenshots/04%20dns%20troubleshooting/DNS_03_Client_Configuration_Diagnosis.jpg)

### 3. IPv6 DNS Lookup Timeout

Further testing showed that DNS queries involving the IPv6 loopback address `::1` were timing out. This helped narrow the investigation toward the DNS server configuration rather than the forward lookup zone itself.

![NSLookup IPv6 Timeout](screenshots/04%20dns%20troubleshooting/DNS_05_NSLookup_IPv6_Timeout.jpg)

### 4. DNS Server Listening Addresses

I checked the DNS server listening configuration to verify that the DNS service was listening on the required network interfaces and IP addresses. This helped confirm that the DNS service itself was available to receive queries.

![DNS Server Listening Addresses](screenshots/04%20dns%20troubleshooting/DNS_06_Server_Listening_Addresses.jpg)

### 5. DNS Port 53 Connectivity Test

I tested connectivity to DNS port 53 to verify that the DNS server was reachable and that DNS traffic was not being blocked at the network level.

![DNS Port 53 Connectivity Test](screenshots/04%20dns%20troubleshooting/DNS_07_Port_53_Connectivity_Test.jpg)

### 6. Explicit DNS Query

I then performed an explicit DNS query against DC01's IPv4 address `192.168.10.10`. This bypassed the default DNS server selection and allowed me to test whether DC01 could answer the DNS query directly.

![Explicit DNS Query](screenshots/04%20dns%20troubleshooting/DNS_08_Explicit_DNS_Query.png.jpg)

### 7. IPv6 DNS Resolution Test

I performed an additional DNS resolution test involving IPv6 to further investigate the `::1` address observed during the earlier `nslookup` tests and compare the DNS response with the IPv4 results.

![IPv6 DNS Resolution Test](screenshots/04%20dns%20troubleshooting/DNS_09_IPv6_DNS_Resolution_Test.png.jpg)

### 8. Reverse DNS Lookup Failure

I then tested reverse DNS resolution for the Domain Controller's IPv4 address `192.168.10.10`. The lookup failed because a reverse lookup zone and corresponding PTR record had not yet been configured.

![Reverse DNS Lookup Failed](screenshots/04%20dns%20troubleshooting/DNS_10_Reverse_Lookup_Failed.jpg)

### 9. Reverse Lookup Zone Created

I created an IPv4 reverse lookup zone in DNS Manager for the `192.168.10.0/24` network to support IP-address-to-hostname resolution.

![Reverse Lookup Zone Created](screenshots/04%20dns%20troubleshooting/DNS_11_Reverse_Lookup_Zone_Created.jpg)

### 10. PTR Record Created

I created a Pointer (PTR) record to map the Domain Controller's IPv4 address `192.168.10.10` back to its hostname `DC01.ajimohlab.local`.

![PTR Record Created](screenshots/04%20dns%20troubleshooting/DNS_12_PTR_Record_Created.jpg)

### 11. Explicit IPv4 DNS Server Test

After creating the reverse lookup zone and PTR record, I repeated the DNS query by explicitly specifying DC01's IPv4 address `192.168.10.10` as the DNS server.

The test successfully identified the DNS server as `dc01.ajimohlab.local`, confirming that the PTR record was resolving the server's IPv4 address back to its hostname.

However, DNS timeout messages were still present, so I continued troubleshooting rather than treating this as the final resolution.

![Explicit IPv4 DNS Server Test](screenshots/04%20dns%20troubleshooting/DNS_13_NSLookup_IPv4_Success.jpg)


### 12. TCP and UDP Port 53 Verification

I verified DNS communication over both TCP and UDP port 53 to confirm that the required DNS transport protocols were functioning correctly.

![TCP UDP Port 53 Verification](screenshots/04%20dns%20troubleshooting/DNS_14_TCP_UDP_Port_53_Verification.png.jpg)

### 13. Forward DNS Resolution Verified

I performed a final forward resolution test and confirmed that the domain controller's hostname resolved successfully to the expected IPv4 address.

![Forward DNS Resolution Successful](screenshots/04%20dns%20troubleshooting/DNS_15_Forward_Resolution_Successful.jpg)

### 14. DNS Server Address Configuration Corrected

Although forward DNS resolution was working, a standard `nslookup` test was still initially displaying `Server: UnKnown`, using the IPv6 loopback address `::1`, and returning a DNS request timeout.

Further investigation showed that the DNS configuration was using the IPv6 loopback address for DNS queries.

I corrected the DNS server configuration so that DC01 used its static IPv4 address `192.168.10.10` for DNS resolution instead of `::1`.

### 15. Final NSLookup Verification

After correcting the DNS server configuration, I repeated the same `nslookup DC01.ajimohlab.local` command.

The second test successfully identified the DNS server and resolved the hostname without the previous timeout:

`Server: DC01.ajimohlab.local`  
`Address: 192.168.10.10`

The forward lookup also returned:

`Name: DC01.ajimohlab.local`  
`Address: 192.168.10.10`

This confirmed that the DNS server was now being identified correctly and that forward name resolution was functioning without the previous `::1` timeout.

![Final NSLookup Verification](screenshots/04%20dns%20troubleshooting/DNS_17_Final_NSLookup_Verification.jpg)

### 16. Final DNS Health Verification

Finally, I used `dcdiag` to perform DNS diagnostic testing on the Domain Controller. The DNS tests passed, providing an additional verification that the Domain Controller's DNS services were functioning correctly.

![DCDiag DNS Passed](screenshots/04%20dns%20troubleshooting/DNS_16_DCDiag_DNS_Passed.jpg)

## Troubleshooting Summary

This project included several real troubleshooting scenarios that required systematic diagnosis rather than simply following the initial configuration steps.

### ICMP Connectivity Issue

**Problem:**

The Windows host and DC01 were initially unable to communicate successfully using ICMP ping. Ping tests in both directions timed out.

**Investigation:**

I verified the IP configuration on both systems and confirmed that the network settings were correct. I then tested connectivity in both directions and reviewed the Windows Firewall rules associated with ICMP traffic.

The investigation showed that the required inbound ICMP Echo Request rule was not enabled, preventing the systems from responding to ping requests.

**Resolution:**

I enabled the `File and Printer Sharing (Echo Request - ICMPv4-In)` Windows Firewall rule on both DC01 and the Windows host.

**Result:**

I repeated the ping tests and successfully established bidirectional communication between the Windows host and DC01 with no packet loss.

### DNS Resolution Issue

**Problem:**

DNS queries initially returned `Server: UnKnown` / `Address: ::1` with a timeout message, even though the forward lookup successfully resolved `DC01.ajimohlab.local` to `192.168.10.10`.

**Investigation:**

I reviewed the DNS configuration, checked the DNS server's listening addresses, tested connectivity to port 53, and performed both explicit IPv4 and IPv6 DNS queries.

Forward name resolution was working, but reverse DNS resolution for `192.168.10.10` failed because a reverse lookup zone and PTR record had not yet been configured.

I created the reverse lookup zone and PTR record and tested again. An explicit query against `192.168.10.10` correctly identified the DNS server as `DC01.ajimohlab.local`.

However, a standard `nslookup` test continued to use the IPv6 loopback address `::1`, resulting in `Server: UnKnown` and a DNS request timeout.

This showed that the reverse DNS issue had been corrected, but there was still a separate DNS server configuration issue to investigate.

**Root Cause:**

The troubleshooting identified two separate configuration issues:

1. Reverse DNS had not been configured, so `192.168.10.10` could not initially resolve back to `DC01.ajimohlab.local`.

2. The DNS configuration was using the IPv6 loopback address `::1` for DNS queries, which caused the standard `nslookup` test to display `Server: UnKnown` and return a timeout.

**Resolution:**

I created an IPv4 reverse lookup zone and PTR record for `192.168.10.10`, then corrected the DNS server configuration so that DC01 used its static IPv4 address `192.168.10.10` for DNS resolution instead of `::1`.

**Result:**

After repeating the same `nslookup DC01.ajimohlab.local` test, the DNS server was correctly identified as:

`Server: DC01.ajimohlab.local`  
`Address: 192.168.10.10`

The hostname also resolved successfully to `192.168.10.10` without the previous DNS timeout.

Final DNS diagnostic testing with `dcdiag` also passed.

### Reverse DNS Resolution Issue

**Problem:**

Forward DNS resolution was working, but a reverse lookup of the Domain Controller's IPv4 address `192.168.10.10` failed.

**Investigation:**

I confirmed that an IPv4 reverse lookup zone and PTR record for the Domain Controller had not yet been configured.

**Resolution:**

I created an IPv4 reverse lookup zone for the `192.168.10.0/24` network and added a PTR record mapping:

`192.168.10.10 → DC01.ajimohlab.local`

**Result:**

Reverse DNS resolution succeeded, allowing the Domain Controller's IPv4 address to resolve back to its hostname.

This corrected the reverse DNS configuration. The separate `::1` DNS server issue required additional troubleshooting, as documented above.

## Skills Demonstrated

- Windows Server 2022 administration
- Hyper-V configuration and virtual machine deployment
- Active Directory Domain Services installation and configuration
- Domain controller deployment
- Active Directory forest creation
- Active Directory Users and Computers administration
- IPv4 and static IP configuration
- IPv4 and IPv6 DNS troubleshooting
- Windows Firewall troubleshooting
- ICMP connectivity troubleshooting
- DNS configuration, name resolution, and troubleshooting
- Forward and reverse DNS resolution
- DNS reverse lookup zone configuration
- PTR record creation
- `ping`, `ipconfig`, `nslookup`, and `dcdiag` diagnostic tools
- TCP/UDP port 53 connectivity testing
- Systematic troubleshooting and verification

## Tools and Technologies

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
- DNS server configuration was corrected to resolve the `::1` loopback-related lookup issue, and successful `nslookup` resolution was verified.
- Final DNS diagnostic testing completed successfully.

The project strengthened my practical experience in Windows Server administration, Active Directory, networking, DNS, firewall configuration, and structured troubleshooting.

## Known Limitations / Follow-Up Items

- The internal lab network remains classified under the **Public** firewall profile rather than Private. An attempt to change the profile using `Set-NetConnectionProfile` returned a permission-denied error. Further investigation into the applicable network policies and permissions remains a follow-up task.

## Lessons Learned

This project reinforced the importance of using a structured troubleshooting process rather than making configuration changes without first identifying the cause of a problem.

The ICMP connectivity issue demonstrated how Windows Firewall rules and network profiles can affect communication even when IP addressing is configured correctly.

The DNS troubleshooting process also showed the importance of testing each layer separately. Verifying the DNS server's listening configuration, port 53 connectivity, forward resolution, reverse resolution, and final domain controller diagnostics made it possible to isolate and resolve each issue systematically.

Most importantly, this project gave me practical experience deploying, configuring, testing, and troubleshooting an Active Directory environment rather than only studying the concepts theoretically.

## Project Demonstration

I recorded the complete implementation and troubleshooting process as a four-part video series. The videos demonstrate the configuration steps, issues encountered, troubleshooting process, and final verification of the Windows Server 2022 Active Directory home lab.

> **Video editing note:** The demonstration videos were edited to remove waiting periods, repetitive actions, restarts, and other non-essential footage. As a result, the system clock may appear to jump forward between some sections. The technical steps are presented in the order they were performed.

### Part 1 – Hyper-V and Windows Server Setup

Covers virtualization verification, Hyper-V configuration, virtual networking, DC01 virtual machine creation, and Windows Server 2022 installation.

▶️ [Watch Part 1 on YouTube](https://youtu.be/XZWxVUwlBLY)

### Part 2 – Network and Firewall Troubleshooting

Covers static IP configuration, connectivity testing, ICMP failures, Windows Firewall investigation, firewall rule and network profile troubleshooting, and successful bidirectional communication.

▶️ [Watch Part 2 on YouTube](https://youtu.be/0anyhC7JYyg?si=Oi6XLDyAsd5g_saj)

### Part 3 – Active Directory Domain Services Deployment

Covers AD DS installation, creation of the `ajimohlab.local` forest, domain controller promotion, domain verification, ADUC verification, and DNS integration.

▶️ [Watch Part 3 on YouTube](https://youtu.be/-jAFozUGix0)

### Part 4 – DNS Troubleshooting and Verification

Covers DNS testing and troubleshooting, DNS port 53 connectivity, explicit DNS queries, IPv4 and IPv6 testing, reverse lookup zone and PTR record configuration, forward and reverse resolution, and final DNS health verification.

▶️ [Watch Part 4 on YouTube](https://youtu.be/aSFSaEkqp-U)


---

## Related Active Directory Project

### Active Directory Administration & Troubleshooting Home Lab

Building on the Windows Server and Active Directory environment created in this project, I completed a second hands-on lab focused on day-to-day Active Directory administration and IT support troubleshooting.

The project covers:

- Organisational Unit (OU) design and management
- Domain user and security group administration
- PowerShell-based Active Directory administration
- Windows 11 deployment and domain joining
- Group Policy configuration and testing
- Password reset and authentication troubleshooting
- Account lockout diagnosis and recovery
- Security group membership and departmental folder access
- Share and NTFS permissions
- Disabled user account troubleshooting

➡️ [View the Active Directory Administration & Troubleshooting Project](project-3-ad-administration-troubleshooting/)
