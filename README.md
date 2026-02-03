# Splunk SIEM Log Analysis & Threat Detection
A TryHackMe challenge simulating a SOC investigation using Splunk to analyze custom web and firewall logs from a staged ransomware attack. The lab focuses on SIEM log monitoring, threat detection, and attack‑chain reconstruction using SPL.

### Tools Used
- Splunk Enterprise
- SPL (Search Processing Language)
- Web Traffic logs
- Firewall logs
### Overview
This project showcases a step-by-step SOC investigation of malicious activity inside Splunk. The analysis identifies attacker behavior across reconnaissance, exploitation, webshell deployment, ransomware staging, and outbound C2 communication. Let me walk you through each stage where I used SPL, what I was looking for, and what I found.  

1. As a first step, I loaded the custom datasets (web_traffic and firewall_logs) into Splunk and ran the initial search to get an overview of the log volume using the statment
   `index=main`.  I then made sure the timeframe was selected to "all time".
2. To find out what days have a high volume of logs, I used to search query `index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse` what this does is, it brings on top of the list, the day which had the maximum amount of logs. To visualize the spike in log data, I shifted to the "Visualization" tab.
3. I turned to "user-agent" field to find out any agents that weren't the legitimate user agents like Mozilla's or Chrome's variants.
4. Splunk's ip_client field also helped identify certain ip addresses.
5. To list out the suspicious user-agents and see which Ip address they're associated to, I ran the search query `index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*` this excluded all the legitimate user agents, and what was left, were the suspicious ones. 
