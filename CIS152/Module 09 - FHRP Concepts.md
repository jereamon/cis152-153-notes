Module 9 - FHRP Concepts

# 9.1 First Hop Redundancy Protocols (FHRP)
## 9.1.1 - Default Gateway Limitations
If the device configured as a host's default gateway goes down that host can no longer send packets off the local network even if another router exists that could route its traffic.

## 9.1.2 Router Redundancy
Virtual Router - allows multiple physical routers to work together to present the illusion of a single router to the hosts.
The IPv4 addr of the virtual router is configured as the default gateway for the hosts. 

## 9.1.3 - Steps for Router Failover
When the active router fails:
1. The standby router stops seeing Hello messages from the forwarding router.
2. The standby router assumes the role of the forwarding router.
3. The new forwarding router adopts the IPv4 & MAC of the virutal router so the hosts see no difference.

## 9.1.4 - FHRP Options
* Hot Standby Router Protocol (HSRP) - Cisco Proprietary, designed to allow for transparent failover of a first-hop IPv4 device.
* HSRP for IPv6
* Virtual Router Redundancy Protocol version 2 (VRRPv2) - Non-proprietary, dynamically assigns for virtual routers to the VRRP routers on an IPv4 lan.
* VRRPv3 - provides support for IPv4 and IPv6. Works in multi-vendor environments & is more scalable than VRRPv2.
* Gateway Load Balancing Protocol (GLBP) - Cisco Proprietary FHRP that works like HSRP & VRRP while also allowing load balancing between a group of redundant routers.
* GLBP for IPv6
* ICMP Router Discovery Protocol (IRDP) - LEGACY FHRP solution. Specified in RFC 1256 for IPv4

# 9.2 HSRP
## 9.2.1 - HSRP Overview
## 9.2.2 - HSRP Priority and Preemption
**HSRP Priority** can be used to determine the active router. Router with the *HIGHEST* HSRP priority becomes active. HSRP priority is 100 by default.
if priorities are equal router with numerically highest IPv4 address is the active router.

**HSRP Preemption** - Once a router is active it remains the active router even if another comes online with a higher HSRP Priority.
`standby preempt` - from interface config to enable preemption. This will force a new HSRP election process when a higher priority router comes online.

## 9.2.3 - HSRP States and Timers
| HSRP State | Description |
| --- | --- |
| Initial | when an interface first becomes available. |
| Learn | The router doesn't know the virutal IP addr & hasn't seen a hello message from the active router. It waits to hear from the active router |
| Listen | The router knows the virtual IP addr but isn't the active router or standby router. It listens for hello messages from those. |
| Speak | router sends periodic hello messages & participates in election of active & standby router. |
| Standby | Router is a candidate to become next active router & sends periodic hello messages. |
| Active | Router won the election | 