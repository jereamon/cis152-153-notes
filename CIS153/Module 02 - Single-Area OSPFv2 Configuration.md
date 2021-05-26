Module 2 - Single-Area OSPFv2 Configuration

# 2.1 OSPF Router ID

## 2.1.1 OSPF Reference Topology

### 2.1.2 - Router Configuration Mode of OSPF

`router ospf process-id` \- as a global config command enables OSPFv2. *process-id* is selected by the network admin. *process-id* does not have to be the same on the other OSPF routers but it is considered best practice to make them the same.

### 2.1.3 - Router IDs

OSPF router ID is a 32 bit value, represented as an IPv4 addr. All OSPF packets include the router ID of the originating router.

Router ID is used to:

- participate in the synchronization of OSPF databases - during exchange state, the router with the highest router ID sends their database descriptor (DBD) packets first.
- particiapte in the election of the Designated Router (DR) - in a multiaccess LAN, the router with the highest router ID is elected the DR, & 2nd highest is elected backup designated router (BDR)

### 2.1.4 - Router ID Order of Precedence

Routers derive their ID via 1 of 3 criteria listed in preferential order:

1.  router id is explicitly configured via `router-id rid` router config mode command. this is the recommended method.
2.  if no router id is explicitly configured the router chooses the highest IPv4 addr of any configured loopback interfaces. this is the next best alternative
3.  if neither of the above, the router chooses the highest active IPv4 addr of any of its physical interfaces. This is least recommended because it makes it difficult to distinguish between routers.

### 2.1.5 - Configure a Loopback Interface as the Router ID

Instead of relying on a physical interface for the router ID configure a loopback interface with a host route (one with a subnet mask of 255.255.255.255). A host route would not get advertised as a route to other OSPF routers.

- `interface Loopback 1`
- `ip address 1.1.1.1 255.255.255.255`
- `end`

Verify with:
`show ip protocols | incude Router ID`

### 2.1.6 - Explicitly Configure a Router ID

`router ospf 10` \- to get to the ospf config
`router-id 1.1.1.1` \- sets router ID to 1.1.1.1
`end`

Verify with:
`show ip protocols | include Router ID`

### 2.1.7 - Modify a router ID

once a router ID is selected the router ID cannot be changed until the router is reloaded or the OSPF process is reset.

`clear ip ospf process` \- to reset the ospf process, forcing the router to renegotiate adjacencies with its neighbors.
restart <-- this is here for ctrl+f searching

# 2.2 - Point to Point OSPF Networks

## 2 2.1 - The Network Command Syntax

You can specify the interfaces that belong to a point-to-point network by configuring the network command:
from router config mode (`router ospf process-id` <\-\- from global config):
`Router(config-router)# network network-addr wlidcard-mask area area-id`

- the *network-addr* *wildcard-mask* syntax enables OSPF on interfaces.
- the **area** *area-id* sytanx refers to the OSPF area. the network command must be configured with the same area-id on all routers. it is good practice to use area-id of 0 with single-area OSPFv2.

## 2.2.2 - The Wildcard Mask

This is typically the inverse of the subnet mask. With subnet mask any 1 bit is a match, with wildcard mask any 0 bit is a match, 1's are ignored.
Easiest way to calculate the wildcard mask is to subtract the network mask from 255.255.255.255.
So /24 subnet --> 255.255.255.0
255.255.255.255 - 255.255.255.0 = 0.0.0.255 <-- wildcard mask

## 2.2.4 - Configure OSPF Using the Network Command

Use wildcard mask to allow router to identify the interface and network to advertise to:
`network network-addr wildcard-mask area area-id`
So, say the network is 10.10.1.0/24 specify:
`network 10.10.1.0 0.0.0.255 area 0`

The alternative is to specify the exact interface IPv4 addr using a **quad zero** wildcard mask.
so if the router has an interface on which is the network *10.1.1.4/30* and you specify:
`network 10.1.1.5 0.0.0.0 area 0`
*10.1.1.5* is a specific addr on that network so the router will advertise on that interface.
No wildcard mask calculation is necessary when doing it this way.

## 2.2.6 - Configure OSPF Using the ip ospf Command

you can also configure ospf on the interface using `ip ospf` command:
`Router(config-if)# ip ospf process-id area area-id`

## 2.2.8 - Passive Interface

by default, OSPF messages are sent out enabled interfaces whether they are connecting to other OSPF-enabled routers or not.
This is bad because:

