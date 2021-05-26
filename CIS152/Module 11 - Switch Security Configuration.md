Module 11 - Switch Security Configuration

# 11.1 - Implement Port Security
## 11.1.1 - Secure Unused Ports
Disable all unused ports!
`interface range fao0/8 - 24` --> `no shutdown`

## 11.1.2 - Mitigate MAC Address Table Attacks
Enable port security! Port security limits the number of valid MAC addresses allowed on a port

## 11.1.3 - Enable Port Security
Port security can only be configured on manually configured access ports or manually configured trunk ports.
Ports by default are set to dynamic auto (trunking on)
* `inteface f0/1`
* `switchport mode access`
* `switchport port-security`

To verify config use:
`show port-security interface f0/1`

## 11.1.4 - Limit and Learn MAC Addresses
To set max number of MAC addresses allowed on a port use:
`switchport port-security maximum *value*`
the default port security value is 1. the max is 8192

3 ways a switch can be configured to learn about MAC addresses on a security port:
1. Manually configured - administrator must manually configure static MAC address(es) using:
	* `switchport port-security mac-address *mac-address*`
2. Dynamically Learned - the current source MAC for the device connected to the port is automatically secured but is not added to the startup configuration. if the switch is rebooted it will have to re-learn the device's MAC addr.
3. Dynamically Learned - Sticky - administrator can enable the switch to dynamically learn MAC address & "stick" them to the running config using:
	* `switchport port-security mac-address sticky` - they will be saved in RAM
	**STICKY Addresses will persist between boots**
	
Use:
* `show port-security interface` and
* `show port-security address`
to verify the configuration


## 11.1.5 - Port Security Aging
Sets the aging time for static and dynamic secure addresses on a port
2 types of aging supported per port:
1. Absolute - secure addresses on the port are deleted after the specified aging time
2. Inactivity - secure addresses on the port are deleted only if they are inactive for the specified aging time

Use:
* `switchport port-security aging { static | time *time* | type {absolute | inactivity}}`
Parameters for above command:

| Parameter | Description |
| ---- | ---- |
| static | enable aging for statically configured secure addresses on this port. |
| time *time* | specify the aging time for this port. range is 0 to 1440 minutes. if time == 0 aging is disabled for this port |
| type *absolute* | set the absolute aging time. all secure addrs on this port age out exactly after the time specified & are removed from the secure addr list. |
| type *inactivity* | set inactivity aging type. secure addrs on this port age out only if they were inactive for the duration of the specified time period. |


## 11.1.6 - Port Security Violation Modes
Port violation occurs when the MAC addr of a connected device differs from the list of secure addrs. By default the port enters the error-disabled state.
To set port security violation mode use:
* `switchport port-security violation { protect | restrict | shutdown}`
Violation Modes described: 

| Mode | Description |
| --- | --- |
| shutdown (default) | port immediately transitions to error-disabled, turns of port LED, sends a syslog message, & increments the violation counter. Must be reenabled by an administrator |
| restrict | port drops packets with unknown source addrs until sufficient number of secure MAC addrs are removed or the max value is increased. this mode causes security violation counter to increment and generates a syslog message. |
| protect | least secure. drops packets with unkown MAC addr until sufficient number of secure MAC addrs are removed or max value is increased. No syslog msg is sent. |

## 11.1.7 - Ports in error-disabled state
to reenable a port that is in error-disabled use:
1. `shutdown`
2. `no shutdown`

## 11.1.8 - Verify Port Security
show port security for all interfaces:
* `show port-security`

Show port security for a specific interface:
* `show port-security interface fastethernet 0/1`

Verify learned MAC addresses:
* `show run interface fa0/1`

Verify secure MAC addresses:
* `show port-security address`


# 11.2 - Mitigate VLAN Attacks
## 11.2.1 - VLAN Attacks Review
VLAN hopping attacks can be launched in 1 of 3 ways:
* spoofing DTP messages from the attacking host to cause the switch to enter trunking mode. Traffic tagged with the target VLAN can then be sent and the switch will deliver it.
* introducing a rogue switch and enabling trunking. Attacker can then access all VLANs on the victim switch from the rogue switch.
* Double-tagging (double encapsulated attack).

