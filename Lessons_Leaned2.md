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
### Correct approach:


**Lesson Learned:**  
Using `ANY` breaks segmentation and reduces visibility. Always define source, destination, and interface.

---

## 3. pfSense is the Core Traffic Control Point

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

### Fix:
```bash
ip a

## 9. Layered Troubleshooting Method Works Best

**Issue:**  
Troubleshooting issues were inconsistent and unclear.

**Fix:**  
```text
Layer 3 → IP / Routing
Layer 4 → Firewall Rules
Layer 7 → Application (Apache / PHP)
Backend → Database (MySQL)

## 10. Apache Running Does NOT Mean PHP is Working

**Issue:**
Raw PHP displayed in browser.

**Fix:**

sudo apt install php libapache2-mod-php php-mysql
sudo systemctl restart apache2

Lesson Learned:
If PHP appears in the browser, it is not being executed.
