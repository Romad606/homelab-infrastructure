```markdown
# TACACS+ AAA Enterprise Lab

## Overview

This lab simulates an enterprise TACACS+ Authentication, Authorization, and Accounting (AAA) environment using TACACS.net, Microsoft Active Directory, Proxmox, pfSense, and centralized logging.

The lab is designed to develop hands-on experience with TACACS+ deployment, Active Directory integration, network-device administration, group-based authorization, command authorization, accounting, centralized logging, troubleshooting, and secure backup workflows.

---

## Lab Architecture

The TACACS+ AAA lab is hosted in Proxmox and is designed to simulate a mixed enterprise network environment.

### Current Infrastructure

- **Proxmox VE** – Virtualization platform
- **pfSense** – Firewall, routing, and upstream DNS forwarding
- **DC01** – Windows Server Domain Controller
  - Active Directory Domain Services (AD DS)
  - Domain Name System (DNS)
  - Domain: `lab.local`
- **TACACS-SRV** – Windows Server running TACACS.net 4.1.6
- **NetworkAdmins** – Active Directory security group used for TACACS+ authentication and authorization testing

### Planned Infrastructure

- **SYSLOG-SRV** – Centralized Syslog collection and analysis
- **Simulated network devices** – TACACS+ clients for Authentication, Authorization, and Accounting testing
- **SFTP-SRV** – Windows Server/OpenSSH lab for testing secure automated backup transfers

---

# Progress

## Phase 1 – Infrastructure and TACACS.net Installation

- [x] Configured Windows Server virtual machines in Proxmox
- [x] Configured Active Directory and DNS
- [x] Configured DNS forwarding through pfSense
- [x] Verified internal and external DNS resolution
- [x] Joined TACACS-SRV to the `lab.local` domain
- [x] Created the `NetworkAdmins` Active Directory security group
- [x] Created TACACS+ test users and assigned group membership
- [x] Updated Windows Server
- [x] Transferred the TACACS.net installer using virtual ISO media
- [x] Installed TACACS.net 4.1.6
- [x] Verified the TACACS.net Windows service
- [x] Created a clean Proxmox snapshot before configuration
- [x] Installed Notepad++ for XML configuration management

---

## Phase 2 – TACACS+ AAA Configuration

- [x] Configured Active Directory authentication
- [x] Configured Active Directory group matching
- [x] Configured the `NetworkAdmins` group for TACACS+ access
- [x] Modified TACACS.net XML configuration files for the lab environment
- [x] Validated TACACS.net configuration files with TACDiag
- [x] Verified connectivity between TACACS.net and Active Directory
- [x] Verified Lightweight Directory Access Protocol (LDAP) connectivity
- [x] Verified Active Directory user lookup
- [x] Verified Active Directory group membership matching
- [x] Verified `user1` matched the intended TACACS.net authentication group
- [x] Created a known-good Proxmox snapshot after successful configuration
- [ ] Configure TACACS+ network-device clients
- [ ] Configure command authorization
- [ ] Test Authentication, Authorization, and Accounting (AAA) using simulated network devices

### Active Directory Integration

TACACS.net was configured to integrate with the `lab.local` Active Directory environment.

A dedicated Active Directory security group named `NetworkAdmins` is used to test group-based TACACS+ access.

The TACACS.net XML configuration was modified to match the lab's Active Directory structure and authentication requirements.

This configuration allows TACACS.net to locate users in Active Directory and evaluate their group membership before applying the appropriate TACACS+ policy.

### TACDiag Validation

TACDiag, TACACS.net's diagnostic utility, was used to validate the TACACS.net configuration and Active Directory integration.

Testing confirmed that:

- TACACS.net configuration files passed configuration validation.
- The configured Active Directory server was reachable.
- TACACS.net successfully connected to Active Directory using LDAP.
- The test account `user1` was located within the configured Active Directory search path.
- The user's Active Directory group membership matched the configured `NetworkAdmins` group.
- TACDiag determined that `user1` matched the intended TACACS.net authentication group.

The validated path was:

TACACS.net  
↓  
LDAP  
↓  
Active Directory  
↓  
User Lookup  
↓  
AD Group Membership  
↓  
TACACS.net Authentication Group

This established a known-good baseline for continued TACACS+ authorization and network-device testing.

### Troubleshooting and Lessons Learned

Several issues encountered during the Active Directory integration provided useful troubleshooting experience.

Active Directory administrative tasks were initially attempted while using an account that did not have the required administrative privileges. Windows User Account Control (UAC) therefore required separate administrator credentials when elevated operations were performed.

Active Directory management commands such as:

`Get-ADUser`

`dsquery`

`dsget`

were also initially unavailable.

Troubleshooting showed that the required Remote Server Administration Tools (RSAT) components were available in Windows Server but had not yet been installed.

This reinforced several important concepts:

- A domain user is not automatically a local administrator.
- A local administrator does not automatically have domain administrative privileges.
- Administrative tools require both appropriate permissions and the required Windows management components.
- The account used to perform administrative tasks matters when configuring and troubleshooting Active Directory.
- A running TACACS.net service does not prove that the entire authentication path is functioning.
- Active Directory connectivity, LDAP communication, user lookup, and group matching should be independently validated.
- Diagnostic testing should be performed before moving on to network-device integration.

After resolving these issues and successfully validating the configuration with TACDiag, a new Proxmox snapshot was created to preserve the known-good configuration.

---

## Phase 3 – Centralized Logging

- [ ] Deploy dedicated Syslog server
- [ ] Configure TACACS.net Enhanced Logging
- [ ] Forward System and Accounting events
- [ ] Verify and analyze received logs
- [ ] Test logging failures and troubleshooting

---

## Phase 4 – Network Device Simulation

- [ ] Deploy simulated network devices
- [ ] Configure devices as TACACS+ clients
- [ ] Test administrator authentication
- [ ] Test group-based authorization policies
- [ ] Test command authorization
- [ ] Generate Accounting records
- [ ] Troubleshoot failed TACACS+ transactions

---

## Phase 5 – SFTP Backup Lab

- [ ] Deploy Windows Server SFTP destination
- [ ] Install and configure OpenSSH
- [ ] Configure backup directory permissions
- [ ] Test SFTP authentication
- [ ] Upload backup files
- [ ] Delete backup files
- [ ] Document the configuration and testing process

---

# Skills Practiced

- TACACS+ Authentication, Authorization, and Accounting (AAA)
- TACACS.net deployment and configuration
- Microsoft Active Directory
- Active Directory Domain Services (AD DS)
- Active Directory group-based access control
- Lightweight Directory Access Protocol (LDAP)
- Windows Server administration
- Remote Server Administration Tools (RSAT)
- Domain Name System (DNS)
- pfSense routing and firewalling
- Proxmox virtualization
- XML configuration and validation
- TACACS+ diagnostics and troubleshooting
- Syslog and centralized logging
- SFTP and OpenSSH
- Network troubleshooting
- Virtual media and ISO management
- Cross-platform Windows and Linux administration

---

# Security and Repository Scope

This repository documents the architecture, lab methodology, troubleshooting process, and sanitized examples used to build the environment.

Passwords, TACACS+ shared secrets, private keys, proprietary TACACS.net configuration files, internal company documentation, customer information, licensing information, and other sensitive data are intentionally excluded from this repository.

Any configuration examples, screenshots, logs, or diagnostic output added to this repository will be reviewed and sanitized before publication.

---

# Next Steps

The next stage of the lab will move from validating the TACACS.net and Active Directory integration to testing TACACS+ against simulated network infrastructure.

Planned work includes:

1. Deploy a simulated network device.
2. Configure the device as a TACACS+ client.
3. Establish TACACS+ communication over TCP port 49.
4. Authenticate an Active Directory administrator through TACACS+.
5. Test group-based authorization.
6. Implement command authorization.
7. Generate and review Accounting records.
8. Test failed authentication and authorization scenarios.
9. Forward TACACS.net events to centralized logging.
10. Document troubleshooting scenarios and recovery procedures.
```
