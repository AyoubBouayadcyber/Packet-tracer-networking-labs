# Project 8 — Wireless Router Interconnection

## What I Built
A wireless network (laptop, smartphone, and PC connecting via a 
WRT300N wireless router) interconnected with a separate wired 
enterprise network through a main router, requiring a 
point-to-point WAN-style link between the two.

## Topology

Wireless devices — WRT300N — [point-to-point link] — Router1 — Switch — Server/PCs


## The Problem
Wireless clients successfully received DHCP addresses on the 
WRT300N's local network (192.168.0.0/24), but could not reach 
devices on the other side of Router1 (192.45.58.0/24).

## Root Causes Found
1. Both ends of the point-to-point link between the WRT300N and 
   Router1 had no IP address assigned at all.
2. Router1's LAN-facing interface (connecting to its own internal 
   switch/server) also had no IP configured, disconnecting it 
   from its own network.
3. Neither router had a route describing how to reach the 
   other's internal subnet.

## Configuration

**Router1 — point-to-point interface:**

interface GigabitEthernet0/0/1
ip address 10.0.0.1 255.255.255.252
no shutdown
exit


**Router1 — LAN-facing interface:**

interface GigabitEthernet0/0/0
ip address 192.45.58.1 255.255.255.0
no shutdown
exit


**WRT300N — Internet/WAN settings (via GUI):**

Internet Connection Type: Static IP
Internet IP Address: 10.0.0.2
Subnet Mask: 255.255.255.252
Default Gateway: 10.0.0.1


**Router1 — Static route to reach the wireless network:**

ip route 192.168.0.0 255.255.255.0 10.0.0.2


## Key Discovery — NAT Handled The Return Path Automatically

The WRT300N is a consumer-grade router that performs NAT (Network 
Address Translation) by default — the same behavior as a typical 
home internet router. Attempting to configure a manual static 
route on the WRT300N's side through its Advanced Routing page was 
unreliable in this simulation. Instead, NAT automatically 
translated outbound traffic from wireless clients through the 
WRT300N's WAN IP (10.0.0.2), meaning only Router1 needed an 
explicit static route to send return traffic back correctly.

## Test

C:>ping 192.45.58.1

Pinging 192.45.58.1 with 32 bytes of data:
Reply from 192.45.58.1: bytes=32 time<1ms TTL=127
Reply from 192.45.58.1: bytes=32 time<1ms TTL=127
Reply from 192.45.58.1: bytes=32 time<1ms TTL=127
Reply from 192.45.58.1: bytes=32 time<1ms TTL=127


## What This Proves
Connecting two different network types (consumer wireless router 
versus enterprise router) requires understanding how each device 
handles routing differently. NAT-enabled devices manage outbound 
translation automatically, but the router on the other side of 
that connection still needs an explicit route to know how to 
send replies back — the routing relationship isn't always 
symmetric.

## Real World Application
This exact scenario occurs constantly in small offices and 
branch locations — a consumer Wi-Fi router bridging into a 
proper business network through a dedicated router or firewall. 
Understanding NAT's default behavior versus explicit static 
routing is core knowledge for network security and 
infrastructure roles, and mirrors real troubleshooting work done 
by network engineers.