- inneficient use of bandwidth
- inneficient use of resources - all devices must process & discard unnecessary messages
- increased security risk - OSPF message could be intercepted. routing updates could be modified and sent back to the router, corrupting the routing table.

## 2.2.9 - Configure Passive Interfaces

use `passive-interface` command to prevent unnecessary OSPF messages being sent on interfaces that aren't connected to OSPF enabled routers.
Pretending *loopback 0* interface is an ethernet network connected to users:
from router config mode (`router ospf 10`)
`passive-interface loopback 0`

all interfaces can be made passive using:
`passive-interface default`
reenable an interface using:
`no passive-interface interface-id`

verify config with: `show ip protocols`

## 2.2.11 - OSPF Point-to-Point Networks

cisco routers elect a designated router (DR) and backup designated router (BDR) by default.
This is unneccessary on a point-to-point network where there can be only two routers.
Change a network to a point-to-point network using:
`ip ospf network point-to-point`
this disables the DR/BDR election process

# 2.3 - Multiaccess OSPF Networks

## 2.3.1 - OSPF Network Types

In multiaccess networks one router controls the distribution of LSAs.
Routers can be connected to the same switch to form a multiaccess network.

## 2.3.2 - OSPF Designated Router

the Designated Router (DR) uses the multicast IPv4 addr **224.0.0.5** to distribute LSAs. This addr is meant for all OSPF routers.

The BDR listens and maintains a relationship with all routers so it can take over if the DR stops sending Hello packets.

Every other router becomes a **DROTHER**, neither a DR nor BDR, they use the multicast addr **224.0.0.6** to send packets to dr & bdr, only the dr & bdr listen for 224.0.0.6

## 2.3.2 - Multiaccess Reference Topology

## 2.3.3 - Verify OSPF Router Roles

`show ip ospf interface` to verify the roles of the OSPFv2 router

DROTHER output:

- look for *state DROTHER* with a default priority of 1
- further down will be a 'neighbor count' showing adjacent neighbors and their roles as DR & BDR

BDR output:

- look for *state BDR* & priority 1
- also, it will list the DR ID & its interface address

DR output:

- virtually identical to BDR but it will be *state DR*

## 2.3.5 - Verify DR/BDR Adjacencies

`show ip ospf neighbor` \- verify ospf neighbor stuff

The 'state' of neighbors in multiaccess networks can be:

- FULL/DROTHER - a DR or BDR router fully adjacent with a non-DR or BDR router. Exchanges Hello packets, updates, queries, replies, & acknowledgements with its neighbor.
- FULL/DR - full adjacent with the indicated DR neighbor. Exchanges Hello packets, updates, queries, replies, & acks with its neighbor.
- FULL/BDR - fully adjacent with indicated BDR neighbor. they exchange all packets as well ^^.
- 2-WAY/DROTHER - non-DR or BDR router that has neighbor relationship with another non-DR or BDR router. They exchange Hello packets.

## 2.3.6 - Default DR/BDR Election Process

OSPF DR & BDR election is based on:

1.  router with the highest interface priority. The priority can be any number between 0-255. if a router's priority is set to 0 it cannot be a DR nor BDR. The default priority is 1.
2.  if interface priorities are equal, the highest router ID is made DR & 2nd highest is BDR.

## 2.3.7 - DR Failure & Recovery

the DR remains the DR until one of the following occurs:

- DR fails
- OSPF process on the DR is stopped
- the multiaccess interface on the DR fails or is shutdown

If the DR fails the BDR is automatically promoted to DR. Even if another DROTHER has a higher priority or router ID after the initial election. Once the BDR is promoted a new election occurs and the DROTHER with the highest priority/router ID is made BDR.

## 2.3.8 - The ip ospf priority command

Entered from the interface config, use:
`ip ospf priority` \- to set the interface priority. This is a better way of controlling the DR/BDR election than setting the router ID.

## 2.3.9 - Configure OSPF Priority

again, routers with priority of 0 can never be DR or BDR

# 2.4 - Modify Single-Area OSPFv2

## 2.4.1 - Cisco OSPF Cost Metric

lower cost indicates a better path than a higher cost.
**cost = reference bandwidth / interface bandwidth**

The default reference bandwidth is 10^8 (100,000,000), making the formula:
cost = 100,000,000 bps / interface bandwidth in bps

OSPF cost value must be an integer making FastEthernet, Gigabit Ethernet, & 10 Gig Ethernet to have the same cost. To remedy this you can use:

- `auto-cost reference-bandwidth mbps` \- from router config to adjust the reference bandwidth on each ospf router
- `ip ospf cost` \- to manually set the ospf cost value on necessary interfaces

