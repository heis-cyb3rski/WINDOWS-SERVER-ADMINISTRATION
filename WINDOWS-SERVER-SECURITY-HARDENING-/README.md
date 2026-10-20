# Project 2: Windows Server Security & Hardening Lab

## 1. Project Overview

### Project Description

This project involved assessing and hardening a Windows Server 2025 environment configured as an Active Directory Domain Controller. The objective was to identify security weaknesses, implement appropriate security controls, reduce unnecessary attack surfaces, strengthen account and network security, configure security auditing, and verify the effectiveness of the changes.

The project was performed in a local virtualised lab using Microsoft Hyper-V. The environment was based on an existing Active Directory domain, `thasage.local`.

The implementation followed a structured security workflow:

1. Prepare the lab and define the security scope.
2. Assess the existing security configuration.
3. Review account security and administrative privileges.
4. Harden Windows Defender Firewall.
5. Reduce the attack surface by disabling unnecessary services.
6. Strengthen Microsoft Defender protections.
7. Review Remote Desktop security.
8. Harden SMB configuration and file-sharing access.
9. Configure security auditing and event monitoring.
10. Perform controlled security-event tests.
11. Verify implemented security controls.
12. Perform final validation and document outstanding issues.
13. Prepare the project for publication on GitHub.

### Project Objectives

The main objectives were to:

* Assess the existing Windows Server security posture.
* Establish a security baseline before making changes.
* Strengthen firewall configuration and restrict unnecessary network access.
* Reduce exposure by disabling services that were not required.
* Enable relevant Microsoft Defender security features.
* Improve SMB protocol security.
* Review account security and administrative access.
* Configure Windows security auditing.
* Generate and investigate security events.
* Compare the original configuration with the final configuration.
* Document the implementation process and supporting evidence.

## 2. Lab Environment and Architecture

### Lab Components

| Component              | Configuration                            |
| ---------------------- | ---------------------------------------- |
| Hypervisor             | Microsoft Hyper-V                        |
| Server                 | DC01                                     |
| Operating system       | Windows Server 2025                      |
| Server role            | Active Directory Domain Services and DNS |
| Domain                 | thasage.local                            |
| DC01 IP address        | 192.168.10.10                            |
| Client workstation     | CLIENT01                                 |
| CLIENT01 IP address    | 192.168.10.11                            |
| Additional workstation | CLIENT02                                 |
| Lab network            | 192.168.10.0/24                          |

The environment used a private lab network to support the implementation and testing of Windows Server security controls.

DC01 was the primary server used for configuration and verification. CLIENT01 was used for selected client-side tests, including an attempted failed-authentication test.

### Tools Used

**1. Microsoft Hyper-V**

Used to host the virtual machines and provide an isolated environment for implementing and testing security configurations.

A Hyper-V checkpoint was created before the hardening activities. This provided a recovery point if a configuration change caused an unexpected problem.

**2. Server Manager**

Used to inspect server roles and configuration and to review the Remote Desktop setting.

**3. Active Directory Users and Computers (ADUC)**

Used to create the `IT.Admin` account and configure its membership in the Domain Admins group.

**4. Windows PowerShell**

Used to inspect and configure Windows security settings, including firewall rules, services, Microsoft Defender, SMB configuration, Active Directory accounts, and security-related settings.

**5. Windows Defender Firewall with Advanced Security**

Used to review and modify inbound firewall rules and restrict access to selected network services.

**6. Windows Security and Microsoft Defender settings**

Used to enable additional endpoint security protections, including Tamper Protection and potentially unwanted application blocking.

**7. Event Viewer**

Used to investigate Windows Security events, including process-creation events generated during testing.

**8. Audit Policy tools**

The `auditpol` command-line utility was used to configure and verify Windows audit policies.

**9. Command Prompt**

Used on CLIENT01 for selected network and authentication-related troubleshooting.

**10. GitHub**

Used as the intended publication platform for the project documentation, configuration evidence, test results, and lessons learned.

---

# 3. Implementation Stages

## Stage 1: Lab Preparation and Security Scope

### Purpose

The purpose of this stage was to prepare the virtual environment before making security changes.

Applying security settings without a recovery point can introduce operational problems, particularly on a domain controller that provides authentication and directory services.

### Activities Performed

1. Identified DC01 as the primary system for security hardening.
2. Confirmed that the project would use the existing `thasage.local` Active Directory environment.
3. Identified the lab network and the participating virtual machines.
4. Created a Hyper-V checkpoint before implementing the hardening changes.
5. Established the project scope around account security, firewall configuration, service reduction, endpoint protection, SMB security, and auditing.

