# Reven_Security


## <strong><bold>Summary:</bold></strong>




## <strong>Tools Used in The Project </strong>: 

Kali Linux | Nmap | dirb | wpscan | john | Python | nikto | gobuster | searchsploit | netcat (nc) | Kibana | Network Traffic Analysis | System Hardening | Wireshark | Packet Analysis 


## <strong>Network Topology <strong>:
  
  
<img src="/Network Topology/Network.png">
  
  
  
  
 | Hostname  | IP Address  | Role on Network  |
|---|---|---|
|  ML-RefVM-684427 | 192.168.1.1 | Default Gateway  |
| Kali  |  192.168.1.90 | Red Team Machine  |
|  ELK | 192.168.1.100  |  SIEM System |
| Capstone  | 192.168.1.105  |  Forward logs to the ELK machine | 
| Target 1  | 192.168.1.110 |  Vulnerable WordPress server | 
| Target 2  | 192.168.1.115 |  Vulnerable WordPress server 2 |  
