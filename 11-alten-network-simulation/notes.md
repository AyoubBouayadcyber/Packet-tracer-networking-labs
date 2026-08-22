# Build Log — ALTEN Global Network Simulation

## Full Addressing Plan

### Fès Site (VLANs)

VLAN 10 — IT Support 192.168.10.0/24 Gateway: .1
VLAN 20 — Réseau Local 192.168.20.0/24 Gateway: .1
VLAN 30 — Développement 192.168.30.0/24 Gateway: .1
VLAN 99 — Server Room 192.168.99.0/24 Gateway: .1


### WAN Interconnect

WAN Subnet: 172.16.0.0/24 (shared via WAN Switch)
Fès Router0: 172.16.0.1
Toulouse Router: 172.16.0.2
Boulogne Router: 172.16.0.3


### Remote Sites

Toulouse LAN: 10.10.10.0/24 Gateway: 10.10.10.1
Boulogne LAN: 10.20.20.0/24 Gateway: 10.20.20.1


---

## Phase 1 — VLAN Configuration (Switch1, Fès)

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

interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,99
exit


## Phase 1 — Router-on-a-Stick (Router0, Fès)

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


**Verification:**

Switch1#show interfaces trunk
Port Mode Encapsulation Status Native vlan
Gig0/1 on 802.1q trunking 1
Port Vlans allowed on trunk
Gig0/1 10,20,30,99


---

## Phase 2 — WAN Connectivity

### Router0 (Fès) — WAN interface

interface GigabitEthernet0/1
ip address 172.16.0.1 255.255.255.0
no shutdown
exit


### Toulouse Router

interface GigabitEthernet0/0
ip address 172.16.0.2 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 10.10.10.1 255.255.255.0
no shutdown
exit


### Boulogne Router

interface GigabitEthernet0/0
ip address 172.16.0.3 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1
ip address 10.20.20.1 255.255.255.0
no shutdown
exit


### Static Routes

**Router0 (Fès):**

ip route 10.10.10.0 255.255.255.0 172.16.0.2
ip route 10.20.20.0 255.255.255.0 172.16.0.3


**Toulouse:**

ip route 10.20.20.0 255.255.255.0 172.16.0.3
ip route 192.168.10.0 255.255.255.0 172.16.0.1
ip route 192.168.20.0 255.255.255.0 172.16.0.1
ip route 192.168.30.0 255.255.255.0 172.16.0.1
ip route 192.168.99.0 255.255.255.0 172.16.0.1


**Boulogne:**

ip route 10.10.10.0 255.255.255.0 172.16.0.2
ip route 192.168.10.0 255.255.255.0 172.16.0.1
ip route 192.168.20.0 255.255.255.0 172.16.0.1
ip route 192.168.30.0 255.255.255.0 172.16.0.1
ip route 192.168.99.0 255.255.255.0 172.16.0.1


### Troubleshooting — Missing Route On Fès Router0

Initial testing from Toulouse PCs failed completely. Comparing 
`show ip route` across all three routers revealed Router0's WAN 
interface and both static routes to the remote sites were 
completely absent from its routing table — despite Toulouse and 
Boulogne both correctly listing routes pointing back to Fès. 
Adding the missing interface IP and static routes on Router0 
resolved connectivity immediately.

**Verification (Router0 after fix):**

Router0#show ip route
S 10.10.10.0/24 [1/0] via 172.16.0.2
S 10.20.20.0/24 [1/0] via 172.16.0.3
C 172.16.0.0/24 is directly connected, GigabitEthernet0/1
C 192.168.10.0/24 is directly connected, GigabitEthernet0/0.10
C 192.168.20.0/24 is directly connected, GigabitEthernet0/0.20
C 192.168.30.0/24 is directly connected, GigabitEthernet0/0.30
C 192.168.99.0/24 is directly connected, GigabitEthernet0/0.99


### Test (from Toulouse PC)

C:>ping 192.168.99.10

Pinging 192.168.99.10 with 32 bytes of data:
Request timed out.
Reply from 192.168.99.10: bytes=32 time<1ms TTL=126
Reply from 192.168.99.10: bytes=32 time<1ms TTL=126
Reply from 192.168.99.10: bytes=32 time<1ms TTL=126

Ping statistics for 192.168.99.10:
Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)

TTL=126 confirms the packet crossed exactly 2 hops (Toulouse 
Router → Fès Router0), matching the topology.

---

## Phase 3 — Helpdesk Portal (DNS + HTTP)

**Server99 — DNS Service:**

Service: On
Record Type: A Record
Name: helpdesk.alten-fes.com
Address: 192.168.99.10


**Server99 — HTTP Service:**

Service: On
Custom index.html deployed (ServiceNow-styled ITSM portal —
see html/index.html)


**Every PC across all 3 sites — DNS Server field set to:**

192.168.99.10


### Test (from Toulouse PC's Web Browser)

URL: helpdesk.alten-fes.com
Result: Page loaded successfully — full incident submission
form and ticket table rendered correctly


This confirmed the complete chain: DNS resolution across a WAN 
link → routing through Fès's VLAN structure → HTTP delivery of 
the hosted page, all working together end to end.

---

## Phase 4 — Security ACL

**Objective:** Restrict Développement (VLAN 30) to HTTP-only 
access toward the Server Room (VLAN 99), enforcing least 
privilege.

ip access-list extended DEVOPS-TO-SERVER
permit tcp 192.168.30.0 0.0.0.255 host 192.168.99.10 eq 80
deny ip 192.168.30.0 0.0.0.255 192.168.99.0 0.0.0.255
permit ip any any
exit

interface GigabitEthernet0/0.30
ip access-group DEVOPS-TO-SERVER in
exit


### Test Results

From Développement PC → Web Browser → helpdesk.alten-fes.com
Result: Page loaded successfully (HTTP explicitly permitted)

From Développement PC → ping 192.168.99.10
Result: Request timed out (blocked by ACL deny rule)

From IT Support PC → ping 192.168.99.10
Result: Reply received (IT Support not restricted by this ACL)


## What This Project Demonstrates

- Multi-site enterprise network design mirroring real 
  infrastructure I observed firsthand during an internship
- VLAN segmentation and router-on-a-stick trunking
- Static routing across three geographically distinct sites
- Full DNS-to-HTTP service chain functioning across a WAN
- Security policy enforcement through extended ACLs, applying 
  the least-privilege principle to a specific department
- Real troubleshooting: interpreting routing tables to find 
  missing configuration, and diagnosing connectivity issues 
  methodically rather than by trial and error
