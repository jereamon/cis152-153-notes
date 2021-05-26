Module 6 - NAT for IPv4

# 6.1 - NAT Characteristics
## 6.1.1 - IPv4 Private Address Space
private ip addresses:
| class | RFC 1918 Internal Address Range | Prefix |
| --- | --- | --- |
| A | 10.0.0.0 - 10.255.255.255 | 10.0.0.0/8 |
| B | 172.16.0.0 - 172.31.255.255 | 172.16.0.0/12 |
| C | 192.168.0.0 - 192.168.255.255 | 192.168.0.0/26 |

NAT provides translation from private addrs to public addrs
Without NAT IPv4 addr space would have been exhausted well before the year 2000.


## 6.1.2 - What is NAT
primary use is to convserve public IPv4 addrs.

Routers can be configured with 1 or more valid public IPv4 addrs known as a NAT pool. Internal traffic to be routed off the network is translated to one of these addrs.

a NAT router typically operates at the edge of a stub network.

## 6.1.3 - How NAT works
## 6.1.4 - NAT terminology
inside network -- set of networks subject to translation

NAT includes 4 types of addrs:
* inside local - address of the source as seen from inside the network (private addr). a private IP address the company/customer can control.
* inside global - address of the source as seen from outside the network (public addr). a globally routable address that is under the company/customer's control
* outside local - addr of the destination seen from inside the network (private addr). Private IP address outside of the company/customer's control (on a remote network they don't own).
* outside global - addr of the destination seen from outside the network (public addr). A globally routable adress located on a remote network outside the company/customer's control.

**inside address** -- addr of the device which is being translated by NAT. under control of the company or customer. Residing inside the network
**outside address** -- addr of the destination device. customer or company can't control. Resides outside the customer network
**local address** -- any addr appearing on the inside portion of the network. Private IP addresses
**global address** -- any addr appearing on the outside portion of the network. public IP addresses which are globally routable


# 6.2 - Types of NAT
## 6.2.1 - Static NAT
uses one-to-one mapping of local & global addresses. They are configured by an admin and remain constant.

useful for web servers or devices that must have a consistent address that is accessible from the internet.


## 6.2.2 - Dynamic NAT
uses a pool of public addresses & assigns them on a first come first serve basis.

**static and dynamic NAT both require** that enough public addrs are available for each user session to have its own.

## 6.2.3 - Port Address Translation (PAT)
aka NAT overload -- maps multiple private IPv4 addrs to a single public IPv4 addr or a few public addrs.

Each private address is also tracked by a port number. The NAT router uses the TCP or UDP port number or the ICMP query ID to uniquely identify the session.

## 6.2.4 - Next Available Port
PAT attempts to preserve the original source port, but, if that port is already used, PAT assigns the first available port number starting from the beginning of the appropriate port group (0-511, 512-1023, 1024-65,535). When the ports run out PAT moves the next addr to try to allocate the original source port.


## 6.2.5 - NAT and PAT Comparison
| NAT | PAT |
| --- | --- |
| one-to-one mapping between inside local & inside global addrs | one inside global addr mapped to many inside local addrs |
| uses only IPv4 addrs in translation | uses IPv4 addr & TCP or UDP source port number |
| a unique inside global addr is required for each session | a single inside global addr is shared by many sessions |

## 6.2.6 - Packets without a Layer 4 Segment
PAT translates most common protocols carried by IPv4, not just TCP & UDP.
example: ICMPv4


# 6.3 - NAT Advantages and Disadvantages
## 6.3.1 - Advantages of NAT
NAT benefits:
* conserves public ip addrs
* increases flexibility of connections to the public network.
* provides consistency for internal network addressing schemes
* using RFC 1918 IPv4 addrs, NAT hides the addrs of users & other devices

## 6.3.2 - Disadvantages of NAT
* NAT increases delays for real time protocols like VoIP. It has to look at every packet to determine if it needs translation and if it does the IPv4 header & possibly TCP or UDP headers must be modified and the checksum recalculated
* end to end addressing is lost. some protocols and applications require this to work.
* end to end IPv4 traceability is lost making it hard to trace packets that undergo many hops for troubleshooting.
* complicates the use of tunneling protocols


# 6.4 - Static NAT
## 6.4.1 - Static NAT Scenario
## 6.4.2 - Configure Static NAT
2 basic tasks when configuring static NAT translations:
1. create mapping between the inside local & inside global addresses
	* `ip nat inside source static 192.168.10.254 209.165.201.5`
