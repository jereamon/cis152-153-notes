Module 12 -- Network Troubleshooting

# 12.1 -- Network Documentation
## 12.1.1 -- Documentation Overview
Common network documentation includes:
* physical & logical network topology diagram
* network device documentation that records all pertinent device info
* network performance baseline documentation


## 12.1.2 -- Network Topology Diagram
* Physical Topology -- includes:
	* device name
	* device location
	* interface & ports used
	* cable type
* Logical Topology -- includes:
	* device identifiers
	* ip addresses & prefix lengths
	* interface identifiers
	* layer 2 information


## 12.1.3 -- Network Device Documentation
should contain accurate, up to date records of network hardware & software.

Device Docuentation Exmples:
* Router -- includes: model, description, location, IOS version, license, & all info describing each interface and their addressing.
* Lan switch -- Includes: same ^^ includes mgt. IP addr & VTP as well & description of each interface & their vlan, trunk status, etherChannel status, native vlan, etc..
* End-System Documentation Files -- Focuses on hardware & software used in servers, network management consoles, & user workstations. Includes:
	* device name, OS, services (http, pop3, etc..), mac addr, ip addr/subnet, default gateway, & dns

## 12.1.4 -- Establish a Network Baseline
collect performance data from ports & devices that are essential to network operation.

A network baseline should answer:
* How does the network perform during a normal or average day?
* what are the most errors occurring?
* what part of the network is most heavily used? & Least used?
* Which devices should be monitored & what alert thresholds should be set?
* Can the network meet the identified policies?


## 12.1.5 -- Step 1 - Determine What Types of Data to Collect
Start out simply & fine-tune along the way. select only a few variables to measure that represent the defined policies.

interface utilization & cpu utilization are good places to start.

## 12.1.6 -- Step 2 - Identify Devices & Ports of Interest
Use the network topology to identify devices & ports whos performance should be measured.
These devices include:
* network devices that connect to other network devices
* servers
* key users
* anything else critical to operations

## 12.1.7 -- Step 3 - Determine the Baseline Duration
when capturing data for analysis, the period specied should be a minium of **7 days long**.

Over time, weekly, monthly, & yearly trends can be captured.
Typically a baseline needs to last no more than **6 weeks**, generally **2 to 4** weeks is adequate, unless specific long term trends need to be measured.


## 12.1.8 -- Data Measurement
Obvious, useful commands for gathering info from routers & switches include:
`ping`, `traceroute`, & `telnet`. As well as many `show` commands:
* `show version` -- uptime, version info for device software & hardware.
* `show ip interface [brief]` -- displays all config options set on an interface. `brief` displays only up/down status & ip addr.
* `show interfaces` -- displays detailed output for each interface, add `interface-name` to see for a specific interface.
* `show ip route` -- shows routing table. Additional options include: `static`, `eigrp`, `ospf`
* `show cdp neighbors detail` -- shows info about directly connected Cisco devices
* `show arp` or `show ipv6 neighbors` -- shows arp table & neighbor table respectively
* `show running-config`
* `show vlan` -- shows status of VLANs on a switch
* `show port` -- shows status of ports on a switch
* `show tech-support` -- useful for collecting large amount of info about the device.

Collecting data manually this way is time consuming & not scalable.


# 12.2 -- Troubleshooting Process
## 12.2.1 -- General Troubleshooting Procedures
Troubleshooting steps are not always the same nor are they always executed in the same order.

## 12.2.2 -- Seven-Step Troubleshooting Process
1. Define the problem -- verify there is a problem. Gather symptoms, ask questions & investigate the issue. Determine # of affected devices, scope of the problem.
2. Gather Information -- targets to be investigated must be identified, access to those targets must be obtained, & information gathered. More symptoms may be gathered in this step.
3. Analyze information -- identify possible causes. interpret & analyze the gathered information.
4. Eliminate Possible causes -- eliminate possible causes until you're left with the most likely one.
5. Propose Hypothesis -- once most probable cause is identified a solution must be formulated.
6. Test Hypothesis -- 
	* asses impact & urgency of a problem before testing a solution. 
	* Determine if the solution could have adverse affects. 
	* Create a rollback plan identifying how to quickly reverse a solution.
	* Implement the solution & verify it has solved the problem.
	* If the solution fails, document it & remove the changes. Go back to gathering info & isolate the issue.
