# Project 9 — IoT Registration Server

## What I Built
A centralized IoT registration and control system, where smart 
devices authenticate against a server before being remotely 
controlled through a web-based dashboard.

## Topology

Server (HTTP + IoT services) — Switch — IoT Devices, Control PC


## Configuration

**Server — HTTP Service (required dependency):**

Service: On

Note: The IoT service runs on top of HTTP/HTTPS — it must be 
enabled first, otherwise the IoT registration fields remain 
empty and non-functional.

**Server — IoT (Registration Server) Service:**

Service: On
Username: ayoub
Password: [set password]


**IoT Devices:**

IP Configuration: DHCP
IoT Server: [Server's IP address]
Username / Password: [matching credentials above]


## Test

From a PC's Web Browser, accessed the IoT server's registration 
dashboard using the server's IP address, logged in with the 
configured credentials, and successfully toggled a registered 
IoT device's state (on/off) — with the change reflected visually 
in the device's icon on the topology.

## What This Proves
IoT devices require a trust relationship with a central 
controller before they can be managed remotely — authentication 
isn't optional. The dependency between IoT and HTTP services 
also demonstrates how modern IoT dashboards are typically 
web-based interfaces layered on top of standard web protocols, 
rather than a separate proprietary system.

## Real World Application
IoT security is one of the fastest-growing concerns in 
cybersecurity — billions of connected devices, many with weak 
default credentials, represent a massive attack surface. 
Understanding registration, authentication, and control flow for 
IoT devices is foundational for anyone working toward IoT 
security specialization, a niche but rapidly expanding field 
within the broader AI/cybersecurity market.
