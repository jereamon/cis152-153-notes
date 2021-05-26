Module 4 - ACL Concepts

# 4.1 - Purpose of ACLs
## 4.1.1 - What is an ACL?
ACL - a series of IOS commands used to filter packet based on info found in the packet header

by default routers have no ACL configured. When one is applied to an interface all packets are inspected as they pass through.

ACL uses access control entries (ACEs)
**packet filtering** - packet data is compared against each ACE sequentially

## 4.1.2 - Packet Filtering
packet filtering can occur at layer 3 or 4 (network or transport) based on the criteria

Cisco routers support two types of ACLs:
* Standard ACLs - only filter at Layer 3 using source IPv4 addr only
* Extended ACLs - filter at layer 3 using source & or destination IPv4 addr and can filter at Layer 4 using TCP, UDP ports, & optional protocol type information for finer control


## 4.1.3 - ACL Operation
ACLs define rules that give control for inbound interfaces, packets being relayed through the router, and outbound packets. They don't act on packets they originate from the router itself.

* **Inbound ACL** filters packets before they are routed to the outbound interface. This is efficient. Best used when the network attached to an inbound interface is the only source of packets that need to be examined.
* **Outbound ACL** filters packets after they're routed, regardless of the inbound interface. Best used when the same filter will be applied to packet coming from multiple inbound interfaces before exiting the same outbound interface.

Procedure followed when an ACL is applied to an interface:
1. router extracts the source IPv4 address from the packet header
2. From the top, the router compares the source IP addr to each ACE in the ACL in sequential order
3. When a match is made, the router either permits or denies the packet depending on the instruction, & remaining ACEs in the ACL are not analyzed.
4. if the source IP addr doesn't match any ACE the packet is discarded.


# 4.2 - Wildcard Masks in ACLs
## 4.2.1 - Wildcard Mask Overview
ACL uses wildcard masks like the ones used in OSPF. A 32 it mask.
Same thing uses the ANDing process but match 0s instead of 1s.

**EXAMPLES TO UNDERSTAND WILDCARD MASKS**
**Example 1**
specified ip addr: 192.168.1.0
wildcard mask:     0  .0  .0.255
affects all addrs: 192.168.1.0 through 192.168.1.255
so, it has to match the first 24 bits (192.168.1) and then anything after that counts as a match, doesn't care what it is.

**Example 2**
specified ip addr: 10.0.1.20
wildcard mask:     0 .0.0.31 (00000000.00000000.00000000.00011111)
affects all addrs: 10.0.1.20 through 10.0.1.31


specified IP addr: 192.168.16.0   (11000000.10101000.00010000.00000000)
wildcard mask:     0  .0  .7 .255 (00000000.00000000.00000111.11111111)
affects all addrs: 192.168.16.0 through 192.168.23.255


Wildcard masks & what they signify:
| Wildcard mask | last Octet (in binary) | Meaning(0 - match, 1 - ignore) |
| --- | --- | --- |
| 0.0.0.0 | 00000000 | match all octets |
| 0.0.0.63 | 00111111 | match first 3 octets, match two left most bits of last octet, ignore last 6 bits |
| 0.0.0.15 | 00001111 | match 1st 3 octets, match 4 left most bits of last octet, ignore last 4 bits of last octet |
| 0.0.0.252 | 11111100 | match 1st 3 octets, ignore six left most bits of last octect, match last 2 bits |
| 0.0.0.255 | 11111111 | match 1st 3 octets, ignore last octet |


## 4.2.2 - Wildcard Mask Types
* Wildcard to match a host - to match a specific host IPv4 addr the wildcard mask must be 0.0.0.0. This means every bit has to match. So the table entry would look like:
	* IPv4 Address: 192.168.1.1
	* Wildcard Mask: 0.0.0.0
	* ACE: `access-list 10 permit 192.168.1.1 0.0.0.0`
* Wildcard Mask to Match an IPv4 Subnet - example 2, an ACE that permits all hosts in the 192.168.1.0/24 network.
	* IPv4 Address: 192.168.1.1
	* Wildcard Mask: 0.0.0.255
	* Permitted IPv4 addrs: 192.168.1.1 through 192.168.1.254
	* ACE: `access-list 10 permit 192.168.1.0 0.0.0.255`
* Wildcard Mask to Match an IPv4 Address Range - example 3, ACE that permits all hosts in 192.168.17.0/24, ..., 192.168.31.0/24,
	* IPv4 Address: 192.168.16.0
	* Wildcard Mask: 0.0.15.255
	* permitted IPv4 addrs: 192.168.16.1 to 192.168.31.254
	* ACE: `access-list 10 permit 192.168.16.0 0.0.15.255`

## 4.2.3 - Wildcard Mask Calculation
one shortcut to calculate wildcard mask is to subtract the subnet mask from 255.255.255.255

Calc wildcard mask examples:
* we want an ACE in ACL 10 to permit all users in the 192.168.3.0/24 network
	*   255.255.255.255
	* - 255.255.255.0
	* = 0.0.0.255 <-- resulting wildcard mask