7. Solve the Problem -- inform any users & those involved in the troubleshooting process the problem has been resolved. Document the fix.


## 12.2.3 -- Question End Users
Recommendations when communicating with a user:
* speak at a technical level they can understand
* listen or read carefully what they are saying. Take notes.
* Be considerate & empathize, they're likely anxious & under stress to solve the problem. Reassure them.


## 12.2.4 -- Gather Information
Use common commands like those described in 12.1.8
* `ping`
* `traceroute`
* `telnet`
* `ssh -l`
* `show ip interface brief`
* `show ip route`
* `show protocols` -- displays configured protocols
* `debug` -- displays list of options for enabling or disabling debugging events


## 12.2.5 -- Troubleshooting with Layered Models
Consider the OSI & TCP/IP layer that the problem is ocurring at.


## 12.2.6 -- Structure Troubleshooting Methods
* bottom up -- start at the physical layer & work up. Good approach when the problem is suspected to be physical. Downside is it requires you to check every device & interface.
* Top-down -- start at the application layer & work down. 
* Divide-and-Conquer -- network admin makes an informed guess as to which layer to start with & tests in both directions starting from there.
* Follow-the-Path -- discover the traffic path from source to destination. Check the links on that path. This usually complements one of the other approaches.
* Substitution -- swap out devices with known working ones & see if the problem is fixed.
* Comparison -- spot-the-differences. Compare nonoperational elements with operational ones. May solve the problem without revealing its root cause.
* Educated Guess -- make a guess based on the symptoms. 


## 12.2.7 -- Guidelines for Selecting a Troubleshooting Method
Determine the type of problem, analyze its symptoms, apply previous experience & depending on if it appears to be software related use top-down method, hardware related use bottom up, if it has been experienced before use divide-and-conquer.


# 12.3 -- Troubleshooting Tools
## 12.3.1 -- Software Troubleshooting Tools
* Network management system (NMS) tools -- include device-level monitoring, configuration, & fault management tools.
* knowledge Bases -- online network device vendor knowledge bases combined with search engines kick ass.
* Baselining Tools -- tools for automating the network docmentation & baselining process. They can draw network diagrams & lots of other stuff.

## 12.3.2 -- Protocol Analyzers
Wireshark -- decodes various protocol layers in a recorded frame. 


## 12.3.3 -- Hardware Troubleshooting Tools
* Digital Multimeters
* Cable Testers -- handheld, for testing various types of cabling. Can detect broken wires, crossed-over wiring, shorted connections, improperly paired connections. From cheap to expensive: continuity testers, data cabling testers, time-domain relfectometers (TDRs).  TDRs can pinpoint a break in the cable.
* Cable Analyzers -- used to test & certify copper & fiber cables for different services & standards.
* Portable Network Analyzers -- used to troubleshoot switched networks & VLANs. Can provide much info about network usage, VLAN config, & interace details. Typically can output to a pc
* Cisco Prime NAM - Network Analysis Module -- includes hardware & software designed for performance analysis in routing & switching environments.


## 12.3.4 -- Syslog Server as a Troubleshooting Tool
Devices can send event messages to one or more of the following locations:
* console
* terminal lines
* Buffered logging -- messages are stored in memory for a time, but are cleared when device is rebooted.
* SNMP traps -- events exceeding a certain threshold can be processed & forwarded as SNMP traps.
* syslog -- cisco routers & switches can send messages to an external syslog service.

**See 10.5.3 for ios log message levels**


`logging host 209.165.200.225` -- sends messages to syslog server at 209.165.200.225
`logging trap notifications` -- only reports messages from level 5 (notifications) to level 0 (emergencies)
`logging on` -- enables logging on the device


