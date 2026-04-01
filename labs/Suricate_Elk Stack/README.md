# 🛡️ SIEM Pipeline Lab — Suricata + ELK Stack

## 📌 Overview

This project documents the design, implementation, and validation of a Security Information and Event Management (SIEM) pipeline using Suricata and the ELK Stack (Elasticsearch, Logstash, Kibana).

The goal of this lab is to move beyond tool usage and develop a deeper understanding of how **detection systems, log pipelines, and visualization platforms** work together in real-world security operations environments.

This project is part of a broader cybersecurity home lab focused on building **security engineering and SOC-level skills through hands-on system design, troubleshooting, and validation**.

---

# 🎯 Objectives

- Deploy Suricata for network-based intrusion detection  
- Build a log ingestion and processing pipeline  
- Store and index security events for search and analysis  
- Visualize alerts and detection patterns  
- Simulate attacks and validate detection end-to-end  
- Develop structured troubleshooting and tuning methodologies  

---

# 🧱 High-Level Architecture

```text
Kali (Attacker)
        ↓
pfSense (Routing / Firewall)
        ↓
Target Systems (DVWA / Servers)
        ↓
Suricata (Detection Engine)
        ↓
Logstash (Parsing)
        ↓
Elasticsearch (Indexing)
        ↓
Kibana (Visualization / Analysis)
🏗️ Project Phases
🔹 Phase 1 — Detection Foundation (Suricata)

Established network-based intrusion detection capability.

Key Work:

Deployed Suricata on a dedicated monitoring VM
Configured interface binding and rule updates
Validated traffic visibility using tcpdump
Generated attack traffic using Nmap

Outcome:
Ability to detect and log network activity in real time

👉 View Phase 1

🔹 Phase 2 — Log Pipeline (ELK Stack)

Built centralized log ingestion, parsing, and storage.

Key Work:

Ingested Suricata logs (eve.json) into Logstash
Parsed and indexed logs in Elasticsearch
Designed storage architecture (SSD for compute, HDD for data)
Verified index creation and data flow

Outcome:
Ability to process, store, and search structured security data

👉 View Phase 2

🔹 Phase 3 — Visualization & Detection Engineering

Transformed raw data into actionable insights and validated detection workflows.

Key Work:

Built Kibana index patterns and visualizations
Created dashboards for alerts, IPs, and signatures
Simulated attacks and validated detection pipeline
Analyzed alert patterns and began tuning detections

Outcome:
Ability to visualize, analyze, and interpret security events

👉 View Phase 3

🌐 Lab Environment

This project is deployed within a segmented home lab environment built on Proxmox.

Key components include:

pfSense (routing and firewall)
Segmented networks (LAN, DMZ, Security)
Kali Linux (attack simulation)
DVWA (vulnerable target application)

Ubuntu Server (SIEM pipeline host)
🛠️ Technologies Used
Suricata (IDS/IPS)
Elasticsearch (data storage and indexing)
Logstash (log ingestion and parsing)
Kibana (data visualization)
Proxmox (virtualization platform)
pfSense (network segmentation and routing)
jq, tcpdump, Nmap (analysis and testing tools)

📊 Skills Demonstrated
Network segmentation and routing
Firewall rule design and troubleshooting
Intrusion detection system deployment
SIEM pipeline engineering (ELK stack)
Log parsing and normalization
Security event analysis and visualization
Multi-layer troubleshooting (OSI model)
Detection validation through attack simulation

🧠 Key Learning Focus

This project emphasizes:
Building security systems from the ground up
Understanding how detection pipelines actually function
Troubleshooting across multiple infrastructure layers
Validating detection through controlled attack simulation
Improving signal-to-noise through detection tuning

💥 Key Takeaways
Detection depends on network visibility and placement
Security tools require intentional configuration and tuning
Logs must be parsed and structured to be actionable
SIEM systems are pipelines, not standalone tools
Effective security requires end-to-end validation

🚀 Future Enhancements
Advanced detection tuning and rule suppression
Expanded Kibana dashboards and alerting workflows
Traffic mirroring (SPAN) for full network visibility
Cloud logging integration (AWS / Azure)
AI/ML-based detection exploration

📁 Repository Structure
homelab-infrastructure/
│
├── phase-1-detection-foundation.md
├── phase-2-log-pipeline-elk.md
├── phase-3-visualization-and-detection-engineering.md
│
├── screenshots/
├── diagrams/
└── README.md

👤 Author

Christopher Albrecht
Cybersecurity | Network Engineering | Cloud Security

Certifications:

CompTIA A+
CompTIA Network+
CompTIA Security+
ISC2 Certified in Cybersecurity (CC)
AWS Cloud Certification
Microsoft Azure Certification


