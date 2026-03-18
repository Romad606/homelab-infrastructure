# 🧠 Proxmox Home Lab – DVWA + pfSense + Network Segmentation

## 📌 Overview
This lab demonstrates the design, deployment, and troubleshooting of a segmented home lab environment using Proxmox, pfSense, and DVWA.

The focus was not only on building the environment, but developing a structured troubleshooting methodology across networking, systems, and application layers — mirroring real-world IT and cybersecurity workflows.

---

# 🏗️ Lab Architecture

## 🔌 Proxmox Network Bridges (vmbr)

```text
vmbr0 → WAN (ISP Router / Internet)
vmbr1 → LAN (10.10.10.0/24)
vmbr2 → DMZ (10.10.20.0/24)
vmbr3 → SECURITY (10.10.30.0/24)

                Internet
                    │
                [vmbr0]
                    │
               ┌──────────┐
               │ pfSense  │
               └──────────┘
        ┌────────┼────────┬────────┐
        │        │        │
     vmbr1    vmbr2    vmbr3
      LAN      DMZ     SECURITY
 10.10.10.x 10.10.20.x 10.10.30.x
     │         │         │
 Windows     DVWA    Security Onion
 AD/DC       Server     (Planned)


# 🧠 Lessons Learned – Proxmox Home Lab (DVWA + pfSense + Segmentation)

## Overview
This section captures the key lessons learned while building, troubleshooting, and validating a segmented home lab environment using Proxmox, pfSense, Windows Server, and DVWA.

---

## 1. Network Segmentation Enables Security
- Built 4 Proxmox bridges (vmbr0–vmbr3)
- Assigned each bridge its own subnet
- Routed all traffic through pfSense

**Lesson Learned:**  
Segmentation enables traffic control, realistic firewall rule creation, attack simulation, and monitoring.

---

## 2. Firewall Rules Must Be Specific (NOT “ANY”)
### Initial mistake:
Source: ANY
Destination: ANY

### Correct approach:
Source: DMZ net
Destination: ANY

**Lesson Learned:**  
Using `ANY` breaks segmentation and reduces visibility. Always define source, destination, and interface.

---

## 3. pfSense is the Core Traffic Control Point
LAN ↔ pfSense ↔ DMZ ↔ SECURITY ↔ WAN

**Lesson Learned:**  
pfSense acts as the router, firewall, and enforcement point for all network traffic.

---

## 4. WAN Uses DHCP, Internal Interfaces Use Static IPs
- WAN receives IP from ISP/router via DHCP
- LAN, DMZ, and SECURITY interfaces use static IPs

**Lesson Learned:**  
Gateways and infrastructure should use static addressing for stability and predictability.

---

## 5. Internal Interfaces Do NOT Need Upstream Gateway
- When configuring LAN, DMZ, SECURITY → leave upstream gateway blank

**Lesson Learned:**  
Only WAN needs an upstream gateway. pfSense is the gateway for internal networks.

---

## 6. Proxmox Bridges Should Not Have IPs (Internal)
- vmbr1, vmbr2, vmbr3 created with no IPv4 address

**Lesson Learned:**  
Proxmox bridges are Layer 2 switches. pfSense handles Layer 3 routing.

---

## 7. Documentation Improves Troubleshooting
- Added descriptions/comments to vmbr interfaces instead of renaming

**Lesson Learned:**  
Clear labeling improves readability, troubleshooting, and scalability.

---

## 8. Interface Selection Matters in Linux
### Issue:
No IP address (only loopback)

---
### Fix:
```bash
ip a

##Lesson Learned:
Always verify the correct interface (ens18, ens19, etc.) before troubleshooting.

---
## 9. Layered Troubleshooting Method Works Best

**Issue:**  
Troubleshooting issues were inconsistent and unclear.