# 12.4 -- Symptoms & Causes of Network Problems
## 12.4.1 -- Physical Layer Troubleshooting
potential symptoms:
* Lower than baseline performance -- requires previous baseline for comparison. Usually due to overloaded or underpowered servers, bad switch/router config, congestion, & chronic frame loss.
* Loss of connectivity -- could be failed or disconnected cable. Verify w/ping test. Intermittent connectivity could be loose connection.
* Network bottlenecks/congestion -- 
* High CPU utilization -- device is operating at or above its limits.
* Console error messages -- could indicate physical layer problem.


Possible causes:
* Power-related -- check if fans & exhaust  vents are clear, check if nearby machines are powered down.
* Hardware faults -- faulty NIC. 'Jabber' - when a network device continually transmits random data.
* Cabling Faults -- reseat cables, look for damaged cables, improper cable types, bad crimps.
* Attenuation -- cables too long or poorly plugged in.
* Noise -- local EMI. Could be: FM radio, police radio, building securiity, crosstalk, large electric motors, etc...
* Interface Configuration errors -- 
* Exceeding design limits
* CPU overload -- can wreak havoc.


## 12.4.2 -- Data Link layer Troubleshooting
Symptoms:
* no functionality/connectivity at layer 3 or above
* lower than baseline performance -- frames could be taking suboptimal path, or some frames could be getting dropped. Try an extended, continuous ping to see if frames are dropped.
* excessive broadcasts -- limit broadcast domain size
* console messages -- most common is line protocol down message.

Causes:
* Encapsulation Errors -- encapsulation at one end of WAN link is configured differently than at other end.
* Address mappng errors -- ensure devices can use ARP and that ARP isn't being misused by an attacker.
* Framing errors -- occurs when a frame doesn't end on an 8-bit byte boundary. Makes it hard for receiver to determine where one frame ends & next begins. Could be a noisy serial line, too long cable, faulty NIC, duplex mismatch.
* STP Failures or loops -- could be because of a mistmatch between real & documented topology, config error, overloaded switch CPU during convergence.


## 12.4.3 -- Network layer Troubleshooting
Symptoms:
* Network failure -- no connectivity
* suboptimal performance 

Combination of static & dynamic addressing can cause issues. Solving routing problems requires a methodical process.

Causes:
* General network issues -- change in topology: down link, new routes or removal of routes. Determine if anything recently changed.
* Connectivity issues -- check for power problems. Check layer 1 shit.
* Routing table -- check for missing or unexpected routes. use `debug` commands
* neighbor issues -- if using OSPF or similar check to see if there are issues forming adjacencies.
* topology database -- if topology table or database is used check for anything unexpected.

## 12.4.4 -- Transport Layer Troubleshooting - ACLs
Common misconfigurations:
* selection of traffic flow -- ensure ACL is applied to correct interface in correct direction.
* order of ACL entries -- go from specific to general. 
* implicit deny any
* Adresses & IPv4 wildcard masks -- complex IPv4 wildcard masks can provide improved efficiency but can also more easly be misconfigured.
* Selection of transport layer protocol -- be sure the correct protocol is specified and be as specific as possible. Dont allow tcp & udp if only one is needed.
* source & dest. ports
* use of the established keyword -- `established` increases security provided by ACLs. Applied incorrectly, it fucks things up.
* uncommon protocols -- VPN & encryption protocols

`log` keyword generates log entry whenever an entry condition is matched. Can be very helpful.


## 12.4.5 -- Transport layer Troubleshooting - NAT for IPv4
Common areas inoperable with NAT:
* BOOTP & DHCP -- both manage automatic IPv4 addr assignment. Neither work well over NAT
* DNS -- ipv4 helper can help with dns issues.
* SNMP -- ipv4 helper can help here too
* Tunneling & encryption protocols -- often require traffic be sourced from a specific UDP or TCP port or use a protocol NAT cannot process.


## 12.4.6 -- Application Layer Troubleshooting
Protocols:
* SSH/Telnet
* HTTP
* FTP -- interactive file transfers between hosts
* TFTP -- basic interactive file transfer, typically between hosts & networking devices
* SMPT -- message delivery
* POP -- connects to mail server & downloads mail
* SNMP -- network management
* DNS
* Network File System (NFS) -- enables computers to mount drives on remote hosts & use them as if they were local.


