\## SSH Brute Force Detection Alert



\### Detection Query

index=\* sourcetype=auth "Failed password"

| stats count by src\_ip

| where count > 10



\### Trigger Condition

\- Alert triggers when results > 0



\### Alert Type

\- Scheduled search (every 5 minutes)



\### Severity

\- Medium (can be High if threshold exceeded repeatedly)



\### Description

This alert detects multiple failed SSH login attempts from a single IP address, indicating a potential brute-force attack.



\### Observed Attack (From Lab)

\- Attacker IP: 192.168.157.129

\- Repeated login failures using invalid users

\- Confirmed using Hydra attack simulation



\### Action Taken

\- Log event generated in Splunk

\- (Optional improvement: send email alert or webhook)

