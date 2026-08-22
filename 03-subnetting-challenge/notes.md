# Project 3 — Subnetting Challenge

## What I Built
Split a single /24 network into four /26 subnets, each served 
by its own physical router interface — using a router with 
Gigabit modules added specifically to provide the 4 required 
physical ports.

## Subnet Plan

192.168.10.0/24 split into 4 x /26 subnets:

Subnet 1: 192.168.10.0/26 Gateway: 192.168.10.1
Subnet 2: 192.168.10.64/26 Gateway: 192.168.10.65
Subnet 3: 192.168.10.128/26 Gateway: 192.168.10.129
Subnet 4: 192.168.10.192/26 Gateway: 192.168.10.193


## Configuration

interface GigabitEthernet0/0/0
ip address 192.168.10.1 255.255.255.192
no shutdown
exit

interface GigabitEthernet0/0/1
ip address 192.168.10.65 255.255.255.192
no shutdown
exit

interface GigabitEthernet0/0/2
ip address 192.168.10.129 255.255.255.192
no shutdown
exit

interface GigabitEthernet0/0/3
ip address 192.168.10.193 255.255.255.192
no shutdown
exit


## Verification

Router#show ip interface brief
Interface IP-Address Status Protocol
GigabitEthernet0/0/0 192.168.10.1 up up
GigabitEthernet0/0/1 192.168.10.65 up up
GigabitEthernet0/0/2 192.168.10.129 up up
GigabitEthernet0/0/3 192.168.10.193 up up


## Troubleshooting Encountered
The router's default interfaces weren't enough to support 4 
separate subnets — physical Gigabit modules (NIM) had to be 
added to the router's expansion slots. One switch-to-router 
connection appeared broken (shown as a red cable in the 
topology), but running `show cdp neighbors` on the router 
revealed the connection was actually already established — the 
error had come from attempting a duplicate connection on an 
already-occupied port, not a missing link.

Router#show cdp neighbors
Device ID Local Intrfce Capability Port ID
Switch Gig 0/0/0 S Fas 3/1
Switch Gig 0/0/1 S Fas 0/1
Switch Gig 0/0/2 S Fas 4/1
Switch Gig 0/0/3 S Fas 0/1


## Test
Pinged across all 4 subnets from PCs connected to different 
switches — all successful, confirming the router correctly 
routes between every subnet simultaneously.

## What This Proves
A single router can serve multiple subnets at once, each 
requiring its own IP range, gateway, and dedicated physical or 
logical interface. Troubleshooting connectivity issues requires 
more than checking cables visually — `show cdp neighbors` 
reveals what's actually connected at the protocol level, which 
resolved a problem that looked like a hardware fault but was 
actually a configuration misunderstanding.

## Real World Application
Subnetting is how organizations divide address space efficiently 
instead of wasting an entire /24 on a small department. This 
same math and interface logic scales directly into the VLAN and 
multi-site routing work in later projects in this repository.
