# Project 4 — DHCP Server

## What I Built
A dedicated server automatically assigning IP configuration 
(IP address, subnet mask, default gateway, DNS server) to 
client PCs, instead of manually configuring each device.

## Topology

Server — Switch — PC(s)


## Configuration

**Server static IP:**

IP Address: 192.168.10.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1


**DHCP Service (on server):**

Service: On
Pool Name: LAN1
Default Gateway: 192.168.10.1
DNS Server: 192.168.10.100
Start IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Maximum number of users: 50


**Client PC:**

IP Configuration: DHCP (instead of Static)


## Verification

After switching the PC to DHCP mode, it automatically received:

IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
DNS Server: 192.168.10.100
Status: DHCP request successful


## What This Proves
DHCP eliminates the need to manually configure every device on 
a network. When a client boots up with DHCP enabled, it 
broadcasts a discovery request, the server offers an available 
IP from its pool, and the client accepts and configures itself 
automatically — matching the Discover/Offer/Request/Acknowledge 
process covered in networking fundamentals.

## Real World Application
Every corporate network relies on DHCP rather than manually 
assigning IPs to hundreds or thousands of devices. This is one 
of the first services any network administrator configures when 
setting up a new site — later reused directly in the ALTEN 
multi-site simulation for every department and remote location.
