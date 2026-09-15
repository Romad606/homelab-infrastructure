# TACACS+ AAA Enterprise Lab

## Overview

This lab simulates an enterprise TACACS+ Authentication, Authorization, and Accounting (AAA) environment using TACACS.net, Microsoft Active Directory, Proxmox, pfSense, and centralized logging.

The lab is designed to develop hands-on experience with TACACS+ deployment, Active Directory integration, network-device administration, accounting, centralized logging, troubleshooting, and secure backup workflows.

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
- **NetworkAdmins** – Active Directory security group used for TACACS+ authorization testing

### Planned Infrastructure

- **SYSLOG-SRV** – Centralized Syslog collection and analysis
- **Simulated network devices** – TACACS+ clients for Authentication, Authorization, and Accounting testing
- **SFTP-SRV** – Windows Server/OpenSSH lab for testing secure automated backup transfers

## Progress

### Phase 1 – Infrastructure and TACACS.net Installation

- [x] Configured Windows Server virtual machines in Proxmox
- [x] Configured Active Directory and DNS
- [x] Configured DNS forwarding through pfSense
- [x] Verified internal and external DNS resolution
- [x] Joined TACACS-SRV to the `lab.local` domain
- [x] Created the `NetworkAdmins` Active Directory security group
- [x] Created a TACACS+ test user and assigned group membership
- [x] Updated Windows Server
- [x] Transferred the TACACS.net installer using virtual ISO media
- [x] Installed TACACS.net 4.1.6
- [x] Verified the TACACS.net Windows service
- [x] Created a clean Proxmox snapshot before configuration
- [x] Installed Notepad++ for XML configuration management

### Phase 2 – TACACS+ AAA Configuration

- [ ] Configure Active Directory authentication
- [ ] Configure group-based authorization
- [ ] Configure TACACS+ clients
- [ ] Configure command authorization
- [ ] Test Authentication, Authorization, and Accounting (AAA)

### Phase 3 – Centralized Logging

- [ ] Deploy dedicated Syslog server
- [ ] Configure TACACS.net Enhanced Logging
- [ ] Forward System and Accounting events
- [ ] Verify and analyze received logs
- [ ] Test logging failures and troubleshooting

### Phase 4 – Network Device Simulation

- [ ] Deploy simulated network device
- [ ] Configure TACACS+ client
- [ ] Test administrator authentication
- [ ] Test authorization policies
- [ ] Generate Accounting records
- [ ] Troubleshoot failed TACACS+ transactions

### Phase 5 – SFTP Backup Lab

- [ ] Deploy Windows Server SFTP destination
- [ ] Install and configure OpenSSH
- [ ] Configure backup directory permissions
- [ ] Test SFTP authentication
- [ ] Upload backup files
- [ ] Delete backup files
- [ ] Document the configuration and testing process

## Skills Practiced

- TACACS+ Authentication, Authorization, and Accounting (AAA)
- Microsoft Active Directory
- Windows Server administration
- Domain Name System (DNS)
- pfSense routing and firewalling
- Proxmox virtualization
- XML configuration
- Syslog and centralized logging
- SFTP and OpenSSH
- Network troubleshooting
- Virtual media and ISO management
- Cross-platform Windows and Linux administration

## Security

Passwords, TACACS+ shared secrets, private keys, proprietary configuration files, internal documentation, and other sensitive information are excluded from this repository.
