\# Splunk Setup (SIEM)



* Install Splunk Enterprise

Download and install Splunk on host machine.



* Add Data Input

&#x20; Source Type: auth

&#x20; Log Path: /var/log/auth.log



* Configure Forwarding (if used)

Install Splunk Universal Forwarder on Ubuntu and connect to Splunk server.



* Verify Data Ingestion

Search:

index=\* sourcetype=auth



* Detection Query

index=\* sourcetype=auth "Failed password"

| stats count by src\_ip

| where count > 10



* Dashboard

Created dashboard to visualize:

&#x20; Failed login trends

&#x20; Attacker IPs



**Alert**

Configured alert to trigger when brute-force threshold is exceeded.

