# Lab 01 – Configure Server for Proxmox Installation and Install Proxmox

## Objective

Prepare a Dell PowerEdge server for virtualization, configure storage correctly, and install Proxmox VE on an SSD virtual disk.

---

## Lab Purpose

This lab documents the step-by-step process used to prepare enterprise server hardware for a Proxmox installation. It is intended to help students and home lab builders understand the hardware preparation, RAID configuration, and installation process required before beginning virtualization.

---

## Learning Outcomes

By completing this lab, the learner will be able to:

- Access server boot and hardware configuration menus
- Identify where RAID and storage settings are managed
- Initialize a newly installed drive
- Create a virtual disk for Proxmox installation
- Explain why SSD storage is preferred for virtualization
- Install Proxmox VE on the correct target disk
- Verify initial Proxmox access after installation

---

## Hardware Used

- Dell PowerEdge server
- SSD for Proxmox and VM operating systems
- HDD storage array for bulk storage
- Monitor
- Keyboard
- Mouse
- USB drive with Proxmox ISO

---

## Prerequisites

Before beginning this lab, ensure you have:

- Physical access to the server
- A dedicated monitor, keyboard, and mouse connected or available
- Proxmox ISO downloaded
- Bootable USB installer created
- SSD physically installed in the server
- Knowledge of the server’s current storage layout
- A planned static IP address for Proxmox management

Example management IP:
- `192.168.1.11`

---

## Why This Lab Matters

Virtualization hosts must be configured carefully before installation. Enterprise servers such as Dell PowerEdge systems often require storage configuration through the RAID controller before the operating system can see the disk. If this step is skipped, installation can fail or the disk may appear in a failed state.

In this lab, the SSD is used for:

- Proxmox installation
- VM operating systems
- faster disk I/O for virtualization workloads

The HDDs are reserved for:

- bulk storage
- backups
- datasets
- future VM storage expansion

---

# Part 1 – Prepare the Server Hardware

## Step 1 – Connect Local Console Access

Connect the following directly to the server:

- monitor
- keyboard
- mouse

### Why
Direct physical access makes it easier to:

- navigate BIOS and RAID menus
- troubleshoot boot problems
- recover from network misconfiguration
- install the hypervisor

### Note
Even if remote management is planned later, local console access should always be available during setup.

---

## Step 2 – Power On the Server and Explore Boot Menus

Turn on the server and watch the boot screen carefully.

On Dell PowerEdge systems, configuration options may exist in different places such as:

- BIOS / System Setup
- Boot Manager
- RAID Controller Configuration
- Lifecycle Controller

### Action
Take time to explore these menus before starting the install.

### Lesson Learned
Important hardware configuration settings are often spread across multiple boot utilities.

---

## Step 3 – Verify the SSD Is Detected

Enter the storage or RAID controller menu and confirm the new SSD appears in the system.

### Check for:
- SSD listed as available
- correct size shown
- no fault or failed state
- not assigned incorrectly to another virtual disk

If the SSD is not visible:
- reseat the drive
- check cabling/backplane connection
- confirm slot compatibility
- rescan storage if the controller supports it

---

# Part 2 – Configure RAID / Virtual Disk

## Step 4 – Open the RAID Configuration Utility

Use the RAID controller utility to manage storage.

### Important Concept
Dell enterprise servers do not usually present raw physical disks directly to the operating system. The disk must first be configured as a **Virtual Disk**.

---

## Step 5 – Initialize the SSD

Select the newly added SSD and initialize it if required.

### Why
A new or previously used drive may not be ready for installation until it has been initialized by the RAID controller.

### Warning
If the SSD is not properly initialized, the installation may fail or the virtual disk may enter a failed state.

---

## Step 6 – Create a Virtual Disk for the SSD

Create the SSD as its own standalone virtual disk.

### Recommended Configuration
- Use the SSD as a dedicated virtual disk
- Do not combine it with the bulk HDD array
- Keep Proxmox and VM operating systems on the SSD

### Why
This provides better performance and keeps the virtualization host separate from bulk storage.

### Example Purpose of This SSD Virtual Disk
- Proxmox hypervisor
- VM operating systems
- fast storage for virtualization workloads

---

## Step 7 – Confirm the Virtual Disk Status

After creating the virtual disk, verify it shows a healthy status such as:

- Ready
- Online
- Optimal

Do not continue until the SSD virtual disk appears healthy.

### Troubleshooting Note
If the virtual disk shows **Failed**:
1. delete the failed virtual disk
2. re-initialize the physical SSD
3. recreate the virtual disk
4. verify healthy status before retrying installation

### Lesson Learned
A failed virtual disk during setup is often caused by incomplete initialization.

---

# Part 3 – Prepare for Proxmox Installation

## Step 8 – Create a Bootable Proxmox USB

On another computer, download the Proxmox VE ISO and create a bootable USB installer.

