# Project 2 — Router Connecting Two Networks

## What I Built
Two separate subnets connected through a single router, 
proving that devices on different networks require a router 
(Layer 3 device) to communicate — unlike Project 1, where a 
switch alone was sufficient.

## Topology

PC1 — Switch1 — Router — Switch2 — PC2


## Configuration

**Router:**

interface GigabitEthernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit


**PC1:**

IP: 192.168.1.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1


**PC2:**

IP: 192.168.2.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1


## Verification

Router#show ip route
C 192.168.1.0/24 is directly connected, GigabitEthernet0/0
C 192.168.2.0/24 is directly connected, GigabitEthernet0/1


## Test

C:>ping 192.168.2.10

Pinging 192.168.2.10 with 32 bytes of data:
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127
Reply from 192.168.2.10: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.2.10:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)


## What This Proves
Devices on different subnets cannot communicate directly, even 
if physically connected — they require a Layer 3 device (router) 
with an interface on each subnet acting as their gateway. The 
TTL value of 127 (one less than the default 128) confirms the 
packet crossed exactly one router hop, matching the topology.

## Real World Application
This is the fundamental building block of every corporate 
network — separating departments, buildings, or security zones 
into different subnets, with routers controlling and directing 
traffic between them. Every larger project in this repository 
(subnetting, VLANs, the ALTEN simulation) builds directly on 
this concept.
