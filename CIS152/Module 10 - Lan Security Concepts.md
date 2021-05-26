Module 10 - Lan Security Concepts

# 10.1 - Endpoint Security
**AAA - Authentication, Authorization, Accounting **

## 10.1.1 - Network Attacks Today
DDoS - Distributed Denial of Service
Data Breach - Servers or hosts are compromised
malware - mean software infects things. Wannacry.

## 10.1.2 - Network Security Devices
* Vpn enabled router - Provides secure connection to remote users across a public network.
* NGFW - Next Generation Firewall, profvides stateful packet inspection, intrusion protection, advanced malware protection, url filtering, & more.
* NAC - Network Access Control, authentication, authorization, and accounting.

## 10.1.3 - Endpoint Protection
Today endpoints are best protected by a combination of NAC, host based **AMP (Advance Malware Protection)** software, an email security appliance (ESA), and a web security appliance (WSA).

## 10.1.4 - Cisco Email Security Appliance
Cisco ESA is a device designed to monitor SMPT. It is constantly updated by real-time feeds from the Cisco Talos.
It wil: Block known threats, remediate against stealth malware, discard emails with bad links, block access to newly infected sites, encrypt content in outgoing mail.

## 10.1.5 - Cisco Web Security Appliance (WSA)
Cisco WSA is a mitigation technology for web-based threats. It provides complete control over how users access the internet.


# 10.2 - Access Control
## 10.2.1 - Authentication with a Local Password
Simplest method of remote access authentication is to configure a login & password combination on console, vty lines, and aux ports.
`line vty 0 4` --> `password ci5co` --> `login`

SSH Example: (from global config)
`ip domain-name example.com` --> `crypto key generate rsa general-keys modulus 2048` --> `username Admin secret Pa55word` --> `ssh version 2` --> `line vty 0 4` --> ` transport input ssh` --> `login local`

The local database method is limited in that user accounts must be configured locally on each device and it provides no fallback authentication method.
For large enterprise environments a better solution is to have all devices refer to the same database of usernames & passwords from a central server.

## 10.2.2 - AAA Components
Credit Card analogy: *Authentication* - Who are you?, *Authorization* - How much can you spend, *Accounting* - What did you spend it on?

## 10.2.3 - Authentication
Local AAA Authentication - stores usernames & passwords locally in a cisco device (router).
1. client establishes connection with router, 2. AAA router prompts user for username & password, 3. router authenticates credentials.

Server based AAA Authentication - router accesses central AAA server for credentials. Router uses either **RADIUS** - Remote Authentication Dial-In User Service, or **TACACS+** - Terminal Access Control System protocols.
1. client establishes connection with router, 2. AAA router prompts for username & password, 3. routher authenticates credentials using AAA server

## 10.2.4 - Authorization
Authorization is automatic & does not require users to perform additional steps.
Uses a set of attributes that describes the user's access to the network.
1. Once user is authenticated a session is established between the router & AAA server, 2. router requests authorization from AAA server for the client's requested service, 3. AAA Server returns a PASS/FAIL response

## 10.2.5 - Accounting
	AAA Acounting collects and reports usage data. Collected data may include start & stop connection times, executed commands, # of packets, & # of bytes.
	1. once user is authenticated the AAA acounting process generates a start message to begin the accounting process, 2. when user finishes a stop message is recorded & accounting process ends.
	
## 10.2.6 - 802.1X
IEEE 802.1X is a port based access control and authentication protocol. Restricts unauthorized workstations from connecting to a LAN through publicly accessible switch ports.
* Client (Supplicant) - a device running 802.1X-complient client software which is available for wired or wireless devices
* Switch (Authenticator) - acts as an intermediary between the client and the authentication server. Requests identifying info from client, verifies that info with the server, & relays the response to the client. A wireless access point could act as an authenticator as well.
* Authentication Server - validates the identity of the client & notifies the switch or WAP that the client is or is not authorized to access the LAN.


# 10.3 - Layer 2 Security Threats
## 10.3.1 - Layer 2 Vulnerabilities
## 10.3.2 - Switch Attack Categories
Layer 2 Attacks: 
* MAC Table Attacks - includes MAC address flooding attacks.
* VLAN Attacks - VLAN hopping and VLAN double tagging attacks. also includes attacks between devices on a common VLAN.
* DHCP Attacks - includes DHCP starvation and DHCP spoofing atacks.
* ARP attacks - ARP spoofing and ARP poisoning attacks.
* Address Spoofing Attacks - includes MAC & IP spoofing attacks
* STP - Spanning tree protocol manipulation attacks

## 10.3.3 - Switch Attack Mitigation Techniques
* Port Security - Prevents many attacks including MAC addr flooding & DHCP starvation.
* DHCP Snooping - Prevents DHCP starvation & spoofing.
* Dynamic ARP Inspection (DAI) - Prevents ARP spoofing and ARP poisoning attacks.
* IP Source Guard (IPSG) - Prevents MAC & IP Addr spoofing attacks.