### Tools Used

* Microsoft Hyper-V
* Windows Server 2025
* Server Manager

### Outcome

The lab was prepared, and a recovery checkpoint was created before the implementation began.

**Evidence to include:** A screenshot of the Hyper-V virtual machines and the checkpoint, if available.

---

## Stage 2: Security Baseline Assessment

### Purpose

The purpose of the baseline assessment was to understand the server's existing security configuration before changing it.

A baseline provides a reference point for determining which controls were already enabled, which settings required improvement, and how the environment changed after hardening.

### Activities Performed

The existing configuration was inspected in the following areas.

#### 2.1 Windows Defender Firewall

The firewall configuration was examined across the Domain, Private, and Public profiles.

The initial assessment identified that the firewall was enabled and that its effective inbound policy blocked unsolicited inbound traffic by default.

The baseline also identified enabled inbound rules for services such as SMB, Windows Remote Management, and OpenSSH.

#### 2.2 Microsoft Defender

The existing endpoint protection configuration was reviewed.

The assessment confirmed that core Microsoft Defender protections, including antivirus and real-time protection, were enabled. Other protections were subsequently reviewed and configured during Stage 6.

#### 2.3 Accounts and Administrative Privileges

Active Directory accounts were inspected to understand the existing account configuration.

The built-in Administrator account was identified as a highly privileged account. The Guest account was also examined, along with the existing domain user accounts.

#### 2.4 Services and Network Exposure

Running services, startup configurations, and relevant listening ports were examined.

The assessment identified services associated with Active Directory, DNS, Kerberos, SMB, Windows Remote Management, and other server functions.

The presence of a service or listening port was not automatically treated as a vulnerability. Essential domain controller services must remain available for Active Directory and DNS to function correctly.

The Print Spooler and Remote Registry services were identified for further review.

#### 2.5 Security Auditing

The existing Windows audit policy was examined using Windows audit-policy tools.

The baseline showed that some security events were already being audited, while several relevant audit categories were not configured as required for the project.

### Tools Used

* PowerShell
* Active Directory Users and Computers
* Windows Defender Firewall with Advanced Security
* Windows Security
* `auditpol`
* Windows service and network inspection commands

### Outcome

The baseline established the original configuration against which the hardening changes could be compared.

It also identified areas for improvement, including selected inbound firewall rules, unnecessary services, additional Defender protections, SMB security, and auditing.

**Evidence to include:** Screenshots or saved outputs showing the initial firewall configuration, Defender status, account configuration, service status, and audit-policy settings.

---

## Stage 3: Account Security and Least Privilege

### Purpose

This stage focused on administrative account management and the principle of least privilege.

The principle of least privilege means that an account should receive only the permissions necessary to perform its assigned responsibilities.

Separating administrative accounts from ordinary user accounts can reduce the risk of privileged credentials being exposed during routine activities.

### Activities Performed

1. Opened Active Directory Users and Computers.
2. Created a new administrative account named `IT.Admin`.
3. Added `IT.Admin` to the Domain Admins group.
4. Captured a screenshot of the account configuration.

The new account provided a separate identity for administrative activities rather than relying exclusively on the built-in Administrator account.

### Tools Used

* Active Directory Users and Computers
* Active Directory Domain Services

### Outcome

The `IT.Admin` account was created and added to Domain Admins.

However, Domain Admins membership grants extensive privileges throughout the domain. Consequently, this implementation demonstrates separate administrative account creation, but it does **not** establish a fully least-privileged administrative model.

A production environment would normally use delegated administrative permissions wherever possible and separate highly privileged accounts from routine user accounts.

### Security Consideration

The account's creation and group membership should not be presented as proof that all administrative privileges were reduced.

**Evidence to include:** The screenshot showing the creation of `IT.Admin` and its administrative group configuration.

---

## Stage 4: Windows Defender Firewall Hardening

### Purpose

The purpose of this stage was to reduce unnecessary inbound network exposure and restrict access to selected management services.

Windows Defender Firewall provides network traffic filtering based on configured rules, profiles, protocols, ports, and remote addresses.

### Activities Performed

#### 4.1 Reviewed the Effective Firewall Policy

The effective firewall configuration was examined for the Domain, Private, and Public profiles.

The verification confirmed that the firewall was enabled across all three profiles.

The effective default inbound action was Block, while the default outbound action was Allow.

