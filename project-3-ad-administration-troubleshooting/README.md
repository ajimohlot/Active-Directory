# Active Directory Administration & Troubleshooting Home Lab

## Project Overview

This project demonstrates hands-on Active Directory administration and IT support troubleshooting in a Windows domain environment.

Using an existing Windows Server domain lab, I configured an organisational structure for a fictional company, Londonbridge Solutions Ltd, created and managed domain users and security groups, deployed a Windows 11 client, joined it to the domain, and applied Group Policy.

I then created several common IT support scenarios involving password resets, account lockouts, group membership, departmental folder access, and disabled user accounts. Each issue was reproduced, investigated, resolved, and verified using tools including Active Directory Users and Computers, PowerShell, Group Policy Management, and Windows administrative tools.

## Project Objectives

- Design and configure an organised Active Directory OU structure.
- Create and manage domain users and security groups.
- Use PowerShell for Active Directory administration and verification.
- Configure a Windows 11 client and join it to the domain.
- Organise the domain-joined computer within the appropriate OU.
- Create, apply, and verify Group Policy settings.
- Troubleshoot user password and authentication issues.
- Diagnose and resolve account lockouts.
- Troubleshoot security group membership and departmental resource access.
- Configure and test shared-folder permissions.
- Diagnose and re-enable disabled Active Directory accounts.
- Verify each resolution from both the administrative and end-user perspectives.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Microsoft Hyper-V |
| Domain Controller | DC01 |
| Server OS | Windows Server 2022 |
| Active Directory Domain | ajimohlab.local |
| Domain Controller IP | 192.168.10.10 |
| Windows Client | WIN11-CLIENT01 |
| Client OS | Windows 11 |
| Client IP | 192.168.10.20 |
| DNS Server | 192.168.10.10 |
| Network | 192.168.10.0/24 |

### Core Technologies

- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- DNS
- Group Policy Management
- Windows PowerShell
- Windows 11
- Hyper-V
- NTFS and shared-folder permissions

## Active Directory Structure

To simulate a small business environment, I created an organisational structure for **Londonbridge Solutions Ltd** within the `ajimohlab.local` domain.

The structure separates users, computers, and security groups so that accounts and policies can be managed according to their purpose and department.

```text
ajimohlab.local
│
└── Londonbridge Solutions
    │
    ├── Users
    │   ├── Finance
    │   ├── HR
    │   ├── IT
    │   └── Sales
    │
    ├── Groups
    │
    └── Computers
        └── Workstation

```

## Part 1 — OUs, Users, Groups & PowerShell

The first stage focused on building a structured Active Directory environment for Londonbridge Solutions Ltd.

I created separate organisational units for users, computers, and security groups, with departmental OUs for Finance, HR, IT, and Sales. This provides a logical structure for managing users and applying administrative policies.

### User and Group Administration

Domain user accounts were created and organised within their appropriate departmental OUs. Security groups were then created for each department and users were assigned to the relevant groups.

This structure allows access to resources to be managed through security group membership rather than assigning permissions directly to individual users.

### PowerShell Administration

In addition to using Active Directory Users and Computers, I used PowerShell to create additional domain users and verify that the accounts had been created successfully.

This demonstrates both GUI-based and command-line Active Directory administration.

### Evidence

#### Organisational Unit Structure

The completed OU structure separates departmental users, security groups, and workstation computer objects.

![Londonbridge Solutions OU Structure](screenshots/01-ad-users-groups-powershell/ADGP_02_Londonbridge_OU_Structure.jpg)

#### Departmental Users

User accounts were organised into their corresponding departmental OUs.

![Departmental User Structure](screenshots/01-ad-users-groups-powershell/ADGP_04_Departmental_User_Structure.jpg)

#### Security Groups

Departmental security groups were created to support group-based resource access.

![Department Security Groups](screenshots/01-ad-users-groups-powershell/ADGP_06_Department_Security_Groups.jpg)

#### PowerShell User Creation