* we want an ACE in ACL 10 to permit access for 14 users of 192.168.3.32/28 subnet
	*   255.255.255.255
	* - 255.255.255.240
	* = 0.0.0.15
* ACE in ACL 10 to permit networks 192.168.10.0 & 192.168.11.0. These two could be summarized as 192.168.10.0/23
	*   255.255.255.255
	* - 255.255.254.0
	* = 0.0.1.255
* ACE in ACL 10 to match networks 192.168.16.0/24 to 192.168.31.0/24. this could be summarized as 192.168.16.0/20
	*   255.255.255.255
	* - 255.255.240.0
	* = 0.0.15.255

## 4.2.4 - Wildcard Mask Keywords
Since working with binary wildcard masks can be tedious Cisco IOS provides 2 keywords to identify their most common uses:
* **host** - substitutes for 0.0.0.0 mask & states that all IPv4 address bits must match to filter just one host address
* **any** - substitutes for 255.255.255.255 mask & says to ignore the entire IPv4 address or accept any addresses.


# 4.3 - Guidelines for ACL Creation
## 4.3.1 - Limited Number of ACLs per interface
an interface with IPv4 & IPv6 can have 4 ACLs:
* one inbound IPv4
* one inbound IPv6
* one outbound IPv4
* one outbound IPv6

## 4.3.2 - ACL Best Practices
Planning is required before implementing ACLs because a misconfiguration could result in extended downtime, troubleshooting, and poor network performance.

Guidelines:
| Guidelines | Benefit |
| --- | --- |
| Base ACLs on the organizational security policies | this ensures you implement organizational security guidelines |
| Write out what you want the ACL to do | helps avoid creating access problems |
| use a text editor to create, edit, & save all of your ACLs | helps create a library of reusable ACLs |
| Document the ACLs using `remark` command | helps you & others understand an ACLs purpose |
| test ACLs on a development network before pushing to production | helps avoid costly errors |


# 4.4 - Types of IPv4 ACLs
## 4.4.1 - Standard and Extended ACLs
2 types of IPv4 ACLs:
* Standard ACLs - permit or deny packets based only on source IPv4 addr
* Extended ACLs - permit or deny packets based on source IPv4 addr & dest IPv4 addr, protocol type, source & destination TCP or UDP ports & more

**ACL command examples** (from global config?):
**STANDARD ACL**
`access-list 10 permit 192.168.10.0 0.0.0.255` <-- this blocks all traffic from 192.168.10.0/24 network

**EXTENDED ACL**
`access-list 100 permit tcp 192.168.10.0 0.0.0.255 any eq www` <-- permits traffic from any host on 192.168.10.0/24 network to any IPv4 network if the destination host port is 80 (http) 


## 4.4.2 - Numbered and Named ACLs
**Numbered ACLs**
* standard ACLs - 1 - 99 and 1300 - 1999
* extended ACLs - 100 - 199 and 2000 - 2699

view this using:
`access-list ?`

**Named ACLs**
this is the preferred method.

`ip access-list` - global config command is used to create a named ACL
example: `ip access-list extended FTP-FILTER` <-- specifies an extended ACL named FTP-FILTER and takes you to access-list config mode:
`R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq ftp` <-- an ACE for FTP-FILTER

**RULES for** named ACLs:
* assign a name to identify the purpose of the ACL
* names can contain alphanumeric characters
* names cannot contain spaces or punctuation
* it is suggested that the name be written in CAPITAL LETTERS
* entries can be added or deleted within the ACL

## 4.3.3 - Where to Place ACLs
every ACL should be placed where it has the greatest impact on efficiency.

ACL placement guidelines:
* extended ACLs should be located as close to the source of the traffic to be filtered as possible.
* standard ACLs should be located as close to the destination as possible.

Other factors influencing ACL placement:
| Factor | Explanation |
| --- | --- |
| extent of organizational control | placement can depend on whether the organization has control of the source & desination networks |
| bandwidth of the networks involved | may want to filter unwanted traffic at the source, preventing transmission of bandwidth consuming traffic |
| ease of configuration | maybe easier to implement ACL at destination but badwidth will be used unnecessarily. An extended ACL could be used on each router where traffic originates, saving bandwidth, but requiring additional configuration. |


## 4.4.4 - Standard ACL Placement Example
Following above guidelines, when placing a standard ACL as close to the destination as possible there are still options.

an ACL on a router routing to a destination could be applied as an inbound rule on the interface coming into the router or as an outbound rule on the interface going to the destination network.

The first option would block traffic regardless of its destination while the second would only block traffic headed for a specific destination network.

## 4.4.5 - Extended ACL Placement Example
Following above guidelines, extended ACLs would ideally be placed on the first router unwanted traffic leaves through. This will depend on which devices the organization controls.

Always consider whether an ACL should be placed as an inbound or outbound rule on one interface vs another.


## Verify ACL Stuff
`show ip interface`
`show run` or more specifically `show run | include interface|access`


**remove ACL from interface**:
`no access-list acl-number` <-- from global config to remove an ACL from the router entirely

`no ip access-group acl-number in/out` <-- from interface config to remove that interface from an ACL group
