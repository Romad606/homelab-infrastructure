🧠 Lessons Learned
1. Visualization Turns Logs Into Operations
Raw logs are difficult to interpret at scale
Kibana makes detections visible in a way analysts can quickly understand

Key Insight:

Visualization transforms data into operational awareness

2. Searchability is Critical for Analysis
Being able to filter on fields like src_ip, dest_ip, and alert.signature made analysis much faster
Discover is often the first place to validate whether a pipeline is actually useful

Key Insight:

Searchable data is the foundation of security analysis

3. Dashboards Provide Context, Not Just Data
A single alert is useful
A dashboard shows patterns, frequency, and relationships over time

Key Insight:

Dashboards help analysts move from single events to situational awareness

4. Not Every Alert is Equally Valuable
Some detections are high-signal
Others are expected noise or repeated activity from testing

Key Insight:

Detection engineering is about improving signal-to-noise ratio, not just generating alerts
5. Testing Must Continue After Visualization
Building the pipeline was not enough
Fresh traffic generation was required to validate that the dashboard reflected real activity

Key Insight:

Monitoring systems must be continuously validated with known test activity

6. Detection Engineering Begins With Interpretation
Before tuning rules, it is necessary to understand what alerts mean and why they fired
Context matters as much as signatures

Key Insight:

Effective tuning starts with understanding detection behavior

7. SIEM Value Comes From Integration
Suricata alone provides alerts
ELK alone stores and displays data
Together they create operational visibility

Key Insight:

Security monitoring value comes from integrated systems, not isolated tools

🚀 Next Steps
Expand dashboard coverage for additional event types
Tune noisy signatures and improve alert quality
Create saved searches for common analyst workflows
Add more attack scenarios to validate detections
Explore rule suppression and threshold tuning
Improve visibility through traffic mirroring / SPAN design
