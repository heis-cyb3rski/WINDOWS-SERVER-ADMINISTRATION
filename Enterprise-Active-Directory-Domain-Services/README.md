# Enterprise-Active-Directory-Domain-Services

## Project Overview

This project is a fully local Windows Server 2025 Enterprise Active Directory lab built using Microsoft Hyper-V.

The objective was to simulate a small enterprise environment and demonstrate practical experience with:

Windows Server administration
- Active Directory Domain Services (AD DS)
- DNS
- Organizational Units (OUs)
- User and security group management
- Domain-joined Windows clients
- Group Policy
- SMB file sharing
- NTFS/share permissions
- PowerShell administration
- Network and service troubleshooting

The environment was designed and configured from the ground up without relying on Microsoft 365, Azure, Entra ID, or paid cloud services.

                         ┌─────────────────────────┐
                         │       DC01              │
                         │   Windows Server 2025   │
                         │                         │
                         │ AD DS                   │
                         │ DNS                     │
                         │ PowerShell Management   │
                         │ File Shares             │
                         │                         │
                         │ 192.168.10.10           │
                         └───────────┬─────────────┘
                                     │
                              AD-LAB-SWITCH
                                     │
                    ┌────────────────┴────────────────┐
                    │                                 │
             ┌──────▼──────┐                  ┌──────▼──────┐
             │  CLIENT01   │                  │  CLIENT02   │
             │ Windows 11  │                  │ Windows 11  │
             │ Domain      │                  │ Domain      │
             │ Joined      │                  │ Joined      │
             └─────────────┘                  └─────────────┘

                    Active Directory Domain:
                         thasage.local


## Lab Environment
| Component               | Configuration                                       |
| ----------------------- | --------------------------------------------------- |
| Hypervisor              | Microsoft Hyper-V                                   |
| Domain Controller       | DC01                                                |
| Server OS               | Windows Server 2025                                 |
| Client OS               | Windows 11                                          |
| Active Directory Domain | `thasage.local`                                     |
| DC01 IP Address         | `192.168.10.10`                                     |
| Virtual Network         | `AD-LAB-SWITCH`                                     |
| Domain Clients          | CLIENT01, CLIENT02                                  |
| Directory Service       | Active Directory Domain Services                    |
| DNS                     | Windows Server DNS                                  |
| File Sharing            | SMB                                                 |
| Management              | Server Manager, PowerShell, Group Policy Management |


## DC01 Virtual Machine
- Generation 2 VM
- 2 vCPU
- 4 GB RAM
- 60 GB virtual disk
- Windows Server 2025
- Connected to AD-LAB-SWITCH

## Project Objectives

The project was designed to demonstrate the ability to:

- Deploy a Windows Server environment using Hyper-V.
- Configure a Windows Server as an Active Directory Domain Controller.
- Configure DNS for Active Directory.
- Design an organizational structure using OUs.
- Create and manage domain users and security groups.
- Join Windows clients to the domain.
- Implement and test Group Policy.
- Configure departmental SMB file shares.
- Apply access permissions based on departmental groups.
- Perform administrative tasks using PowerShell.
- Troubleshoot Windows networking and firewall issues.
- Validate the completed environment.

## Implementation

## Stage 1 — Hyper-V Lab Infrastructure

The first stage involved creating the virtualized infrastructure required for the lab.

A dedicated Hyper-V virtual network named: AD-LAB-SWITCH
was created to allow the virtual machines to communicate with each other.

DC01 was deployed as a Generation 2 Windows Server 2025 virtual machine

### DC01 Configuration

Hostname: DC01
OS: Windows Server 2025
CPU: 2 vCPU
RAM: 4 GB
Storage: 60 GB
Network: AD-LAB-SWITCH
IP: 192.168.10.10

### Evidence

Screenshot: Hyper-V Manager showing DC01 and the lab virtual machines.

Shows the virtualized enterprise lab infrastructure and deployed virtual machines.

