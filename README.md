# Windows Server 2025 Active Directory Home Lab

**Project Status:** In Progress

## Project Overview

This project documents the deployment, configuration, and administration of a virtualized Windows Server 2025 Active Directory environment using Oracle VirtualBox.

The lab simulates a small-business IT infrastructure and provides hands-on experience with common Help Desk, IT Support, and Systems Administration responsibilities.

The primary objectives include Active Directory administration, Windows client management, DNS configuration, user authentication, network troubleshooting, and technical documentation.

## Technologies Used

- Oracle VirtualBox
- Windows Server 2025
- Windows 11 Pro (Virtual Client)
- Windows 11 (Host Operating System)
- Active Directory Domain Services (AD DS)
- Windows DNS Server
- Windows PowerShell
- Active Directory Users and Computers (ADUC)
- TCP/IP Networking

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Domain Controller | SERVER01 |
| Server Operating System | Windows Server 2025 |
| Client Workstation | CLIENT01 |
| Client Operating System | Windows 11 Pro |
| Active Directory Domain | corp.example.test |
| NetBIOS Domain | CORP |
| Internal Network | CorpLab |
| Network Subnet | 192.168.50.0/24 |
| Domain Controller IP | 192.168.50.10 |
| Client Workstation IP | 192.168.50.20 |
| DNS Server | SERVER01 (192.168.50.10) |
| Server Memory | 6 GB |
| Server Virtual CPUs | 2 |
| Server Virtual Disk | 80 GB |

## Network Architecture

The lab uses an isolated VirtualBox internal network named `CorpLab` to support communication between the domain controller and Windows 11 Pro workstation.

**SERVER01** uses two virtual network adapters:

- **Adapter 1 (NAT):** Provides internet connectivity.
- **Adapter 2 (Internal Network):** Provides communication with domain-connected devices.

**CLIENT01** uses the CorpLab internal network to communicate with SERVER01.

The internal network uses the `192.168.50.0/24` subnet, with SERVER01 configured as the DNS server for CLIENT01.

This configuration allows domain authentication and DNS communication to occur within an isolated virtual environment without modifying the physical home network.

## Completed Configuration

### 1. Windows Server Deployment

- Created and configured the SERVER01 virtual machine.
- Installed Windows Server 2025.
- Configured the server hostname and virtual hardware.
- Configured network adapters for internal networking and internet connectivity.
- Assigned a static IPv4 address to the internal network interface.
- Verified network connectivity.

### 2. Active Directory Domain Deployment

- Installed Active Directory Domain Services (AD DS).
- Created the `corp.example.test` forest and domain.
- Promoted SERVER01 to a domain controller.
- Configured the NetBIOS domain name `CORP`.
- Installed and configured the DNS Server role.
- Verified domain configuration using PowerShell.

#### Active Directory Domain Verification

Used the following PowerShell command to verify the domain configuration:

`Get-ADDomain | Select-Object DNSRoot, NetBIOSName, PDCEmulator`

**Verified results:**

- DNS Domain: `corp.example.test`
- NetBIOS Name: `CORP`
- PDC Emulator: `SERVER01.corp.example.test`

This exercise demonstrated the ability to inspect and verify Active Directory domain configuration using PowerShell.

[View Active Directory Domain Verification Screenshot](screenshots/Active-Directory-domain-verification.png)

### 3. DNS Configuration and Troubleshooting

Configured DNS services to support Active Directory name resolution and domain authentication.

During verification, I discovered that the domain controller was registering two IPv4 addresses:

- `192.168.50.10` — Internal CorpLab network
- `10.0.2.15` — VirtualBox NAT network

The unwanted NAT address could cause domain-connected clients to resolve the domain controller to an unreachable address.

**Troubleshooting steps:**

1. Used PowerShell DNS queries to identify duplicate records.
2. Inspected network adapter DNS registration settings.
3. Disabled unwanted DNS registration on the NAT adapter.
4. Removed incorrect DNS records using DNS Manager.
5. Verified DNS Server configuration.
6. Cleared the DNS client cache.
7. Retested DNS resolution using PowerShell.

**Verification command:**

`Resolve-DnsName corp.example.test -Server 192.168.50.10 -Type A`

**Result:** The domain successfully resolved to the correct internal IP address, `192.168.50.10`.

This exercise demonstrated practical DNS troubleshooting, network adapter configuration, and systematic problem resolution.

[View DNS Resolution Verification Screenshot](screenshots/dns-resolution-verification.png)

