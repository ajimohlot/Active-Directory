# Windows Server 2022 Active Directory Home Lab

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

---

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


---
## Network Configuration and Connectivity Troubleshooting

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
