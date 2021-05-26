Module 1 - Single-Area OSPFv2 Concepts

# 1.1 - OSPF Features and Characteristics

OSPF includes single area and multi-area.
OSPFv2 is used for IPv4 and OPSFv3 is used for IPv6

OSPF is a link-state routing protocol that was developed as an alternative for the distance vector Routing Information Protocol (RIP).
RIP relied on hop count as the only metric for routing. It does not scale well.

OSPF uses the concept of areas, allowing a network admin to divide the routing domain into distinct areas to control routing update traffic. A **link** is an interface on the router. It is also a network segment that connects two routers, or a stub network.

**link-state** \- the state of a link. includes the network prefix, prefix length, and cost.

## 1.1.2 - Components of OSPF

- Routing Protocol Messages - OSPF exchange messages have five types of packets used to discover neighbor routers & exchange routing info:
    - Hello packet
    - Database description packet
    - Link-state request packet
    - Link-state update packet
    - Link-state acknowledgement packet
- Data Structures - OSPF messages are used to create and maintain 3 OSPF databases:
    - Adjacency database - neighbor table -> a table unique for each router which lists all neighbor routers with which a router has bidirectional communication. **View using** \- `show ip ospf neighbor`
    - Link-state Database - topology table -> lists info about all others routers in the network & represents the network topology. All routers in an area have an **identical LSDB**. **View using** \- `show ip ospf database`
    - Forwarding Database - routing table -> list of routes which is generated when an algorithm is run on the link-state database. Routing table of each router is unique. **View using** \- `show ip route`
- Algorithm - router builds a topology table using results from calculations based on Dijkstra shortest-path first (SPF) algorithm. SPF algorithm is based on the cumulative cost to reach a destination. It creates an SPF tree by placing each router at the root of the tree and calculating the shortest path to each node. It calculates the best routes from there and those are placed in the routing table.

## 1.1.3 - Link-State Operation

OSPF routers complete a generic link-state routing process to maintain routing information. Each link between routers is labeled with a cost value.
Routers follow these steps:

1.  Establish Neighbor Adjacencies - OSPF-enabled router sends Hello packets to all OSPF-enabled interfaces. If it is determined that a neighbor is present the router attempts to establish a neighbor adjacency with that neighbor.
2.  Exchange Link-State Advertisements - once adjacencies are established routers exchange **link-state advertisements (LSAs)**. These contain the state and cost of each directly connected link. LSAs are flooded to neighbors & adjacent neighbors receiving LSAs flood the LSA to other directly connected neighbors until all routers in an area have all LSAs.
3.  Build the Link State Database - once LSAs are received OSPF enabled routers build the LSDB (see above) based on the LSAs. Eventually this database holds all info about the topology of the area.
4.  Execute the SPF algorithm - routers then execute the SPF algorithm to create the SPF tree.
5.  Choose the best route - once the SPF tree is built the best routes are offered to the routing table. The routes will be inserted unless there is already a route in the table with a lower administrative distance.

## 1.1.4 - Single-Area and Multiarea OSPF

An OSPF area is a group of routers with the same link-state info in their LSDBs.
OSPF can be implemented in one of two ways:

- Single-Area OSPF - all routers are in one area. **Best practice is to use area 0**
- Multiarea OSPF - OSPF implemented with multiple areas in a hierarchical fashion. All areas must connect to the backbone area (area 0). routers interconnecting the areas are called **Area Border Routers (ABRs)**

## 1.1.5 - Multiarea OSPF

one large routing domain can be divided into smaller areas. Routing still occurs between these areas but many of the processor intensive routing operations (recalculating databases) are kept within an area.

Hierarchical-topology design options with multiarea OSPF offer advantages:

- smaller routing tables - network addresses can be summarized between areas. Route summarization is not enabled by default.
- Reduced link-state update overhead - multiarea OSPF with smaller areas minimizes processing & memory requirements.
- Reduced frequency of SPF calculations - routers do not have to recalculate if the topology changes in a different area.

## 1.1.6 - OSPFv3

is for exchanging IPv6 prefixes. Has the same functionality as OSPFv2 but uses IPv6 as the network layer transport. Still uses **SPF algorithm&** to determine best paths.

OSPFv3 does have separate processes from OSPFv2. They're basically the same, but run independently from OSPFv2. OSPFv2 & 3 each have separate adjacency tables, OSPF topology tables, and IP routing tables.

# 1.2 - OSPF Packets

## 1.2.1 - video

## 1.2.2 - Types of OSPF Packets

- Type 1 - Hello Packet -> used to establish and maintain adjacency with other OSPF routers.
- Type 2 - Database Description (DBD) Packet -> Checks for database synchronization between routers. Specifically, it contains abbreviated list of LSDB of the sending router. It is used by receiving routers to check against the local LSDB. The LSDB must be identical on all link-state routers within in area to construct an accurate SPF tree.
- Type 3 - Link-State Request (LSR) -> used to request addition info about entries in a previously received DBD packet.
- Type 4 - Link-State Update (LSU) -> used to reply to LSRs and to announce new information. LSUs contain several different types of LSAs.
- Type 5 - Link-State Acknowledgement (LSAck) packet - when an LSU is receieved, the router sends an LSAck to confirm its receipt of the LSU. LSAck data field is empty.