2. configure participating interfaces a inside or outside relative to NAT.
	* `interface s0/1/0`
	* `ip address 192.168.1.2 255.255.255.252` <-- the inside local address
	* `ip nat inside`
	*
	* `interface s0/1/1`
	* `ip address 209.165.200.1 255.255.255.252` <-- the outside local address
	* `ip nat outside`


## 6.4.3 - Analyze Static NAT
## 6.4.4 - Verify Static NAT
`show ip nat translations` -- this shows active NAT translations. if a translation is static it will always show there.

`clear ip nat statistics` -- to clear statistics before viewing them
`show ip nat statistics` -- shows info about total number of active translations, NAT config parameters, number of addrs in the pool, & number of addrs that have been allocated


# 6.5 - Dynamic NAT
## 6.5.1 - Dynamic NAT Scenario
dynamic NAT automatically maps addresses between inside local and inside global

## 6.5.2 - Configure Dyanmic NAT
1. `ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224`
	* define the pool of public addresses that will be used. netmask indicates which address bits belong to the network & which belong to the host
2. `access-list 1 permit 192.168.0.0 0.0.255.255`
	* configure a standard ACL to permit only those addresses that are to be translated
3. `ip nat inside source list 1 pool NAT-POOL1`
	* bind the ACL to the pool
4. `ip nat inside` <-- from interface config
	* identify which interfaces are inside, in relation to NAT
5. `ip nat outside` <-- from interface config
	* identify which interfaces are outside, in relation to NAT

## 6.5.3 - Analyze Dynamic NAT - Inside to Outside
an inside host sends packets with a destination outside the network. The router receives these packets on an interface configured as an inside NAT interface. The router checks the NAT table and if there is no current translation for the inside local source address it gets mapped to an available inside global address & the packet is forwarded on with that new address replacing the old one.

## 6.5.4 - Analyze Dynamic NAT - Outside to Inside
A device on a remote network receives a packet and responds to the source address on that packet. When the router recieves that response it looks in its NAT table to translate the inside global address to the inside local address and forwards the packet to the local device.

## 6.5.5 - Verify Dynamic NAT
`show ip nat translations` - displays all static translations that have been configured & any dynamic translations that have been created
`show ip nat translation verbose` - shows more info including how long ago the entry was created & used

`clear ip nat translation *` - clears all translations. this command accepts arguments too though
| Comand | Description|
| --- | --- |
| `clear ip nat translation inside global-ip local-ip [outside local-ip global-ip]` | clears simple dynamic translation entry containing an inside translation or both inside & outside translation |
| `clear ip nat translation protocol inside global-ip global-port local-ip local-port [outside local-ip local-port global-ip global-port]` | clears an extended dynamic translation entry |

`show ip nat statistiics` shows info about total active translations, NAT config parameters, number of addrs in the pool, & how many addrs have been allocated.

`show running-config | include NAT`


# 6.6 - PAT (port address translation [NAT overload])
## 6.6.1 - PAT Scenario
2 ways to configure PAT, 1. you have one public IP address, 2. You have more than one public IP address
.WAN Fundamentals.Technology
Max Points: 10

Earned Points: 3

Percentage: 30.0%
## 6.6.2 - Configure PAT to use a single IPv4 Address
Since it won't be bound to a NAT pool the the `ip nat inside source` command is slightly different. Instead of specifying the pool name **specify the interface** through which traffic flow will need to be translated
also, add `overload` to the end:
`ip nat inside source list 1 interface s0/1/0 overload`

then follow the dynamic nat steps (**6.5.2**). set up an ACL permitting hosts to be translated, set interfaces to be inside/outside, etc.

## 6.6.3 - Configure PAT to use an Address pool
configure the address pool as seen in **6.5.2**
then, when binding the address pool to the previously defined ACL using `ip nat inside source etc.` add `overload` at the end.


## 6.6.6 - Verify PAT
`show ip nat translations`
`show ip nat statistics`


# 6.7 - NAT64
## 6.7.1 - NAT for IPv6
ipv6 has 340undecillion fucking addresses. It says fuck nat, but it also plays nice and does include its own IPv6 private address space, unique local addresses (ULAs).

ULAs are only meant for local communications within a site.

## 6.7.2 - NAT64
NAT for IPv6 is meant to provide access between IPv6-only & IPv4-only networks. It isn't for private IPv6 to global IPv6 translation.

There have been several NAT for IPv6 types over the years:
* Network address translation protocol translation (NAT-PT) <-- deprecated for NAT64
* NAT64