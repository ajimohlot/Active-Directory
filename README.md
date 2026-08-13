
## Phase 1: Hyper-V and Windows Server Setup

I first verified that virtualization was enabled on the host computer and prepared Microsoft Hyper-V for the lab.

![Virtualization Enabled](screenshots/01 hyperv setup/01-Virtualization-Enabled.png)

I created an internal Hyper-V virtual switch to provide a dedicated network for the home lab.

![Internal Virtual Switch](screenshots/01 hyperv- setup/07-LAB-Internal-Virtual-Switch.png)

I then created a Generation 2 virtual machine named **DC01** and configured the required memory, processor, storage and network settings.

![DC01 VM](screenshots/01 hyperv setup/04-DC01-VM-Name.png)

Windows Server 2022 Standard Evaluation with Desktop Experience was installed on DC01.

![Windows Server 2022](screenshots/01 hyperv setup/13-Windows-Server-2022-Edition-Selection.png)

---

## Phase 2: Server and Network Configuration

After installing Windows Server, I renamed the server to **DC01** and configured the network settings required for the lab.

![Rename DC01](screenshots/02-network-configuration/16-Rename-Server-DC01.png)

DC01 was configured with the following static network settings:

- IP address: `192.168.10.10`
- Subnet mask: `255.255.255.0`
- DNS server: `192.168.10.10`

![Static IP](screenshots/02-network-configuration/19-DC01-Static-IP-Configuration.png)

### Connectivity Troubleshooting

Initial connectivity tests between the Hyper-V host and DC01 failed.

![Initial Ping Failure](screenshots/02-network-configuration/21-Host-to-DC01-Ping-Failed.png)

I investigated the virtual switch configuration, IP addressing, network profiles and Windows Defender Firewall rules.

The ICMP Echo Request firewall rules were disabled for the relevant network profile, preventing successful ping communication.

![ICMP Rules Before](screenshots/02-network-configuration/28-DC01-ICMP-Firewall-Rules-Before.png)

After enabling the appropriate ICMP firewall rules, connectivity from the host to DC01 was restored.

![Ping Success](screenshots/02-network-configuration/30-Host-to-DC01-Ping-Success.png)

I then identified and corrected the corresponding firewall/profile issue affecting communication in the opposite direction.

![Profile Investigation](screenshots/02-network-configuration/32-Host-ICMP-Rule-Profile-Mismatch.png)

Bidirectional communication between DC01 and the host was successfully verified.

![Bidirectional Connectivity](screenshots/02-network-configuration/34-DC01-to-Host-Ping-Successful.png)

---

## Phase 3: Active Directory Domain Services Deployment

I installed the Active Directory Domain Services role together with the required management tools.

![AD DS Installation](screenshots/03-active-directory/ADDS_03_Installation_Confirmation.png)

The AD DS role installation completed successfully.

![AD DS Installed](screenshots/03-active-directory/ADDS_04_ADDS_Installation_Successful.png)

I promoted DC01 to the first domain controller in a new forest and created the domain:

`ajimohlab.local`

![New Forest](screenshots/03-active-directory/ADDS_05_New_Forest_Configuration.png)

Before promotion, I reviewed the domain configuration including DNS, Global Catalog, NetBIOS name and functional levels.

![Configuration Review](screenshots/03-active-directory/ADDS_08_Configuration_Review.png)

All prerequisite checks passed successfully before installation.

![Prerequisites Passed](screenshots/03-active-directory/ADDS_09_Prerequisites_Check_Passed.png)

After promotion and reboot, the server presented the AJIMOHLAB domain login.

![Domain Login](screenshots/03-active-directory/ADDS_10_AJIMOHLAB_Domain_Login.png)

Server Manager confirmed that DC01 was a member of the `ajimohlab.local` domain and that both AD DS and DNS roles were installed.

![Domain Verification](screenshots/03-active-directory/ADDS_11_DC01_Domain_Verification.png)

Active Directory Users and Computers also confirmed DC01 as the domain controller.

![ADUC Verification](screenshots/03-active-directory/ADDS_12_DC01_ADUC_Verification.png)

---

## Phase 4: DNS Troubleshooting and Validation

After promoting DC01, I tested DNS name resolution and identified inconsistent `nslookup` behaviour.

![Initial DNS Test](screenshots/04-dns-troubleshooting/DNS_02_NSLookup_Initial_Test.png)

I reviewed the DNS client configuration and investigated the DNS server settings.

![DNS Client Diagnosis](screenshots/04-dns-troubleshooting/DNS_03_Client_Configuration_Diagnosis.png)

Testing also showed timeout behaviour when the IPv6 loopback address was used during name resolution.