## 11.2.2 - Steps to Mitigate VLAN hopping attacks
1. Disable DTP (auto trunking) negotiations on non-trunking ports by using `switchport mode access`
2. Disable unused ports and put them in an unused VLAN
3. Manually enable the trunk link on a trunking port by using `switchport mode trunk`
4. Disable DTP (auto trunking) negotiations on trunking ports by using `switchport nonegotiate`
5. set the native vlan to something other than vlan 1 by using `switchport trunk native vlan *vlan_number*`

# 11.3 - Mitigate DHCP Attacks
## 11.3.1 - DHCP Attack Review
* DHCP starvation - can be mitigated using port security
* DHCP spoofing - Can be mitigated using DHCP snooping on trusted ports.

## 11.3.2 - DHCP Snooping
Determines whether DHCP messages are from an administratively configured trusted or untrusted source. Doesn't rely on source MAC addr.
Devices under your administrative control are trusted sources. Devices beyond your firewall or outside your network are not.
All access ports are generally treated as untrusted.

## 11.3.3 - Steps to Implement DHCP Snooping
1. `ip dhcp snooping` - global config command to enable DHCP snooping
2. `ip dhcp snooping trust` - interface command on trusted ports
3. `ip dhcp snooping limit rate *num_per_second*` - to limit # of DCHP discovery messages that can be received per second on untrusted ports (interface command).
4. `ip dhcp snooping vlan *vlan_1,vlan_2,vlan_3` - to enable DHCP snooping by VLAN

`show ip dhcp snooping` - VERIFY DHCP SNOOPING
`show ip dhcp snooping binding` - View clients that have received DHCP info

# 11.4 - Mitigate ARP Attacks
## 11.4.1 - Dynamic ARP Inspection (DAI)
Threat actor can send unsolicited ARP messages to other hosts with the MAC addr of the threat actor & IP addr of the default gateway.
Dynamic ARP Inspection requires DHCP snooping & helps prevent ARP attacks by:
* not relaying invalid or gratuitous ARP requrest on to other ports in the same VLAN
* intercepting all ARP requests and replies on untrusted ports
* verifying each intercepted packet for valid IP-to-MAC binding
* dropping and logging ARP requests coming from invalid sources to prevent ARP poisoning
* Error-disabling the interface if the configured DAI number of ARP packets is exceeded.

## 11.4.2 - DAI Implementation Guidelines
* Enable DHCP snooping globally
* Enable DHCP snooping on selected VLANs
* Enable DAI on selected VLANs
* Configure trusted interfaces for DHCP snooping and ARP inspection.

## 11.4.3 - DAI Configuration Example
* `ip arp inspection vlan 10,20,30-49` - to enable arp inspection on vlan 10
* `ip arp inspection trust` - as an interface command to trust a port

`ip arp inspection validate {[src-mac] [dst-mac] [ip]}` - global config command is used to configure DAI to drop ARP packets when IP addresses are invalid. 
* dst-mac will check destination MAC addr in the ethernet header against the target MAC addr in the arp body
* src-mac will check the source MAC in ethernet header against the sender MAC addr in the ARP body.
* ip will check the ARP body for invalid & unexpected IP addrs (0.0.0.0, 255.255.255.255, & all IP multicast addrs)


# 11.5 - Mitigate STP Attacks
## 11.5.1 - PortFast and BPDU Guard
Attackers can manipulate STP by spoofing the root bridge and changing the topology of the network
Mitigate this with:
* PortFast - bypasses listening and learning states on access ports instead going straight to the forwarding state. Apply to all end user ports, but should only be applied to ports attached to end devices.
* BPDU Guard - immediately error disables a port that receives a BPDU. Only configure this on ports connected to end devices as well.


## 11.5.2 - Configure PortFast
`spanning-tree portfast` - as an interface command to enable PortFast
`spanning-tree portfast default` - as global config command to configure PortFast on all access ports.

## 11.5.3 - Configure BPDU Guard
`spanning-tree bpduguard enable` - to enable BPDU guard on an interface
`spanning-tree portfast bpduguard default` global config command to enable it on all PortFast-enabled ports.

A port running BPDU guard that receives a BPDU will shutdown.
To reenable it use:
`errdisable recovery cause bpduguard` - as a global command

verify with:
`show spanning-tree summary`