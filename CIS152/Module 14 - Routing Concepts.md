Module 14 - Routing Concepts

# 14.1 - Path Determination
## 14.1.1 - Two Functions of Router
Primary functions of a router are to determine the best path to forward packets and to forward packets to their destination.
The router uses its routing table to make forwarding decisions.

## 14.1.2 - Router Functions Example
## 14.1.3 - Best Path Equals Longest Match
The routing table contains entries consisting of a prefix (network address) and prefix length. A minimum number of far left bits must match between the IP address of the packet and the route in the routing table. The prefix length of the route in the routing table determines the minimum number of far-left bits that must match.

The *Longest Match* is the route that has the greatest number of far-left bits matching that of the destination IP address in the packet.

## 14.1.4 - IPv4 Address Longest Match Example
| Destination IPv4 Addr | Address in Binary |
| --- | --- |
| 172.16.0.10 | 10101100.00010000.00000000.00001010 |

| Route Entry | Prefix/length | Address in Binary |
| --- | --- | --- |
| 1 | 172.16.0.0/12 | 10101100.00010000.00000000.00001010 |
| 2 | 172.16.0.0/18 | 10101100.00010000.00000000.00001010 |
| 3 | 172.16.0.0/26 | 10101100.00010000.00000000.00001010 |

So route #3 would be the choice since it has 26 matching bits.


## 14.1.5 - IPv6 Address Longest Match Example
Destination Addr: 2001:db8:c000::99

| Route Entry | Prefix/length | Does it match? |
| --- | --- | --- |
| 1 | 2001:db8:c000::/48 | matches 40 bits |
| 2 | 2001:db8:c000::/48 | matches 48 bits |
| 3 | 2001:db8:c000:5555::/64 | doesn't match 64 bits |

so route #2 would be the choice since dest addr doesn't match the full 64 bits required by route #3.

## 14.1.6 - Build the Routing Table
* Directly Connected Networks - Get added to the routing table when an interface is configured with an IP addr, subnet mask, and is active.
* Remote Networks - networks not directly connected to the router. 
	* Static Route - added to routing table when manually configured
	* Dynamic routing protocols - added to the table when routing protocols dynamically learn about the remote network. Protocols examples: Enhanced Interior Gateway Routing Protocol (EIGRP), Open Shortest Path First (OSPF), etc...
* Default Route - specifies next hop router to use when the routing table doesn't contain a route that matches the dest IP addr. Can be manually entered or dynamically learned. Default route over IPv4 has a route entry of **0.0.0.0/0** and IPv6 entry of **::/0**.


# 14.2 - Packet Forwarding
## 14.2.1 - Packet Forwarding Decision Process
1. data link frame with encapsulated IP packet arrives
2. router examines the dest IP addr & consults the routing table
3. router finds longest matching prefix in routing table
4. router encapsulates the packet in a data link frame & forwards it.
5. If no matching route entry, including to default route, the packet is dropped

3 things the router can do with the packet after determining best path:
* Forward the packet to a device on a directly connected network - dest device would typically be an end device in this case. Router then needs to determine the dest MAC address. This process varies:
	* IPv4 packet - router checks its ARP table, if no IPv4/MAC entry is found it sends an ARP request to find out where to forward the frame.
	* IPv6 packet - router checks its neighbor cache, if no IPv6/MAC entry is found it sends an ICMPv6 Neighbor Solicitation message.
* Forward the packet to a next-hop router - if the forwarding router & next-hop router are on an Ethernet network it uses ARP or ICMPv6 Neighbor Discovery to get next-hop MAC.
* Drop the Packet - if no match & no default route the packet is dropped.

## 14.2.2 - End-to-End Packet Forwarding
Primary responsibility of packet forwarding function is to encapsulate packets with the appropriate Layer 2 protocol (Point-to-Point (PPP) protocol, High-Level Data Link Control (HDLC) protocol, or other).

## 14.2.3 - Packet Forwarding Mechanisms
* Process Switching - older mechanism, slow & rarely implemented in modern networks. When a packet arrives it is forwarded to the control plane where the CPU matches the dest addr with routing table entry & forwards it. It does this for every packet, even if the dest addr is the same for a stream of packets.
* Fast Switching - older as well, the successor to process switching. Uses a fast-switching cache to store next-hop info. When packet arrives it is forwarded to control plane where CPU searches for a match in fast-switching cache, if no match there the packet is process switched. If another packet going to same dest arrives the next-hop info is reused w/out CPU intervention.
* Cisco Express Forwarding (CEF) - **most recent, fastest, & default on cisco switches & routers**. Like fast-switching, CEF builds a Forwarding Information Base (FIB), & an adjacency table. Table entries are not packet-triggered like fast-switching, but change-triggered, such as when network topology changes. Once a network has converged the FIB & adjacency table contain all the info a router needs to forward packets.


# 14.3 - Basic Router Configuration Review
## 14.3.1 - Topology
## 14.3.3 - Verification Commands
* show ip interface brief
* show running-config interface *interface-type number*
* show interfaces
* show ip interface
* show ip route
* ping

## 14.3.4 - Filter Command Output
use the | to filter

Filtering parameters can include: 
* section - displays the entire section that starts with the filtering expression.
* include - includes all output lines that match the filtering expression
* exclude - excludes all output lines that match the filtering expression
* begin  - displays all output lines from a certain point starting w/the line that matches filtering expression

example:
* `show running-config | section line vty`
* `show ipv6 interface brief | include up`
* `show ip interface brief | exclude unassigned`