![Image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/42f7aaad297358908adf8eeb49222f97a715e7b9/Enterprise-Active-Directory-Domain-Services/Hyper-V%20Switch%20.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01-%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%203.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%205.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%206.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%207.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/5391fbc19f8757f3edbb515bdeb260ff5948d0a4/Enterprise-Active-Directory-Domain-Services/Creating%20New%20DC01%20-%208.png)

## Stage 2 — Windows Server Configuration

Windows Server 2025 was installed on DC01 and the server was configured as the central infrastructure server.

The server was assigned a static IP address:

The hostname was configured as: DC01
This server would eventually provide Active Directory, DNS and file-sharing services.

### Evidence

Screenshot: DC01 Server Manager / system configuration.

Shows the configured Windows Server 2025 domain controller.

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20setup%201.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%203.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%205.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%206.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%207.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%208.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%2010.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%2011.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/51d7f359ca8f01469484102fd465a623e218b0f0/Enterprise-Active-Directory-Domain-Services/Windows%20Server%20Setup%2012.png)

## Stage 3 — Active Directory Domain Services

The Active Directory Domain Services role was installed on DC01.

DC01 was then promoted to a new forest and domain.

Domain: thasage.local
After promotion, DC01 became the domain controller responsible for authentication and directory services.

### Key Services
- Active Directory Domain Services
- DNS Server
- Kerberos authentication
- LDAP directory services


### Evidence
Screenshot: Server Manager showing AD DS installed.

Confirms that Active Directory Domain Services was installed on DC01.

Screenshot: Active Directory Users and Computers showing the thasage.local domain.

Confirms successful creation of the Active Directory domain.

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%201.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%203.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%205.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%206.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%207.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%208.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%209.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2010.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2011.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2012.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2013.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2014.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2015.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2016.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2017.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2018.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2019.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/ea21450f8792017ec9811b9a0bdfe18f4bf2061e/Enterprise-Active-Directory-Domain-Services/AD%20Role%20Installation%2020.png)


## Stage 4 — Organizational Units and Directory Structure

Five departmental Organizational Units (OUs) were created in Active Directory to represent the organization's departmental structure:

- IT
- HR
- CLIENTSERVICE
- PROCUREMENT
- FINANCE

The OUs were used to logically organize users and provide appropriate targets for Group Policy and administrative management.

thasage.local
│
├── IT
├── HR
├── CLIENTSERVICE
├── PROCUREMENT
└── FINANCE

### Purpose of the OU structure

The departmental OUs provide a structured Active Directory environment that allows administrators to:

- Organize users according to department
- Apply department-specific Group Policies
- Manage users and resources more efficiently
- Separate administrative boundaries
- Scale the directory as the organization grows

### Evidence — Active Directory Users and Computers

Shows the five departmental OUs created within the thasage.local Active Directory domain.

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/OU%20creation%201.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/OU%20creation%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/OU%20creation%203.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/OU%20creation%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/OU%20creation%205.png)


## Stage 5 — User and Security Group Management

Users and security groups were created and organized according to the departmental structure established in Active Directory.

The departments represented in the lab were:

- IT
- HR
- CLIENTSERVICE
- PROCUREMENT
- FINANCE

Security groups were used to manage access to departmental resources rather than assigning permissions individually to each user.

For example, the HR security group was later used to control access to the HR departmental file share.

This demonstrated the principle of:

User
  ↓
Security Group
  ↓
Resource Permission
  ↓
Access / Denied

### SECURITY GROUPS CREATED:
- GG_IT
- GG_HR
- GG_PROC
- GG_ClientService
- GG_Finance

### Evidence — Active Directory Users and Computers

Shows the departmental users, security groups, and organizational structure used to manage access within the domain. 

### Creating Security Groups for Each OU

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%201.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%203.png)


### Creating Each Users and adding them to their OUs

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Creating%20User%20.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%205.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%206.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%207.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%208.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%209.png)

### Using Powershell to View Created Security Groups:

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%2010.png)

### Created a Powershell Script that automated the creation of new users, adding them to their OUs, Security groups

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%2011.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%2012.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%2013.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/d43f3fb9f6f2f59034343eff465635a384ef9777/Enterprise-Active-Directory-Domain-Services/Security%20group%2014.png)








