# ALTEN Global Network Simulation 🏆

A complete multi-site enterprise network simulation modeling 
ALTEN's real infrastructure — inspired directly by my internship 
observation at ALTEN Fès, where I saw how IT incidents from 
ALTEN France (Toulouse, Boulogne) were received and processed by 
the local support team.

## Why I Built This
During my internship in IT Support at ALTEN Fès, I observed real 
IT tickets flowing constantly between our team and ALTEN's 
French sites. This project reconstructs that environment from 
the ground up — three departments at Fès, connected via WAN to 
two French sites, with a centralized helpdesk portal and proper 
network segmentation.

## Architecture Overview
                ALTEN Fès (Central Site)
    ┌──────────────┬──────────────┬──────────────┐

VLAN 10 VLAN 20 VLAN 30 VLAN 99
IT Support Réseau Local Développement Server Room
192.168.10.0/24 192.168.20.0/24 192.168.30.0/24 192.168.99.0/24
└──────────────┴──────────────┴──────────────┘
Router0 (router-on-a-stick)
|
WAN Switch
(172.16.0.0/24)
┌─────────────┴─────────────┐
Toulouse Router Boulogne Router
10.10.10.0/24 10.20.20.0/24


## What's Implemented

### 1. VLAN Segmentation (Fès Site)
Four isolated departments — IT Support, Réseau Local, 
Développement, and Server Room — each on its own subnet, 
connected through router-on-a-stick with 802.1Q trunking.

### 2. Multi-Site WAN Routing
Fès, Toulouse, and Boulogne connected in a hub-and-spoke 
topology through point-to-point-style links over a shared WAN 
switch, with static routing giving every site full reachability 
to every other site.

### 3. Centralized Helpdesk Portal
An HTTP server hosted at Fès, styled after ServiceNow (the ITSM 
tool used in production at ALTEN), resolved through a custom DNS 
record (helpdesk.alten-fes.com). A PC in Toulouse or Boulogne 
can resolve this domain and load a fully functional incident 
submission interface across the WAN.

### 4. Security ACL — Least Privilege
An extended access control list restricts the Développement 
VLAN to HTTP-only access toward the Server Room — developers can 
reach the helpdesk but cannot directly access server 
infrastructure by any other protocol.

## Key Troubleshooting Moments

- Diagnosed a completely unconfigured WAN interface by comparing 
  routing tables across all three routers side by side
- Traced an "invalid interface" error caused by a misidentified 
  port name back to the correct interface
- Discovered that a wireless-style NAT device only required a 
  static route on ONE side of a connection, not both

## Files In This Folder
- `configs/` — full router and switch configurations for all 
  three sites
- `html/index.html` — the helpdesk portal source code
- `notes.md` — detailed build log, addressing plan, and testing 
  results

---
*Part of the [packet-tracer-networking-labs](../) repository — 
ENSA Fès GSCSI, building toward AI Security.*
