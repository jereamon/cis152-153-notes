Module 8 - SLAAC & DHCPv6

# 8.1 IPv6 GUA Assignment
## 8.1.1 - IPv6 Host Configuration
`ipv6 address *ipv6-address/prefix-length` - to manually configure an IPv6 address on a router

## 8.1.2 - IPv6 Host Link-Local Address
## 8.1.3 - IPv6 GUA Assignment
Dynamic GUA Assignment includes:
* Stateless (no device tracks the IPv6 addr assignment)
	* SLAAC Only - router sends RA messages & provides all IPv6 addressing info. Hosts use the RA info exclusively for their addressing.
	* SLAAC with DHCP Server (Stateless DHCPv6) - RA messages provide IPv6 info & inform them to contact a stateless DHCPv6 server for additional info.
* Stateful (DHCPv6 server manages IPv6 addr assignment)
	* DHCPv6 Server (Stateful DHCPv6) - RA messages tell host to contact stateful DHCPv6 server for all IPv6 config info except the **default gateway**. The RA provides the default gateway addr.


## 8.1.4 - Three RA Message Flags
ICMPv6 includes 3 flags:
* A flag - Address Autoconfiguration flag. Use SLAAC to create an IPv6 GUA
* O Flag - Other Configuration flag. Other info is available from stateless DHCPv6 server.
* M Flag - Managed Address Configuration flag. Use a stateful DHCPv6 server to obtain IPv6 GUA.



# 8.2 - SLAAC
## 8.2.1 - SLAAC Overview
## 8.2.2 - Enabling SLAAC
* `show ipv6 interface *interface-id*` - to verify a router port has the proper ipv6 addr
* `ipv6 unicast-routing` - from global config to enable IPv6 routing
* `show ipv6 interface *interface-id* | section Joined` - to verify SLAAC is enabled

## 8.2.3 - SLAAC only method
SLAAC only is enabled by default when `ipv6 unicast-routing` is configured.
all enabled ethernet interfaces will begin sending RA messages with A flag set to 1 & O=0, M=0. Flag meanings above ^^.


## 8.2.4 - ICMPv6 RS (Router Solicitation) Messages
routers send RA messages every 200 seconds. They will also send an RA if they receive an RS from a host.

If configured to automatically obtain addressing information hosts send RS messages to the IPv6 all-routers multicast addr: **ff02::2**
Routers that receive an RS will respond to the IPv6 all-nodes multicast address of **ff02::1**

## 8.2.5 - Host Process to Generate Interface ID
SLAAC provides the 64-bit IPv6 subnet info but the host must generate the other 64 bit in one of two ways:
1. Randomly generated
2. EUI 64 - uses MAC addr. The host inserts fffe into the middle of the MAC & flips the 7th bit of the interface ID.

## 8.2.6 Duplicate Address Detection (DAD)
DAD is implemented using ICMPv6. The host sends a Neighbor Solicitation message with a solicited-node multicast address which duplicates the last 24 bits of the hosts IPv6 address. If no other device responds with an NA message the addr is expected to be unique.

IETF recommends DAD is used on all IPv6 unicast addresses.


# 8.3 - DHVPv6
## 8.3.1 - DHCPv6 Operation Steps
DCHPv6 is defined in RFC 3315
DHCP clients use **UDP port 547** to send DHCPv6 messages

Steps for DHCPv6 operation:
1. Host sends an RS message
2. router responds with RA message
3. host sends a DHCPv6 SOLICIT message
4. DHCPv6 server responds with an ADVERTISE message
5. host responds to the DHCPv6 server
	* if Stateless DHCPv6 host sends a DHCPv6 INFORMATION-REQUEST message
	* if statefull, host sends DHCPv6 REQUEST message
7. DHCPv6 server sends a REPLY message

## 8.3.2 - Stateless DHCPv6 Operation
Stateless DHCP RA message flags will be: M=0, A=1, O=1
M specifies if stateful DHCP, A tells client to use SLAAC, O tells client additional info is available from stateless DHCP server.

## 8.3.3 - Enable Stateless DHCPv6 on an Interface
from interface config: `ipv6 nd other-config-flag` put `no` in front to disable it
to verify configuration: `show ipv6 interface *interface-id* | begin ND`

## 8.3.4 - Stateful DHCPv6 Operation
All addressing information comes from DHCP server except for the default gateway address
DHCP RA message flags will be: O=0, M=1, A=0 (see flag meaning above in 8.3.2)

## 8.3.5 - Enable Stateful DHCPv6 on an Interface
from interface config: `ipv6 nd managed-config-flag` then `ipv6 nd prefix default no-autoconfig`