| Interface Type | Reference Bandwidth in bps / Default bandwidth in bps | cost |
| --- | --- | --- |
| 10 Gbps | 100,000,000 / 10,000,000,000 | 0.01 = 1 |
| 1 Gbps | 100,000,000 / 1,000,000,000 | 0.1 = 1 |
| 100 Mbps | 100,000,000 / 100,000,000 | 1   |
| 10 Mbps | 100,000,000 / 10,000,000 | 10  |

## 2.4.2 - Adjust the Reference Bandwidth

`auto cost reference-bandwidth mbps`
So to set the reference bandwidth to 10 Gigabit ethernet you would specify:
`auto cost reference-bandwidth 10000`

**The `auto cost reference-bandwidth` command must be configured the same accross all router to ensure accurate route calculations**

## 2.4.3 - OSPF Accumulates Costs

the cost of an OSPF route is the accumulated value from one router to the destination

## 2.4.4 - Manually Set OSPF Cost Value

Only adjust interface cost values when the outcome is fully understood! It could result in undesired consequences.

`ip ospf cost value` \- to manually set an interface's ospf cost

## 2.4.7 - Hello Packet Intervals

OSPFv2 Hello Packets are transmitted to multicast addr 224.0.0.5 every 10 seconds.

The dead interval, by default, is 4 times that of the hello interval. If no hello packet is recieved within the dead interval that neighbor is removed from the LSDB.

## 2.4.8 - Verify Hello and Dead Intervals

hello and dead intervals are configurable on a per-interface basis. These intervals must match or an adjacency does not occur.

`show ip ospf interface` command to verify these intervals
`show ip ospf neighbor` to see the dead time counting down from 40 seconds.

## 2.4.9 - Modify OSPFv2 Intervals

`ip ospf hello-interval seconds` \- to change hello interval
`ip ospf dead-interval seconds` \- to change the dead interval

`no ip ospf hello-interval` and `no ip ospf dead-interval` \- to reset them

# 2.5 - Default Route Propagation

## 2.5.1 - Propagate a Default Static Route in OSPFv2

Usually a router connected to the internet would be an edge router or a default gateway.
In OSPF terminology a router located between an OSPF routing domain & a non-OSPF network (the internet) is called the **autonomous system boundary router (ASBR)**

all that is required for an ASBR to reach the internet is a default static route to the service provider.
To propagate a default route the edge router must be configured with:

- a default static route using the `ip route 0.0.0.0 0.0.0.0 [next-hop addr | exit-intf]` command from global config
- the `default-information originate` router config command. this tells the edge router to be the source of the default route info & to propagate that info in OSPF updates

## 2.5.2 - Verify the Propagated Default Route

`show ip route` to verify the default route settings. use this on other routers to check if they recieved a default route.

# 2.6 - Verify Single-Area OSPFv2

## 2.6.1 - Verify OSPF Neighbors

useful for verifying routing:

- `show ip interface brief`
- `show ip route`
- `show ip route ospf` \- to show ospf routes

help determine if OSPF is operating as expected:

- `show ip ospf neighbor` \- verify if router has formed an adjacency with neighbors
    - Will show the following:
        - Neighbor ID - router ID of the neighbor
        - Pri - OSPFv2 priority of the interface (used in DR & BDR election)
        - State - Full means full neighbor adjacency, 2WAY means full adjacency on multiaccess network, a dash means no DR or BDR election necessary.
        - Dead Time - time remaining that the router will wait for a Hello packet
        - Address - the IPv4 addr of the neighbor
        - interface - the interface on which the adjacency is formed (g/0/0/0 or f/01, etc)
- `show ip protocols`
- `show ip ospf`
- `show ip ospf interface`

## 2.6.2 - Verify OSPF Protocol Settings

`show ip protocols` \- quick way to verify vital OSPF config info including: if ospf is \*\*enabled\*\*, and provides a full list of the networks that are being advertised. Will show interfaces configured as **passive**

## 2.6.3 - Verify OSPF Process Information

`show ip ospf` \- can also be used to examine the process ID & router ID.

## 2.6.4 - Verify OSPF Interface Settings

`show ip ospf interface` \- provides detailed list for every OSPFv2 enabled interface. Shows the process ID, local router ID, type of network, OSPF cost, DR & BDR on multiaccess links, & adjacent neighbors. **shows hello & dead time intervals**
specify an interface to see the settings of just that interface

`show ip ospf interface brief` \- for quick summary of OSPFv2 enabled interfaces