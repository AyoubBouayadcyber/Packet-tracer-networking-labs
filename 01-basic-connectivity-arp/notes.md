# Project 1 — Basic Connectivity & ARP Resolution

## What I Built
Two PCs connected through a single switch, both configured 
with static IPs on the same subnet.

## Configuration
- PC1: 192.15.20.1 / 255.255.255.0
- PC2: 192.15.20.2 / 255.255.255.0
- No default gateway needed — same subnet, Layer 2 communication only

## Test 1 — Basic Connectivity
Sent an ICMP ping from PC1 to PC2. Result: Successful, 0% packet loss.

## Test 2 — ARP Resolution
Cleared the ARP cache with `arp -d`, then pinged PC2 again. 
Checked the ARP table with `arp -a` afterward:

## What This Proves
Devices on the same subnet communicate directly through a 
switch without needing a router. Before two hosts can exchange 
data, the sender must resolve the destination's MAC address via 
ARP if it isn't already cached. The "dynamic" type confirms 
this entry was learned automatically — exactly the request/
reply/cache cycle used in real networks.
