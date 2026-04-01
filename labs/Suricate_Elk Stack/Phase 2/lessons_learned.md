🧠 Lessons Learned
1. Logs Become Valuable Only When Centralized
Raw logs on a single system are difficult to use
ELK transforms logs into searchable, structured data

Key Insight:

Centralization is required for effective monitoring and analysis

2. Storage Design is Critical
Elasticsearch is storage-intensive
Moving data to HDD prevents SSD exhaustion

Key Insight:

SIEM systems require deliberate storage architecture

3. Parsing is Required for Usability
Raw JSON logs are not immediately usable
Logstash converts unstructured logs into indexed data

Key Insight:

Parsing transforms data into actionable intelligence

4. Pipelines Must Be Validated End-to-End
Verified:
Log generation
Log ingestion
Index creation
Visualization

Key Insight:
A working pipeline must be validated at every stage

5. Visibility Extends Beyond Detection
Phase 1 = detection
Phase 2 = visibility + analysis

Key Insight:

Detection without visibility limits operational value

6. Performance and Resource Planning Matters
ELK stack is resource-heavy
Requires RAM and storage planning

Key Insight:

SIEM systems must be designed with scalability in mind

7. SIEM is a Pipeline, Not a Tool
Multiple components must work together:
Suricata
Logstash
Elasticsearch
Kibana

Key Insight:

SIEM functionality is achieved through integrated systems

🚀 Next Steps
Build dashboards in Kibana
Tune detection rules
Reduce false positives
Implement alerting workflows
Expand visibility using traffic mirroring (SPAN)