![IPv6 Timeout](screenshots/04-dns-troubleshooting/DNS_05_NSLookup_IPv6_Timeout.png)

I confirmed that the DNS service was running and checked the addresses on which the service was listening.

![DNS Listening](screenshots/04-dns-troubleshooting/DNS_06_Server_Listening_Addresses.png)

I then tested connectivity to DNS port 53.

![Port 53 Test](screenshots/04-dns-troubleshooting/DNS_07_Port_53_Connectivity_Test.png)

Forward DNS resolution was successfully validated.

![Forward DNS Success](screenshots/04-dns-troubleshooting/DNS_10_Forward_Resolution_Successful.png)

### Reverse DNS Troubleshooting

A reverse lookup for `192.168.10.10` initially failed because reverse DNS had not yet been configured.

![Reverse Lookup Failure](screenshots/04-dns-troubleshooting/DNS_11_Reverse_Lookup_Failed.png)

I created a reverse lookup zone for the `192.168.10.0/24` network.

![Reverse Lookup Zone](screenshots/04-dns-troubleshooting/DNS_12_Reverse_Lookup_Zone_Created.png)

A PTR record was then created to map:

`192.168.10.10 → dc01.ajimohlab.local`

![PTR Record](screenshots/04-dns-troubleshooting/DNS_13_PTR_Record_Created.png)

A final `nslookup` test using the IPv4 DNS server successfully identified DC01 as the DNS server and resolved the hostname correctly.

![NSLookup Success](screenshots/04-dns-troubleshooting/DNS_14_NSLookup_IPv4_Success.png)

Finally, I ran `dcdiag` to validate Active Directory and DNS health. DC01 successfully passed both the Connectivity and DNS tests.

![DCDiag Passed](screenshots/04-dns-troubleshooting/DNS_15_DCDiag_DNS_Passed.png)

---

## Problems Encountered and Resolved

### 1. Host and DC01 Could Not Communicate

**Problem:** Ping tests between the host and DC01 initially failed.

**Investigation:** I checked IP addressing, Hyper-V virtual switch configuration, Windows network profiles and ICMP firewall rules.

**Resolution:** The appropriate inbound ICMP Echo Request firewall rules were enabled for the relevant network profiles.

**Result:** Bidirectional communication between the host and DC01 was restored.

### 2. DNS Resolution Behaviour

**Problem:** `nslookup` initially produced timeout and unknown-server behaviour.

**Investigation:** I checked DNS client settings, DNS service status, listening addresses and connectivity to port 53.

**Resolution:** DNS configuration was corrected and direct DNS queries were tested against DC01.

**Result:** Forward name resolution worked successfully.

### 3. Reverse DNS Lookup Failure

**Problem:** PTR lookup for `192.168.10.10` timed out.

**Cause:** A reverse lookup zone and PTR record had not yet been configured.

**Resolution:** I created a reverse lookup zone and configured a PTR record for DC01.

**Result:** Forward and reverse DNS configuration was successfully validated.

---

## Skills Demonstrated

- Microsoft Hyper-V
- Windows Server 2022
- Active Directory Domain Services
- DNS administration
- Windows Defender Firewall
- TCP/IP networking
- Static IPv4 configuration
- PowerShell
- Command Prompt
- `ipconfig`
- `ping`
- `nslookup`
- `Resolve-DnsName`
- `Test-NetConnection`
- `dcdiag`
- Network troubleshooting
- DNS troubleshooting
- Virtual machine administration

---

## Video Demonstration

The complete project was recorded and documented in four parts.

### Part 1
Environment preparation, Hyper-V setup, Windows Server deployment and initial configuration.

[Watch Part 1 on YouTube](PASTE_PART_1_LINK_HERE)

### Part 2
Network and firewall troubleshooting.

[Watch Part 2 on YouTube](PASTE_PART_2_LINK_HERE)

### Part 3
Active Directory Domain Services deployment and domain controller configuration.

[Watch Part 3 on YouTube](PASTE_PART_3_LINK_HERE)

### Part 4
DNS troubleshooting, reverse lookup configuration and final validation.

[Watch Part 4 on YouTube](PASTE_PART_4_LINK_HERE)

---

## Conclusion

This project provided hands-on experience deploying and troubleshooting a Windows Server Active Directory environment from the ground up.

Beyond installing Active Directory, the project required diagnosing network connectivity and DNS issues, testing different layers of the configuration, implementing fixes and validating the final environment.

The completed lab includes a functioning Windows Server 2022 domain controller, Active Directory Domain Services, DNS, forward and reverse name resolution, and validated host-to-server connectivity.
