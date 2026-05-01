##### Kali Linux - Brute Force Attack (Hydra)



###### Tool Used

Hydra



* **Command Used**

hydra -l jan -P /usr/share/wordlists/rockyou.txt ssh://192.168.157.130



* **Explanation**
1. \-l jan → target username
2. \-P → password list
3. ssh:// → attack protocol
4. Target IP → Ubuntu server



* **Expected Result**

Multiple failed login attempts are generated in the target system logs.



**Evidence**

These attempts are captured in:

* /var/log/auth.log (Ubuntu)
* Splunk SIEM (forwarded logs)

