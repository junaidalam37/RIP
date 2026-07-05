# RIP
# RIP Routing Lab — 4-Router Ring Topology (Cisco Packet Tracer)

A 4-router ring topology configured with RIP v1 to demonstrate dynamic routing convergence across multiple LANs, built in Cisco Packet Tracer.

# Overview

Four routers (Router8, Router4, Router6, Router7) are connected in a ring/square topology each hosting its own LAN. RIP v1 is used so every router dynamically learns routes to all remote LANs instead of relying on static routes — including the redundant path around the ring.

# Topology


        Laptop1 (192.168.10.1)         Laptop2 (192.168.40.1)
              │                              │
           Switch3                        Switch2
              │                              │
   192.168.10.100 ── Router8 ── 40.1.1.0/24 ── Router4 ── 192.168.40.100
              │        (Gig0/2)      (Gig0/1↔Gig0/1)   (Gig0/2)   │
         10.1.1.0/24                                       30.1.1.0/24
              │                                                   │
   192.168.20.100 ── Router7 ── 20.1.1.0/24 ── Router6 ── 192.168.30.100
              │        (Gig0/1↔Gig0/0)                    (Gig0/1)   │
           Switch0                                              Switch1
              │                                                   │
        Laptop0 (192.168.20.1)         Laptop3 (192.168.30.1)


 Addressing Table

| Router | Interface | IP Address | Connects To |
|---|---|---|---|
| Router8 | Gig0/0 | 192.168.10.100/24 | Switch3 → Laptop1 |
| | Gig0/1 | 40.1.1.2/24 | Router4 |
| | Gig0/2 | 10.1.1.1/24 | Router7 |
| Router4 | Gig0/0 | 192.168.40.100/24 | Switch2 → Laptop2 |
| | Gig0/1 | 40.1.1.1/24 | Router8 |
| | Gig0/2 | 30.1.1.2/24 | Router6 |
| Router7 | Gig0/0 | 10.1.1.2/24 | Router8 |
| | Gig0/1 | 20.1.1.1/24 | Router6 |
| | Gig0/2 | 192.168.20.100/24 | Switch0 → Laptop0 |
| Router6 | Gig0/0 | 20.1.1.2/24 | Router7 |
| | Gig0/1 | 30.1.1.1/24 | Router4 |
| | Gig0/2 | 192.168.30.100/24 | Switch1 → Laptop3 |

WAN links: 40.1.1.0/24 (Router8–Router4), 30.1.1.0/24 (Router4–Router6), 20.1.1.0/24 (Router6–Router7), 10.1.1.0/24 (Router7–Router8)

Configuration

Router8

interface Gig0/0
 ip address 192.168.10.100 255.255.255.0
 no shutdown
interface Gig0/1
 ip address 40.1.1.2 255.255.255.0
 no shutdown
interface Gig0/2
 ip address 10.1.1.1 255.255.255.0
 no shutdown

router rip
 version 1
 network 192.168.10.0
 network 40.1.1.0
 network 10.1.1.0


Router4
interface Gig0/0
 ip address 192.168.40.100 255.255.255.0
 no shutdown
interface Gig0/1
 ip address 40.1.1.1 255.255.255.0
 no shutdown
interface Gig0/2
 ip address 30.1.1.2 255.255.255.0
 no shutdown

router rip
 version 1
 network 192.168.40.0
 network 40.1.1.0
 network 30.1.1.0

 Router7 
interface Gig0/0
 ip address 10.1.1.2 255.255.255.0
 no shutdown
interface Gig0/1
 ip address 20.1.1.1 255.255.255.0
 no shutdown
interface Gig0/2
 ip address 192.168.20.100 255.255.255.0
 no shutdown

router rip
 version 1
 network 10.1.1.0
 network 20.1.1.0
 network 192.168.20.0


Router6

interface Gig0/0
 ip address 20.1.1.2 255.255.255.0
 no shutdown
interface Gig0/1
 ip address 30.1.1.1 255.255.255.0
 no shutdown
interface Gig0/2
 ip address 192.168.30.100 255.255.255.0
 no shutdown

router rip
 version 1
 network 20.1.1.0
 network 30.1.1.0
 network 192.168.30.0


Testing / Verification

1. On any router, check that RIP has learned all remote networks:
   
   show ip route rip
 
2. Verify RIP neighbors/updates:

   show ip protocols
   debug ip rip
  
3. Ping across LANs to confirm end-to-end reachability, e.g. Laptop1 (192.168.10.1) → Laptop3 (192.168.30.1).
4. Test ring redundancy: shut down one WAN link (e.g. Router8 Gig0/1) and confirm RIP reconverges, routing traffic the long way around the ring instead of failing.

# Files
rip33.pkt — Packet Tracer lab file

# Concept Recap

RIP (Routing Information Protocol) is dynamic interior gatway protocol(IGP) that shares directly connected and share information to the nieghbors networks is called updates its share these updates in every 30 second when some changes or adding new network then its share all the updates with each other.
it is also decision making for the shortest path only see the shotest path for the traffic on the bases of matrics number metrics number is the number of hop count between the two routers when the hop count is less the rip will send the traffic on that line it is only decision for the shortest path not for the bandwidth or speed.
it v1 is multicasting and v1 is broadcasting 

# Topics

Cisco CCNA Packet Tracer  RIP RIPv1 Dynamic Routing Ring Topology Networking