Additional Active Directory users were created using PowerShell.

![PowerShell User Creation](screenshots/01-ad-users-groups-powershell/ADGP_07_PowerShell_User_Creation.jpg)

#### ADUC Verification

The PowerShell-created accounts were verified in Active Directory Users and Computers.

![ADUC User Verification](screenshots/01-ad-users-groups-powershell/ADGP_08_PowerShell_Users_ADUC_Verification.jpg)

### Video Demonstration

▶️ **Part 1 — Active Directory Home Lab: OUs, Users, Groups & PowerShell**

https://youtu.be/0gYJaJS8o2g

---

## Part 2 — Windows 11 Deployment & Domain Join

The next stage involved configuring a Windows 11 virtual machine as a client workstation and integrating it into the `ajimohlab.local` Active Directory domain.

Before attempting the domain join, I configured the client's network settings so that the domain controller, `DC01`, was used as its DNS server. I then verified DNS resolution and confirmed that the client could locate the domain controller.

### Client Network Configuration

The Windows 11 client was configured with the following network settings:

| Setting | Value |
|---|---|
| Computer Name | WIN11-CLIENT01 |
| IPv4 Address | 192.168.10.20 |
| Subnet Mask | 255.255.255.0 |
| DNS Server | 192.168.10.10 |
| Domain | ajimohlab.local |

Correct DNS configuration was essential because Active Directory relies on DNS to locate domain services.

### Domain Connectivity and Discovery

Before joining the client to the domain, I verified:

- Connectivity between WIN11-CLIENT01 and DC01.
- DNS resolution for `ajimohlab.local`.
- Successful discovery of the domain controller.

This confirmed that the client could communicate with the required Active Directory services before the domain join was attempted.

### Domain Join

`WIN11-CLIENT01` was successfully joined to the `ajimohlab.local` domain.

After restarting the workstation, I signed in using a domain user account to confirm that Active Directory authentication was functioning correctly.

### Computer Object Management

Following the domain join, the `WIN11-CLIENT01` computer object initially appeared in the default **Computers** container.

I moved the computer object to:

`Londonbridge Solutions > Computers > Workstation`

This provides a structured location for managed workstation objects and allows Group Policy to be targeted appropriately.

PowerShell was then used to verify the computer object's Distinguished Name and confirm its final location in Active Directory.

### Evidence

#### Windows 11 Client

The Windows 11 virtual machine was prepared as the domain client.

![Windows 11 Client](screenshots/02-windows11-domain-join/01-hyperv-windows11-client.jpg)

#### DNS Configuration

The client was configured to use DC01 as its DNS server.

![Windows 11 DNS Configuration](screenshots/02-windows11-domain-join/01-win11-dns-configuration.jpg)

#### Domain Controller Discovery

The client successfully located the domain controller before joining the domain.

![Domain Controller Discovery](screenshots/02-windows11-domain-join/03-domain-controller-discovery.jpg)

#### Successful Domain Join

WIN11-CLIENT01 was successfully joined to `ajimohlab.local`.

![Successful Domain Join](screenshots/02-windows11-domain-join/04-domain-join-success.jpg)

#### Domain User Authentication

Successful domain-user sign-in confirmed that the workstation could authenticate against Active Directory.

![Domain User Login](screenshots/02-windows11-domain-join/05-domain-user-login.jpg)

#### Workstation OU

The computer object was moved into the Workstation OU.

![Client Workstation OU](screenshots/02-windows11-domain-join/07-client-workstation-ou.jpg)

#### PowerShell Verification

PowerShell was used to verify the Distinguished Name of `WIN11-CLIENT01`, confirming that the computer object was located in the correct OU.

![AD Computer Verification](screenshots/02-windows11-domain-join/08-ad-computer-verification.jpg)

### Video Demonstration

▶️ **Part 2 — Active Directory Home Lab: Windows 11 Deployment & Domain Join**

https://youtu.be/PrVv_r_26vE

---

## Part 3 — Group Policy Configuration & Testing

