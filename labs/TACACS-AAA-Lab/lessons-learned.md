````markdown
# TACACS+ AAA Lab – Lessons Learned

## Purpose

This document records the technical lessons, troubleshooting discoveries, and practical knowledge gained while building the TACACS+ AAA Enterprise Lab.

The goal is to document not only what worked, but also the problems encountered, how they were diagnosed, and what was learned from resolving them.

---

# 1. Active Directory Accounts and Administrative Privileges

One of the first issues encountered was understanding the difference between an Active Directory (AD) domain account and an administrative account.

A domain account does not automatically have administrative privileges simply because it exists in Active Directory.

While logged in with the TACACS+ lab domain account, Windows User Account Control (UAC) repeatedly requested credentials for the administrative account when attempting to perform elevated tasks.

This demonstrated the distinction between:

- Domain user privileges
- Local administrator privileges
- Domain administrative privileges
- Active Directory group membership
- UAC elevation

### Key Lesson

Always verify which account is currently being used and what permissions that account actually has before troubleshooting an administrative failure.

An authentication problem and a permissions problem can appear similar but have very different causes.

---

# 2. Active Directory Management Tools Must Be Installed

While configuring Active Directory integration, several expected administrative commands were unavailable.

Examples included:

```powershell
Get-ADUser -Identity <username>
````

and:

```cmd
dsquery user -samid <username>
```

The commands initially failed because the required Active Directory management components were not installed.

Windows Server showed the Remote Server Administration Tools (RSAT) components as **Available**, which did not mean they were actually installed.

After identifying this distinction, the appropriate Active Directory management tools could be installed and used.

### Key Lesson

Do not assume that a Windows feature is installed simply because it appears in Server Manager.

Verify the actual feature state:

```powershell
Get-WindowsFeature RSAT-AD-Tools, RSAT-AD-PowerShell
```

This experience reinforced the importance of verifying dependencies before troubleshooting the application itself.

---

# 3. Active Directory User and Group Verification

Active Directory user and group membership must be verified when troubleshooting TACACS+ authentication and authorization.

Useful PowerShell commands include:

```powershell
Get-ADUser -Identity <username>
```

and:

```powershell
Get-ADPrincipalGroupMembership -Identity <username>
```

Active Directory command-line tools can also be used:

```cmd
dsquery user -samid <username>
```

and:

```cmd
dsquery user -samid <username> | dsget user -memberof -expand
```

These commands help verify that the user exists and belongs to the expected Active Directory security groups.

### Key Lesson

Do not begin troubleshooting TACACS+ authorization until the underlying Active Directory user and group membership have been verified.

---

# 4. TACACS.net XML Configuration

TACACS.net uses XML configuration files to define how the server handles authentication, authorization, clients, groups, and other TACACS+ behavior.

The configuration files were modified to match the lab environment and Active Directory structure.

Working with the XML configuration reinforced the importance of:

* Correct XML structure
* Correct directory paths
* Correct Active Directory group names
* Correct authentication group configuration
* Consistent naming
* Careful syntax validation
* Maintaining backups before configuration changes

### Key Lesson

A small XML configuration error can affect the behavior of the entire TACACS+ authentication or authorization process.

Configuration changes should therefore be made carefully and validated before additional troubleshooting begins.

---

# 5. Validate Configuration Before Troubleshooting the Network

TACDiag was used to validate the TACACS.net configuration before moving forward with simulated network devices.

The diagnostic process confirmed that the TACACS.net configuration files contained no detected configuration errors.

This is important because troubleshooting a network-device authentication failure while the TACACS+ server configuration itself is invalid can waste significant time.

### Key Lesson

Troubleshoot from the server outward.

A useful order is:

```text
Configuration
      ↓
TACACS.net Service
      ↓
Active Directory Connectivity
      ↓
User Lookup
      ↓
Group Membership
      ↓
TACACS+ Client Connectivity
      ↓
Authentication
      ↓
Authorization
      ↓
Accounting
```

Validate each layer before moving to the next.

---

# 6. TACDiag Active Directory Validation

TACDiag was used to validate the TACACS.net and Active Directory integration.

The diagnostic test confirmed:

* TACACS.net configuration validation succeeded.
* The Active Directory server was reachable.
* Lightweight Directory Access Protocol (LDAP) connectivity succeeded.
* The configured LDAP account could communicate with Active Directory.
* The test user could be located in the configured directory structure.
* The user's Active Directory group membership could be evaluated.
* The `NetworkAdmins` group matched the configured TACACS.net authentication group.

The validated path was:

```text
TACACS.net
    ↓
