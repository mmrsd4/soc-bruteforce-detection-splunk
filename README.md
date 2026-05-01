### **SOC Detection Lab: SSH Brute-Force Attack Simulation \& Detection using Splunk**



#### Overview



This project demonstrates an end-to-end Security Operations Center (SOC) workflow by simulating an SSH brute-force attack and detecting it using Splunk SIEM.



The lab replicates a real-world attack scenario where an adversary attempts unauthorized access via repeated SSH login attempts. System authentication logs are collected, forwarded, analyzed, and correlated to identify malicious behavior, followed by alert generation and visualization.





#### 

#### Objectives



* Simulate a brute-force attack using industry-relevant tools
* Collect and centralize logs using a SIEM platform
* Develop detection logic for brute-force patterns
* Create actionable alerts for security incidents
* Visualize attack data through dashboards







#### Architecture Overview



The environment consists of an attacker system, a target host, and a centralized SIEM for monitoring and detection.



![Architecture](screenshots/1-architecture.png)







#### Data Flow Pipeline





Kali Linux (Attacker - Hydra)

&#x20;       ↓

Ubuntu Server (Target - SSH logs generated)

&#x20;       ↓

Splunk Universal Forwarder (Log collection)

&#x20;       ↓

Splunk Enterprise (SIEM - Analysis \& Detection)

&#x20;       ↓

Alerts + Dashboards + Incident Visibility









#### Technology Stack




&#x20;Kali Linux                 | Attack simulation

&#x20;Hydra                      | Brute-force attack tool

&#x20;Ubuntu Server              | Target system

&#x20;OpenSSH                    | Authentication service

&#x20;Splunk Universal Forwarder | Log forwarding agent

&#x20;Splunk Enterprise          | SIEM platform

&#x20;auth.log                   | Authentication log source







##### Lab Implementation



###### 1\. Target Configuration (Ubuntu)



* &#x20;Installed and enabled OpenSSH service
* &#x20;Verified SSH accessibility
* &#x20;Monitored authentication logs at:



/var/log/auth.log





###### 2\. Attack Simulation (Kali Linux)



A brute-force attack was launched using Hydra against the SSH service.



hydra -l root -P rockyou.txt ssh://192.168.157.134





![Hydra Attack](screenshots/2-hydra-attack.png)







###### 3\. Log Generation \& Analysis



The attack generated multiple failed authentication attempts recorded in:





/var/log/auth.log





Key indicators observed:



* &#x20;Repeated “Failed password” entries
* &#x20;Source IP address correlation
* &#x20;High-frequency login attempts



![Auth Log](screenshots/3-auth-log.png)





###### 4\. Log Forwarding Architecture



Installed Splunk Universal Forwarder on Ubuntu

Configured monitoring for:



&#x20; /var/log/auth.log

&#x20;



* Forwarded logs to Splunk Enterprise via port 9997





![Splunk Data Input](screenshots/4-splunk-data-input-1.png)

![Splunk Data Input](screenshots/4-splunk-data-input-2.png)





###### 5\. Detection Engineering (SPL)



A detection rule was developed to identify brute-force behavior based on failed login frequency.



spl

index=linux\_logs sourcetype=auth\_log "Failed password"

| stats count by src\_ip

| where count > 5





###### Detection Logic



* &#x20;Aggregates failed login attempts per source IP
* &#x20;Flags IPs exceeding threshold
* &#x20;Identifies potential brute-force activity



![Splunk Search](screenshots/5-splunk-search.png)







###### 6\. Alerting Strategy



An alert was configured with the following parameters:



Type: Scheduled

Frequency: Every 1 minute

Trigger Condition: Number of results > 204

Throttle: Enabled



This ensures timely detection while reducing alert noise.



![Alert](screenshots/7-alert.png)







###### 7\. Visualization \& Monitoring



A Splunk dashboard was developed to provide real-time visibility into attack patterns:



* &#x20;Failed login attempts over time
* &#x20;Top attacking IP addresses
* &#x20;Event frequency distribution



![Dashboard](screenshots/6-dashboard.png)







#### Key Detection Insight



This project demonstrates how brute-force attacks can be identified using simple statistical thresholds applied to authentication logs.





##### Key correlation:



Attack source IP (Hydra) → Appears in auth.log → Detected in Splunk



This validates end-to-end visibility across the SOC pipeline.







### Project Structure





SOC-BruteForce-Detection/

│

├── README.md

├── screenshots/

│   ├── 1-architecture.png

│   ├── 2-hydra-attack.png

│   ├── 3-auth-log.png

│   ├── 4-splunk-data-input.png

│   ├── 5-splunk-search.png

│   ├── 6-dashboard.png

│   ├── 7-alert.png

│

├── splunk/

│   ├── queries.txt

│   ├── alert-config.md

│

├── setup/

│   ├── ubuntu-setup.md

│   ├── kali-attack.md

│   ├── splunk-setup.md







### MITRE ATT\&CK Mapping



| Technique   | ID    | Description                                           |

| ----------- | ----- | ----------------------------------------------------- |

| Brute Force | T1110 | Adversaries attempt to guess passwords to gain access |







#### Skills Demonstrated



* &#x20;Security Monitoring (SOC Operations)
* &#x20;SIEM Implementation (Splunk)
* &#x20;Log Analysis \& Correlation
* &#x20;Detection Engineering (SPL)
* &#x20;Alert Tuning \& Noise Reduction
* &#x20;Threat Identification \& Analysis







#### Limitations



* Detection is threshold-based (may generate false positives)
* No automated response (e.g., IP blocking)
* Limited to SSH authentication logs





#### Conclusion



This project successfully demonstrates an end-to-end SOC workflow by simulating a real-world SSH brute-force attack and detecting it using centralized log analysis in Splunk.



The implementation highlights how raw authentication logs can be transformed into actionable security insights through proper log collection, correlation, and detection engineering. By applying threshold-based analysis on failed login attempts, suspicious activity was identified and escalated through alerting mechanisms.



Beyond basic setup, this lab emphasizes key SOC principles including visibility across systems, event correlation, and alert tuning to reduce noise. The ability to trace a single attacker IP across multiple stages—from attack generation to SIEM detection—validates the effectiveness of the monitoring pipeline.



While the detection logic in this project is intentionally simple, it provides a solid foundation for building more advanced threat detection use cases involving behavioral analysis, enrichment, and automated response.



This project reflects practical, hands-on experience with SIEM workflows and demonstrates the core responsibilities of a SOC analyst in identifying and responding to potential security incidents.

