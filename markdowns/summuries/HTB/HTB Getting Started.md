# Getting a shell

**Shell Type** | **Description**
|--|--|
`Reverse shell` | Initiates a connection back to a "listener" on our attack box.
`Bind shell` | "Binds" to a specific port on the target host and waits for a connection from our attack box.
`Web shell` | Runs operating system commands via the web browser, typically not interactive or semi-interactive. It can also be used to run single commands (i.e., leveraging a file upload vulnerability and uploading a `PHP` script to run a single command.

### Netcat
- interacting with TCP/UDP ports.
- **Banner Grabbing** 
## Service Scanning
### Nmap
- `-sC` for more details `-sV` for version `-p-` to scan all ports 
- to run custom script `nmap --script <script name> -p<port> <host>`. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTM5MDQyMzIsNjgyNzAyNjU4LC01Nz
gzMjExOTAsODExMDY5MDE1LC0yMDg4NzQ2NjEyXX0=
-->