Module 5 - STP Concepts (Spanning Tree Protocol)

## 5.1 Purpose of STP
### 5.1.1 - Redundancy in Layer 2 Switched Networks
### 5.1.2 - Spanning Tree Protocol
Spanning tree protocol is a layer 2 loop-prevention protocol. It allows for redundancy and loop free layer 2.

layer 2 loops will result in network failure & mac address instability in switches since they are receiving frames that say they're from the same end device on different ports.

### 5.1.4 - Issues with Redundant Switch Links
Layer 2, unlike layer 3 protocols (ex: IPv4 TTL), doesn't include any mechanism to eliminate endlessly looping frames.

### 5.1.6 - Broadcast Storm
an anormally high number of broadcasts which overwhelm the network
hosts caught in a broadcast storm are not accessible to other hosts. Also, switches won't know which ports to forward traffic out of.

### 5.1.7 - The Spanning Tree Algorithm
STP based on an algorithm invented by Radia Perlman in 1985
* STA (spanning tree algorithm) Scenario Topology - many switches connected to one another almost in a mesh topology
* Select the root bridge - a root bridge (switch) is chosen and each other switch determines the 'least cost path' from itself to that bridge
* Block Redundant Paths - ports connected to redundant paths are blocked so that traffic isn't sent back to a switch that had already received it (resulting in a loop)
* Loop-Free Topology - once redundant paths are blocked each switch has only one forwarding path to the root bridge
* Link Failure Causes Recalculation - the physical redundancy still exists in the network, so, in the even of a failure on one path STP will recalculate and use another path.


## 5.2 STP Operations
### 5.2.1 Steps to a Loop-Free Topology
switches use *Bridge Protocol Data Units (BPDUs)* during STA & STP functions
BPDUs are used to elect the root bridge, root ports, designated ports, and alternate ports.

Each BPDU contains a bridge ID (BID). The BID contains:
* Bridge priority - default value is 32768. Range is from 0 to 61,440 in increments of 4096. Lower bridge priority is preferable with a priority of 0 taking precendence over all others.
* Extended system id - a decimal value added to the priority value to identify the VLAN for this BPDU
* MAC Address - if two switches have the same priority & same extended system ID the one with the lowest MAC address value will have the lower BID.

### 5.2.2 - 1.Elect the Root Bridge
after a switch boots it begins sending BPDU frames every 2 seconds. Each frame contains its BID & the BID of the root bridge (root ID). Initially all switches declare themselves as the root bridge. This is adjusted as BPDUs are exchanged.

#### 5.2.3 - Impact of Default BIDs
since two or more switches may have the same priority & the one with the lowest MAC address will become the root bridge, it may be a good idea for network administrators to configure the desired root bridge with a lower priority.

#### 5.2.4 - Determine the Root Path Cost
**root path cost** = the sum of individual port costs along the path from the switch to the root bridge.

default port costs are defined by the speed that port operates at.
802.1w RSTP Cost:
* 10Gbps - 2000
* 1Gpbps - 20,000
* 100Mbps - 200,000
* 10Mvps - 2,000,000

### 5.2.5 - 2.Elect the Root Ports
after a root bridge is determined the STA algorithm is used to determine the root port. the root port is the closest to the root bridge according to overall cost (best path).

Internal root path cost = sum of all the port costs on the path to the root bridge
once root path is chosen all redundant paths are blocked.

### 5.2.6 - 3.Elect Designated Ports
The designated port is the one with the best path to receive traffic leading to the root bridge. If a root port is on one end of a network segment a designated port is on the other end.

how STA elects designated ports:
* designated ports on root bridge - all the ports on the root bridge are designated ports
* Designated port when there **is a** root port - if one end of a segment is a root port then the other end is a designated port.
* Designated port when there is **no** root port - if neither of the switches are the root bridge the port on the switch with the least-cost path to the root bridge is the designated port for the segment.

### 5.2.7 - 4.Elect Alternate (blocked) Ports
a port becomes an alternate port if it is not a designated port or root port.

### 5.2.8 - Elect a Root Port from Multiple Equal-Cost Paths
if a switch has multiple equal-cost paths to the root bridge it chooses a port using:
1. Lowest Sender BID - 
2. Lowest Sender Port Priority - if sender BID is a tie port priority is used. 128 is default port priority
3. Lowest Sender Port ID - if BID & port priority tie then the lower port ID is used. So f0/1 would be chosen before f0/2.