#### 4.2 Disabled the OpenSSH Inbound Rule

The inbound firewall rule named `OpenSSH SSH Server (sshd)` was disabled.

This change reduced the permitted inbound exposure associated with that rule because SSH access was not required for the lab's intended administration workflow.

The rule was disabled rather than deleted.

#### 4.3 Restricted Windows Remote Management

The Domain-profile `Windows Remote Management (HTTP-In)` rule was configured to restrict remote addresses to the lab subnet:

`192.168.10.0/24`

This limited the permitted source addresses for that rule to the lab network.

The rule uses TCP port 5985 for HTTP-based WinRM management.

The inspection also identified a separate enabled WinRM rule associated with the Public profile. The project did not establish that this Public-profile rule had been restricted in the same way, so the restriction must not be described as applying to every WinRM rule or profile.

#### 4.4 Restricted SMB Inbound Access

The enabled `File and Printer Sharing (SMB-In)` rule was modified to restrict remote addresses to:

`192.168.10.0/24`

The rule's remote-address configuration was subsequently checked and confirmed.

This limited SMB inbound access through that rule to the specified lab subnet.

### Tools Used

* Windows Defender Firewall with Advanced Security
* PowerShell
* `Get-NetFirewallProfile`
* `Get-NetFirewallRule`
* `Get-NetFirewallAddressFilter`
* `Set-NetFirewallAddressFilter`

### Outcome

The firewall remained enabled across all profiles, with inbound traffic blocked by default.

The OpenSSH inbound rule was disabled, and the selected WinRM and SMB inbound rules were restricted as described above.

### Security Benefit

Restricting inbound access to trusted network ranges reduces exposure to systems outside the intended administration network.

**Evidence to include:**

* Effective firewall profile configuration.
* OpenSSH rule showing that it is disabled.
* WinRM rule and its configured remote-address scope.
* SMB inbound rule showing the `192.168.10.0/24` remote-address restriction.

---

## Stage 5: Services and Attack Surface Reduction

### Purpose

This stage focused on reducing the server's attack surface by disabling services that were not required for the lab's intended server functions.

Every unnecessary service can introduce additional software components, permissions, or network functionality that must be maintained and secured.

Services must nevertheless be evaluated carefully on a domain controller because disabling essential services can interrupt authentication, DNS, directory replication, or other critical functions.

### Activities Performed

#### 5.1 Disabled Print Spooler

The Print Spooler service was initially running with Automatic startup.

The service was stopped, and its startup type was changed to Disabled.

The service was not required for the intended DC01 lab role because the server was not being used as a print server.

#### 5.2 Disabled Remote Registry

Remote Registry was initially stopped but configured with Automatic startup.

Its startup type was changed to Disabled.

This prevented the service from starting normally while disabled and reduced unnecessary remote registry-management functionality.

#### 5.3 Reviewed Remote Desktop

Remote Desktop was inspected through Server Manager.

The service was already disabled, so no additional configuration change was required.

#### 5.4 Reviewed Other Components

The Xbox Live Auth Manager service was inspected but left unchanged because it was not considered necessary to alter for the scope of this lab.

The inbound OpenSSH firewall rule had already been disabled during Stage 4.

### Final Verification

PowerShell was used to verify the state and startup configuration of the two services.

The results confirmed:

| Service         | State   | Startup type |
| --------------- | ------- | ------------ |
| Print Spooler   | Stopped | Disabled     |
| Remote Registry | Stopped | Disabled     |

### Tools Used

* Server Manager
* PowerShell
* Windows Services management
* `Get-CimInstance`

### Outcome

Print Spooler and Remote Registry were confirmed stopped and disabled.

Remote Desktop was already disabled.

### Security Benefit

Disabling unnecessary services reduces the number of components that could be exposed to misuse or exploitation.

**Evidence to include:** The final PowerShell output showing the state and startup type of Print Spooler and Remote Registry.

---

## Stage 6: Microsoft Defender Security

### Purpose

The purpose of this stage was to strengthen built-in endpoint security protections on DC01.

Microsoft Defender provides antivirus, real-time monitoring, reputation-based protection, and other security capabilities that help detect and prevent malicious activity.

### Activities Performed

#### 6.1 Reviewed Cloud-Delivered Protection

Cloud-delivered protection was inspected and confirmed to be enabled.

This feature supports threat detection by using cloud-based security intelligence.

#### 6.2 Reviewed Automatic Sample Submission

Automatic sample submission was inspected and confirmed to be enabled.