The next stage focused on using Group Policy to centrally manage user settings within the domain.

I created a Group Policy Object for the HR department and linked it to the HR OU. The policy was configured to prevent HR users from accessing Control Panel and Windows Settings.

This provided a practical demonstration of how Group Policy can be scoped to a specific organisational unit rather than applying the restriction across the entire domain.

### HR Group Policy

The following GPO was created:

`GPO-HR-Restrict-ControlPanel`

The policy was linked to the HR OU and configured under:

`User Configuration > Policies > Administrative Templates > Control Panel`

The following setting was enabled:

`Prohibit access to Control Panel and PC settings`

### Policy Deployment and Testing

After configuring the GPO, I signed in to `WIN11-CLIENT01` using an HR domain account and forced a Group Policy refresh using:

```powershell
gpupdate /force
```

I then attempted to access Windows Settings to verify that the restriction had been applied.

The policy was also verified from the command line using:

```powershell
gpresult /r
```

The HR GPO appeared under the applied Group Policy Objects, confirming that the workstation had processed the policy successfully.

### Scope Verification

To confirm that the restriction was scoped correctly, I also tested the configuration using a user from another department.

The non-HR user was able to access Windows Settings normally, demonstrating that the restriction applied to the intended HR users rather than the entire domain.

### Evidence

#### HR Group Policy Created

The GPO was created and linked to the HR organisational unit.

![HR GPO Created](screenshots/03-group-policy/01-hr-gpo-created.jpg)

#### Control Panel Restriction Configured

The policy preventing access to Control Panel and Windows Settings was enabled.

![Control Panel Policy Enabled](screenshots/03-group-policy/02-control-panel-policy-enabled.jpg)

#### Group Policy Update

The Windows 11 client successfully processed the updated Group Policy.

![Group Policy Update Successful](screenshots/03-group-policy/03-User%20Policy%20update%20has%20completed%20successfully.jpg)

#### Policy Restriction Verified

The HR user was prevented from accessing the restricted Windows settings.

![HR Policy Restriction](screenshots/03-group-policy/04-hr-control-panel-restriction.jpg)

#### Applied GPO Verification

`gpresult` confirmed that the HR Group Policy Object had been applied to the user session.

![GPO Result Verification](screenshots/03-group-policy/05-gpresult-hr-gpo.jpg)

#### GPO Scope Verification

A Finance user was used to confirm that the restriction was limited to the intended HR scope.

![GPO Scope Verification](screenshots/03-group-policy/06-gpo-scope-finance-user.jpg)

Video Demonstration

▶️ Part 3 — Active Directory Home Lab: Group Policy & User Account Troubleshooting

https://youtu.be/2KMTo59hupU

---

## Part 4 — User Access & Account Troubleshooting

The final stage of the project focused on reproducing and resolving common user-support issues in an Active Directory environment.

Rather than only demonstrating administrative configuration, I created realistic support scenarios and followed a structured troubleshooting process to identify the cause, apply the appropriate resolution, and verify that normal access had been restored.

The scenarios included:

- Password and login troubleshooting
- Account lockout
- Security group membership and departmental folder access
- Disabled user account troubleshooting

---

### Scenario 1 — Password Reset

#### Issue

A domain user was unable to sign in using the existing password.

#### Investigation and Resolution

I reproduced the login failure on the Windows 11 domain client and then used Active Directory Users and Computers to reset the user's password.

The account was configured to require a password change at the next sign-in, allowing the user to establish a new password after authenticating with the temporary password.

#### Verification

The user successfully completed the password change and authenticated to the domain using the new credentials.

![Password Login Failure](screenshots/04-user-access-troubleshooting/01-password-login-failure.jpg)

![AD Password Reset](screenshots/04-user-access-troubleshooting/02-ad-password-reset-success.jpg)

![Password Change Required](screenshots/04-user-access-troubleshooting/03-password-change-required.jpg)

