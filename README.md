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
