# Project 5 — DHCP, DNS & HTTP Server

## What I Built
A single server providing three integrated services — automatic 
IP assignment (DHCP), domain name resolution (DNS), and web page 
hosting (HTTP) — demonstrating how a client can go from having 
no network configuration to loading a website by name in one 
connected workflow.

## Topology

Server — Switch — PC


## Configuration

**Server static IP:**

IP Address: 192.168.10.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1


**DHCP Service:**

Service: On
Pool Name: LAN1
Default Gateway: 192.168.10.1
DNS Server: 192.168.10.100
Start IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Maximum number of users: 50


**DNS Service:**

Service: On
Record Type: A Record
Name: www.ayoub.com
Address: 192.168.10.100


**HTTP Service:**

Service: On
Default index.html page customized


**Client PC:**

IP Configuration: DHCP


## Verification

Client PC received full configuration automatically via DHCP:

IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
DNS Server: 192.168.10.100
Status: DHCP request successful


## Test

From the PC's Web Browser:

URL: www.ayoub.com
Result: Page loaded successfully


## What This Proves
This project chains three services together to mirror exactly 
how real web access works: DHCP configures the device with 
network settings and a DNS server address; DNS translates a 
human-readable domain name into an IP address; HTTP delivers 
the actual page content once that IP is known. A user never 
sees this chain — they just type a name and a page appears — but 
all three services must function correctly in sequence for that 
to happen.

## Real World Application
This exact chain (DHCP → DNS → HTTP) is what happens every time 
any device connects to any network and browses to any website. 
This project became the technical foundation for the helpdesk 
portal built later in the ALTEN Global Network Simulation, where 
a custom domain (helpdesk.alten-fes.com) is resolved and loaded 
across a multi-site WAN.
