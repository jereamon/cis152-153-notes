Module 16 - Troubleshoot Static and Default Routes

# 16.1 - Packet Processing with Static Routes
## 16.1.1 - Static Routes and Packet Forwarding


# 16.2 - Troubleshoot IPv4 Static and Default Route Configuration
## 16.2.1 - Network Changes
## 16.2.2 - Common Troubleshooting Commands
* `ping`
* `traceroute`
* `show ip route`
* `show ip interface brief`
* `show cdp neighbors detail`


## 16.2.3 - Solve a Connectivity Problem
If a router is saying it can't access resources on a lan connected to another router:
1. `ping 192.168.2.1 source g0/0/0` - ping the remote lan & use the interface on the local router that is connected to the lan which can't access remote resources as the source.
2. If the above wasn't successful then ping the Next-Hop router
3. If #2 is successful ping the remote router without specifying a source. If this ping is successful it means that only packets sourced from the router's lan interface can't reach the remote router.
4. `show ip route | begin Gateway` - verify there is an accurate next-hop address for packets from the lan to take to get to the remote router.
5. `show running-config | include ip route` - to check if an incorrect route in the routing table is also incorrect in the running config. Remove it with `no ip route *dest-network-ip subnet next-hop-intf-addr*` and add the correct with `ip route *dest-network-ip subnet next-hop-intf-addr`
6. Verify new static route is installed using `show ip route | begin Gateway` again