![Password Reset Login Verification](screenshots/04-user-access-troubleshooting/04-password-reset-login-verification.jpg)

---

### Scenario 2 — Account Lockout

#### Issue

A domain account became locked after repeated unsuccessful authentication attempts.

#### Investigation

I reviewed the domain account lockout policy and reproduced the issue by exceeding the configured failed-login threshold.

PowerShell was then used to confirm that the account was locked.

#### Resolution

The account was unlocked administratively and its status was checked again to confirm that the lockout condition had been cleared.

#### Verification

The user was able to sign in successfully after the account was unlocked.

![Account Lockout Policy](screenshots/04-user-access-troubleshooting/02-account-lockout-policy-configured.jpg)

![Account Locked](screenshots/04-user-access-troubleshooting/04-grace%20account-locked.jpg)

![PowerShell Lockout Confirmation](screenshots/04-user-access-troubleshooting/05-powershell-account-lockout-confirmed.jpg)

![PowerShell Account Unlocked](screenshots/04-user-access-troubleshooting/06-powershell-account-unlocked.jpg)

![Login After Unlock](screenshots/04-user-access-troubleshooting/07-grace%20login-after-unlock.jpg)

---

### Scenario 3 — Security Group Membership & Departmental Folder Access

#### Issue

A Sales user required access to a departmental shared folder, but the account was not initially a member of the security group used to control access to the resource.

This created an access problem that could be investigated from both the Active Directory and file-permission sides.

#### Access Configuration

A departmental Sales folder was configured and permissions were assigned to the `SG-Sales Users` security group.

Using a security group rather than granting permissions directly to individual users provides a more manageable approach to resource access.

#### Reproducing the Problem

Before correcting the user's group membership, I attempted to access the Sales resource using the affected account.

Access was denied, confirming the reported issue.

#### Investigation

I checked the user's current Active Directory group memberships and confirmed that the required Sales security group was missing.

#### Resolution

The user was added to `SG-Sales Users`.

Because group membership information is included in the user's Windows security token, the user session was refreshed so that the new membership could be recognised.

#### Verification

After the updated security token was obtained, access to the Sales folder was restored.

I also verified write access to confirm that the permissions allowed the user to work with the departmental resource rather than simply view it.

![Sales Share Permissions](screenshots/04-user-access-troubleshooting/02-sales-share-permissions.jpg)

![Sales NTFS Permissions](screenshots/04-user-access-troubleshooting/03-sales-ntfs-permissions.jpg)

![Sales Folder Access Denied](screenshots/04-user-access-troubleshooting/04-sales-folder-access-denied.jpg)

![Group Membership Before](screenshots/04-user-access-troubleshooting/5-daniel-group-membership-before.jpg)

![Group Membership After](screenshots/04-user-access-troubleshooting/06-daniel-group-membership-after.jpg)

![Updated Security Token](screenshots/04-user-access-troubleshooting/08-daniel-updated-security-token.jpg)

![Sales Folder Access Restored](screenshots/04-user-access-troubleshooting/09-sales-folder-access-restored.jpg)

![Sales Folder Write Access Verified](screenshots/04-user-access-troubleshooting/10-sales-folder-write-access-verified.jpg)

---

### Scenario 4 — Disabled User Account

#### Issue

A domain user was unable to authenticate even though the credentials were otherwise valid.

#### Investigation

The failed sign-in was reproduced from the Windows 11 client.

I then checked the account status in Active Directory and used PowerShell to verify whether the account was enabled.

The investigation confirmed that the account had been disabled.

#### Resolution

The user account was re-enabled in Active Directory.

PowerShell was then used again to confirm that the account's enabled status had changed successfully.

#### Verification

The user was able to authenticate successfully after the account was re-enabled, confirming that the disabled account state had been the cause of the login problem.

![Disabled Account Login Failure](screenshots/04-user-access-troubleshooting/01-disabled-account-login-failure.jpg)

![Disabled Account PowerShell Diagnosis](screenshots/04-user-access-troubleshooting/02-disabled-account-powershell-diagnosis.jpg)