LDAP
    ↓
Active Directory
    ↓
User Lookup
    ↓
Group Membership
    ↓
TACACS.net Authentication Group
```

### Key Lesson

A running TACACS.net Windows service does not prove that Active Directory integration is working.

The complete dependency chain must be validated.

---

# 7. Authentication and Authorization Are Different

Building the lab reinforced the separation between Authentication and Authorization.

**Authentication** answers:

> Who are you?

Active Directory can verify the user's identity.

**Authorization** answers:

> What are you allowed to do?

TACACS.net can use Active Directory group membership and configured authorization policies to determine what level of access the authenticated user receives.

For example, membership in an Active Directory security group such as:

```text
NetworkAdmins
```

can be used as part of the TACACS+ authorization process.

### Key Lesson

Successful authentication does not automatically mean that authorization is configured correctly.

Each part of Authentication, Authorization, and Accounting (AAA) should be tested independently.

---

# 8. TACACS+ Uses TCP Port 49

TACACS+ communication uses Transmission Control Protocol (TCP) port 49.

Basic IP connectivity and TACACS+ service connectivity are not the same thing.

A successful ping only confirms that Internet Control Message Protocol (ICMP) communication is possible.

It does not prove that TCP port 49 is reachable.

A useful Windows test is:

```powershell
Test-NetConnection <TACACS-SERVER-IP> -Port 49
```

### Key Lesson

Always test the actual protocol and port used by the application.

Ping alone is not sufficient for troubleshooting TACACS+ connectivity.

---

# 9. Known-Good Snapshots Are Extremely Useful

Proxmox snapshots were created at important stages of the deployment.

A snapshot was created before major TACACS.net configuration changes.

Another snapshot was created after TACDiag successfully validated the working configuration.

This provides a known-good recovery point.

### Key Lesson

Snapshots make a lab much more useful for troubleshooting practice.

Instead of being afraid of breaking the environment, configurations can intentionally be changed or broken to study failure conditions.

The environment can then be restored to a known-good state.

---

# 10. Troubleshooting Should Be Layered

One of the biggest lessons from the lab has been to avoid changing multiple components at once.

Instead, troubleshooting should isolate each layer.

For this lab, the troubleshooting sequence is:

```text
Windows Server
      ↓
Active Directory
      ↓
DNS
      ↓
LDAP
      ↓
TACACS.net Configuration
      ↓
TACACS.net Service
      ↓
TACACS+ TCP/49 Connectivity
      ↓
Network Device
      ↓
Authentication
      ↓
Authorization
      ↓
Accounting
```

If one layer fails, troubleshoot that layer before moving farther down the chain.

### Key Lesson

The goal is not simply to make the system work.

The goal is to understand **why it works, where it can fail, and how to identify which component is responsible for the failure.**

---

# 11. Current Known-Good State

At the current stage of the lab:

* Proxmox infrastructure is operational.
* pfSense provides routing and DNS forwarding.
* Active Directory Domain Services is operational.
* TACACS-SRV is joined to the domain.
* TACACS.net is installed and running.
* TACACS.net XML files have been configured for the lab.
* Active Directory connectivity has been validated.
* LDAP connectivity has been validated.
* Active Directory user lookup has been validated.
* Active Directory group matching has been validated.
* TACDiag configuration validation succeeds.
* A known-good Proxmox snapshot has been created.

This provides the baseline for the next stage of testing.

---

# 12. Next Learning Objectives

The next phase will focus on moving from server-side validation to actual network-device AAA transactions.

Planned testing includes:

* Deploying simulated network devices
* Configuring TACACS+ clients
* Testing TCP port 49 connectivity
* Authenticating Active Directory users through a network device
* Testing group-based authorization
* Testing privilege levels
* Implementing command authorization
* Testing allowed and denied commands
* Generating Accounting records
* Reviewing TACACS.net logs
* Testing failed authentication scenarios
* Testing failed authorization scenarios
* Testing network and service failures
* Forwarding events to centralized logging

These tests will extend the lab from a functioning TACACS.net and Active Directory integration into a complete enterprise-style AAA environment.

---

# Security Note

This repository contains only sanitized lab documentation.

Passwords, TACACS+ shared secrets, private keys, proprietary TACACS.net configuration files, internal documentation, customer information, and other sensitive information are intentionally excluded.

```
```
