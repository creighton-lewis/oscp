# Enumeration 
- Used to send emails 
- Exists on port 25 and port 587
- **Technical Details **
	- Several commands that exist when interacting with SMTP server 
	- Mail headers contain valuable information 
==where is userlist found????==
```
sudo nmap -sV -p 25 --script smtp*

sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v


smtp-user-enum -M VRFY -U users.txt -t target.com

smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7


nuclei -u http://url -tag smtp 




```

```
telnet {ip} 25 
VRFY {expected-username}
EXPN {user-name}

```


**Dangerous Settings **
- **Open Relay Config:** Server accepts mail from any source and forwards it to any destination without verifying the sender's identity or permissions 
- **Missing SMTP Auth:** Weak credentials, lack of authentication can be exploited 
- **Cleartext transmission:** Allowing on port 25 or 587 without enforcing STARTTLS or SMTPS exposes credentials and email content to interception 

# Exploitation 

```
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server 10.10.11.213
```
