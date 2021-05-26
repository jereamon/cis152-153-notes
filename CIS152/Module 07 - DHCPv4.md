Module 7 - DHCPv4

# 7.1 - DHCPv4 Concepts
## 7.1.1 DHCPv4 Server & Client
## 7.1.2 DHCPv4 Operation
## 7.1.3 Steps to Obtain a Lease
4 steps:
1. DHCp Discover (DHCPDISCOVER) - clients broadcasts a DHCPDISCOVER message with its own MAC address to discover servers.
2. DHCP Offer (DHCPOFFER) - when DHCPv4 server recieves DHCPDISCOVER it reserves an available IPv4 address & creates an ARP entry using the client MAC & reserved IP addr. then it sends the DHCPOFFER.
3. DHCP Request (DHCPREQUEST) - when client receives DHCPOFFER it responds w/DHCPREQUEST (sent as broadcast). DHCPREQUEST is used for lease origination & renewal. it serves as binding acceptance notice.
4. DHCP Acknowledgement (DHCPACK) - once DHCPREQUEST is received the server may ping that IP addr to ensure it's not alread in use & then responds to the client with DHCPACK. DHCPACK is almost identical to DHCPOFFER except for the message type field.


## 7.1.4 Steps to Renew a Lease
2 steps:
1. DHCPREQUEST - client sends DHCPREQUEST directly to its server. If DHCPACK is not recieved w/in specified time it broadcasts DHCPREQUEST in case another DHCPv4 server is listening.
2. DHCPACK - server verifies lease info and responds to DHCPREQUEST



# 7.2 Configure Cisco IOS DHCPv4 Server
## 7.2.1 Cisco IOS DHCPv4 Server
## 7.2.2 Steps to Configure a Cisco IOS DHCPv4 Server
3 steps:
1. `ip dhcp excluded-address *low-address* [high-address]` - Exclude IPv4 Addresses - set aside addresses that may be assigned statically - from global conf
2. `ip dhcp pool *pool-name*` - Define a DHCPv4 Pool Name
3. Configure the DHCPv4 Pool:
	1. `network *network-IP* [subnet-mask | / prefix-length` - define the address pool
	2. `default-router address [address2... address8]` - Define default router or gateway
	3. `dns-server *address*` - Define a DNS server
	4. `domain-name *domain*` - define the domain name
	5. `lease {days [hours [minutes]] | infinite}` - define DCHP lease duration
	6. `netbios-name-server *address*` - define NetBIOS WINS server


## 7.2.3 Configuration Example
## 7.2.4 DHCPv4 Verification Commands
* `show running-config | section dhcp` - displays configured DHCPv4 commands
* `show ip dhcp binding` - displays all IPv4 addrs to MAC bindings provided by the DHCPv4 service
* `show ip dhcp server statistics` - displays count info for number of DHCPv4 messages sent & received

## 7.2.5 Verify DHCPv4 is Operational
## 7.2.7 Disable the Cisco IOS DHCPv4 Server
DHCPv4 is enabled by default. to disable, from global config:
`no service dhcp` to enable: `service dhcp`

## 7.2.8 DHCPv4 Relay
in enterprise networks DHCP servers are often standalone servers. Therefore a clients dhcp broadcast may not be received by the server if the client's router isn't configured to forward DHCP broadcasts.

configure the interface(port) that goes to the server with an ip helper-address:
from the interface config: 
`ip helper-address *ip-addr*`
The helper IP addr should be the DHCP server's IP addr
this helper addr causes the router to relay DHCPv4 broadcasts to the server.

## 7.2.9 Other Service Broadcasts Relayed
`ip helper-address` can relay 8 other UDP services in addition to DHCPv4:
* Port 37: time, Port 49: TACACS, Port 53: DNS, Port 67: DHCP/BOOTP server, Port 68: DHCP/BOOTP client, Port 69: TFTP, Port 137: NetBIOS name service, Port 138: NetBIOS datagram service


# 7.3 Configure a DHCPv4 Client
## 7.3.1 - Cisco Router as a DHCPv4 Client
## 7.3.2 - Configuration Example
to configure an ethernet interface as a DHCP client use: `ip address dhcp` from the interface config
verify config with: `show ip interface *interface-id*`

## 7.3.3 - Home Router as a DHCP Client
home routers are typically already set to receive IPv4 addr info automatically from the ISP