### 5.2.9 STP Timers and Port States
STP requires 3 timers:
* Hello Timer - hello time is the interval between BPDUs. Default is 2 seconds but can be set between 1 & 10.
* Forward Delay Timer - forward delay is the time spent in the listening & learning state. default is 15 seconds but can be set between 4 & 30.
* Max Age Timer - max age is the maximum time a switch waits before attempting to cahnge the STP topology. Default is 20 seconds but can be set between 6 & 40.

*NOTE* - the default times are changed on the root bridge which dictates the value of the timers for the STP domain.

5 port states for STP:
* blocking - the port is an alternate port & does not forward frames. It does receive BPDU frames. 
* Listening - transitions to listening after blocking. It receives BPDUs and sends its own.
* Learning - transitions to learning after listening. It receives & processes BPDUs and prepares to participate in frame forwarding. begins to populate the MAC address table. User frames are not forwarded.
* Forwarding - it's part of the active topology. Forwards user traffic & sends & receives BPDU frames.
* Disabled - doesn't participate in STP & does not forward frames.

### 5.2.10 - Operational Details of Each Port State
| Port State | BPDU | MAC Table | Forwarding Data Frames |
| ------     | ---- | ------    | ----------             |
| blocking   | Receive Only | No update | No |
| Listening  | Receive & Send | No Update | No |
| Learning | Receive & Send | Updating Table | No |
| Forwarding | Receive & Send | Updating Table | Yes |
| Disabled | None Sent or Received | No Update | No |


### 5.2.11 - Per-VLAN Spanning Tree (PVST)
with PVST a root bridge is elected for each STP instance. That way there can be different root bridges for different sets of VLANs. 


## 5.3 - Evolution of STP
### 5.3.1 - Different Versions of STP
| STP Variety | Description |
| -----       | ------      |
| STP | **original IEEE 802.1D version**. Also called Common Spanning Tree (CST). assumes one stp instance for entire network, regardless of # of vlans |
| PVST+ | Per-VLAN Spanning Tree (PVST+). **Cisco** enhancement of STP. Provides separate STP instance for each VLAN |
| RSTP | Rapid Spanning Tree Protocol. **IEEE 802.1W**. Evolution of STP that provides faster convergence than STP |
| 802.1D-2004 | Updated version of STP standard incorporating IEEE 802.1w. |
| Rapid PVST+ | **Cisco** enhancement of RSTP. Uses PVST+ and provides separate 802.1w instance per vlan |
| MSTP | Multiple Spanning Tree Protocol. **IEEE** standard inspired by earlier Cisco proprietary MISTP. maps multiple VLANs to the same STP instance. |
| MST | Multiple Spanning Tree. **Cisco implementation of MSTP**. Provides up to 16 instances of RSTP & combines many VLANs with the same physical & logical topology into a common RSTP instance. |

cisco switches running IOS 15.0 or later run PVST+ by default.


### 5.3.2 RSTP Concepts
IEEE 802.1w supersedes 802.1D and retains backward compatibility.
is mostly the same but increases the speed of the recalculation of the spanning tree when the layer two topology changes.
If a port is configured to be an alternate port it can immediately change to a forwarding state w/out waiting for the network to converge.

#### 5.3.3 RSTP Port States and Port Roles
STP & RSTP Port States:
| STP | RSTP |
| --- | ---- |
| disabled |  |
| blocking | Discarding |
| listening |   |
| learning | learning |
|forwarding | forwarding |

STP & RSTP Port Roles:
| STP | RSTP |
| --- | ---- | 
| Root Port | Root Port |
| Designated Port | Designated port |
| blocked port | backup port |
| blocked port | alternate port |

### 5.3.4 PortFast and BPDU Guard
a switch port configured with portfast transitions from blocking to forwarding immediately, bypassing listening & learning. This may need to be enabled to ensure DHCP functions correctly.

It should only be used on access ports. Ports that connect to end devices.


### 5.3.5 Alternatives to STP
* Multi System Link Aggregation (MLAG)
* Shortest Path Bridging (SPB)
* Transparent Interconnect of Lots of Links (TRILL)