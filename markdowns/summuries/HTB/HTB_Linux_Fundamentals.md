# Used things
**[GTFbins](https://gtfobins.github.io/)** - privileges escalation
` grep -r word . ` find a word in folder
`find . -type f -exec grep word /dev/null {} +` find a word in folder

# 🖥️ System Information
- `-h`, `man`, `info` – Command info/help

- `whoami` – Show current user
- `id` – Returns user identity
- `hostname` – Show system's host-name
- `uname` – Print basic info about the OS and hardware


---
# 🔄 Workflow
## 📁 Navigation  

- `pwd`, `ls`, `cd` – Navigation 📁
- `tree .` – Show directory structure 🌲
- `touch`, `mkdir` – Create file or folder 📄
- `cp`, `mv` – Copy, cut/rename files 🗄️

- `vim`, `nano` – Edit files 📝
----------
## 🔍 Find Files and Directories
### Using `find`
```bash
find <location> <options>
```
- `-type f` – 'f' stands for 'file'
- `-name *.conf`
- `-user root`
- `-size +20k`
- `-newermt 2020-03-03`
### Faster Search with 'locate'
```bash
sudo updatedb | locate *.conf
```
----------
## 🔧 File Descriptors and Redirections

-   **Input**: `STDIN` – 0
    
-   **Output**: `STDOUT` – 1
    
-   **Error**: `STDERR` – 2
    
|command|  description|
|--|--|
|  `[command] 2>/dev/null`|Redirect error to null  |
|  `[command> 2>/dev/null 1>results.txt `|Error to null, output to file |
|  `[command] < stdout.txt`|Use file as input |
|  `[command] >>results.txt`|Append output to file (no overwrite)  |
|  `[command1] | [command2]`|Pipe: use command2 on the result of command1  |

----------
## 🧹 Filter Contents

-   `cat`, `more`, `less` – Display content of file
    
-   `head`, `tail` – Show first/last lines of file
    
-   `cat <file> | sort` – Sort file lines
    
-   `cat <file> | grep "str"` – Find lines containing `str`
    
-   `cat <file> | grep -v "false\|nolog"` – Exclude lines with `false` or `nolog`
    

### Cut and Replace Examples

```bash
cat <file> | cut -d":" -f1
# Example:
# root:x:0:0:root:/root:/bin/bash  -->  root

cat <file> | tr ":" " "
# Example:
# root:x:0:0:root:/root:/bin/bash  -->  root x 0 0 root /root /bin/bash
```

----------
## 🔐 Permissions

-   `chmod` – Change permissions
   
-   `chown <user>:<group> <file>` – Change owner

---
# ⚙️ System Management

## 🧠 Service and Process Management

-   `systemctl start ssh` – Start process in the background (`ssh` here)
    
-   `systemctl status ssh` – Get status of the process
    
-   `systemctl enable ssh` – Auto-run service on startup
    
-   `ps -aux | grep ssh` – Check running process info
    
-   `systemctl list-units --type=service` – List all services
    

### Kill Processes

-   `kill -l` – Show kill signal options
    
-   `kill 9 <PID>` – Force kill process with signal 9
    

### Background Jobs

-   `[Ctrl + Z]` – Pause (background) current process
    
-   `jobs` – Display all background processes
    
-   `bg` – Resume a process in the background
    
-   `fg 1` – Bring process 1 to the foreground
----------
## ⏰ Task Scheduling

### 🔄 Systemd Timers
1- Create a timer (schedules when your mytimer.service should run)
2- Create a service (executes the commands or script)
3- Activate the timer

1.  **Create a Timer**
    

```bash
sudo mkdir /etc/systemd/system/mytimer.timer.d
sudo vim /etc/systemd/system/mytimer.timer
```

> inside **mytimer.timer**

```
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min           # Run once after system boot
OnUnitActiveSec=1hour    # Repeat every hour

[Install]
WantedBy=timers.target
```

2.  **Create a Service**
    

```bash
sudo vim /etc/systemd/system/mytimer.service
```

> inside **mytimer.service**
```
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

>  **Reload Systemd**

```bash
sudo systemctl daemon-reload
```

3.  **Start the Timer & Enable It**
    

```bash
sudo systemctl start mytimer.timer
sudo systemctl enable mytimer.timer
```

----------

### 🕰️ Cron Jobs
Cron stores tasks in a file called `crontab`.

> `Minute` `Hour` `Day_of_Month` `Month` `Day_of_Week` `Command_Path`
```
# Format:
# Minute Hour Day_of_Month Month Day_of_Week Command_Path

# System Update every 6 hours
0 */6 * * * /path/to/update_software.sh

# Execute scripts on the 1st of every month
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB every Sunday at midnight
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backup every Sunday at midnight
0 0 * * 7 /path/to/scripts/backup.sh
```
----------
## 🌐 Network Services

### 🔐 SSH

-   `ssh username@10.129.17.122` – Connect to SSH server
    
-   `/etc/ssh/sshd_config` – SSH configuration file
    

### 📂 NFS (Network File System)

-   Install NFS server:
    

```bash
sudo apt install nfs-kernel-server -y
```

-   Config file: `/etc/exports`
    

#### Create NFS Share

```bash
mkdir nfs_sharing
echo '/home/<user>/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cat /etc/exports | grep -v "#"
```

#### Mount NFS Share

```bash
mkdir ~/target_nfs
mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
tree ~/target_nfs
```

### 🌐 Web Server / VPN

-   VPN:
```bash
sudo openvpn --config internal.ovpn
```
----------
## 🛍️ Working with Web Services

### 🔢 Apache Modules

-   `mod_ssl` – Secure communication by encrypting data between browser and server
    
-   `mod_proxy` – Acts as a traffic controller, redirecting requests
    
-   `mod_headers` and `mod_rewrite` – Modify HTTP headers and rewrite URLs
    
> Start Apache server
> 
```bash
sudo systemctl start apache2   
```

-   Config file: `/etc/apache2/ports.conf`
    

----------

## 📃 Backup and Restore

```bash
# Install rsync (backup tool)
sudo apt install rsync -y  

# Backup local directory to remote server
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory

# Restore backup from remote server
rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory

# Secure transfer with SSH
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory

# ...
```
----------
## 📦 Containerization

### 🐳 Docker

 > First, creating Dockerfile
> Then, Build Docker image
```bash
docker build -t FS_docker .                      
```
>  Run container
```bash
docker run -p <host port>:<docker port> -d FS_docker  
```
 > Example:
```
docker run -p 8022:22 -p 8080:80 -d FS_docker
```

#### Docker Management
| command | comment |
|--|--|
|`docker ps` | # List running containers |
|`docker stop <id>` | # Stop a container |
|`docker start <id>` | # Start a stopped container |
|`docker restart <id>` | # Restart a container |
|`docker rm <id>` | # Remove a container |
|`docker rmi <image>` | # Remove an image |
|`docker logs <id>` | # View container logs |

### 🧱 Linux Containers (LXC)

|  |  |
|--|--|
| `sudo apt-get install lxc lxc-utils -y` |  Install LXC|
| `sudo lxc-create -n linuxcontainer -t ubuntu` | Create Ubuntu container|
| `lxc-ls` | List containers |
|`lxc-start -n <container>` | Start container |
| `lxc-stop -n <container>` |Stop container |
| `lxc-restart -n <container>` | Restart container |
| `lxc-config -n <name> -s storage` | Manage storage |
| `lxc-config -n <name> -s network` | Manage network |
| `lxc-config -n <name> -s security` | Manage security |
| `lxc-attach -n <container>` | Connect to container |
| `lxc-attach -n <container> -f /path/to/share` | Share dir/file with container |
| `sudo vim /usr/share/lxc/config/linuxcontainer.conf` | Config file for container |

> Example config file for container :
```
lxc.cgroup.cpu.shares = 512
lxc.cgroup.memory.limit_in_bytes = 512M
```

----------
# 🌐 Network Configuration

|  |  |
|--|--|
| `ifconfig , ip addr` | Network Settings |
| `sudo ifconfig eth0 up` | Activate Network Interface|
| `sudo ip link set eth0 up` | Activate Network Interface|
| `sudo ifconfig eth0 192.168.1.2` |   IP Address to an Interface |
| `sudo ifconfig eth0 netmask 255.255.255.0` | Assign a Netmask to an Interface |
| `sudo route add default gw 192.168.1.1 eth0` | Assign the Route to an Interface |
| `sudo vim /etc/resolv.conf ` | Editing DNS Settings |
| `sudo vim /etc/network/interfaces `|  Editing Interfaces |
| `sudo systemctl restart networking ` | Restart Networking Service |


### Network Access Control

-   Discretionary access control (DAC)
    
-   Mandatory access control (MAC)
    
-   Role-based access control (RBAC)
    

### Troubleshooting

-   **Ping** 
    
-   **Traceroute** : Traces the route
    
-   **Netstat** : Display active network connections and ports
    
-   **Tcpdump**
    
-   **Wireshark**
    
-   **Nmap**
    

----------

## 💻 Remote Desktop Protocols in Linux

### RDP & VNC Ports

-   TCP/6001-6009 (6000 = display 0)
    
    -   RDP (Remote Desktop Protocol) – for Windows
        
    -   VNC (Virtual Network Computing) – for Linux
        

### ✨ XServer Forwarding (via SSH)

```bash
cat /etc/ssh/sshd_config | grep X11Forwarding   # X11Forwarding yes
ssh -X htb-student@10.129.23.11 /usr/bin/firefox  # Start app from client
```

### 🔐 VNC Setup (TigerVNC)

-   Ports: TCP/5001-5009 (5000 = display 0)
    
-   Tools:
    
    -   TigerVNC
        
    -   TightVNC
        
    -   RealVNC (most used)
        
    -   UltraVNC (most used)
        

#### Install TigerVNC

```bash
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
vncpasswd
```

#### Configure VNC

```bash
touch ~/.vnc/xstartup ~/.vnc/config
cat <<EOT >> ~/.vnc/xstartup
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
EOT
chmod +x ~/.vnc/xstartup
```

#### Start VNC Server

```bash
vncserver
```

#### Manage Sessions

```bash
vncserver -list
```

#### SSH Tunnel for VNC

```bash
ssh -L 5901:127.0.0.1:5901 -N -f -l htb-student 10.129.14.130
```

#### Connect to VNC

```bash
xtightvncviewer localhost:5901
```
# 🔒 Linux Hardening

##  Linux Security 
### Some security settings

-   Removing or disabling all unnecessary services and software
    
-   Removing all services that rely on unencrypted authentication mechanisms
    
-   Ensure NTP is enabled and Syslog is running
    
-   Ensure that each user has its own account
    
-   Enforce the use of strong passwords
    
-   Set up password aging and restrict the use of previous passwords
    
-   Locking user accounts after login failures
    
-   Disable all unwanted SUID/SGID binaries
    

### 🔹 TCP Wrappers

-   `/etc/hosts.allow` – Services/hosts allowed access:
    

```
# Allow access to SSH from the local network
sshd : 10.129.14.0/24

# Allow access to FTP from a specific host
ftpd : 10.129.14.10

# Allow access to Telnet from .inlanefreight.local domain
telnetd : .inlanefreight.local
```

-   `/etc/hosts.deny` – Services/hosts denied access:
    

```
# Deny access to all services from .inlanefreight.com domain
ALL : .inlanefreight.com

# Deny access to SSH from a specific host
sshd : 10.129.22.22

# Deny access to FTP from IPs 10.129.22.0 - 10.129.22.255
ftpd : 10.129.22.0/24
```

----------

## Firewall Setup
### Iptables Components:

-   **Tables** – Organize firewall rules
    
-   **Chains** – Group rules by traffic type
    
-   **Rules** – Define filtering criteria/actions
    
-   **Matches** – Match specific traffic conditions
    
-   **Targets** – Action (ACCEPT, DROP, REJECT, etc.)
    

[More Info](https://academy.hackthebox.com/module/18/section/2099)

----------

## 📈 Logs

-   **Kernel Logs**: `/var/log/kern.log` kernel, including hardware drivers, system calls, and kernel events.
    
-   **System Logs**: `/var/log/syslog` system-level events, such as service starts and stops, login attempts, and system reboots
    
-   **Authentication Logs**: `/var/log/auth.log` user authentication attempts, including successful and failed attempts
    
-   **Application Logs**: `/var/log/<app>/error.log` activities of specific applications running on the system
    
-   **Security Logs**: `/var/log/fail2ban.log`, `/var/log/ufw.log`, etc. recorded in a variety of log files, depending on the specific security application or tool in use. 

    
----------

# ➕ Linux Distributions vs Solaris
|  |Linux |Solaris|
|--|--|--|
| System Information | `uname -a` | `showrev -a` |
| Installing Packages | `sudo apt-get install apache2` | `pkgadd -d SUNWapchr` |
| Permission Management | `find / -perm 4000` | `find / -perm -4000` |

----------

# ⏩ Shortcuts

### Auto-Complete

-   `[TAB]` – Auto-complete
    

### Cursor Movement

-   `[CTRL] + A` – Move the cursor to the beginning of the current line.
    
-   `[CTRL] + E` – Move the cursor to the end of the current line.
    
-   `[CTRL] + [←] / [→]` – Jump at the beginning/previous word.
    
-   `[ALT] + B / F` – Jump backward/forward one word.
    

### Erase The Current Line

-   `[CTRL] + U` – Erase to beginning of the line.
    
-   `[CTRL] + K` – Erase to end of the line.
    
-   `[CTRL] + W` – Erase the word before the cursor.
    

### Paste Erased Contents

-   `[CTRL] + Y` – Paste the erased text or word.
    

### Ends Task

-   `[CTRL] + C` – Ends the current task/process (sends SIGINT).
    

### End-of-File (EOF)

-   `[CTRL] + D` – Closes STDIN (End-of-File).
    

### Clear Terminal

-   `[CTRL] + L` – Clears terminal. (Same as `clear` command)
    

### Background a Process

-   `[CTRL] + Z` – Suspend current process (SIGTSTP).
    

### Search Through Command History

-   `[CTRL] + R` – Search previous commands.
    
-   `[↑] / [↓]` – Navigate command history.
    

### Switch Between Applications

-   `[ALT] + [TAB]` – Switch between apps.
    

### Zoom

-   `[CTRL] + [+]` – Zoom in.
    
-   `[CTRL] + [-]` – Zoom out.


[by abdessami](https://academy.hackthebox.com/achievement/1084819/18)
![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjExMjc1MDM3Ml19
-->