## 1.2.3 - Link-State Updates

Difference between LSU and LSA: these are often used interchangeably. **An LSU contains one or more LSAs**

LSUs are used to reply to an LSR packet and also forwards OSPF routing updates, such as link changes.
an LSU can contain **11 different** types of OSPFv2 LSAs. OSPFv3 renamed several of these and contains two additional LSAs.

LSA Types:

| LSA Type | Description |
| --- | --- |
| 1   | Router LSAs |
| 2   | Network LSAs |
| 3 or 4 | Summary LSAs |
| 5   | Autonomous systems external LSAs |
| 6   | Multicast OSPF LSAs |
| 7   | Defined for not-so-stubby- areas |
| 8   | external attributes LSA for border gateway patrol (BGPs) |

## 1.2.4 - Hello Packet

Hello packets:

- discover OSPF neighbors and establish neighbor adjacencies
- advertise parameters on which two routers much agree to become neighbors
- elect the Designated Router (DR) and Backup Designated Router (BDR) on multiaccess networks like ethernet.

OSPF packet headers include:

- version
- Type - this is 1 for hello packet
- packet length
- router ID
- area ID
- checksum
- AuType
- authentication
- authentication

Then comes the Hello Packet Content, which includes:

- network mask
- hello interval
- option
- router priority
- dead interval - usually 4 times the hello interval
- designated router (DR)
- backup designated router (BDR)
- list of neighbors

# 1.3 - OSPF Operation

## 1.3.1 - Video - OSPF Operation

## 1.3.2 - OSPF Operational States

when an OSPF enabled router is first connected it attempts to:

- create adjacencies with neighbors
- exchange routing information
- calculate the best routes
- reach convergence

ospf router progresses through each of these states

| State | Description |
| --- | --- |
| Down state | no hello packets recieved = Down. Router sends Hello packets & by doing so transitions to Init State. |
| Init State | Hello packets recieved from neighbor which contain Router ID of the sender. transition to Two-Way State. |
| Two- Way State | communication between the 2 routers is bidirectional. on multiaccess links routers elect a DR & BDR (1.2.4). transition to ExStart |
| ExStart State | on point-to-point networks, the 2 routers decide which will initiate DBD packet exchange & what the DBD packet sequence # is. |
| Exchange State | routers exchange DBD packets. **if** additional info required transition to Loading, else transition to Full State |
| Loading State | LSRs & LSUs are used to gain additional route info. routes processed using PSF algorithm. transition to Full State |
| Full State | LSDB of the router is fully synchronized |

## 1.3.3 - Establish Neighbor Adjacencies

When OSPF is enabled the router sends a Hello packet containing its router ID out on the multicast address **224.0.0.5** to determine if there are OSPF neighbors on the link.

**Router ID** is a 32 bit number formatted like an IPv4 addr assigned to uniquely identify each router.

Receiving a Hello packet causes an OSPF enabled router to attempt to establish adjacency with the packet's sender.

1.  Down State to Init State - OSPFv2 is enabled & the router starts sending Hello packets out all OSPF enabled interfaces.
2.  The Init State - router recieves Hello packet, adds the router ID in it to its neighbor list & responds with a Hello packet containing the each router's ID in its list of neighbors on the same interface.
3.  Two-Way State - 1st router receives Hello response & adds sender to its neighbor list & sees its own router ID in the packet causing it to transition to the Two-Way State.
    - if the adjacent neighbors are connected via point-to-point link they transition to ExStart State
    - if the routers are connected via ethernet a DR & BDR (1.2.4) must be elected.
4.  Elect the DR & BDR - router with highest router ID becomes DR & second highest becomes BDR.

## 1.3.4 - Synchronizing OSPF Databases

Process of exchanging & synchronizing LSDBs is a 3 step process:

1.  Decide First Router - in ExStart state routers decide which will send DBD packets first. higher router ID goes first.
2.  Exchange DBDs - in Exchange State they exchange 1 or more DBD packets. These include the **LSA header** from the LSDB which contains info about the link-state type, addr of the advertising router, cost of the link, & the sequence number. **Sequence #** is used to determine newness of the info.
3.  Send an LSR - DBD packet data is compared to that of the local LSDB & if more current info is found in the DBD the router transitions to the Loading state. Router sends an LSR and the other router responds with complete info via an LSU packet. Once routers are fully synchronized LSUs are only sent if a change is percieved or every 30 minutes.

## 1.3.5 - The Need for a DR

**DRs are elected** to disseminate LSAs to other OSPF Routers

DR and BDR are needed for multiaccess networks because of the following challenges:

- creation of multiple adjacencies - ethernet networks could have many interconnected OSPF routers all trying to create adjacencies with eachother resulting in large numbers of LSAs being exchanged.
- extensive flooding of LSAs - link-state router flood LSAs when OSPF is initialized & when there is a topology change.

## 1.3.6 - LSA Flooding with a DR

With no DR each router sends its LSAs to every other. With a DR & BDR the other routers only communicate to those two and rely on them for up to date network information.