**Fix:**  
```text
Layer 3 → IP / Routing
Layer 4 → Firewall Rules
Layer 7 → Application (Apache / PHP)
Backend → Database (MySQL)

Lesson Learned:
Troubleshoot systematically by layer instead of guessing. If everythign is designed right and test locally and works there its most likely a firewall rule blocking or misconfigured
---

## 10. Apache Running Does NOT Mean PHP is Working

##Issue:
Raw PHP displayed in browser.

##Fix:

sudo apt install php libapache2-mod-php php-mysql
sudo systemctl restart apache2

##Lesson Learned:
If PHP appears in the browser, it is not being executed.

---

## 11. Missing PHP Modules Break Applications

##Issue:
DVWA returned a fatal error related to mysqli.

##Fix:

sudo apt install php8.3-mysql
sudo systemctl restart apache2

##Lesson Learned:
Applications depend on required PHP modules. Missing dependencies cause runtime failures.

---

##  12. File Structure Integrity is Critical

##Issue:
Key DVWA files such as setup.php were missing.

##Fix:

cd /var/www/html
sudo rm -rf DVWA
sudo git clone https://github.com/digininja/DVWA.git

##Lesson Learned:
File operations can silently break applications. Always verify structure after moving or copying files.

---

## 13. Linux is Case-Sensitive

##Issue:
Incorrect file paths and URLs caused access problems.

##Fix:

DVWA ≠ dvwa

##Lesson Learned:
Case matters for commands, file paths, and URLs.

---

## 14. Database Connectivity Must Be Verified Independently

##Issue:
The application could not connect to the database.

##Fix:

mysql -u dvwa -p

##Lesson Learned:
Always validate database access separately before troubleshooting the application.

---

## 15. Configuration Files Control Behavior

##Issue:
DVWA was not connecting correctly to the database.

##Fix:
Edit the DVWA configuration file:

/var/www/html/DVWA/config/config.inc.php

Set:

$_DVWA['db_user'] = 'dvwa';
$_DVWA['db_password'] = 'password';

##Lesson Learned:
Small configuration mistakes can break the entire application.

---

## 16. PHP Settings Can Cause 500 Errors

##Issue:
DVWA returned a 500 Internal Server Error.

##Fix:
In php.ini, set:

allow_url_include = On
display_errors = On

Then restart Apache:

sudo systemctl restart apache2

##Lesson Learned:
Application behavior can depend on PHP settings.

---

## 17. 500 Errors Usually Mean Backend Failures

##Issue:
The web server was reachable, but the application failed internally.

##Fix:
Check:

PHP configuration

Missing PHP modules

Database connectivity

Apache error logs

##Lesson Learned:
A 500 error means the application failed internally, not that the web server is unreachable.

---

## 18. Permissions Matter

Issue:
Application failed or behaved unexpectedly due to file access issues.

##Fix:

sudo chown -R www-data:www-data /var/www/html/DVWA
sudo chmod -R 755 /var/www/html/DVWA

##Lesson Learned:
Incorrect permissions can silently break web applications.

---

## 19. OpenSSH Can Be Installed After Deployment

##Issue:
OpenSSH Server was not installed during initial setup.

##Fix:

sudo apt install openssh-server -y
sudo systemctl enable ssh

##Lesson Learned:
Missing services can be installed after deployment without rebuilding the system.

---

## 20. ISO/CD Unmount Errors Are Often Non-Critical

##Issue:
System displayed Failed unmounting cdrom.mount during reboot.

##Fix:
Verify system boots correctly. Optionally detach ISO after installation.

##Lesson Learned:
Not all errors are critical — validate impact before troubleshooting.

---

## 21. Storage Tiering Matters

##Issue:
Different workloads required different performance levels.

##Fix:

Use SSD for critical systems

Use HDD for lab workloads

##Lesson Learned:
Match storage performance to workload requirements.

---

## 22. qcow2 is Better for Lab VMs

##Issue:
Choosing the correct disk format for lab environments.

##Fix:
Use qcow2 instead of raw.

##Lesson Learned:
qcow2 supports snapshots and rollback, making it ideal for lab and testing environments.
