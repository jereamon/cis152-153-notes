Module 15 - IP Static Routing

# 15.1 - Static Routes
## 15.1.1 - Types of Static Routes
IPv4 and IPv6 support:
* Standard static route
* default static route
* floating static route
* summary static route

static routes are configured using:
* `ip route`
* `ipv6 route`

## 15.1.2 - Next-Hop Options
next hop can be identified by an IP addr, exit interface, or both

* Next-Hop route - only next hop IP addr is specified
* Directly connected static route - only router exit interface is specified
* Fully specified static route - next-hop ip addr & exit interface are specified


## 15.1.3 - IPv4 Static Route Command
Configured from global config using:
`ip route *network-addr* *subnet-mask* {ip-addr | exit-intf [ip-addr]} [distance]`
either *ip-addr, exit-intf*, or *ip-addr* and *exit-intf* must be configured

| Parameter | Description |
| --- | --- |
| network-address | identifies dest. IPv4 network addr of the remote network to add to the routing table |
| subnet-mask | identifies subnet mask of remote network. Can be modified to summarized a group of netoworks |
| ip-address | identifies next-hop router ipv4 addr. Typically used with broadcast networks. Could create a recursive static route where the router performs an additional lookup to find the exit interface. |
| exit-intf | identifies exit interface to forward packets. Creates directly connected static route. Typically used in point-to-point config. |
| exit-intf ip-address | creates a fully specified route |
| distance | optional. can be used to assign administrative distance value between 1 & 255. |

## 15.1.4 - IPv6 Static Route Command
`ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-intf [ipv6-address]} [distance]`

| Parameter | Description |
| --- | --- |
| ipv6-prefix | identifies dest. IPv6 network addr of the remote network to add to the routing table |
| /prefix-length | identifies the prefix length of the remote network |
| ipv6-address | identifies next-hop router IPv6 addr. typically used w/broadcast networks. router must perform a recursive route table lookup to find an exit interface. |
| exit-intf | identifies exit interface to forward packets. typically used w/point-to-point networks |
| exit-intf ipv6-address | creates fully specified route |
| distance | same as ipv4 distance |


## 15.1.5 - Dual-Stack Topology
## 15.1.6 - IPv4 Starting Routing Tables


# 15.2 - Configure IP Static Routes
## 15.2.1 - IPv4 Next-Hop Static Route
remote ip addr is specified
`ip route 172.16.1.0 255.255.255.0 172.16.2.2`
first ip is the remote network addr, then subnet mask, then the interface ip addr on the next-hop router.

## 15.2.2 - IPv6 Next-Hop Static Route
`ipv6 unicast-routing`
`ipv6 route 2001:db8:acad:1::/64 2001:db8:acad:2::2`

## 15.2.3 - IPv4 Directly Connected Static Route
exit interface is specified
`ip route 172.16.1.0 255.255.255.0 s0/1/0`

## 15.2.4 - IPv6 Directly Connected Static Route
`ipv6 route 2001:db8:acad:1::/64 s0/1/0`


## 15.2.5 - IPv4 Fully Specified Static Route
ip addr and exit interface are specified
`ip route 172.16.1.0 255.255.255.0 GigabitEthernet 0/0/1 172.16.2.2`


## 15.2.6 - IPv6 Fully Specified Static Route
`ipv6 route 2001:db8:acad:1::/64 s0/1/0 fe80::2`
if using a link-local addr then a fully qualified static route must be declared.
This is because that link-local addr could be valid on multiple networks so the exit interface must be specified as well.

## 15.2.7 - Verify a Static Route
* `show ip route static`
* `show ip route *network*`
* `show running-config | section ip route`

replace ip with ipv6 for ipv6 versions


## 15.3 - Configure IP Default Static Routes
## 15.3.1 - Default Static Route
default static route is similar to any other IPv4 static route except its network address is 0.0.0.0 and the subnet mask is 0.0.0.0
`ip route 0.0.0.0 0.0.0.0 {ip-addr | exit-intf}`

`ipv6 route ::/0 {ipv6-addr | exit-intf}`


## 15.3.2 - Configure a Default Static Route
## 15.3.3 - Verify a Default Static Route
`show ip route static`
`show ipv6 route static`



# 15.4 - Configure Floating Static Routes
## 15.4.1 - Floating Static Routes
Floating static routes provide a backup path to a primary static or dynamic route in the event of a link failure.

## 15.4.2 - Configure IPv4 and IPv6 Floating Static Routes
`ip route 0.0.0.0 0.0.0.0 10.10.10.2 5`
the 5 at the end increases the administrative distance so this route won't be the first choice

`ipv6 route ::/0 2001:db8:fee:10::2 5`


## 15.4.3 - Test the Floating Static Route



# 15.5 - Configure Static Host Routes
## 15.5.1 - Host Routes
host routes can be added to the routing table:
* Automatically installed when an IP addr is configured on the router
* configured as a static host route
* host route automatically obtained through other methods

## 15.5.2 - Automatically Installed Host Routes
Cisco IOS automatically installs a host route (aka a Local Route) when an interface addr is configured on the router.

Local route marked with **L** in the routing table
in addition to the **C** designation which shows the network addr of the interface

## 15.5.3 - Static Host Routes
manually configured host route
## 15.5.4 - Configure Static Host Routes
always uses /32 mask (255.255.255.255) for IPv4 and /128 prefix for IPv6
first ip addr is that of the remote host. Then subnet mask. Then ip addr of next hop device
`ip route 209.165.200.238 255.255.255.255 198.52.100.2`
`ipv6 route 2001:db8:acad:2::238/128 2001:db8:acad:1::2`

## 15.5.5 - Verify Static Host Routes
`show ip route | begin Gateway`
`show ipv6 route`


## 15.5.6 - Configure IPv6 Static Host Route with Link-Local Next-Hop
must specify link-local addr & exit interface if using link-local
`ipv6 route 2001:db8:acad:2::238/128 serial 0/1/0 fe80::2`

