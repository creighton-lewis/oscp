# Scans 
## Rustscan 
Open 10.129.30.252:53
Open 10.129.30.252:139
Open 10.129.30.252:389
Open 10.129.30.252:445
Open 10.129.30.252:464
Open 10.129.30.252:593
Open 10.129.30.252:636
Open 10.129.30.252:3268
Open 10.129.30.252:3269
Open 10.129.30.252:5985
Open 10.129.30.252:8443
Open 10.129.30.252:9389
## Nmap Scan 
```
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-07-18 03:06:14Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-07-18T03:08:13+00:00; +3h59m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN:AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-07-18T03:08:13+00:00; +4h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN:AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-07-18T03:08:13+00:00; +3h59m59s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN:AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: authority.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-07-18T03:08:14+00:00; +4h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: othername: UPN:AUTHORITY$@htb.corp, DNS:authority.htb.corp, DNS:htb.corp, DNS:HTB
| Not valid before: 2022-08-09T23:03:21
|_Not valid after:  2024-08-09T23:13:21
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
8443/tcp open  ssl/http      Apache Tomcat (language: en)
| ssl-cert: Subject: commonName=172.16.2.118
| Not valid before: 2026-07-16T02:41:32
|_Not valid after:  2028-07-17T14:19:56
|_http-title: Site doesn't have a title (text/html;charset=ISO-8859-1).
|_http-trane-info: Problem with XML parsing of /evox/about
|_ssl-date: TLS randomness does not represent time
Aggressive OS guesses: Microsoft Windows Server 2016 (93%), Microsoft Windows Server 2019 (93%), Microsoft Windows 10 21H2 (90%), Microsoft Windows 10 (89%), Microsoft Windows 10 1709 - 1803 (89%), Microsoft Windows 10 1709 - 21H2 (89%), Microsoft Windows 10 1809 - 21H2 (89%), Microsoft Windows 11 21H2 (89%), Microsoft Windows 10 1703 (89%), Microsoft Windows 10 1803 (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: AUTHORITY; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3h59m59s, deviation: 0s, median: 3h59m59s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-07-18T03:08:03
|_  start_date: N/A

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   79.98 ms  10.10.16.1
2   303.96 ms 10.129.30.252

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 152.01 seconds
oscp-prep/authority » 
```
## Nuclei-scan 
# Observations 
- Obviously a windows machine
- Several ports open
- Services to check
- SMB
   - Can I connect?
   - No, access denied
   - Not smbV1 version
 
- LDAP
  