Symptoms & causes depend on the application itself.


# 12.5 -- Troubleshooting IP Connectivity
## 12.5.1 -- Components of Troubleshooting End-to-End Connectivity
Example bottom up troubleshooting steps:
1. Check physical connectivity.
2. check for duplex mismatches
3. check data link & network layer addressing (ipv4 arp table, ipv6 neighbor table, mac table, vlan assignments)
4. verify default gateway is correct
5. ensure devices are determining correct path from source to destination (routing)
6. verify transport layer is functioning properly. Maybe use telnet to test connections.
7. verify no ACLs are blocking traffic
8. ensure DNS settings are correct & a dns server is accessible


## 12.5.2 -- End-to-End Connectivity Problem Initiates Troubleshooting.
`ping` & `traceroute` to check end-to-end connectivity
`tracert` -- on windows


## 12.5.3 -- Step 1 - Verify the Physical Layer
Most common Cisco IOS commands to verify hardware:
* `show processes cpu`
* `show memory`
* `show interfaces`

`show interfaces interface name` -- includes output for:
* input queue drops -- tells you whether more traffic arrived at some point than the router could process
* output queue drops -- indicates that packets were dropped due to congestion on the interface
* input errors -- indicates errors during reception of the frame. (CRC errors)
* output errors -- indicates errors (ex: collisions) during transmission of a frame. If there are errors its likely a duplex mismatch.

## 12.5.4 -- Step 2 - Check for Duplex Mismatches
IEEE 802.3ab Gigabit Ethernet standard mandates the use of autonegotiation for speed & duplex

Duplex confg guidelines:
* use autonegotiation -- `duplex auto`
* if ^^ fails set speed & duplex manually
* point-to-point ethernet links should always be full-duplex
* half-duplex is uncommon. Erradicate it.


## 12.5.5 -- Step 3 - Verify Addressing on the Local Network
`arp -a` -- check the arp table on windows
`arp -d` -- clears windows arp cache
`netsh interface ipv6 show neighbor` -- show ipv6 neighbors windows

`show ipv6 neighbors` -- show ipv6 neighbors Cisco IOS
`show mac address-table` -- show mac table cisco ios

## 12.5.6 -- Troubleshoot VLAN Assignment Example
`show vlan` -- cisco ios to verify vlan assignments

Use `show vlan` and the commands in 12.5.5 to discern interfaces set to the wrong vlan.
Correct their vlan, from interface config with:
`switchport mode access`
`switchport access vlan correct-number`


## 12.5.7 -- Step 4 - Verify Default Gateway
 ` show ip route | include Gateway` -- cisco ios verify default route
 `route print` -- shows routes on windows including default route (gateway)
 
 
 ## 12.5.8 -- Step 5 - Troubleshoot IPv6 Default Gateway Example
 `show ipv6 route` -- cisco ios, to check default route
 `ipconfig` -- on pc
 check interfaces on router & correct any issues
 
 
 ## 12.5.9 -- Step 5 - Verify Correct Path
 Check the routing table:
 `show ip route | begin Gateway`
 `show ipv6 route`
 
 Routing tables can be populated by:
 * directly connected networks
 * local host or local routes
 * static routes
 * dynamic routes
 * default routes

## 12.5.10 -- Step 6 - Verify the Transport Layer
Most common transport issues are ACLs & NAT config.
telnet is common tool for testing transport layer stuff.

try pinging. If ping successful, try telnetting: `telnet ip-address`
If telnetting on its default port works try on other ports:
`telnet ip-address 80`


## 12.5.11 -- Step 7 - Verify ACLs
`show ip access-lists` -- displays contents of all IPv4 ACLs
`show ipv6 access-list` -- shows contents of all IPv6 ACLs

Specify ACL name or number to see one.

`show ip interfaces` -- will show which ACLs are applied to which interfaces


## 12.5.12 -- Step 8 - Verify DNS
`show running-cofig` -- to view DNS config info
`ip host ipv4-server 172.16.1.100` -- to tell it to use a name instead of the IPv4 addr