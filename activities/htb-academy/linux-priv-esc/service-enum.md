# Service-Based Privilege Escalation 
## Linux Capabilities 
| Capability Name |Capability description |
|----------|----------|
| cap_dac_override  |  Can bypass permission checks on any file     |
| cap_sys_admin  |   Can perform actions with admin privileges  |
| cap_chown | Can make aribitrary changes to file UIDs and GIDs |
| cap_audit_control | Can enable/disable kernel auditing, change filters and retrieve status and filtering roles           |
| cap_audit_write  | Can write records to kernel auditing log   |

## Cron Job Abuse 
>[!NOTE]
>Attacker must have write access
>
>Script should be ran in shared directory
>
>Best if script is scheduled to run as root
>
>Capturing cron job in psspy output is encouraged & possible

> Where cron jobs are located

```
/etc/cron.hourly
/etc/cron.weekly
/etc/cron.monthly
```

```
crontab -e
crontab -l
```
## Containers
```
Process >> Identify containers that exist  >> Enumerate and exploit
                                           >> Identify operating system and import your own
```
### Docker 
>[!NOTE]
> How to find out if docker container exists on system 
```
wget https://github.com/stealthcopter/deepce/raw/main/deepce.sh

```
