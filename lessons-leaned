# Home Lab Build – Lessons Learned

This document tracks important lessons learned while building and configuring my cybersecurity home lab. The goal of documenting these lessons is to improve future lab builds, troubleshoot issues faster, and share knowledge with others building similar environments.

---

# 1. Define the Lab Mission Before Building

Before purchasing hardware or installing software, clearly define the **purpose of the home lab**.

My lab was designed to support learning and practice in:

- Network administration
- Active Directory deployment and management
- Network engineering concepts
- Blue Team / SOC analyst skills
- Security monitoring and SIEM analysis
- Vulnerability scanning and system hardening

By identifying these goals first, it became easier to determine the required hardware resources.

**Key takeaway:**  
Design the environment first, then build the infrastructure to support it.

---

# 2. Work Backwards From Resource Requirements

Once the lab architecture was defined, I estimated the resources required for each system.

Example VM resource estimates:

| Virtual Machine | Estimated RAM |
|-----------------|---------------|
| Windows Domain Controller | 4–8 GB |
| Windows Client | 4–8 GB |
| Linux Web Server | 1–2 GB |
| Security Onion (SIEM / Monitoring) | 16 GB recommended |

Security Onion is significantly more resource intensive because it performs:

- network traffic analysis
- log aggregation
- intrusion detection
- packet capture
- SIEM operations

Because of this, Security Onion requires much more RAM than typical virtual machines.

This analysis showed that my original **16GB RAM was insufficient**, which led to upgrading the server with an additional **32GB ECC RAM**, bringing the total to **48GB RAM**.

**Key takeaway:**  
Plan VM resource needs early to avoid performance bottlenecks, especially when deploying monitoring or SIEM tools.

---

# 3. Virtual Machines Require Fast Storage

The server originally had **14TB of HDD storage**, but traditional hard drives typically operate at:

- 7200 RPM
- ~100–150 MB/s read/write speeds

Virtual machines require much faster disk access due to:

- multiple operating systems running simultaneously
- constant read/write activity
- logging and monitoring processes
- snapshots and system updates

To improve performance, I installed an **SSD** and configured it as its own **Virtual Disk** through the RAID controller.

The SSD is used to host:

- Proxmox hypervisor
- virtual machine operating systems

The HDD array will be used for:

- backups
- lab datasets
- bulk storage

**Key takeaway:**  
Use SSD storage for virtualization workloads whenever possible.

---

# 4. RAID Configuration Must Be Initialized

Enterprise servers do not present physical disks directly to the operating system.

Instead, disks must be configured through the **RAID controller** and presented as **Virtual Disks**.

When installing a new drive, the following steps are required:

1. Enter the RAID controller configuration menu
2. Initialize the disk
3. Create a Virtual Disk
4. Present the disk to the operating system

If this step is skipped, the operating system installation may fail.

**Key takeaway:**  
Always initialize new disks and create Virtual Disks in the RAID controller before installing the operating system.

---

# 5. Disk Initialization Errors Can Cause Installation Failures

During the initial installation process, the SSD virtual disk entered a **failed state** because it was not properly initialized.

The solution required:

1. Deleting the failed virtual disk
2. Re-initializing the physical disk
3. Recreating the virtual disk
4. Restarting the installation

After properly initializing the disk, the installation succeeded.

**Key takeaway:**  
If a disk fails during setup, deleting and recreating the virtual disk often resolves the issue.

---

# 6. Use ECC Memory in Enterprise Servers

Enterprise servers such as **Dell PowerEdge systems require ECC (Error Correcting Code) memory**.

ECC RAM provides:

- automatic correction of memory bit errors
- increased system stability
- protection against data corruption
- improved reliability for virtualization workloads

To support additional virtual machines, the server was upgraded with **32GB additional ECC RAM**, bringing total memory to **48GB DDR4 ECC RAM**.

**Key takeaway:**  
Always verify memory compatibility and use ECC RAM in enterprise servers.

---

# 7. Maintain Direct Physical Access to the Server

While remote management tools are convenient, direct access to the server is sometimes necessary.

Examples include:

- BIOS configuration
- RAID controller configuration
- boot troubleshooting
- hypervisor installation
- network configuration errors that prevent remote access

Because of this, it is helpful to keep a **dedicated monitor, keyboard, and mouse connected or available near the server**.

**Key takeaway:**  
Maintain local console access as a backup management method.

---

# 8. Explore Boot and Hardware Configuration Menus

Enterprise servers often include multiple configuration utilities during boot, including:

- BIOS
- Boot Manager
- RAID controller configuration
- Lifecycle Controller

Important system settings may exist in different locations within these menus.

**Key takeaway:**  
Spend time exploring boot and hardware configuration menus before installing the operating system.

---

# 9. Design the Lab for Future Expansion

A home lab should be designed with **future upgrades and expansion in mind**.

Instead of building everything at once, it is more effective to:

1. Design the overall architecture first
2. Build the core infrastructure
3. Expand the environment gradually

This prevents the project from becoming overwhelming.

Example growth stages:

Stage 1  
Core server setup and hypervisor installation

Stage 2  
Active Directory domain environment

Stage 3  
Linux servers and web applications

Stage 4  
Security monitoring tools such as Security Onion

Stage 5  
Attack simulation and security testing environments

**Key takeaway:**  
Build the core infrastructure first and expand the lab as skills develop.

---

# 10. Build a Lab That Supports Easy Experimentation

Virtualization makes it possible to quickly create and remove lab environments.

This allows systems to be:

- quickly deployed
- reset after testing
- isolated from other systems
- rebuilt without affecting the entire environment

This flexibility makes the home lab a powerful platform for learning and experimentation.

**Key takeaway:**  
Design the lab so new environments can be easily spun up for practice and testing.

---

# Summary

Building this home lab provided hands-on experience with:

- enterprise server hardware
- RAID storage configuration
- virtualization infrastructure
- hardware resource planning
- troubleshooting installation failures
- designing scalable lab environments

The lab will continue evolving as new technologies, monitoring tools, and security environments are added.