This setting supports the submission of certain suspicious samples to Microsoft for analysis, subject to Microsoft's applicable policies and configuration.

#### 6.3 Enabled Tamper Protection

Tamper Protection was initially found to be disabled.

It was subsequently enabled through the Microsoft Defender security settings.

Tamper Protection helps prevent certain security settings from being changed through unauthorised or unsupported methods.

#### 6.4 Enabled Potentially Unwanted Application Protection

Potentially unwanted application protection was initially disabled.

It was enabled to improve protection against applications that may exhibit unwanted or undesirable behaviour.

#### 6.5 Enabled Network Protection

Network Protection was initially found to be disabled.

The following PowerShell command was used to enable it:

```powershell
Set-MpPreference -EnableNetworkProtection Enabled
```

The resulting configuration was checked using `Get-MpPreference`.

The output confirmed that Network Protection was enabled.

### Verification Performed

The following settings were checked:

```powershell
Get-MpPreference | Select-Object EnableNetworkProtection, PUAProtection
```

The output confirmed that Network Protection and potentially unwanted application protection were enabled.

The principal Microsoft Defender protections were also verified:

```powershell
Get-MpComputerStatus |
    Select-Object AntivirusEnabled,
                  RealTimeProtectionEnabled,
                  BehaviorMonitorEnabled,
                  IoavProtectionEnabled
```

All four properties returned `True`.

| Protection                                  | Verified result                   |
| ------------------------------------------- | --------------------------------- |
| Antivirus                                   | Enabled                           |
| Real-time protection                        | Enabled                           |
| Behaviour monitoring                        | Enabled                           |
| Downloaded-file and attachment scanning     | Enabled                           |
| Network Protection                          | Enabled                           |
| Potentially unwanted application protection | Enabled                           |
| Tamper Protection                           | Enabled through Defender settings |

### Tools Used

* Windows Security
* Microsoft Defender settings
* PowerShell
* `Set-MpPreference`
* `Get-MpPreference`
* `Get-MpComputerStatus`

### Outcome

The selected Microsoft Defender protections were enabled and verified as described above.

### Security Benefit

These protections improve the server's ability to detect or prevent malicious files, suspicious behaviour, potentially unwanted applications, and certain network-based threats.

**Evidence to include:** Screenshots of Tamper Protection and potentially unwanted application protection, together with the PowerShell output confirming the Defender settings.

---

## Stage 7: Remote Desktop Security

### Purpose

The purpose of this stage was to review Remote Desktop exposure and determine whether remote graphical administration was necessary on DC01.

Remote Desktop can provide convenient administrative access, but unnecessary exposure increases the number of remote-access paths that must be secured.

### Activities Performed

1. Opened Server Manager on DC01.
2. Inspected the Remote Desktop setting under Local Server.
3. Confirmed that Remote Desktop was disabled.
4. Left the existing setting unchanged.

### Tools Used

* Server Manager
* Windows Server configuration settings

### Outcome

Remote Desktop was already disabled, so no additional configuration change was required.

### Security Benefit

Keeping Remote Desktop disabled when it is not required reduces the server's exposure to RDP-based access attempts.

This does not mean that all forms of remote administration were disabled; other management mechanisms, such as Windows Remote Management, were configured separately.

**Evidence to include:** A screenshot of the Server Manager Local Server page showing Remote Desktop disabled.

---

## Stage 8: SMB Security Hardening

### Purpose

The purpose of this stage was to improve Server Message Block (SMB) security and review the permissions controlling access to a shared folder.

SMB supports Windows file sharing and is commonly used in domain environments. Its configuration and access permissions therefore form an important part of server security.

### Activities Performed

#### 8.1 Disabled SMB1

The SMB server configuration was inspected.

The results confirmed that SMB1 was disabled, while SMB2/3 support remained enabled.

The final verified values were:

| Setting            | Result |
| ------------------ | ------ |
| EnableSMB1Protocol | False  |
| EnableSMB2Protocol | True   |

SMB1 was therefore disabled, while modern SMB protocol support remained available.

#### 8.2 Required SMB Signing

The SMB configuration also confirmed that SMB signing was required.

The relevant setting was:

`RequireSecuritySignature = True`

SMB signing helps protect the integrity of SMB communications by applying cryptographic signatures to SMB messages.

#### 8.3 Reviewed SMB Shares

The SMB shares on DC01 were inspected.

The environment included administrative shares, Active Directory-related shares, and an HR share.

The HR share was located at:

`C:\shared\HR`

