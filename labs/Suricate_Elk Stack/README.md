# 🛡️ SIEM Pipeline Lab – Suricata + ELK Stack

## 📌 Overview

This project documents the design and implementation of a Security Information and Event Management (SIEM) pipeline using Suricata and the ELK Stack (Elasticsearch, Logstash, Kibana).

The goal of this lab is to move beyond tool usage and develop a deeper understanding of how detection systems, logging pipelines, and visualization platforms work together in real-world security operations environments.

This project is part of my broader cybersecurity home lab and is focused on building practical skills aligned with security engineering and SOC workflows.

---

# 🎯 Objectives

- Deploy Suricata for network-based intrusion detection  
- Build a log ingestion pipeline using Logstash  
- Store and index events in Elasticsearch  
- Visualize and analyze data using Kibana  
- Simulate attacks and validate detection capabilities  
- Develop troubleshooting and tuning methodologies  

---

# 🧱 High-Level Architecture

Kali (Attacker)
↓
DVWA (Target)
↓
Network Traffic
↓
Suricata IDS
↓
Logstash Pipeline
↓
Elasticsearch
↓
Kibana Dashboard


---

# 🏗️ Project Phases

## 🔹 Phase 1 — Detection Foundation (Suricata)
Establish network-based detection capability by deploying and configuring Suricata.

**Outcome:**  
Ability to detect and log network activity.

---

## 🔹 Phase 2 — Log Pipeline (Logstash)
Ingest and parse Suricata logs into a structured format.

**Outcome:**  
Ability to process and normalize security data.

---

## 🔹 Phase 3 — Data Storage (Elasticsearch)
Store and index log data for efficient searching and querying.

**Outcome:**  
Ability to store and retrieve security events.

---

## 🔹 Phase 4 — Visualization (Kibana)
Create dashboards and visualizations for analysis and monitoring.

**Outcome:**  
Ability to interpret and analyze security data.

---

## 🔹 Phase 5 — Attack Simulation & Validation
Generate attack traffic and validate detection across the pipeline.

**Outcome:**  
End-to-end detection validation.

---

## 🔹 Phase 6 — Tuning & Optimization
Refine detection rules and reduce false positives.

**Outcome:**  
Improved signal-to-noise ratio and detection quality.

---

## 🔹 Phase 7 — Documentation & Reporting
Document architecture, build steps, and lessons learned.

**Outcome:**  
Professional project documentation and portfolio artifact.

---

# 🌐 Lab Environment

This project is deployed within a segmented home lab environment built on Proxmox.

Key components include:

- pfSense (routing and firewall)
- Segmented networks (LAN, DMZ, Security)
- Kali Linux (attack simulation)
- DVWA (vulnerable target application)
- Ubuntu Server (SIEM pipeline host)

---

# 🛠️ Technologies Used

- Suricata (IDS/IPS)
- Elasticsearch (data storage and indexing)
- Logstash (log ingestion and parsing)
- Kibana (data visualization)
- Proxmox (virtualization platform)
- pfSense (network segmentation and routing)

---

# 📊 Skills Developed

- Network security monitoring  
- Intrusion detection and analysis  
- SIEM pipeline design  
- Log parsing and normalization  
- Threat detection workflows  
- Troubleshooting across multiple layers (network → application)  

---

# 🧠 Key Learning Focus

This project emphasizes:

- Understanding how detection systems generate and process data  
- Building security pipelines from scratch  
- Troubleshooting real-world issues across infrastructure layers  
- Applying structured problem-solving approaches  

---

# 🚀 Future Enhancements

- SIEM tuning and rule optimization  
- Integration with cloud platforms (AWS / Azure)  
- Advanced detection engineering  
- Automation and alerting workflows  
- Threat hunting scenarios  

---

# 📁 Repository Structure

/phase-1-suricata/
/phase-2-logstash/
/phase-3-elasticsearch/
/phase-4-kibana/
/phase-5-testing/
/docs/


---

# 👤 Author

Christopher Albrecht  
Cybersecurity | Network Engineering | Cloud Security  

Certifications:
- CompTIA A+
- CompTIA Network+
- CompTIA Security+
- ISC2 Certified in Cybersecurity (CC)
- AWS Cloud Certification
- Microsoft Azure Certification

---

# 📌 Summary

This project demonstrates the ability to design, build, and validate a security monitoring pipeline from the ground up, with a focus on practical application, troubleshooting, and real-world security workflows.

The goal is not just to use tools, but to understand how they work together to detect, analyze, and respond to security events.
