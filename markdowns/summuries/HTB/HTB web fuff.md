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
# add ip as domain using command: ```
sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts'
# then u can use this domain to find subdomains if it was public
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.academy.htb/
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxNTkyODA0NSwyMDQ0OTg2NDA2LC0xNj
YzNTI3NzY1XX0=
-->