# 14.4 - IP Routing Table
## 14.4.1 - Route Sources
Routing table contains a list of routes to known networks. This info is derived from:
* Directly connected networks 
* Static routes
* Dynamic routing protocols

Common Routing Table Codes:
* L - identifies addr assigned to a router interface. allows router to efficiently determine when it receives a packet for its own interface, not one to be forwarded.
* C - identifies directly connected network
* S - identifies static route
* O - identifies dynamically learned network from another router using OSPF
* * - a candidate for the default route 

## 14.4.2 - Routing Table Principles
| Routing Table Principle | Example |
| --- | --- |
| Every router makes its decision alone based on its own routing table | R1 forwards packets using its own routing table & doesn't know what routes are in other routers tables |
| The information in a routing table of one router may not match that in another router | |
| routing info about a path doesn't provide return routing info | |


## 14.4.3 - Routing Table Entries
IPv4 Routing Table 1st row, IPv6 2nd row
| Route Source | Dest network | Administrative Distance & Metric | Next Hop | Route Timestamp | Exit Interface |
| --- |---|---|---|---|---|
| O | 10.0.4.0/24 | [110/50] | via 10.0.3.2, | 00:13:29, | Serial10/1/1 |
| O | 2001:DB8:ACAD:4::/64 | [110/50] | via FE80::2:C, |  | Serial10/1/1 |

## 14.4.4 - Directly Connected Networks
A router must have an active interface configured with an IP addr & subnet mask before it can learn about any remote networks.

## 14.4.5 - Static Routes
Static routes use less bandwidth than dynamic routing protocols. No CPU cycles are used to calculate & communicate routes.


## 14.4.6 - Static Routes in the IP Routing Table
## 14.4.7 - Dynamic Routing Protocols
## 14.4.8 - Dynamice Routes in the IP Routing Table
IPv6 routing protocols use the link-local addr of the next-hop router

## 14.4.9 - Default Route
IPv4 entry of 0.0.0.0/0 and IPv6 entry of ::/0
zero bits need to match it.

## 14.4.10 - Structure of an IPv4 Routing Table
some route entries are indented because of the outdated 'classful' IPv4 structure. It is no longer used, but the routing table entries are still indented.
An indented entry is known as a child route. The entry is indented if it is the subnet of a classful address (class A, B, or C network).

Directly connected networks will always be indented. 

## 14.4.11 - Structure of an IPv6 Routing Table
every IPv6 addr is formatted & aligned in the same way.

## 14.4.12 - Administrative Distance
Except for specific cirmstances only 1 dynamic routing protocol should be implemented on a router.
it is possible to configure both OSPF & EIGRP though.
Cisco IOS uses administrative distance (AD) to determine which route to install in the routing table when the same route is learned by multiple protocols.
AD represents the trustworthiness of a route. Lower AD means more trustworthy.

Protocols & their associated AD:
| Route Source | Administrative Distance |
| --- | --- |
| Directly Connected | 0 |
| Static Route | 1 |
| EIGRP summary route | 5 |
|External BGP | 20 |
| Internal EIGRP | 90 |
| OSPF | 110 |
| IS-IS | 115 | 
| RIP | 120 |
| External EIGRP | 170 |
| Internal BGP | 200 |

# 14.5 - Static and Dynamic Routing
## 14.5.1 - Static or Dynamic?
Static route use cases:
* as a default route forwarding packets to a service provider
* for routes outside the routing domain & not learned by dynamic protocol
* to explicitly define a path
* for routing between stub networks

Dynamic Routing Protocol use cases:
* networks with more than a few routers
* when a change in network topology requires the network to automatically determine another path
* for scalability.

Differences between the two:
| Feature | Dynamic Routing | Static Routing |
| --- | --- | --- |
| Configuration complexity | indepentdent of netowork size | increases w/network size |
| topology changes | automatically adapts | administrator must intervene |
| scalability | good for simple to complex networks | good for simple topologies | 
| security | security must be configured | security is inherent |
| resource usage | uses CPU, memory, & link bandwidth | no additional resources needed |
| path predictability | route depends on topology & protocol used | explicitly defined |


## 14.5.2 - Dynamic Routing Evolution
Dyanmic Routing Protocols have been used since the late 1980s

RIP protocol was one of the first. RIPv1 released in1988.
OSPF & IS-IS (Intermediate System-to-Intermediate System) introduced to address the needs of larger networks (1990-ish)
IGRP developed by cisco (1985) & later replaced by EIGRP (1992)

Border Gateway Protocol (BGP) (1995), the successor of EGP (1982), is introduced to facilitate communication between service providers. 

## 14.5.3 - Dynamic Routing Protocol Concepts
main components:
* Data Structures - routing protocols typically use tables or databases whose info is stored in RAM.
* Routing Protocol Messages - routing protocols use various types of messages.
* Algorithm - routing protocols use algorithms to facilitate routing info & for best path determination.

## 14.5.4 - Best Path
dynamic protocols & their metrics for best path:
| Routing Protocol | Metric | 
| --- | --- |
| Routing Information Protocol (RIP) | the metric is 'hop count'. maximum of 15 hops allowed |
| OSPF | metric is 'cost', the cumulative bandwidth from source to destination. faster links assigned lower cost than slower links |
| EIGRP | calculates a metric based on the slowest bandwidth & delay values. could include load & reliability in the metric |

## 14.5.5 - Load Balancing
when a router has two or more paths with the same cost to the same destination network it forwards packets using both paths equally.
this is equal cost load balancing.