![Disabled Account ADUC](screenshots/04-user-access-troubleshooting/03-disabled-account-aduc.jpg)

![Account Enabled ADUC](screenshots/04-user-access-troubleshooting/04-account-enabled-aduc.jpg)

![Account Enabled PowerShell Verification](screenshots/04-user-access-troubleshooting/05-account-enabled-powershell-verification.jpg)

![Enabled User Login Verification](screenshots/04-user-access-troubleshooting/06-enabled-user-login-verification.jpg)

---

### Video Demonstration

▶️ **Part 4 — Active Directory Home Lab: Group Membership, Folder Access & Account Troubleshooting**

https://youtu.be/9gbBT981Lso

---

## Skills Demonstrated

This project provided practical experience across several areas of Windows and Active Directory administration:

- Active Directory Domain Services administration
- Organisational Unit design and management
- User and computer account administration
- Active Directory security groups
- PowerShell administration
- Windows 11 domain integration
- DNS configuration and verification
- Group Policy creation, linking, testing, and scope verification
- Password reset and authentication troubleshooting
- Account lockout diagnosis and recovery
- Disabled account troubleshooting
- Security group membership troubleshooting
- Shared-folder configuration
- Share and NTFS permissions
- Group-based resource access
- Windows security token awareness
- Structured troubleshooting and post-resolution verification

## Troubleshooting Approach

Across the support scenarios, I followed a consistent troubleshooting process:

1. **Reproduce the issue** to confirm the user's reported problem.
2. **Gather information** about the affected user, computer, policy, or resource.
3. **Identify the cause** using Active Directory tools, PowerShell, and Windows administrative utilities.
4. **Apply the appropriate resolution** while keeping permissions and access aligned with the intended configuration.
5. **Verify the fix** from both the administrative side and the user's perspective.

This approach helped ensure that each issue was not only resolved but that the underlying cause and final system state were also verified.

## Key Takeaways

This project strengthened my understanding of how Active Directory components work together in a domain environment.

Some of the main lessons from the lab were:

- DNS configuration is critical for Active Directory domain discovery and authentication.
- Organisational Units provide a logical structure for managing users, computers, and Group Policy.
- Security groups provide a more scalable way to manage resource permissions than assigning access directly to individual users.
- Group Policy should be tested for both successful application and correct scope.
- A successful administrative change does not always mean the user's issue is immediately resolved. Changes such as group membership may require the user's security token to be refreshed.
- PowerShell provides an effective way to verify Active Directory object properties and account states alongside GUI administration tools.
- Troubleshooting should finish with user-level verification to confirm that the original problem has actually been resolved.

---

## Video Series

The complete project is demonstrated in four videos:

### Part 1 — OUs, Users, Groups & PowerShell

https://youtu.be/0gYJaJS8o2g

Active Directory structure, departmental OUs, user accounts, security groups, group membership, and PowerShell administration.

### Part 2 — Windows 11 Deployment & Domain Join

https://youtu.be/PrVv_r_26vE

Windows 11 client configuration, DNS verification, domain controller discovery, domain join, domain authentication, and computer object management.

### Part 3 — Group Policy & User Account Troubleshooting

https://youtu.be/2KMTo59hupU

Group Policy creation, application, testing, scope verification, and user account administration.

### Part 4 — Group Membership, Folder Access & Account Troubleshooting

https://youtu.be/9gbBT981Lso

Password troubleshooting, account lockout, security group membership, shared-folder access, permissions, disabled account diagnosis, resolution, and verification.

---

## Project Outcome

The completed lab demonstrates the administration of a small Windows domain environment from initial organisational configuration through client integration, policy management, resource permissions, and end-user troubleshooting.

The project combines Active Directory administration with realistic IT support scenarios rather than focusing only on initial configuration. Each troubleshooting scenario was reproduced, investigated, resolved, and verified to demonstrate the complete support process.

This project forms part of my hands-on IT support portfolio.
