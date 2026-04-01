# Phase 2 — Log Pipeline (ELK Stack)

## Objective
Design and implement a centralized log pipeline to ingest, parse, store, and visualize Suricata IDS logs using the ELK stack (Elasticsearch, Logstash, Kibana).

---

## Outcome


- Successfully ingested Suricata logs (`eve.json`) into Elasticsearch  
- Built a working SIEM pipeline using Logstash  
- Visualized alerts in Kibana  
- Enabled searchable, structured security event data  
- Transitioned from raw logs → actionable intelligence  

---

# 🧱 Architecture

```text
Traffic → Suricata
              ↓
        eve.json logs
              ↓
        Logstash (parsing)
              ↓
        Elasticsearch (storage/indexing)
              ↓
        Kibana (visualization)
🛠️ Implementation Steps
1. Elasticsearch Installation

Added Elastic repository and installed Elasticsearch:

sudo apt update
sudo apt install elasticsearch -y
2. Elasticsearch Configuration

Edited configuration:

sudo apt update
sudo apt install elasticsearch -y
2. Elasticsearch Configuration

Edited configuration:

sudo nano /etc/elasticsearch/elasticsearch.yml

Set:

network.host: 0.0.0.0
discovery.type: single-node
path.data: /data/elk
3. Storage Optimization

Created dedicated storage on HDD:

sudo mkdir -p /data/elk
sudo chown -R elasticsearch:elasticsearch /data/elk
4. Start Elasticsearch
sudo systemctl daemon-reexec
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
5. Validation
curl http://localhost:9200

Result:

Confirmed Elasticsearch node running and responding
6. Kibana Installation
sudo apt install kibana -y
7. Kibana Configuration
sudo nano /etc/kibana/kibana.yml

Set:

server.host: "0.0.0.0"
8. Start Kibana
sudo systemctl enable kibana
sudo systemctl start kibana
9. Access Kibana
http://10.10.30.100:5601
10. Logstash Installation
sudo apt install logstash -y
11. Logstash Pipeline Configuration

Created pipeline:

sudo nano /etc/logstash/conf.d/suricata.conf
Configuration:

input {
  file {
    path => "/data/suricata/logs/eve.json"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

filter {
  json {
    source => "message"
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "suricata-%{+YYYY.MM.dd}"
  }
}


12. Start Logstash
sudo systemctl start logstash
13. Generate Test Traffic

From Kali:

nmap -sS -T4 -A 10.10.30.100
14. Validate Index Creation
curl 'http://localhost:9200/_cat/indices?v'

Result:

Confirmed Suricata index created:
suricata-YYYY.MM.DD
15. Kibana Index Pattern

Created index pattern:
suricata-*

Selected time field:

@timestamp
16. Visualization
Viewed alerts in Kibana Discover
Confirmed:
Source IP
Destination IP
Alert signatures
Timestamps
💥 Summary

Phase 2 successfully established:

Centralized log ingestion
Structured event parsing
Searchable log storage
Visual alert analysis via Kibana
