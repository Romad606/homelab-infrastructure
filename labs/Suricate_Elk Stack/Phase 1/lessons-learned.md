# 🧠 Suricata Lab — Lessons Learned

---

## 1. Visibility is Everything

An IDS is only effective if it can observe traffic.

- Initial issue: No detection despite logs being generated  
- Root cause: Traffic was not reaching the monitored interface  
- Resolution: Corrected network placement and routing  

**Key Insight:**  
> Detection is entirely dependent on network visibility  

---

## 2. Network Architecture Drives Security

- pfSense acted as the central routing and segmentation point  
- Multi-subnet design enforced controlled traffic paths  

**Key Insight:**  
> Security tools rely on proper Layer 3 design and segmentation  

---

## 3. Firewall Rules Are Directional

- Traffic was allowed in one direction but blocked in another  
- Misconfiguration: Interface rule used "address" instead of "subnet"  

**Key Insight:**  
> Firewall rules must account for traffic direction and scope  

---

## 4. Host-Based Firewalls Can Break Everything

- UFW was enabled on the Suricata VM  
- Blocked outbound communication  

**Key Insight:**  
> Troubleshooting must include both network and host-level controls  

---

## 5. Default Configurations Are Not Enough

- Suricata required:
  - Rule updates  
  - Interface binding  
  - Log configuration  

**Key Insight:**  
> Security tools must be tuned to be effective  

---

## 6. Logs Are Data — Not Intelligence

- Raw logs were noisy and difficult to interpret  
- Used `jq` to filter meaningful alerts  

**Key Insight:**  
> Detection requires parsing, filtering, and context  

---

## 7. Signature-Based Detection Behavior

- Nmap scans triggered different alerts based on scan type  
- Specific ports generated high-confidence detections  

**Key Insight:**  
> Detection is based on patterns, not attacker intent  

---

## 8. Attack-to-Detection Validation

End-to-end validation:

```text
Attacker → Network → Target → Suricata → Alert

Key Insight:

Detection must be validated through controlled attack simulation

9. Multi-Layer Troubleshooting

Issues were resolved across:

Layer 2: Proxmox bridges
Layer 3: Routing
Layer 4: Firewall rules
Layer 7: Application access

Key Insight:

Effective troubleshooting requires understanding across the OSI model

10. Storage Design Matters
Logs grow rapidly and impact performance
Implemented SSD + HDD tiered storage

Key Insight:

SIEM systems require separation of compute and storage

💥 Final Takeaway

This lab demonstrated that effective detection is not just about tools, but about:

Network design
Visibility
Configuration
Validation
🚀 Next Steps
Integrate ELK stack for log ingestion and visualization
Build dashboards in Kibana
Tune detection rules
Expand traffic visibility (SPAN / mirroring)