Management Protocols (use the secure ones)
| Unsecure | Secure |
| --- | --- | 
| Syslog | SSH |
| SNMP | SCP |
| TFTP | |
| telnet | SSL/TLS |
| FTP | SFTP |
|etc... | |


# 10.4 - MAC Address Table Attack
## 10.4.1 - Switch Operation Review
## 10.4.2 - MAC Address Table Flooding
MAC tables have a fixed size. MAC addr flooding is when a threat actor bombards the switch with fake source MAC addresses until its MAC table is full. Once full the switch treats the incoming frames as unknown unicast and floods all incoming traffic out all ports on the same VLAN without referencing the MAC table. This lets the threat actor capture all frames sent between hosts on that LAN.

**macof** - attack tool for MAC addr table flooding

## 10.4.3 - MAC Address Table Attack Mitigation
catalyst 6500 switch can store 132,000 MAC addresses. macof can flood a switch with up to 8,000 bogus frames per second. MAC address table overflow can occur in seconds.

use **Port Security** to mitigate this!

# 10.5 - LAN Attacks
## 10.5.1 - VLAN & DHCP Attacks
**VLAN Hopping** - enables traffic from one VLAN to be seen by another without the use of a router.
Configures a host to act just like a switch & takes advantage of the default trunking settings on a real switch.

switch ports by default are left in auto trunking mode, meaning that if a fake switch on the other end is set to dynamic desirable trunking will be enabled between the two.u

**VLAN Doule-Tagging** - embed hidden 802.1Q tag inside a frame that already has an 802.1Q tag.
The first tag will be stripped off by the first switch it passes through, the next switch will think it's really from the vlan the second tag specifies.

This attack is unidirectional and only works if the attacker is connected to a port residing in the same VLAN as the native VLAN of the trunk port.

**Prevent VLAN hopping & double tagging** by: disabling trunking on all access ports, disable auto trunking on trunk links, be sure the native vlan is only used for trunk links.

### I used 10.5.2 & 10.5.3 info ^^^^ to fill out 10.5.1
## 10.5.4 - DHCP Messages
DHCPDISCOVER --> DHCPOFFER --> DHCPREQUEST --> DHCPACK

## 10.5.5 - DHCP Attacks
**DHCP Starvation** - attacker uses a tool like Gobbler that tries to lease all available IP addresses from a DHCP server.
**DHCP Spoofing Attack** - Rogue DHCP server is connected to the network & provides false IP configuration parameters to legitimate clients.
Can provide:
* Wrong default gateway - creates man in the middle attack where traffic can be intercepted.
* Wrong DNS server - points users to nefarious websites
* Wrong IP address - creates a DOS attack since users won't be able to access the network.

**Mitigate** these with DHCP snooping

## 10.5.6 - ARP Attacks, STP attacks, & CDP Reconnaissance
## 10.5.7 - ARP Attacks
**gratuitous ARP** - unsolicited ARP that results in other hosts on the subnet storing the MAC & IP addr contained within it.
This means any host can claim to be the owner of any IP & MAC addr combination.

Tools to do this attack: dsniff, Cain & Abel, ettercap, Yersinia, & more.
IPv6 includes strategies to mitigate Neighbor Advertisement spoofing.

**Mitigate** this with DAI (Dynamic ARP Inspection)

## 10.5.8 - Address Spoofing Attack
IP addr spoofing is when a threat actor hijacks a vaild IP addr of another device on the subnet or uses a random IP addr. This is difficult to mitigate.

MAC addr spoofing is when threat actor alters their MAC addr to match another known MAC addr of a target host. When switches on the network receive frames from this spoofed MAC they update their MAC table with an incorrect entry.

**Mitigate** these attacks with IPSG (IP Source Guard)

## 10.5.9 - STP Attack
Threat actors manipulate the Spanning Tree Protocol by spoofing the root bridge & changing the topology of the network. The attacker makes their host appear as the root bridge resulting in all other switches routing traffic through them.

**Mitigate** this by implementing BPDU Guard on all access ports.

## 10.5.10 - CDP Reconnaissance
Cisco Discovery Protocol (CDP) is a proprietary Layer 2 Link discovery protocol. It is enabled on all cisco devices by default & automatically discovers other CDP enabled devices to help auto configure their connection.

CDP info is sent out enabled ports in an unencrypted multicast. This is very useful for network troublehshooting, but the CDP info can be used by a threat actor to discover network infrastructure vulnerabilities.

**Mitigate** this by disabling CDP on edge ports that connect to untrusted devices.
`no cdp run` from global config to disable CDP globally.
`no cdp enable` from interface config to disable CDP on a given port.
`cdp enable` to enable cdp