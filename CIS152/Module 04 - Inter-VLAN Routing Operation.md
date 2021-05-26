Module 4 - Inter-VLAN Routing Operation

## 4.1 Inter-VLAN Routing Operation
### 4.1.1 What is Inter-VLAN Routing
3 Inter-VLAN routing options:
* legacy inter-vlan routing - doesn't scale well
* router-on-a-stick - acceptable for small to medium network
* layer 3 switch using SVIs - most scalable for medium to large networks

### 4.1.2 Legacy Inter-VLAN Routing
relied on a router connected to a switch with each separate VLAN connected to a different router interface (port)

### 4.1.3 Router-on-a-Stick inter-vlan routing
only requires 1 router interface between multiple vlans
software based subinterface are configured on the router so that VLAN-tagged traffic is forwarded to them on arrival.

the router determines the exit interface & if that interface is configured as 802.Q the frames are VLAN-tagged with the new VLAN.

This only scales to 50 VLANs

### 4.1.4 Inter-VLAN Routing on a Layer 3 Switch
SVIs are created for individual VLANs which perform the same functions that a physical router interface would.
This is:
* faster than the previous 2 methods
* no need for external links from the switch to the router for routing
* not limited to one link because layer 2 etherchannels can be used as trunk links
* lower latency since data doesn't need to leave the switch to be routed to a different network
* more commonly deployed in a campus LAN than routers

Only downside is layer 3 switches are more expensive

## 4.2 Router-on-a-Stick Inter-VLAN Routing
### 4.2.1 Router-on-a-Stick Scenario
for the switches:
1. Create and name your vlans
2. create the management interface
3. configure access ports (the ones with end devices connected)
4. configure trunking ports (the ones going to other switches/routers)


### 4.2.4 Router Subinterface Configuration
created using (global config): `interface interface_id.subinterface_id`
example: `interface G0/0/1.10` (not required, but it's customary to match the subinterface number with the vlan)

once created configure subinterfaces using:
* `encapsulation dot1q *vlan_id* [native]` - the native keyword is only appended to set the native vlan
* `ip address *ip-address* *subnet-mask*` - this address typically serves as the default gateway for the associated VLAN

if the acutal interface is disabled so are all it's subinterfaces

### 4.2.6 Router-on-a-Stick Inter-VLAN Routing Verification
* `show ip route` - to verify subinterfaces are appearing in the routing table
* `show ip interface brief` - confirm subinterfaces have correct IPv4 addresses
* `show interfaces *subinterface-id*`
* `show interfaces trunk` - shows interfaces trunking status


## 4.3 Inter-VLAN Routing Using Layer 3 Switches
### 4.3.1 Layer 3 Switch Inter-VLAN Routing
layer 3 switches use hardware-based switching & have higher packet processing rates than routers.
They can:
* route from one vlan to another using SVIs
* convert a layer 2 switchport to a layer 3 interface (routed port)

### 4.3.2 Layer 3 Switch Scenario
### 4.3.3 Layer 3 Switch Configuration
1. create the vlans (see 3.3.2)
2. create the svi vlan interfaces:
	* `interface vlan 10` - then set ip address & description & turn on via no shutdown
3. configure access ports (the ports the end devices are connected to):
	* `switchport mode access`
	* `switchport access vlan`
4. enable IP routing - from global config enter --> `ip routing`


### 4.3.4 Layer 3 Switch Inter-VLAN Routing VERIFICATION
verify connectivity using *ping* from a pc & verify that hosts config via `ipconfig`

### 4.3.5 Routing on a layer 3 Switch
for VLANs to be reachable by other layer 3 devices they must be advertised via static or dynamic routing.
create a routed port by disabling the switchport on a layer 2 port that is connected to another layer 3 device. `no switchport`

### 4.3.7 Routing Configuration on a Layer 3 Switch
1. configure the routed port:
	* `interface g1/0/1` then set description
	* `no switchport`
	* `ip address *ip-address*`
	* `no shutdown`
2. Enable Routing - `ip routing` (from global config)
3. Configure Routing (configure OSPF): from global config
	* `router ospf 10`
	* `network 192.168.10.0 0.0.0.255 area 0` - tells open shortest path first (ospf) to advertise on that network
4. verify routing - `show ip route | begin Gateway` - to verify routing table
5. & very connectivity



## 4.4 Troubleshoot Inter-VLAN Routing
### 4.4.1 Common Inter-VLAN issues
issue & helpful commands
* missing vlans - `show vlan brief` `show interfaces switchport`
* switch trunk port issues - `show interfaces trunk` `show running-config`
* switch access port issues - `show interfaces switchport` `show running-config interface` `ipconfig`
* router config issues - `show ip interface brief` `show interfaces`

### Troubleshoot inter-vlan routing scenario