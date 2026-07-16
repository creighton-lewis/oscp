## Nmap scan 

## Nuclei scan 
 > Took much less time than nmap scan

```
[http-trace:trace-request] [http] [info] https://10.129.229.183
[cookies-without-secure] [javascript] [info] 10.129.229.183 ["elastixSession"]
[cookies-without-httponly] [javascript] [info] 10.129.229.183 ["elastixSession"]
[drupal-directory-listing] [http] [low] https://10.129.229.183/modules/
[waf-detect:apachegeneric] [http] [info] https://10.129.229.183
[INF] Skipped 10.129.229.183:9780 from target list as found unresponsive permanently: cause="port closed or filtered" address=10.129.229.183:9780 chain="connection refused; got err while executing https://10.129.229.183:9780/api/v1/user_assets/nfc"
[snmpv3-detect] [javascript] [info] 10.129.229.183:161 ["Enterprise: unknown"]
[rpc-udp-detect] [javascript] [info] 10.129.229.183:111 ["RPC Portmapper UDP Detected"]
[openssh-detect] [tcp] [info] 10.129.229.183:22 ["SSH-2.0-OpenSSH_4.3"]
[tls-version] [ssl] [info] 10.129.229.183:443 ["tls10"]
[weak-cipher-suites:tls-1.0] [ssl] [low] 10.129.229.183:443 ["[tls10 TLS_RSA_WITH_AES_128_CBC_SHA]"]
[deprecated-tls:tls_1.0] [ssl] [info] 10.129.229.183:443 ["tls10"]
[deprecated-tls:ssl_3.0] [ssl] [info] 10.129.229.183:443 ["ssl30"]
```
