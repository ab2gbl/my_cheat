

# 📡 Introduction

## 🌐 Overview

-   Topologies: `mesh`, `tree`, `star`
    
-   Mediums: `ethernet`, `fiber`, `coax`, `wireless`
    
-   Protocols: `TCP`, `UDP`, `IPX`


## 📌 Basic Information

-   **Website Address:**
    
    -   **FQDN** (Fully Qualified Domain Name): `www.hackthebox.eu`
        
    -   **URL** (Uniform Resource Locator): `https://www.hackthebox.eu/example?floor=2&office=dev&employee=17`
        
-   **Router**
    
-   **ISP (Internet Service Provider):** looks in `DNS`
    
-   **DNS (Domain Name Service):** returns IP address

# 🕸️ Networking Structure

## 🖧 Network Types
### 📘 Common Terminology

| **Network Type** | **Definition** |
|--|--|
| **WAN** | Wide Area Network (Internet) 🌍 |
| **LAN** |Local Area Network (Home/Office) 🏠 |
| **WLAN** | Wireless LAN (Wi-Fi access) 📶 |
| **VPN** | Virtual Private Network 🔒 |


#### WAN
- routing protocol such as BGP
- dont use RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

#### LAN/WLAN
- use RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).
- rarely routable ( like some hotels or college )

#### VPN Types
##### Site-To-Site VPN 
- used to join company networks together over the Internet
##### Remote Access VPN
- like `HTB openVPN`, 
- `Split-Tunnel VPN`: the Internet connection is not going out of the VPN.
##### SSL VPN
- stream applications or entire desktop sessions to your web browser 
- like `HTB Pwnbox`


### 📚 Book Terms
| **Network Type** | **Definition** |
|--|--|
| **GAN** | Global Area Network (the Internet) 🌐 |
| **MAN** | Metropolitan Area Network (multiple LANs) 🏙️ |
| **WPAN** | Wireless Personal Area Network (Bluetooth)📱 |


 

## 🌐 Networking Topologies

-   **Nodes (NICs):** Repeaters, Hubs, Bridges, Switches, Routers, Gateways, Firewalls
    
-   **Layouts:** Point-to-Point, Bus, Star, Ring, Mesh, Tree, Hybrid, Daisy Chain

## 🛡️ Proxies

A **proxy** mediates traffic, often inspecting contents (Layer 7).
A proxy is when a device or service sits in the middle of a connection and acts as a **mediator** ( inspect the contents of the traffic ), without mediator it's just a **gateway**.
### 🎯 Types

-   **Dedicated / Forward Proxy**: Internal → Proxy → Internet
    
-   **Reverse Proxy**: Internet → Proxy → Internal Server
    
-   **Transparent Proxy**: Invisible to the client
    
-   **Non-Transparent Proxy**: Requires manual setup

### Dedicated Proxy / Forward Proxy
-   **Direction:** Client → Proxy → Internet
-   **Purpose:** Filters and controls **outgoing** traffic from internal clients.
-   **Use Cases:**
    -   Corporate networks (security & access control).
    -   Penetration testing tools like **Burp Suite**.
-   **Security Note:** Helps block malware C2, especially when DNS or proxy awareness is involved.
-   **Browser Behavior:**
    -   Chrome/Edge use system proxy settings.
    -   Firefox uses its own settings, making it harder for malware to use proxies.
### Reverse Proxy    
-   **Direction:** Internet → Proxy → Server
-   **Purpose:** Filters and protects **incoming** traffic to internal servers.
-   **Use Cases:**
    -   **Cloudflare** for DDoS mitigation and WAF.        
    -   **ModSecurity** for detecting and blocking web-based attacks.
    -   **Red teamers/malware** may use reverse proxies to tunnel through internal networks and bypass IDS.
-   **Security Note:** Hides server details, blocks malicious traffic, enables logging, and evades monitoring when misused.
### (Non-) Transparent Proxy
-   **Transparent Proxy:**
    -   The client doesn’t know it’s using a proxy.
    -   Network intercepts requests automatically.
    -   Often used in public Wi-Fi or by ISPs for content filtering.
-   **Non-Transparent Proxy:**
    -   The client is explicitly configured (e.g., browser or OS settings).
    -   If not configured, **internet access fails** — proxy is the only route out.
# 🧱 Networking Models