The administrative and system shares were left unchanged.

#### 8.4 Reviewed HR Share Permissions

The share-level access configuration for HR was inspected.

The results showed that the domain group `THASAGE\GG_HR` had Allow: Change permission on the share.

This provided evidence that a designated HR group was being used to control share-level access.

#### 8.5 Inspected NTFS Permissions

The NTFS permissions on the HR folder were inspected.

The initial inspection identified inherited permissions involving `BUILTIN\Users`, in addition to administrative and system permissions.

Inheritance was subsequently disabled on `C:\shared\HR`, with the option to convert inherited permissions into explicit permissions selected in the graphical interface.

However, the final removal and verification of the broad `BUILTIN\Users` permissions were not completed as part of the verified work.

Consequently, this project documents the permissions review and inheritance change, but does not claim that the HR folder's NTFS permissions were fully hardened.

#### 8.6 Reviewed SMB Encryption

The `Get-SmbShare` results showed that `EncryptData` was `False` for the inspected shares.

SMB encryption was therefore not established as an implemented control in this project.

The administrative and Active Directory system shares were not modified simply to enable encryption.

### Tools Used

* PowerShell
* SMB server configuration cmdlets
* SMB share and share-access cmdlets
* Windows folder Security properties
* NTFS Access Control Lists (ACLs)

### Outcome

The following controls were verified:

* SMB1 disabled.
* SMB2/3 enabled.
* SMB signing required.
* HR share-level access assigned to `THASAGE\GG_HR`.

The HR NTFS permissions were reviewed, but full NTFS permission hardening was not verified. SMB encryption was also not implemented as a verified control.

### Security Benefit

Disabling SMB1 reduces reliance on a legacy protocol. Requiring SMB signing helps protect message integrity, while reviewing share and NTFS permissions helps identify opportunities to restrict access to sensitive files.

**Evidence to include:**

* SMB server configuration showing SMB1 disabled and SMB signing required.
* HR share-access output showing the designated HR group.
* Any available screenshot documenting the NTFS permission review.

---

## Stage 9: Auditing and Security Event Monitoring

### Purpose

This stage focused on configuring Windows audit policies so that selected security-related activities could be recorded and investigated.

Security auditing provides visibility into actions performed on a server. It can support troubleshooting, incident investigation, account monitoring, and the detection of suspicious activity.

### Activities Performed

#### 9.1 Enabled Process Creation Auditing

Process Creation auditing was enabled for successful events using:

```powershell
auditpol /set /subcategory:"Process Creation" /success:enable
```

The audit policy was then verified.

This configuration allowed Windows to record successful process-creation activity in the Security event log.

#### 9.2 Generated a Test Process-Creation Event

Notepad was opened on DC01 to generate a process-creation event.

The Security log was then queried for Event ID 4688.

The resulting event showed:

* Event ID: `4688`
* New process: `C:\Windows\System32\notepad.exe`
* Creator process: `C:\Windows\explorer.exe`
* Account context: Administrator in the THASAGE domain

The event confirmed that the process-creation audit configuration was working for the test activity.

#### 9.3 Enabled Additional Audit Categories

Success and Failure auditing were enabled for the following categories:

* Account Lockout
* User Account Management
* File Share
* Credential Validation
* Kerberos Authentication Service

The intention was to improve visibility into account-management activity, authentication attempts, account lockouts, and file-share access.

#### 9.4 Reviewed the Security Log

The Security log was inspected during the test activities.

The baseline inspection had shown that the Security log was configured with a 128 MB circular logging capacity.

### Tools Used

* PowerShell
* `auditpol`
* Windows Event Viewer
* Windows Security event log
* Event ID 4688

### Outcome

Process Creation auditing was enabled and verified by generating and locating a real Event ID 4688 event.

The additional audit categories were configured.

However, the subsequent failed-logon test did not produce the expected authentication-failure events in the logs that were inspected.

Therefore, successful capture of the failed-logon test was not established.

### Security Benefit

Process-creation auditing can help administrators identify which processes were launched and under which account context.

Authentication and account-management auditing can provide valuable evidence during investigations, provided the relevant events are generated, recorded, retained, and correctly queried.

**Evidence to include:** The Event ID 4688 record showing Notepad process creation and the relevant audit-policy configuration.

---

## Stage 10: Controlled Security Event Simulation

### Purpose

The purpose of this stage was to generate controlled security-related activity and determine whether the configured auditing could record the expected events.

Testing is important because enabling an audit policy does not, by itself, prove that the intended event will be captured under every test condition.