### 4. Active Directory Organizational Unit Management

Created an organizational unit structure to simulate centralized administration within a small-business environment.

**Organizational Unit Structure:**

```
corp.example.test
└── Company
    ├── Users
    ├── Computers
    └── Security Groups
```

This structure separates user accounts, computer accounts, and security groups to support organized administration and future Group Policy configuration.

[View Active Directory OU Structure Screenshot](screenshots/Creating%20AD%20groups.png)

### 5. User and Security Group Management

Created sample employee accounts and security groups to practice identity and access administration.

**Completed tasks:**

- Created an employee account for Alex Morgan (`amorgan`).
- Created the `HelpDesk-Tier1` global security group.
- Added Alex Morgan to the security group.
- Verified group membership using Active Directory Users and Computers.

This exercise demonstrated user provisioning, security group administration, and membership verification.

[View Security Group Membership Screenshot](screenshots/Assigning%20a%20member%20to%20a%20security%20group.png)

### 6. Windows 11 Pro Client Deployment

Deployed a Windows 11 Pro virtual workstation to simulate an employee computer within the organization.

**Completed tasks:**

- Created the CLIENT01 virtual machine in Oracle VirtualBox.
- Installed Windows 11 Pro.
- Configured the workstation's virtual hardware.
- Connected CLIENT01 to the CorpLab internal network.
- Assigned the static IPv4 address `192.168.50.20/24`.
- Configured `192.168.50.10` as the preferred DNS server.
- Verified communication with the domain controller.

This configuration prepared the workstation for Active Directory domain integration.

### 7. Windows 11 Pro Domain Join

Successfully joined CLIENT01 to the `corp.example.test` Active Directory domain.

**Completed tasks:**

- Configured CLIENT01 to use SERVER01 for DNS resolution.
- Verified domain controller connectivity.
- Joined the workstation to the Active Directory domain.
- Restarted the workstation to apply domain membership.
- Verified that Windows recognized the domain membership.
- Confirmed that the computer account appeared in Active Directory.

This exercise demonstrated practical Windows client deployment and domain integration.

### 8. Active Directory Computer Account Management

Used Active Directory Users and Computers (ADUC) to manage the domain-joined workstation.

**Completed tasks:**

- Located CLIENT01 in the default Computers container.
- Moved the computer account into `Company → Computers`.
- Verified the workstation appeared in the correct organizational unit.
- Updated computer account properties to document the workstation's purpose.

This exercise demonstrated computer account administration, organizational unit management, and directory organization.

### 9. Active Directory Domain User Authentication

Verified successful Active Directory authentication using the domain-joined Windows 11 Pro workstation.

Signed in to CLIENT01 using the sample employee account Alex Morgan.

Executed the PowerShell command:

`whoami`

**Verified output:**

`corp\amorgan`

The result confirmed that the Windows session was running under the Active Directory domain account rather than a local Windows account.

This exercise demonstrated successful domain user authentication and identity verification.

## Planned Help Desk Simulations

The following exercises will expand the lab to include additional real-world IT support responsibilities:

- [ ] Active Directory password resets
- [ ] User account unlock and lockout troubleshooting
- [ ] Group Policy creation and deployment
- [ ] Shared folder and NTFS permissions
- [ ] User account lifecycle management
- [ ] Additional Windows client troubleshooting
- [ ] Advanced DNS and network connectivity diagnostics

## Skills Demonstrated

**Windows Server Administration**
- Windows Server installation and configuration
- Active Directory Domain Services deployment
- Domain controller administration
- DNS configuration and troubleshooting

**Active Directory Administration**
- Organizational unit management
- User account provisioning
- Security group management
- Computer account administration
- Domain user authentication

**Windows Client Support**
- Windows 11 Pro deployment
- Static IP and DNS configuration
- Domain joining
- Workstation management
- Authentication verification

**Networking and Troubleshooting**
- VirtualBox NAT and internal networking
- IPv4 addressing
- DNS record management
- Network connectivity testing
- PowerShell-based diagnostics
- Systematic troubleshooting

**Documentation**
- Technical configuration documentation
- Screenshot-based verification
- Troubleshooting documentation
- Infrastructure documentation

## Project Purpose

This home lab supports my professional development toward an entry-level IT Help Desk or IT Support position, with long-term career goals in Systems Administration and Cybersecurity.

The environment will continue to expand as I complete additional administrative exercises, troubleshooting simulations, and security-focused projects.
