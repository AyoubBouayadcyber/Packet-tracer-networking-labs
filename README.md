# Cisco Packet Tracer — Networking Labs

A progression of hands-on networking projects built in Cisco 
Packet Tracer, from fundamental connectivity concepts through 
a complete multi-site enterprise network simulation.

## About Me
Engineering student at ENSA Fès — GSCSI filière 
(Génie des Systèmes Communicants et Sécurité Informatique)
Building toward AI Security specialization.

## About This Repository
Each folder represents a standalone project, building in 
complexity from basic switching and IP addressing to advanced 
topics like VLAN trunking, multi-site WAN routing, DNS/HTTP 
services, and security ACLs. Every project includes topology 
screenshots, configuration proof, and a written explanation of 
what was built and why.

## Projects

### [01 — Basic Connectivity & ARP](01-basic-connectivity-arp/)
Two hosts, one switch. Proving Layer 2/3 fundamentals and the 
ARP resolution process.

### [02 — Router Connecting Two Networks](02-router-two-networks/)
Introducing routing between two separate subnets through a 
single router.

### [03 — Subnetting Challenge](03-subnetting-challenge/)
Splitting a /24 network into four /26 subnets across a single 
router with four physical interfaces, including real 
troubleshooting using `show cdp neighbors`.

### [04 — DHCP Server](04-dhcp-server/)
Automatic IP address assignment — gateway, subnet mask, and DNS 
delivered dynamically to client devices.

### [05 — DNS & HTTP Server](05-dns-http-server/)
Domain name resolution paired with web page hosting — a client 
resolving a custom domain name and loading a hosted page.

### [06 — FTP Server](06-ftp-server/)
File transfer between hosts using FTP authentication.

### [07 — Email Server](07-email-server/)
SMTP/POP3 mail exchange between two client accounts.

### [08 — Wireless Router Interconnection](08-wireless-router-interconnection/)
Connecting a consumer-grade wireless router (with NAT) to an 
enterprise router, debugging a real multi-layer routing issue 
across mismatched addressing schemes.

### [09 — IoT Registration Server](09-iot-registration-server/)
Registering and controlling IoT devices through a centralized 
server-based dashboard.

### [10 — VLANs](10-vlans/)
Virtual LAN segmentation, trunk vs access ports, and inter-VLAN 
routing using router-on-a-stick — the foundation later applied 
at full scale in the ALTEN simulation.

### [11 — ALTEN Global Network Simulation](11-alten-network-simulation/) 🏆
**Flagship project.** A complete multi-site enterprise network 
modeling ALTEN's real infrastructure — inspired by my internship 
observing IT incidents flow between ALTEN Fès and ALTEN France 
(Toulouse, Boulogne). Includes VLAN segmentation, router-on-a-stick 
trunking, WAN routing across three sites, a DNS-resolved 
ServiceNow-style helpdesk portal, and security ACLs enforcing 
least-privilege access.

## Why This Repository Exists
Networking theory means little until it's built, broken, and 
debugged by hand. Each project here represents real hands-on 
practice — including the mistakes and troubleshooting process, 
not just the final working result.

---
*Part of my ongoing learning journey at ENSA Fès GSCSI, 
building toward a career in AI Security.*
