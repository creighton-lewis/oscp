
# Scans 
## Rustscan 
- only ports open are 22 and 80
## Nmap 
```
# Nmap 7.95 scan initiated Sat Jul 18 20:15:55 2026 as: nmap -sV -sC -T 4 -oN scan-1 10.129.31.123
Nmap scan report for 10.129.31.123
Host is up (0.15s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
80/tcp open     http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://titanic.htb/
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: Host: titanic.htb

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done a
```
