

# Introduction
## overview
- `topologies` (mesh/tree/star), 
- `mediums` (ethernet/fiber/coax/wireless), 
- `protocols` (TCP/UDP/IPX)
##  Basic Information
- **website address:**
  -   **FQDN** ( Uniform Resource Locator ) : `www.hackthebox.eu`
  -   **URL** ( Fully Qualified Domain Name ):`https://www.hackthebox.eu/example? floor=2&office=dev&employee=17` 
-  **Router**
-  **ISP** ( Internet Service Provider ) : looks in `DNS`
- **DNS** ( Domain Name Service ) : return `IP adress`
# Networking Structure
## Network Types
### Common Terminology

| **Network Type** | **Definition** |
|--|--|
| **WAN** (Wide Area Network) | Internet |
| **LAN** (Local Area Network) | Internal Networks (Ex: Home or Office) |
| **WLAN** (Wireless Local Area Network) |Internal Networks accessible over Wi-Fi |
| **VPN** (Virtual Private Network) | Connects multiple network sites to one `LAN` |

 

#### WAN
- routing protocol such as BGP
- dont use RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

#### LAN/WLAN
- use RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).
- rarely routable ( like some hotels or college )

#### VPN  
there's 3 types 
##### Site-To-Site VPN 
- used to join company networks together over the Internet
##### Remote Access VPN
- like `HTB openVPN`, 
- `Split-Tunnel VPN`: the Internet connection is not going out of the VPN.
- 
##### SSL VPN
- stream applications or entire desktop sessions to your web browser 
- like `HTB Pwnbox`

### Book Terms
| **Network Type** | **Definition** |
|--|--|
| **GAN** (Global Area Network) | Global network (the Internet) |
| **MAN** (Metropolitan Area Network) | Regional network (multiple LANs) |
| **WPAN** (Wireless Personal Area Network) |Personal network (Bluetooth) |

 

## Networking Topologies 

- **nodes - Network Interface Controller (NICs):**  Repeaters - Hubs - Bridges - Switches - Router/Modem - Gateways - Firewalls
 - **Classifications:** Point_to_Point - Bus - Star - Ring - Mesh - Tree - Hybrid - Daisy Chain
 ## Proxies
A proxy is when a device or service sits in the middle of a connection and acts as a ``mediator``( inspect the contents of the traffic ), without mediator it's just a `gateway`.
proxies will almost always operate at Layer 7 of the OSI Model
-   `Dedicated Proxy` / `Forward Proxy`
-   `Reverse Proxy`
-   `Transparent Proxy`
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
# Networking Models

![Comparison of OSI and TCP/IP models with PDUs: OSI has 7 layers including Application, Presentation, Session, Transport, Network, Data-Link, and Physical. TCP/IP has 4 layers: Application, Transport, Internet, and Link. PDUs are Data, Segment/Datagram, Packet, Frame, and Bit.](https://academy.hackthebox.com/storage/modules/34/redesigned/net_models_pdu2.png)
![Diagram of packet transfer showing data encapsulation from sender to receiver through layers: Data, TCP, IP, MAC, and Binary Transmission, with corresponding headers and sequence.](https://academy.hackthebox.com/storage/modules/34/packet_transfer.png)
... to be continued 
### 📄 License
This cheat sheet is licensed under the [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You may use and modify this freely, but please give credit. 🙏
![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)