![Comparison of OSI and TCP/IP models with PDUs: OSI has 7 layers including Application, Presentation, Session, Transport, Network, Data-Link, and Physical. TCP/IP has 4 layers: Application, Transport, Internet, and Link. PDUs are Data, Segment/Datagram, Packet, Frame, and Bit.](https://academy.hackthebox.com/storage/modules/34/redesigned/net_models_pdu2.png)
![Diagram of packet transfer showing data encapsulation from sender to receiver through layers: Data, TCP, IP, MAC, and Binary Transmission, with corresponding headers and sequence.](https://academy.hackthebox.com/storage/modules/34/packet_transfer.png)
-   **IP** lives on OSI Layer 3 (Network)
    
-   **TCP** lives on OSI Layer 4 (Transport)



# 🧭 Addressing

## 🌐 Network Layer
Functions:

-   Logical Addressing
    
-   Routing
    

Common Protocols: `IPv4`, `IPv6`, `ICMP`, `IPSec`, `RIP`, `OSPF`, `IGMP`

## 🔢 IP Addresses

-   **IPv4** / **IPv6** - describes the unique postal address and district of the receiver's building.

-   **MAC** - describes the exact floor and apartment of the receiver.

**host part** and a **network part**. The `router` assigns the **host part** of the IP address at home or by an administrator. The respective `network administrator` assigns the **network part**. On the Internet, this is **IANA**
### 🧩 IP Classes
---
| **Network Address** | **First Address** | **Last Address** | **Subnetmask**  | **CIDR**  |  **Subnets** | **IPs** |
|--|--|--|--|--|--|--|--|
| `A` | 1.0.0.0 | 1.0.0.1 | 127.255.255.255 | 255.0.0.0 | /8 | 127 | 16,777,214 + 2 |
| `B` | 128.0.0.0 | 128.0.0.1 | 191.255.255.255 | 255.255.0.0 | /16 | 16,384 | 65,534 + 2|
|`C` | 192.0.0.0 | 192.0.0.1 | 223.255.255.255 | 255.255.255.0 | /24 | 2,097,152 | 254 + 2 | 
| `D` | 224.0.0.0 | 224.0.0.1 | 239.255.255.255 | Multicast | Multicast | Multicast | Multicast
| `E` | 240.0.0.0 | 240.0.0.1 | 255.255.255.255 | reserved | reserved | reserved | reserved |
---
 - **+2** : `network address` and the `broadcast address`
- **default gateway**: IPv4 address of the `router` , common `first` or `last` IPv4 in a subnet.
- **broadcast IP address**: allows one device to talk to `all others` in its network at once , `without response`, `last IPv4`
- **CIDR**: `192.168.10.39`**`/24`**


## 🔧 Subnetting
find out the following outline of the respective network: `Network address` , `Broadcast address` , `First host`,`Last host`,`Number of hosts`

example 
-   IPv4 Address: `192.168.12.160` 
-   Subnet Mask: `255.255.255.192`
-   CIDR: `192.168.12.160/26`


| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** |**Decimal** |
|--|--|--|--|--|--|
| **IPv4** | **1100 0000** | **1010 1000** | **0000 1100** | **10** `|` 10 0000|192.168.12.160/26 |
| **Subnet mask** | **1111 1111** | **1111 1111** | **1111 1111** | **11**`|`00 0000 | 255.255.255.192 |
| **Bits** | /8 | /16 | /24 |  /32 | |

- **`|`** : Separation Of Network & Host Parts
- **Network Address** : all bits to `0` in the `host part` ( `192.168.12.128/26` here )
- **Broadcast Address** : all bits to `1` in the `host part`  ( `192.168.12.191/26` here )
- Now we know that this subnet offers us a total of **64 - 2** (`network address` & `broadcast address`)

### Subnetting Into Smaller Networks
#### Ex: dividing the subnet `4` additional subnets
- we can divide the `64 hosts` we know by `4`. 
- The `4` = `2`**`^2`**
- Now we increase/expand our subnet mask by `2 bits` from `/26` to `/28`, and it looks like this:

| **Details of** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** |**Decimal** |
|--|--|--|--|--|--|
| **IPv4** | **1100 0000** | **1010 1000** | **0000 1100** | **1000** `|` 0000|192.168.12.128`/28` |
| **Subnet mask** | **1111 1111** | **1111 1111** | **1111 1111** | **1111**`|` 0000 | 255.255.255.`240` |
| **Bits** | /8 | /16 | /24 |  /32 | |

- and we have: `64 / 4` = **`16`**

| **Subnet No.** | **Network Address** | **First Host** | **Last Host** | **Broadcast Address** | **CIDR** |
|--|--|--|--|--|--|
| 1 | `192.168.12.128` | 192.168.12.129 | 192.168.12.142 | `192.168.12.143` | 192.168.12.128/28 |
| 2 | `192.168.12.144` | 192.168.12.145 | 192.168.12.158 | `192.168.12.159` | 192.168.12.144/28 |
| 3 | `192.168.12.160` | 192.168.12.161 | 192.168.12.174 | `192.168.12.175` | 192.168.12.160/28 |
| 4 | `192.168.12.176` | 192.168.12.177 | 192.168.12.190 | `192.168.12.191` |192.168.12.176/28 |

---
## 🧬 MAC Addresses
**Media Access Control (MAC)** : `48`-bit (`6 octets`) , `physical address`


**Representation** | **1st Octet** | **2nd Octet** | **3rd Octet** | **4th Octet** | **5th Octet** | **6th Octet** |
|--|--|--|--|--|--|--|
| Binary | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111 |
| Hex | DE | AD | BE | EF | 13  | 37 |


- The **first half** (`3 bytes` / `24 bit`) is the so-called **Organization Unique Identifier (`OUI`)**  defined by the **Institute of Electrical and Electronics Engineers (`IEEE`)** for the respective manufacturers. 
- The **last half** is called the **Individual Address Part** or **Network Interface Controller (`NIC`)**
- if a host with the IP target address is located in:
  - **the same subnet**: directly to the target computer's physical address.
  - **different subnet**: to the `MAC address` of the responsible router (`default gateway`)
  - **its own layer 2 address**: 	to the higher layers.
- **Address Resolution Protocol (`ARP`)**: is used in IPv4 to determine the MAC addresses associated with the IP addresses. 

||||||
|--|--|--|--|--|--|
**Local Range** | 0**2**:00:00:00:00:00 | 0**6**:00:00:00:00:00 | 0**A**:00:00:00:00:00 | 0**E**:00:00:00:00:00

- **The last bit of first octet** identifies the MAC address as `Unicast` (`0`) or `Multicast` (`1`). ( the packet sent will reach only `one` or `many` hosts )
-  **The second last bit in the first octet** identifies whether it is a `global OUI`(`0`), defined by the IEEE, or a `locally administrated`(`1`) MAC address.
- **MAC Broadcast**: FF:FF:FF:FF:FF:FF
- MAC addresses can be `changed`/`manipulated` or `spoofed`,
- **several attack**:
  - `MAC spoofing`  - `MAC flooding` - `MAC address filtering`
### 🔍 ARP (Address Resolution Protocol)
- **ARP** is a network protocol used to **translate an IP address (Layer 3)** into a **MAC address (Layer 2)** on a Local Area Network (**LAN**).
### 🛠️ **How ARP Works**

#### 1. **ARP Request (Broadcast)**

-   A device wants to talk to another device, but it only knows its **IP address**.
    
-   It sends a broadcast message:
    
    > “Who has IP `10.129.12.101`? Tell me, `10.129.12.100`.”
    
-   This goes to **every device** on the LAN.
    

#### 2. **ARP Reply (Unicast)**

-   The device that owns the IP responds:
    
    > “`10.129.12.101` is at MAC `AA:AA:AA:AA:AA:AA`.”
    
-   Now the sender knows the MAC address and can send Ethernet frames directly.
#### **Tshark Capture Example:**
```ruby
1  10.129.12.100 → 10.129.12.255 ARP  Who has 10.129.12.101? Tell 10.129.12.100
2  10.129.12.101 → 10.129.12.100 ARP  10.129.12.101 is at AA:AA:AA:AA:AA:AA
```
### ⚠️ **Security Risk: ARP Spoofing / ARP Poisoning**
- MAC addresses can be `changed`/`manipulated` or `spoofed`,
- **several attack**:
  - `MAC spoofing`  - `MAC flooding` - `MAC address filtering`
-   Attackers can send **fake ARP replies** to poison a device’s ARP cache.
    
-   This makes the victim associate the attacker’s MAC with a **legit IP**.
    
-   Now, all traffic meant for the real device gets redirected to the attacker.
    

#### 🕵️‍♂️ Example of ARP Spoofing:
```ruby
1  Attacker → Victim: 10.129.12.101 is at AA:AA:AA:AA:AA:AA (fake)
2  Victim → Broadcast: Who has 10.129.12.101? Tell me.
3  Legit → Victim: 10.129.12.101 is at BB:BB:BB:BB:BB:BB
4  Attacker → Victim: 10.129.12.101 is at AA:AA:AA:AA:AA:AA (again)
```

## 🌐 IPv6
-   Full IPv6: `fe80:0000:0000:0000:dd80:b1a9:6687:2d3b/64`
-   Short IPv6: `fe80::dd80:b1a9:6687:2d3b/64`



# 📖 Protocols & Terminology


## 🧠 Key Concepts

you can find it on [hack the box course](https://academy.hackthebox.com/module/34/section/1871)
**Protocol** |**Acronym** | **Description**|
|--|--|--|
Secure Shell | `SSH` | A secure network protocol used to log into and execute commands on a remote system |
File Transfer Protocol | `FTP` | A network protocol used to transfer files from one system to another |
Hypertext Transfer Protocol |`HTTP` |A client-server protocol used to send and receive data over the internet|
... more on [hack the box course](https://academy.hackthebox.com/module/34/section/1871)

---
## 🔌 Common Protocol Ports

Internet protocols are standardized rules and guidelines defined in RFCs that specify how devices on a network should communicate. They ensure consistent, reliable communication regardless of the hardware or software used.

Devices communicate through communication channels (wired/wireless) using protocols that define the **format and structure** of data being transmitted. The two most important transport protocols are:
### 📦 Transmission Control Protocol (TCP)

-   **Type:** Connection-oriented
    
-   **Key Feature:** Performs a **Three-Way Handshake** to establish a connection
    
-   **Reliable:** Ensures packets arrive and in the correct order
    
-   **Slower** due to overhead
    

🧪 **Example**: When visiting a website, your browser sends an HTTP request over TCP to the web server, which replies with the website data. TCP ensures the connection stays alive during the transfer.


---
**Protocol** | **Acronym** | **Port** | **Description** |
|--|--|--|--|
Telnet|`Telnet`|`23`|Remote login service|
Secure Shell|`SSH`|`22`|Secure remote login service|
Hyper Text Transfer Protocol|`HTTP`|`80`|Used to transfer webpages|
Hyper Text Transfer Protocol Secure|`HTTPS`|`443`|Used to transfer secure webpages|
Domain Name System|`DNS`|`53`|Lookup domain names|
File Transfer Protocol|`FTP`|`20-21`|Used to transfer files|

... more on [hack the box course](https://academy.hackthebox.com/module/34/section/1872)

---


### 🛰️ User Datagram Protocol (UDP)

-   **Type:** Connectionless
    
-   **Key Feature:** Sends packets without establishing a connection
    
-   **Faster**, but **less reliable** (no delivery guarantee)
    

📺 **Example**: Streaming video on YouTube often uses UDP. If some packets are dropped, the video continues playing with minor quality issues rather than stopping.


**Protocol** | **Acronym** | **Port** | **Description** |
|--|--|--|--|
Domain Name System | `DNS` | `53` |It is a protocol to resolve domain names to IP addresses.
Routing Information Protocol | `RIP` | `520` | It is used to exchange routing information between routers.
Dynamic Host Configuration Protocol |`DHCP` | `67` | It is used to assign IP addresses to devices in a network dynamically.
Telnet | `TELNET` | `23` | It is a text-based remote access communication protocol.
MySQL | `MySQL` | `3306` |It is an open-source database management system.
... more on [hack the box course](https://academy.hackthebox.com/module/34/section/1872)
### 🛰️ **ICMP (Internet Control Message Protocol) Overview**

ICMP is a **network-layer protocol** used by devices (like routers and computers) to send **error messages** and **operational information**—mostly to diagnose issues and maintain connectivity ( ping is ICMP ).

----------

#### 📩 **ICMP Messages:**

There are two main types:

1.  **Requests** – Sent to ask for information.
    
2.  **Replies** – Sent in response to requests.
   

#### 🔁 **Common ICMP Request/Reply Types:**

Type | Description
|--|--|
`Echo Request / Reply` |Used for **pinging**. Tests if a device is reachable.
`Timestamp Request` | Asks for the time on a remote device.
`Address Mask Request` | Asks for a device’s subnet mask.
`Destination Unreachable` | Sent when a packet can't reach its destination.
`Redirect` | Tells a device to send traffic through a **different router**. 
`Time Exceeded` | Sent when a packet’s **TTL reaches zero** (to prevent infinite loops).
`Parameter Problem` | Indicates a **problem in the packet header**.
`Source Quench` _(deprecated)_ | Told the sender to **slow down** packet sending (rate-limiting).

#### 🕐 **TTL (Time To Live):**

-   Every packet has a TTL value that **decreases by 1** each time it goes through a router.
    
-   When TTL reaches **0**, the router drops the packet and sends back a **Time Exceeded** message.
    
-   This prevents packets from looping endlessly on the internet.
    

#### 🎯 Uses of TTL:

-   **Trace network path** (traceroute uses TTL to find each hop).
    
-   **Estimate device distance** by hop count.
    
-   **Guess OS type** from default TTL values:
    
    -  Windows :     128
    -  Linux/macOS : 64
    -  Solaris :     255

### 📞 **VoIP (Voice over Internet Protocol)**

**VoIP** enables voice and multimedia communication over the internet instead of traditional phone lines. It's used in apps like **Skype, WhatsApp, Zoom, Slack**, etc.

----------

#### ⚙️ **Key Protocols and Ports**

-   **SIP (Session Initiation Protocol)** – Most commonly used protocol.
    
    -   **Ports**: TCP/5060 (unencrypted), TCP/5061 (encrypted)
        
-   **H.323 Protocol** – Older and less common.
    
    -   **Port**: TCP/1720

#### 🔁 **Common SIP Methods**

Method | Purpose
|--|--|
`INVITE` | Initiates a call/session
`ACK` | Acknowledges the INVITE 
`BYE` | Terminates a call/session
`CANCEL` | Cancels a pending INVITE
`REGISTER` | Registers the device or user agent with the SIP server
`OPTIONS` | Asks about the capabilities of the SIP server or device

#### 🔍 **Information Disclosure & Enumeration**

-   **SIP OPTIONS requests** can be used to **probe devices** or servers:
    
    -   Discover available features, codecs, media types
        
    -   Check server availability
        
    -   Enumerate users for potential brute-force or targeted attacks
        
#### 🛠️ **VoIP Config Files**

-   **Cisco SEPxxxx.cnf Files**:
    
    -   Used by Cisco CallManager (now Unified Communications Manager)
        
    -   Contains configuration for **Cisco IP phones**
        
    -   Includes phone model, firmware version, network settings, etc.
        
    -   Can be valuable for attackers if discovered during reconnaissance

## Wireless Networks

- Wireless networks use radio frequency `RF`
- **Wireless Access Point(`WAP`)** : like router for ex, connects the wireless network to a wired network

### WEP Challenge-Response Handshake 
**Step** | **Who** | **Description**
|--|--|--|
1 | `Client`| Sends an association request packet to the WAP, requesting access.
2 | `WAP` | Responds with an association response packet to the client, which includes a challenge string.
3 | `Client` | Calculates a response to the challenge string and a shared secret key and sends it back to the WAP.
4 | `WAP` | Calculates the expected response to the challenge with the same shared secret key and sends an authentication response packet to the client.

- **Cyclic Redundancy Check (`CRC`)**: error-detection mechanism in `WEP` protocot
- **CRC** allow decrypt a single packet `without` knowing the `encryption key` (using the `plaintext`)
- **CRC** value is recalculated by **WAP** using `received data` and compared to the original value from the **Device** to see if lose of data happened
### Encryption Protocols
#### WEP
- `WEP` uses a `40-bit` or `104-bit` key to encrypt data
- `WEP` uses the `RC4 cipher` encryption algorithm, which makes it vulnerable to attacks.

**Protocol** | **IV** | **Secret Key**
--|--|--
`WEP-40`/`WEP-64` | 24-bit | 40-bit
`WEP-104` | 24-bit | 80-bit

#### WPA
- `WPA` using `AES` uses a `128-bit`
- `WPA` more secure auth methods (`PSK` or an 802.1X auth server)

### Authentication Protocols
- **LEAP** and **PEAP** are both based on the **Extensible Auth Protocol** (`EAP`), a framework for authentication used in various networking contexts.
-    **LEAP**  uses a  `shared key`  for authentication, which means that the  `same key`  is used for  `encryption and authentication`.
- `PEAP` uses a more secure auth method called (`TLS`) using a `digital certificate`
### Disassociation Attack
`all` wireless network attack that aims to disrupt the communication between a WAP and its clients by sending disassociation frames to one or more clients.
### Wireless Hardening
-   Disabling broadcasting
-   WiFi Protected Access
-   MAC filtering
-   Deploying EAP-TLS

## VPN
- **Virtual Private Network** (`VPN`)
	-  uses the ports `TCP/1723` for **Point-to-Point Tunneling Protocol**  `PPTP`
	- and `UDP/500` for **IKEv1** and **IKEv2** VPN connections.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwMDk1OTUxMSwtMTE4MzU2NTUwMSwtMT
gyMDE4OTYxMSwtMTgzNjQxMTc3Nyw2NDIzNjA2NjEsMTEzNzYy
OTI0Myw3MjUzNjY1ODIsMTUxMzIxODM1NF19
-->