Common tools:
- Rufus
- balenaEtcher
- Ventoy

### File Needed
- Proxmox VE ISO

---

## Step 9 – Insert the USB Installer and Boot From It

Insert the bootable USB into the server.

Use the boot menu to select the USB device.

### If USB Does Not Boot
- verify the USB was created correctly
- confirm boot order
- try UEFI vs legacy mode if needed
- use a different USB port

---

# Part 4 – Install Proxmox

## Step 10 – Start the Proxmox Installer

Once the installer boots:

1. choose **Install Proxmox VE**
2. accept the license agreement
3. continue to disk selection

---

## Step 11 – Select the Correct Installation Disk

Choose the **SSD virtual disk** you created earlier.

### Important
Make sure you do **not** accidentally choose the HDD bulk storage array if both appear.

### Best Practice
Install Proxmox on the SSD virtual disk to ensure better performance for the hypervisor and VM operating systems.

---

## Step 12 – Configure Country, Time Zone, and Keyboard

Select the correct:

- country
- time zone
- keyboard layout

This ensures logs, scheduling, and console input work correctly.

---

## Step 13 – Set Administrator Password and Email

Create a strong root password and enter an email address.

### Best Practice
Use a password manager and store this credential securely.

---

## Step 14 – Configure Network Settings

Set the management network settings for Proxmox.

Example:

- Management IP: `192.168.1.11`
- Subnet: `255.255.255.0`
- Gateway: `192.168.1.1`
- DNS: your router or preferred DNS server

### Best Practice
Use a static IP for Proxmox so you can reliably access the web interface later.

---

## Step 15 – Confirm Installation Summary

Review all settings carefully before proceeding.

Check:
- correct target disk
- correct hostname
- correct IP address
- correct gateway
- correct keyboard / time zone

Then begin the installation.

---

## Step 16 – Let Proxmox Install

The installer will format the selected disk and install the hypervisor.

When complete:
- remove the USB installer if prompted
- reboot the server

---

# Part 5 – Verify Proxmox Access

## Step 17 – Log In From the Local Console

After reboot, confirm the system starts into Proxmox successfully.

You should see a local console screen showing the management URL.

Example:

- `https://192.168.1.11:8006`

---

## Step 18 – Access Proxmox From Your Laptop

From a device on the same network, open a web browser and go to:

- `https://192.168.1.11:8006`

Log in using:
- username: `root`
- password: the password created during install

---

## Step 19 – Verify the Host Is Ready

Once logged in, confirm:

- Proxmox dashboard loads
- local storage is visible
- network interface is active
- hostname is correct
- CPU and RAM are detected properly

---

# Validation Checklist

Use the checklist below to confirm the lab was completed successfully.

- [ ] SSD physically installed
- [ ] SSD detected by RAID controller
- [ ] SSD initialized
- [ ] SSD configured as its own virtual disk
- [ ] Virtual disk shows healthy status
- [ ] Proxmox bootable USB created
- [ ] Server booted from USB
- [ ] Proxmox installed to SSD virtual disk
- [ ] Static management IP configured
- [ ] Proxmox web interface reachable
- [ ] Local console shows correct management URL

---

# Common Mistakes

## Installing to the Wrong Disk
If both SSD and HDD storage are visible, it is easy to select the wrong target. Always verify the installation target before proceeding.

## Skipping RAID Initialization
If the SSD is not initialized and presented correctly as a virtual disk, installation may fail.

## No Local Console Access
Trying to do everything remotely during initial setup can make troubleshooting more difficult.

## Using Only HDD Storage for VMs
Traditional HDD storage is slower and can severely impact VM performance.

---

# Lessons Learned

## 1. Explore the Boot Menus First
Dell PowerEdge servers may place important settings in different boot utilities.

## 2. SSD Storage Is Better for Virtualization
Virtual machines require faster I/O than traditional HDDs provide.

## 3. RAID Setup Must Be Completed First
The operating system cannot use the disk until it is initialized and presented as a virtual disk.

## 4. Local Access Matters
A dedicated monitor, keyboard, and mouse make setup and troubleshooting much easier.

## 5. Plan the Lab Around the End Goal
Hardware decisions such as adding more RAM and SSD storage should be based on the future lab design.

---

# Suggested Next Lab

After Proxmox installation, the next logical lab is:

## Lab 02 – Configure Proxmox Networking and Create Virtual Bridges

Topics would include:
- `vmbr0` management bridge
- internal lab bridges
- web subnet
- domain subnet
- monitoring subnet
- preparing for Security Onion and Active Directory

---

# Instructor Notes

This lab is a strong foundation for future student exercises because it teaches:

- hardware awareness
- storage planning
- virtualization basics
- enterprise server setup
- troubleshooting during installation

Students should understand not just the installation steps, but also *why* SSD storage, RAID initialization, and direct console access matter in real-world deployments.