### Activities Performed

#### 10.1 Process-Creation Test

Notepad was opened on DC01.

The Security log was queried, and Event ID 4688 was found.

The event included the process name and creator process.

**Result: Successful.**

This demonstrated that process-creation auditing was recording the tested activity.

#### 10.2 Failed-Authentication Test

A controlled failed sign-in was attempted on CLIENT01 using a non-privileged test account.

The relevant logs on CLIENT01 and DC01 were inspected for authentication-failure events, including:

* Event ID 4625 — failed logon.
* Event ID 4771 — Kerberos pre-authentication failure.
* Event ID 4776 — credential validation.

The client was confirmed to be joined to `thasage.local`, and a domain controller lookup identified DC01 at `192.168.10.10`.

However, the expected failure events were not located in the inspected logs.

The test was not repeatedly attempted after the expected events failed to appear.

**Result: Inconclusive.**

The absence of the expected records means the test did not establish that the failed authentication was captured by the auditing configuration.

### Tools Used

* CLIENT01
* DC01
* Windows Event Viewer
* PowerShell
* Windows Security event logs
* `nltest` for domain-controller discovery

### Outcome

The process-creation test was successful. The failed-authentication test did not produce the expected event evidence and remains inconclusive.

### Security Consideration

An unsuccessful event-capture test must be documented accurately. It should not be reported as successful simply because the relevant audit categories were enabled.

**Evidence to include:** The Event ID 4688 screenshot. If no failed-authentication event was captured, document the test outcome in the troubleshooting notes rather than presenting an event that does not exist.

---

## Stage 11: Security Verification and Before-and-After Comparison

### Purpose

The purpose of this stage was to verify the implemented security controls and establish an evidence-based comparison between the initial and final configurations.

### Verification Activities

#### 11.1 Firewall Verification

The effective firewall policy was inspected using PowerShell.

The final verification confirmed:

* Firewall enabled on the Domain profile.
* Firewall enabled on the Private profile.
* Firewall enabled on the Public profile.
* Default inbound action set to Block.
* Default outbound action set to Allow.

#### 11.2 SMB Firewall Verification

The `File and Printer Sharing (SMB-In)` rule was inspected.

The remote-address configuration confirmed the restriction to:

`192.168.10.0/24`

#### 11.3 OpenSSH Rule Verification

The OpenSSH SSH Server inbound firewall rule was checked.

The rule was confirmed disabled.

#### 11.4 Microsoft Defender Verification

The Defender configuration was checked again.

The verification confirmed that:

* Antivirus was enabled.
* Real-time protection was enabled.
* Behaviour monitoring was enabled.
* Downloaded-file and attachment scanning was enabled.
* Network Protection was enabled.
* Potentially unwanted application protection was enabled.

Tamper Protection had also been enabled through the Defender security settings.

#### 11.5 SMB Configuration Verification

The SMB server configuration was checked again.

The results confirmed:

* SMB1 disabled.
* SMB2/3 enabled.
* SMB signing required.

#### 11.6 Service Verification

The final service checks confirmed that Print Spooler and Remote Registry were both stopped and disabled.

#### 11.7 Account Verification

The built-in Guest account was inspected using:

```powershell
Get-ADUser Guest | Select-Object Name, Enabled
```

The output showed:

`Enabled = False`

This confirmed that the Guest account remained disabled.

#### 11.8 Auditing Verification

Process-creation auditing was verified using the generated Event ID 4688 record.

The failed-authentication event test remained inconclusive.

### Tools Used

* PowerShell
* Windows Defender Firewall
* Microsoft Defender
* SMB configuration cmdlets
* Windows service inspection
* Active Directory PowerShell module
* Windows Event Viewer

### Outcome

The verification stage confirmed that the documented firewall, Defender, SMB, service, account, and process-creation auditing settings were in place.

The final documentation should distinguish verified controls from incomplete or inconclusive tests.

---

## Stage 12: Troubleshooting and Final Validation

### Purpose

The purpose of this stage was to confirm key security settings, identify outstanding issues, and prepare the environment for final documentation.

The focus was on meaningful verification rather than repeatedly inspecting settings that had already been confirmed.

### Final Validation Results

