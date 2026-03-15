# Cybersecurity Home Lab Infrastructure

## Overview

This repository documents the design and build of my cybersecurity home lab environment.

The lab is used to practice real-world skills related to:

- Network Engineering
- System Administration
- Active Directory Administration
- Vulnerability Analysis
- Server Hardening
- Blue Team Security Monitoring

The environment simulates a small enterprise network where I can practice defensive security monitoring while performing controlled security testing from an external attacker system.

In addition to documenting my own lab, the goal of this repository is to provide a practical guide showing how to **design, build, and expand a cybersecurity home lab environment** for learning network engineering, system administration, and security operations.

This lab supports preparation for certifications including:

- CompTIA Security+
- CompTIA Linux+
- CompTIA CySA+
- AWS Cloud certifications
- Microsoft Azure certifications

---

# Hardware Platform

Primary Server

Dell Precision T330

CPU  
Intel Xeon E3-1240 v5  
4 Cores / 8 Threads

Memory  
64GB DDR4 ECC

## Storage

4x 4TB SATA HDD (data and lab storage)

1x 1TB SSD  
Used for:

- Proxmox installation
- Virtual machine operating systems
- High performance VM workloads

The SSD is used for the hypervisor and active virtual machines to provide faster boot times and better disk performance compared to running operating systems on spinning disks.

---

# Networking

Dual Gigabit Ethernet NICs

---

# Virtualization Platform

Hypervisor

Proxmox VE

Proxmox was selected because it provides:

- KVM virtualization
- efficient VM management
- flexible virtual networking
- snapshot capability
- support for both Linux and Windows environments

This allows the lab to simulate a multi-system network environment on a single physical server.

---

# Lab Network Design

The lab network is designed to simulate a small enterprise environment.

Network segments include:

Management Network  
Used to manage Proxmox and infrastructure services

Internal Domain Network  
Hosts domain services and domain joined systems

Monitoring Network  
Used by security monitoring tools such as Security Onion

External Testing System  
Red team and attack simulations are performed from my laptop running Kali Linux, allowing the environment to mimic an external attacker.

---

# Virtual Machines

The lab currently includes the following systems.

## Windows Server

Active Directory Domain Controller  
DNS Server

Used for:

- domain authentication
- identity management
- enterprise network simulation

## Windows Client

Domain joined workstation used for:

- user activity simulation
- domain administration testing
- policy testing

## Ubuntu Server

Used as a test web server and Linux administration environment.

This server is intentionally used to practice:

- vulnerability scanning
- system hardening
- service configuration

## Security Onion

Security Onion is deployed as the primary monitoring platform for the environment.

It is used for:

- network intrusion detection
- log collection
- traffic analysis
- threat hunting
- security monitoring

Security Onion allows the lab to simulate a **Security Operations Center (SOC)** monitoring environment.

---

# Labs Supported in This Environment

This infrastructure supports multiple security and networking labs including:

Active Directory domain configuration  
DNS management  
User and group administration  
Network traffic monitoring  
Intrusion detection analysis  
Vulnerability scanning  
Server hardening  
Security event analysis

Future labs will include:

SIEM tuning  
Threat detection engineering  
Incident response scenarios  
Cloud security integrations

---

# Repository Structure

This repository documents the infrastructure build process.

hardware.md  
Hardware configuration and server specifications

proxmox-install.md  
Installation and configuration of Proxmox

network-architecture.md  
Network design and segmentation

security-onion-deployment.md  
Deployment and configuration of Security Onion

vm-templates.md  
Virtual machine configuration templates

screenshots/  
Architecture diagrams and environment screenshots

---

# Learning Goals

This lab environment is designed to strengthen hands-on experience with:

- enterprise network architecture
- identity and access management
- Linux administration
- security monitoring
- vulnerability analysis
- intrusion detection systems

It also serves as a guide demonstrating how to **plan, design, and build a scalable cybersecurity home lab** that can evolve as new technologies and skills are learned.

The long-term goal is to build practical experience supporting roles in:

- cloud security
- security engineering
- security operations (SOC)
- infrastructure engineering

---

# Future Improvements

Planned upgrades to the lab include:

- centralized logging
- advanced SIEM tuning
- cloud lab integration with AWS and Azure
- security automation
- detection engineering labs

---

# Author

Christopher Albrecht

Cybersecurity | Network Engineering | Cloud Security

Certifications

CompTIA A+  
CompTIA Network+  
CompTIA Security+  
AWS Cloud Certification  
Microsoft Azure Certification

This lab is part of my ongoing hands-on cybersecurity training and professional development.
