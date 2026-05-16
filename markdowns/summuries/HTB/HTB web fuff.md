- `-ic` to skip comments
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
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
- **Recursive Fuzzing** 
```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
- **Sub-domains**
```bash
# public domains
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.academy.htb/
```
- **Vhost Fuzzing** 
```bash
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
# check the size of responses
# add -fs 900 to remove responses that has size 900
```
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
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI5MDkzOTU4MiwtMTY0NDYwMjE4NCwtMT
UxNDA4MzM4OCwtNDI1OTExODkwLC04MTU5MjgwNDUsMjA0NDk4
NjQwNiwtMTY2MzUyNzc2NV19
-->