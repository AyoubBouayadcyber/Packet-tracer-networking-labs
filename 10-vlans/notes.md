# Project 10 — VLANs

## What I Built
A switch segmented into four Virtual LANs (VLANs), each 
representing a different department, with a single router 
interface handling inter-VLAN routing using the router-on-a-stick 
technique — one physical link carrying tagged traffic for all 
four VLANs simultaneously.

## VLAN Plan

VLAN 10 — IT-Support → 192.168.10.0/24
VLAN 20 — Reseau-Local → 192.168.20.0/24
VLAN 30 — Developpement → 192.168.30.0/24
VLAN 99 — Server-Room → 192.168.99.0/24


## Configuration

**Switch — Creating VLANs:**

vlan 10
name IT-Support
exit
vlan 20
name Reseau-Local
exit
vlan 30
name Developpement
exit
vlan 99
name Server-Room
exit


**Switch — Trunk Port (toward router):**

interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,99
exit


**Switch — Access Ports (toward PCs):**

interface range FastEthernet0/1-6
switchport mode access
switchport access vlan 10
exit

interface range FastEthernet0/7-12
switchport mode access
switchport access vlan 20
exit

interface range FastEthernet0/13-18
switchport mode access
switchport access vlan 30
exit

interface FastEthernet0/24
switchport mode access
switchport access vlan 99
exit


**Router — Sub-interfaces (router-on-a-stick):**

interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface GigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface GigabitEthernet0/0.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
exit

interface GigabitEthernet0/0
no shutdown
exit


## Verification

Switch#show interfaces trunk
Port Mode Encapsulation Status Native vlan
Gig0/1 on 802.1q trunking 1

Port Vlans allowed on trunk
Gig0/1 10,20,30,99


## Test

C:>ping 192.168.20.10

Pinging 192.168.20.10 with 32 bytes of data:
Request timed out.
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127
Reply from 192.168.20.10: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.20.10:
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)


The first packet timing out is expected ARP resolution delay — 
subsequent packets succeed once the MAC address is cached. 
TTL=127 confirms the traffic correctly crossed the router.

## What This Proves
VLANs isolate broadcast domains at Layer 2 even when devices 
share the same physical switch — a device in VLAN 10 cannot 
reach VLAN 20 without passing through a Layer 3 device. 
Router-on-a-stick allows a single physical router interface to 
serve as the gateway for multiple VLANs by tagging traffic with 
802.1Q headers, avoiding the need for one dedicated physical 
interface per VLAN.

## Real World Application
VLAN segmentation is standard practice in virtually every 
corporate network — separating departments, security zones, or 
device types (like IoT) onto isolated logical networks for both 
organizational clarity and security. This exact configuration 
was scaled up and applied directly in the ALTEN Global Network 
Simulation, where each department's VLAN structure mirrors what 
was learned and tested here.
