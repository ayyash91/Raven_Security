

# Blue Team: Summary of Operations
## Table of Contents:

* Network Topology
* Description of Targets
* Monitoring the Targets
* Suggestions for Going Further
* Network Topology

## The following machines were identified on the network:
Name of VM 1

* Operating System: `Kali Linux` 
* Purpose: `Red Team Machine` 
* IP Address: `192.168.1.90/24`

Name of VM 2
* Operating System: `Linux` 
* Purpose: `SIEM / ELK Machine`
* IP Address: `192.168.1.100`

Name of VM 3
* Operating System: `Linux` 
* Purpose: `logging data collector` 
* IP Address: `192.168.1.105`  

Name of VM 4
* Operating System: `Linux` 
* Purpose: `Victim Machine` 
* IP Address: `192.168.1.110`  



<img src="/Blue Team Summary of Operations/Images/crearing-alert.png">


## Description of Targets
The target of this attack was: `192.168.1.110`

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

Monitoring the TargetsTraffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

Name of Alert 1
* Excessive HTTP Errors
Alert 1 is implemented as follows:
* Metric: `WHEN count() GROUPED OVER top 5 ‘http.response.status_code’` 
* Threshold: `Above 400 `
* Vulnerability Mitigated: scanning the website directories and Enumeration 
* Reliability: High reliability in detecting any abnormal error request on the website because it is looking for errors from 400 and above. 
 
Name of Alert 2
* HTTP Request Size Monitor

Alert 2 is implemented as follows: 
* Metric: `WHEN sum() of http.request.bytes OVER all documents`
* Threshold: `is above 3500`  
* Vulnerability Mitigate: High Traffic Event 
* Reliability: High reliability in detecting any abnormal traffic like Nmap scan or dirb 

Name of Alert 3
CPU Usage Monitor

Alert 3 is implemented as follows:
* Metric: `WHEN max() OF system.process.cpu.total.pct OVER all documents`
* Threshold: `IS ABOVE 0.5`
* Vulnerability Mitigated: any usage of the website resource 
* Reliability: low reliability due to false-positive alerts during the use of website recourses or computing.

## Suggestions for Going Further 
Each alert above pertains to a specific vulnerability/exploit.
Recall that alerts only detect malicious behavior, but do not stop it. For each vulnerability/exploit identified by the alerts above, suggest a patch. E.g., implementing a blocklist is an effective tactic against brute-force attacks. It is not necessary to explain how to implement each patch.

The logs and alerts generated during the assessment suggest that this network is susceptible to several active threats, identified by the alerts above. In addition to watching for occurrences of such threats, the network should be hardened against them. The Blue Team suggests that IT implement the fixes below to protect the network:

### Poor SSH configuration 

<b>Patch</b>: using an SSH key pair is the most secure method to authenticate clients to servers. 
  
<b>Why</b> It Works<b>: SSH key pair consists of two long keys. A private key that is kept secret on the client device and a public key that can safely be shared. 
  
### WordPress user Enumeration 
  
<b>Patch</b>: 
* install “ stop user enumeration ” plugin
* block user-enumeration via functions.php 
* Block User Enumeration via .htaccess
  
<b>Why It Works</b>: the above methods will stop and prevent wpscan from revealing any users 

### No file security permission implementation for critical files system: wp-config 
  
  
<b>Patch</b>: Protection through .htaccess file, Moving wp-config.php, Setting up the correct file permissions for wp-config.php

<b>Why It Works</b>: 
* block access to your wp-config.php from internal hacking and code modification.
* The best practice is to move the wp-config.php file to an unpredictable location in order to secure the sensitive data stored inside the file. 
* The appropriate file permission for this file will be 400. This means that the user and groups have permission to only read and others will not be able to access the file.
  
### Python privilege escalation 
<b>Patch</b>: remove sudo access for python to any user
  
<b>Why It Works</b>: removing the sudo access from python will make this vulnerability disappear.

Reference: 
* https://phoenixnap.com/kb/linux-ssh-security
* https://wordpress.org/plugins/stop-user-enumeration/
* https://perishablepress.com/stop-user-enumeration-wordpress/
