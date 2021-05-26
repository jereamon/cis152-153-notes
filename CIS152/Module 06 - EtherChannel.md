Module 6 - EtherChannel

# 6.1 EtherChannel Operation
## 6.1.1 Link Aggregation
Ether channel - allows for redundant links between devices that will not be blocked by STP.

EtherChannel link aggregation groups multiple physical ethernet links into one logical link. It provides fault-tolerance, load sharing, increased bandwidth, & redundancy between switches, routers, & servers.

### 6.1.2 EtherChannel
when an etherchannel is configured the resulting virtual interface is called a port channel.

## 6.1.3 Advantages of EtherChannel
* most configuration can be done on the etherchannel interface instead of the individual port.
* they rely on existing switch ports. no need to upgrade the link to faster & more expensive connection
* load balancing options include:
	* source MAC & destination MAC
	* source IP & destination IP
* when several etherchannel bundles exist between two switches STP may block one of the redundant links
* provides redundancy. the loss of one physical link within the channel does not create a change in topology. 

## 6.1.4 Implementation Restrictions
* interface types cannot be mixed. Fast Ethernet & Gigabit Ethernet cannot be used together
* can consist of up to 8 ports. provides full-duplex bandwidth up to 800Mbps or 8Gbps.
* catalyst 2960 supports up to six EtherChannels
* individual EtherChannel group member port config must be consistent on both devices
* each etherchannel has a logical port interface. changes made to this logical inteface affect all physical ports associated with it.

### 6.1.5 AutoNegotiation Protocols
etherchannels can be formed through one of two protocols:
* Port Aggregation Protocol (PAgP)
* Link Aggregation Control Protocol (LACP)

## 6.1.6 PAgP Operation
PAgP is Cisco proprietary. When enabled PAgP packets are sent every 30 seconds to check for configuration consistency and manage link additions & failures.

PAgP has the following modes:
* On - this forces the interface to channel without PAgP. On interfaces do not exchange PGaP packets. works only if the other side is set to on.
* PAgP Desirable - active negotiating state, it initiates negotiations with other interfaces by sending PAgP packets.
* PAgP Auto - passive negotiating state, it responds to PAgP packets but doesn't send them.

if both sides are set to auto no negotiation will begin.

### 6.1.7 PAgP Mode Settings Example
imagine two switches with a redundant connection
| S1 | S2 | Channel Establishment |
| --- | --- | --- |
| On | On | Yes |
| On | Desirable/Auto | No |
| Desirable | Desirable | Yes |
| Desirable | Auto | Yes |
| Auto | Desirable | Yes |
| Auto | Auto | No |


## 6.1.8 - LACP Operation
part of IEEE specification 802.3ad. performs a similar function to PAgP but can be used in multivendor environments.

LACP Modes:
* On - same as PAgP on
* LACP Active - same as PAgP desirable
* LACP passive - same as PAgP auto

### 6.1.9 LACP Mode Settings Example
same as the PAgP table above


# 6.2 Configure EtherChannel
## 6.2.1 Configuration Guidelines
guidelines & restrictions for an EtherChannel bundle:
* EtherChannel Support - all interfaces must support EtherChannel
* Speed & Duplex - all interfaces must use same speed & duplex
* VLAN Match - all interfaces must be assigned to the same VLAN or configured as a trunk
* Range of VLANs - must have the same range of allowed VLANs

## 6.2.2. LACP Configuration Example
3 steps:
1. specify the interfaces that compose the EtherChannel group using `interface range *interface*` from global config
2. create the port channel interface with `channel-group *identifier* mode active` from the interface range config. the `mode` & `active` keywords identify it as LACP
3. to change port channel interface settings use `interface port-channel *interface-identifier*`


# 6.3 Verify & Troubleshoot EtherChannel
## 6.3.1 Verify EtherChannel
commands to verify EtherChannel configuration:
* `show interfaces port-channel *channel_num*` - displays general status of the port channel interface
* `show etherchannel summary` - shows info about all configured etherchannels
* `show etherchannel port-channel` - shows info about a specific port channel interface
* `show interfaces *interface_id* etherchannel` - provides info about the role of the interface in the etherchannel

### 6.3.2 Common Issues with EtherChannel Configurations
* assigned ports in an EtherChannel are not part of the same VLAN
* trunking was configured on some ports but not all. **recommended** that you configure trunking mode on each individual port in the etherchannel.
* allowed range of VLANs is not the same
* dynamic negotiation options for PAgP & LACP are not compatibly configured

## 6.3.3 Troubleshoot EtherChannel Example
1. View The EtherChannel Summary Information - `show etherchannel summary`
2. View the Port Channel Configuration - `show run | begin interface port-channel`
3. Correct the misconfiguration
4. Verify the EtherChannel is Operational - `show etherchannel summary`