# 🛠️ Suricata Build Guide (Proxmox Lab)

## 📌 Overview
This guide documents the step-by-step process for deploying a Suricata-based intrusion detection system in a segmented Proxmox lab environment, including storage design, network configuration, and detection setup.

---

# 🧱 1. VM Creation (Proxmox)

## VM Configuration

- **Name:** suricata-elk  
- **OS:** Ubuntu Server 24.04 LTS  
- **Machine Type:** q35  
- **CPU:** 4 cores  
- **RAM:** 12 GB  

---

## Storage

### Disk 1 (SSD — OS & Services)
- Size: 100 GB  
- Purpose:
  - Ubuntu OS  
  - Suricata  
  - Future ELK stack  

### Disk 2 (HDD — Logs)
- Size: 500 GB+  
- Purpose:
  - Log storage  
  - Event data  
  - Future SIEM ingestion  

---

## Network

- Bridge: vmbr3 (Security Network)  
- Interface: VirtIO  
- Network Range: 10.10.30.0/24  

---

# 🐧 2. Ubuntu Installation

- Select: **Install Ubuntu Server**
- Configure:
  - Hostname: suricata-elk  
  - Username: socadmin  
  - Install OpenSSH: Yes  
  - Storage: Use entire SSD disk  

---

# 🔧 3. System Preparation

Update system:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt full-upgrade -y
sudo apt autoremove -y
💾 4. Configure HDD for Log Storage
Identify disks
lsblk

Example:

sda → SSD
sdb → HDD
Format HDD
sudo mkfs.ext4 /dev/sdb
Create mount point
sudo mkdir /data
Mount disk
sudo mount /dev/sdb /data
Persist mount
sudo blkid

Add to /etc/fstab:

UUID=XXXX /data ext4 defaults 0 2
Verify
df -h
Create directories
sudo mkdir -p /data/suricata/logs
sudo chown -R $USER:$USER /data
🛡️ 5. Install Suricata
sudo apt install suricata -y
Verify service
sudo systemctl status suricata
🌐 6. Identify Network Interface
ip a

Example:

enp6s18 → 10.10.30.100/24
⚙️ 7. Configure Suricata

Edit config:

sudo nano /etc/suricata/suricata.yaml
Set interface
af-packet:
  - interface: enp6s18
    cluster-id: 99
    cluster-type: cluster_flow
    defrag: yes
Set log directory
default-log-dir: /data/suricata/logs
🏠 8. Enable Full Traffic Visibility (Critical Step)

To ensure Suricata detects traffic from the home network, add the home subnet to monitoring visibility.

Example:

192.168.1.0/24

This required:

Allowing traffic through pfSense firewall
Ensuring routing from home network → lab network
Confirming Suricata interface could see this traffic
🔄 9. Update Rules
sudo suricata-update
🔁 10. Restart Suricata
sudo systemctl restart suricata
🔍 11. Validate Traffic Visibility
sudo tcpdump -i enp6s18
⚔️ 12. Generate Test Traffic

From Kali:

nmap -sS -T4 -A 10.10.30.100
📊 13. Monitor Logs
tail -f /data/suricata/logs/eve.json
Filter alerts
tail -f /data/suricata/logs/eve.json | jq 'select(.event_type=="alert")'
✅ Result
-Suricata successfully detected Nmap scans
-Logs generated and parsed correctly
-Detection pipeline validated