## Stage 6 — Domain-Joining Windows Clients

Windows client machines were connected to the thasage.local domain.

The primary clients used in the lab were: 
- CLIENT01
- CLIENT02

The clients were configured to communicate with the domain controller and authenticate domain users

### Validation

Users were able to sign into Windows using domain credentials.

### Evidence

Screenshot: CLIENT01 showing the domain configuration.

Demonstrates that the Windows client was successfully joined to the Active Directory domain.

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%201.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%202.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%203.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%204.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%205.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%206.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%207.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%208.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%209.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%2010.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%2011.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%2012.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%2013.png)

![image](https://github.com/heis-cyb3rski/WINDOWS-SERVER-ADMINISTRATION/blob/deb69a0d20f6168e264accb3dcbb2f3bdef91197/Domain%20Joining%20Window%20Clients%2014.png)



## Stage 7 — Group Policy

Group Policy was implemented to demonstrate centralized configuration management.

A number of policies were tested during the project.

One of the practical tests involved configuring an Interactive Logon security option:

Interactive logon:
Don't display last signed-in user

The policy was linked to the relevant organizational units and tested from the client environment.

Group Policy Management was used to:

- Create GPOs
- Configure policies
- Link policies to OUs
- Update policy on domain clients
- Validate policy behavior
- Evidence

Screenshot: Group Policy Management showing the configured GPO.
Shows centralized Group Policy configuration and OU-based policy management.

## Stage 8 — SMB File Sharing and Departmental Permissions

A departmental HR file share was created on DC01.

LOCAL FOLDER: C:\shared\HR
NETWORK SHARE: \\DC01\HR

The share was configured with permissions based on the HR security group.

The purpose was to demonstrate departmental resource access rather than giving every domain user unrestricted access.

### HR User Access Test
The HR user successfully accessed: \\DC01\HR
The user was able to:

- Open the folder
- Create a file
- Edit the file
- Save the changes

A test file was created: HR-Test.txt

### Evidence

Screenshot: HR user creating/editing HR-Test.txt.

Demonstrates that an authorized HR user has read/write access to the departmental share.

### Unauthorized Access Test

An IT user attempted to access: \\DC01\HR

The IT user received: Access Denied

This confirmed that departmental permissions were functioning correctly.

### Evidence

Screenshot: IT user receiving Access Denied.

Demonstrates that unauthorized departmental users cannot access the HR share.

## Stage 9 — PowerShell Administration

PowerShell was used extensively to perform administrative and validation tasks.

### Active Directory Domain

</> Powershell:
Get-ADDomain

Used to verify the Active Directory domain configuration.

### Domain Controller

</> Powershell:
Get-ADDomainController

Used to verify domain controller information.

### Domain Users

</> Powershell:
Get-ADUser -Filter * | Select-Object Name, SamAccountName, Enabled

Used to enumerate domain users and account status.

### Security Groups

</> Powershell:
Get-ADGroup -Filter * | Select-Object Name, GroupScope, GroupCategory

Used to verify Active Directory security groups.

### Organizational Units

</> Powershell:
Get-ADOrganizationalUnit -Filter * | Select-Object Name, DistinguishedName

Used to verify the OU structure.


## Stage 10 — Active Directory and Network Health Checks

Several built-in troubleshooting tools were used to validate the environment.

### Domain Controller Diagnostics

</> Powershell: dcdiag

Used to perform domain controller health diagnostics.

### DNS Diagnostics

</> Powershell: dcdiag /test:dns

Used to test the DNS configuration supporting Active Directory.

### Network Configuration

</> Powershell: Get-NetIPConfiguration

Used to inspect IP addressing, gateway and DNS configuration.

### Network Adapter

</> Powershell: Get-NetAdapter

Used to verify network adapter status.

### DNS Resolution

</> Powershell: Resolve-DnsName thasage.local

Used to verify domain DNS resolution.

## Troubleshooting Case Study — DC01 to CLIENT01 Connectivity

During the lab, a real connectivity issue was encountered.

### Problem

DC01 could resolve CLIENT01 through DNS, but connectivity tests initially failed.

### DNS Test:

</> Powershell: Resolve-DnsName CLIENT01

Result: Successful.

This established that DNS name resolution was functioning.

### SMB Connectivity Test

</> Powershell: Test-NetConnection CLIENT01 -Port 445

Initially Returned: 

TcpTestSucceeded : False

### Investigation

The LanmanServer service on CLIENT01 was checked:

</> Powershell: Get-Service LanmanServer

The service was: Running 

The next check examined the Windows Firewall rules:

</> Powershell: Get-NetFirewallRule -DisplayGroup "File and Printer Sharing" |
Select-Object DisplayName, Enabled, Direction, Action

The relevant File and Printer Sharing rules were disabled.

### Resolution

The firewall rules were enabled using PowerShell:

</> Powershell: Set-NetFirewallRule -DisplayGroup "File and Printer Sharing" -Enabled True

The configuration was then verified.

### Verification

</> Powershell: Test-NetConnection CLIENT01 -Port 445

Result
TcpTestSucceeded : True

ICMP connectivity was also retested:
</> Powershell: Test-Connection CLIENT01 -Count 4

The test returned successful replies.

### Troubleshooting Result

- DNS Resolution       → PASS
- Server Service       → Running
- SMB Port 445         → Initially blocked
- Firewall Rules       → Disabled
- Firewall Corrected   → Yes
- SMB Connectivity     → PASS
- Ping Connectivity    → PASS


### What this demonstrated

This troubleshooting exercise demonstrated a practical troubleshooting workflow:

Identify
   ↓
Test
   ↓
Isolate
   ↓
Identify Root Cause
   ↓
Apply Fix
   ↓
Verify


### Evidence

Screenshot: Initial failed Test-NetConnection.

Shows the initial SMB connectivity failure.

Screenshot: Disabled File and Printer Sharing firewall rules.

Shows the identified firewall configuration issue.

Screenshot: PowerShell command enabling the firewall rules.

Shows the remediation performed through PowerShell.

Screenshot: Successful TCP 445 test.

Confirms SMB connectivity was restored.

Screenshot: Successful Test-Connection.

Confirms network connectivity between DC01 and CLIENT01 after remediation.


## Key Skills Demonstrated

### Windows Server Administration
- Windows Server 2025 deployment
- Server configuration
- Server Manager
- Windows services


### Active Directory
- AD DS deployment
- Domain controller configuration
- Domain management
- User administration
- Security groups
- Organizational Units

### DNS
- Active Directory DNS
- DNS resolution
- DNS troubleshooting
- Domain name resolution

### Group Policy
- GPO creation
- Security policy configuration
- OU-based policy application
- Client-side validation

### File Services
- SMB shares
- Departmental file shares
- Share permissions
- Access control
- Permission testing

### PowerShell
- Active Directory administration
- Network diagnostics
- Service management
- Firewall configuration
- Connectivity testing

### Troubleshooting
- DNS troubleshooting
- SMB troubleshooting
- Windows Firewall troubleshooting
- Network connectivity testing
- Root-cause identification
- Verification after remediation


### Tools Used
- Microsoft Hyper-V
- Windows Server 2025
- Windows 11
- Active Directory Users and Computers
- Group Policy Management
- Server Manager
- PowerShell
- Windows Firewall
- SMB/File and Printer Sharing
- dcdiag
- Resolve-DnsName
- Test-NetConnection
- Test-Connection

## Project Outcome

The completed lab successfully simulated a small enterprise Windows environment with centralized identity, authentication, DNS, policy management and departmental file access.

The project also included practical troubleshooting rather than configuration alone. A real SMB connectivity problem was identified, investigated through PowerShell, resolved through Windows Firewall configuration, and verified through subsequent connectivity tests.

This project demonstrates practical foundational skills applicable to:

- IT Support
- Help Desk
- System Administration
- Windows Server Administration
- Active Directory Administration
- Network Administration
- Junior Cloud/Infrastructure roles




