- `-ic` to skip comments
- `-fs xxx` to filter responses by size 
## Basic Fuzzing
- **Directory Fuzzing**
```bash
# ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
- **Extension Fuzzing**
```bash
# we use the page index , and word list has . 
ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```
- **Page Fuzzing** 
```bash
# after getting that the extension was php we use .php
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ -e .php,.html,.txt,.bak,.js -v
# or
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
- **Recursive Fuzzing** 
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
## Domain Fuzzing
- **DNS Record**
```bash
# to access some ip using the domain academy.htb
sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'
```
- **Sub-domains (public domains)**
```bash
gobuster dns -d inlanefreight.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
# or
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.academy.htb/
```
- **Vhost Fuzzing (private on same machine)** 
```bash
gobuster vhost -u http://inlanefreight.htb:PORT -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
# or
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
# check the size of responses
# add -fs 900 to remove responses that has size 900
```
## Param Fuzzing
- **Param Fuzzing - GET**
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```
- **Param Fuzzing - POST**
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
- **Value Fuzzing**
```bash
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
# or
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

## API Fuzzing
- **API types:** RUST, SOAP, GraphQL
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExNzA5NTQ2MywtNDYyNDEyNTQwLDY1MD
QyMTIzOSwtMTI3MDIyNjUxMCwtMTEyMDA5NDQsLTExMjAwOTQ0
LDEyOTA5Mzk1ODIsLTE2NDQ2MDIxODQsLTE1MTQwODMzODgsLT
QyNTkxMTg5MCwtODE1OTI4MDQ1LDIwNDQ5ODY0MDYsLTE2NjM1
Mjc3NjVdfQ==
-->