| Area                                        | Final result                                               | Status                                |
| ------------------------------------------- | ---------------------------------------------------------- | ------------------------------------- |
| Firewall profiles                           | Enabled on Domain, Private, and Public                     | Verified                              |
| Default inbound policy                      | Block                                                      | Verified                              |
| Default outbound policy                     | Allow                                                      | Verified                              |
| SMB inbound firewall rule                   | Restricted to `192.168.10.0/24`                            | Verified                              |
| OpenSSH inbound rule                        | Disabled                                                   | Verified                              |
| Print Spooler                               | Stopped and disabled                                       | Verified                              |
| Remote Registry                             | Stopped and disabled                                       | Verified                              |
| Remote Desktop                              | Disabled                                                   | Verified                              |
| Defender Antivirus                          | Enabled                                                    | Verified                              |
| Real-time protection                        | Enabled                                                    | Verified                              |
| Behaviour monitoring                        | Enabled                                                    | Verified                              |
| Downloaded-file scanning                    | Enabled                                                    | Verified                              |
| Network Protection                          | Enabled                                                    | Verified                              |
| Potentially unwanted application protection | Enabled                                                    | Verified                              |
| Tamper Protection                           | Enabled                                                    | Configured                            |
| SMB1                                        | Disabled                                                   | Verified                              |
| SMB2/3                                      | Enabled                                                    | Verified                              |
| SMB signing                                 | Required                                                   | Verified                              |
| Guest account                               | Disabled                                                   | Verified                              |
| Process Creation auditing                   | Event ID 4688 captured                                     | Verified                              |
| Failed-authentication test                  | Expected events not captured                               | Inconclusive                          |
| HR NTFS permissions                         | Full hardening not verified                                | Outstanding                           |
| SMB encryption                              | Not established as enabled                                 | Not implemented as a verified control |
| WinRM Public-profile rule                   | Separate enabled rule identified; restriction not verified | Outstanding review                    |

### Troubleshooting Notes

#### Failed-Authentication Event Capture

The failed-authentication test did not produce the expected event records in the logs that were inspected.

The test was not repeatedly attempted after the initial investigation failed to locate the expected records.

This issue should remain documented as inconclusive.

#### HR Folder Permissions

The HR folder's permissions were inspected, and inheritance was disabled with inherited permissions converted into explicit permissions.

However, the final removal and verification of broad `BUILTIN\Users` NTFS permissions were not confirmed.

The folder should not be described as fully hardened.

#### WinRM Rule Scope

The Domain-profile WinRM rule was restricted to the lab subnet. A separate enabled Public-profile WinRM rule was also identified.

The Public-profile rule's remote-address restriction was not verified as part of the completed work. Its configuration remains an item for review before treating the WinRM configuration as fully hardened.

### Overall Outcome

The final validation confirmed several important security improvements, including reduced inbound exposure, disabled unnecessary services, stronger Defender protections, improved SMB protocol security, and verified process-creation auditing.

Some items remain outstanding or inconclusive and are documented accordingly.


# 4. Before-and-After Security Summary

The following table summarises the most important configuration changes and verified results.

| Security area                               | Initial state                         | Final state                                         | Security significance                                              |
| ------------------------------------------- | ------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------ |
| Firewall profiles                           | Enabled                               | Enabled across all profiles                         | Maintained firewall protection                                     |
| Default inbound firewall policy             | Block                                 | Block                                               | Prevents unsolicited inbound traffic unless permitted by rules     |
| OpenSSH inbound rule                        | Enabled                               | Disabled                                            | Removes access permitted by that inbound rule                      |
| WinRM Domain rule                           | Broader remote-address scope          | Restricted to `192.168.10.0/24`                     | Limits permitted remote management sources through that rule       |
| SMB inbound rule                            | Remote address Any                    | Restricted to `192.168.10.0/24`                     | Limits SMB access through the rule to the lab subnet               |
| Print Spooler                               | Running, Automatic                    | Stopped, Disabled                                   | Reduces unnecessary service exposure                               |
| Remote Registry                             | Stopped, Automatic                    | Stopped, Disabled                                   | Prevents normal startup of an unnecessary service                  |
| Remote Desktop                              | Disabled                              | Disabled                                            | Maintains the existing reduced RDP exposure                        |
| Defender Network Protection                 | Disabled                              | Enabled                                             | Adds network-based threat protection                               |
| Potentially unwanted application protection | Disabled                              | Enabled                                             | Improves protection against potentially unwanted applications      |
| Tamper Protection                           | Disabled                              | Enabled                                             | Helps protect selected security settings from unauthorised changes |
| SMB1                                        | Disabled                              | Disabled                                            | Maintains the exclusion of the legacy SMB1 protocol                |
| SMB signing                                 | Required                              | Required                                            | Helps protect SMB message integrity                                |
| Guest account                               | Disabled                              | Disabled                                            | Maintains the account's disabled state                             |
| Process Creation auditing                   | Existing audit configuration reviewed | Enabled for success and verified with Event ID 4688 | Provides process-creation visibility                               |