# 8.4 - Configure DHCPv6 Server
## 8.4.1 - DHCPv6 Router Roles
Cisco IOS Routers can be configured to provide DHCPv6 Services:
* DHCPv6 Server - Router provides stateless or statefull DHCPv6 services.
* DHCPv6 Client - Router interface acquires an IPv6 IP configuration from a DHCPv6 server.
* DHCPv6 Relay Agent - Router provides DHCPv6 forwarding services when the client and the server are located on different networks

## 8.4.2 - Configure a Stateless DHCPv6 Server
Five steps (from global config):
1. `ipv6 unicast-routing` - Enable ipv6 routing (required for router to send ICMPv6 RA messages)
2. `ipv6 dhcp pool *POOL-NAME*` - Define a DHCPv6 pool name
3. `dns-server *ipv6-address-of-dns-server*` then `domain-name *example.com*` - configure the DHCPv6 pool
4. Then configure an interface and bind the IPv6 Pool to it:
	1. `interface gig0/0/1`
	2. `ipv6 address fe80::1 link-local`
	3. `ipv6 address *addr/prefix*`
	4. `ipv6 nd other-config-flag` **<---** changes O flag to 0
	5. `ipv6 dhcp server *POOL-NAME*` **<--** binds the dhcp server to the pool
	6. `no shutdown`
5. `ipconfig` - on a host to verify it received IPv6 addressing info

## 8.4.3 - Configure a Stateless DHCPv6 Client
Five steps to configure:
1. `ipv6 unicast-routing` - enable IPv6 routing
2. `interface g0/0/1` then `ipv6 enable` - tells interface to create an LLA
3. `ipv6 address autoconfig` - router will be configured to use SLAAC
4. `show ipv6 interface brief` - to verify it has been assigned a GUA
5. `show ipv6 dhcp interface g0/0/1` - to verify it received other DHCPv6 information

## 8.4.4 - Configure a Stateful DHCPv6 Server
Five steps:
1. `ipv6 unicast-routing` - enable ipv6 routing
2. `ipv6 dhcp pool *POOL-NAME` - define a DHCPv6 Pool Name
3. Configure the Pool
	1. `address prefix 2001:db8:acad:1::/64` - indicates the pool address to be allocated by the server
	2. `dns-server *ipv6-addr-of-dns-server*`
4. Bind the pool to an interface:
	1. `interface g0/0/1`
	2. `ipv6 address fe80::1 link-local`
	3. `ipv6 address *ipv6-addr/prefix*`
	4. `ipv6 nd managed-config-flag` **<--** sets M flag to 1
	5. `ipv6 nd prefix default no-autoconfig` - **<--** Sets A flag to 0. A flag could be left as 1 but setting it to 0 tells client not to use SLAAC. 
	6. `ipv6 dhcp server *POOL-NAME*` **<--** binds the pool to the interface
	7. `no shutdown`
5. `ipconfig /a` - verify on host that it received IPv6 Info

## 8.4.5 - Configure a Stateful DHCPv6 Client
1. `ipv6 unicast-routing` - enable ipv6 routing
2. `interface g0/0/1` then `ipv6 enable` - enables router to create LLA without needing a GUA
3. `ipv6 addr dhcp` - tells router to get IPv6 addr info from DHCPv6 server
4. `show ipv6 interface brief` - verify client has been assigned a GUA
5. `show ipv6 dhcp interface g0/0/1` - verify other DHCPv6 info arrived

## 8.4.6 - DHCPv6 Server Verification Commands
* `show ipv6 dhcp pool` - to verify DHCPv6 pool name & its parameters. Also identifies # of active clients.
* `show ipv6 dhcp binding` - displays IPv6 LLA and GUA assigned by the server

## 8.4.7 - Configure a DHCPv6 Relay Agent
Configure the incoming port that has hosts which need DHCP info. The IPv6 addr is that of the DHCP server and the interface type & number are that of the interface connected to the DHCP server.
From interface config: `ipv6 dhcp relay destination *ipv6-addr* *interface-type interface-number*`
example: `ipv6 dhcp relay destination 2001:db8:acad:1::2 G0/0/0`

## 8.4.8 - Verify the DHCPv6 Relay Agent
Verify DHCPv6 relay agent is operational:
* `show ipv6 dhcp interface` - will tell you if the interface is in relay mode
* `show ipv6 dhcp binding` - on the server to see if any hosts have been assigned IPv6 config
* `ipconfig /a` - on a host to verify it received IPv6 info