**Important interpretation:** Some controls were already correctly configured during the baseline assessment. The project improved the overall security posture through a combination of new changes and verification of existing protections. The table does not imply that every listed setting was changed from insecure to secure during the project.

---

# 5. Security Considerations and Limitations

This was a local learning lab designed to demonstrate practical Windows Server security administration.

The results should be interpreted within that scope.

The following limitations remain:

1. **Administrative privileges:** `IT.Admin` was added to Domain Admins. This demonstrates separate administrative account creation, but not a fully least-privileged administration model.
2. **HR NTFS permissions:** The permission review and inheritance change were performed, but the final NTFS permission hardening was not verified.
3. **SMB encryption:** Encryption was not established as enabled for the inspected shares.
4. **WinRM Public-profile rule:** A separate enabled rule was identified, and its final remote-address restriction was not verified.
5. **Failed-authentication auditing:** The controlled failed-sign-in test did not produce the expected events in the inspected logs.
6. **Production readiness:** A successful lab configuration does not, by itself, establish that the server meets all security, compliance, monitoring, backup, or operational requirements of a production environment.

These limitations are included to maintain an accurate record of the work and identify areas for future improvement.

---

# 6. Lessons Learned

This project provided practical experience in several areas of Windows Server security administration.

### 6.1 Baseline Assessment

Assessing the existing configuration before making changes helped distinguish existing protections from settings requiring improvement.

### 6.2 Firewall Rule Management

The project demonstrated how firewall profiles, inbound rules, protocols, ports, and remote-address filters affect permitted network access.

### 6.3 Attack Surface Reduction

Disabling Print Spooler and Remote Registry illustrated how unnecessary services can be removed from a server's active configuration.

### 6.4 Endpoint Protection

The project provided practical experience enabling and verifying Microsoft Defender security features through Windows Security and PowerShell.

### 6.5 SMB Security

Reviewing SMB protocols, signing requirements, share permissions, and NTFS permissions demonstrated the importance of protecting both file-sharing communications and the underlying data.

### 6.6 Security Auditing

Enabling Process Creation auditing and locating Event ID 4688 demonstrated how Windows records process activity for investigation.

The failed-authentication test also demonstrated an important operational lesson: enabling an audit category does not guarantee that a particular event will be generated or captured under every test condition.

### 6.7 Evidence-Based Verification

The project reinforced the importance of validating configuration changes rather than assuming that a command or graphical-interface change was successful.

### 6.8 Technical Documentation

Recording the tools used, the purpose of each configuration, the observed results, and outstanding issues is an important part of professional system administration.

---

# 7. Skills Demonstrated

The project demonstrates practical exposure to:

* Windows Server 2025 administration.
* Microsoft Hyper-V virtualisation.
* Active Directory account administration.
* Windows Defender Firewall management.
* PowerShell-based security configuration and verification.
* Microsoft Defender security configuration.
* Windows service management.
* Remote Desktop security review.
* SMB protocol and signing configuration.
* Share permissions and NTFS ACL inspection.
* Windows audit-policy configuration.
* Security Event Viewer investigation.
* Basic authentication-event troubleshooting.
* Security baseline assessment.
* Before-and-after configuration comparison.
* Technical documentation and evidence management.

---

# 8. Project Conclusion

The Windows Server Security & Hardening Lab provided practical experience assessing and improving the security configuration of a Windows Server 2025 domain controller.

The project began with a baseline assessment and progressed through account administration, firewall hardening, service reduction, Microsoft Defender configuration, Remote Desktop review, SMB security, and Windows security auditing.

Several controls were verified through PowerShell output and Windows security events. These included the restriction of selected inbound firewall rules to the lab subnet, disabling unnecessary services, enabling Microsoft Defender protections, disabling SMB1, requiring SMB signing, keeping the Guest account disabled, and successfully capturing a process-creation event.

The project also identified areas requiring further attention, including the final NTFS permissions on the HR folder, the separate Public-profile WinRM rule, and the failed-authentication event-capture test.

These limitations have been documented rather than represented as completed security improvements.

Overall, the project demonstrates a structured approach to Windows Server security administration: establish a baseline, implement appropriate controls, verify the results, investigate unexpected outcomes, and document the findings accurately.
