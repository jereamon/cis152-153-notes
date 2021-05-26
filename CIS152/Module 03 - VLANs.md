Module 3 - VLANs

## 3.1 Overview of VLANS
### 3.1.1
### 3.1.2 VLAN Benefits

- smaller broadcast domains
- improved security - only users in the same vlan can communicate together & must use a router for communication outside the vlan.
- improved it efficiency - simplify network management, can be named for easier identification.
- reduce cost
- better performance - smaller broadcast domains reduce unnecessary traffic
- simpler project & application management

### 3.1.3 Types of VLANs

`show vlan brief`

- Default VLAN - on cisco switch is VLAN 1. All ports are assigned to VLAN 1 by default. VLAN 1 cannot be renamed or deleted.
- Data VLAN - referred to as user VLANs. they separate the network into groups of users.
- Native VLAN - User traffic from a vlan must be tagged with its VLAN ID & trunk ports are used to support tagged traffic. Untagged traffic is placed on the native VLAN (default vlan) by the trunk port. **best practice** \- assign native vlan as an unused vlan distinct from vlan 1.
- Management VLAN - specifically for network management traffic. SSH, telnet, https, snmp. VLAN 1 by default.
- Voice VLAN - a separate vlan required by VoIP.

## 3.2 VLANs in a Multi-Switched Environment

### 3.2.1 Defining VLAN Trunks

a vlan trunk is a point to point link between network devices that allows traffic to propagate between switches.  
IEEE 802.1Q for coordinating trunks on fast ethernet, gig ethernet, & 10 gig ethernet.

### 3.2.2 Network without VLANs

broadcast goes to all devices & switches connected to the originating device.

### 3.2.3 Network with VLANs

broadcast only goes to devices within the originating device's VLAN.

### 3.2.4 VLAN identification with a tag

a standard ethernet frame does not contain info about it's VLAN.  
This info is added when the frame is placed on a trunk port using the IEEE 802.1Q header.  
When a frame is recieved on a port configured in access mode a *4 byte tag* is inserted into original frame header, the FCS is recalculated, & the frame is sent out a trunk port.

VLAN Tag Details:

- Type - 2 bytes called the tag protocol ID (TPID). Set to 0x8100 for ethernet.
- User Priority - 3 bits supports level or service implementation
- Canonical Format Identifier (CFI) - 1 bit enables token ring frames to be carried across ethernet links
- VLAN ID (VID) - 12 bit identification number supporting up to 4096 VLAN IDs.

### 3.2.5 Native VLANs & 802.1Q Tagging

according to 802.1Q the native VLAN for trunk links is VLAN 1. Untagged frames arriving on a trunk port are assigned to the native vlan.

with 802.1Q frames received on a trunk port are dropped if their VID is the same as the native VLAN. *Configure switch ports so they don't send tagged frames on the native vlan*

when an untagged frame is recieved it is dropped if there are no devices associated with the native vlan and there are no other trunk ports.

### 3.2.6 Voice VLAN Tagging

VoIP requires a separate VLAN. Cisco IP phones connect directly to a switch port and contains an integrated three port 10/100 switch.  
Its ports provide dedicated connections to:

- port 1 - the switch or other VoIP device
- port 2 - is an internal 10/100 interface taht carries the phone traffic
- port 3 - (access port) connects to pc or other device

the access port sends CDP packets to tell the phone to send traffic in one of three ways which varies based on the type of traffic.

- voice vlan traffic must be tagged with an appropriate layer 2 class of service (CoS) priority value
- access vlan traffic can also be tagged with Cos priority value
- access vlan is not tagged with CoS value

## 3.3 VLAN Configuration

### 3.3.1 VLAN Ranges on Catalyst Switches

Normal range vlans:

- between 1-1005
- 1002 - 1005 reserved for legacy network technologies (Token ring & fiber distributed data interface)
- IDs 1 and 1002-1005 are automatically created and cannot be deleted
- used in small & medium businesses & enterprise networks
- configurations stored in flash memory in vlan.dat
- vlan trunking protocol (VTP) helps synchronize vlan database between switches

Extended range vlans:

- between 1006-4094
- used by service providers to service multiple customers & global enterprises
- config is saved by default in the running config
- support fewer vlan features that normal vlans
- require VTP transparent mode

### 3.3.2 VLAN Creation Commands

vlan config saved in vlan.dat is in flash memory which is persistent and **does not** require saving via `copy running-config startup-config`

vlan config: *from global config*

- `vlan *vlan-id*` \- create a vlan with valid vlan ID number
- `name *vlan-name*` \- specify unique name for vlan

### 3.3.4 VLAN port assignment commands

how to define a port as an access port and assign it to a vlan:  
*from global config*:

- `interface *interface-id*`
- `switchport mode access` \- sets the port to access mode (optional but recommended for security)
- `switchport access vlan *vlan-id*` \- assigns the port to a vlan

### 3.3.6 Data and Voice VLANs

access ports can only belong to one data vlan at a time but can also be associated with a voice vlan.

### 3.3.7 Data and Voice VLAN Example

when configuring an interface use `switchport voice vlan *vlan-id*` to assign a voice VLAN to a port.  
set the trusted state (quality of service) of an interface with: `mls qos trust [cos | device cisco-phone | dscp | ip-precedence] examples:` mls qos trust cos`or`mls qos trust dscp`

### 3.3.8 Verify VLAN Information

`show vlan` has various options:

- `brief` \- shows vlan name, status, & its ports, one vlan per line
- `id *vlan-id*` \- shows info about single specified vlan
- `name *vlan-name*` \- same as id but with vlan name
- `summary` \- shows summary of vlan info

Also useful:

- `show interface *interface-id* switchport`
- `show interface vlan *vlan-id*`

### 3.3.9 Change VLAN Port Membership

`no switchport access` \- from interface configuration will change a port's membership back to vlan 1

### 3.3.10 Delete VLANs

`no vlan *vlan-id*` \- removes vlan from the switch's vlan.dat  
**caution** before deleting a vlan reassign all member ports to a different vlan first. otherwise they will be unable to communicate.

`delete flash:vlan.dat` \- deletes entire vlan.dat file

to *reset* a catalyst switch to factory default: unplug all cables except console & power, from privileged exec mode enter `erase startup-config` then `delete vlan.dat`

## 3.4 VLAN Trunks

### 3.4.1 Trunk Configuration Commands

to enable trunk links *from global config*:

- `interface *interface-id*`
- `switchport mode trunk` \- sets port to permanent trunking mode
- `switchport trunk native vlan *vlan-id*` \- sets native vlan to something other than vlan 1
- `switchport trunk allowed vlan *vlan-list*` \- specifies allowed vlans

### 3.4.3 Verify Trunk Configuration

`show interfaces *interface-id* switchport`

### 3.4.4 Reset the Trunk to the Default State

to remove allow vlans & reset the native vlan of the trunk use:

- `no switchport trunk allowed vlan`
- `no switchport trunk native vlan`

## 3.5 Dynamic Trunking Protocol

### 3.5.1 Introduction to DTP

DTP lets devices automatically negotiate trunking with neighboring devices.  
it is automatically enabled on catalyst 2960 & catalyst 3650 series switches.  
to communicate with a device that does not support DTP use:

- `switchport mode trunk`
- `switchport nonegotiate`  
    this way is won't generate dtp frames

to reenable dtp:  
`switchport mode dynamic auto`

### 3.5.2 Negotiated Interface Modes

`switchport mode` options:

- `access` \- puts the interface into permanent nontrunking mode
- `dynamic auto` \- interface will become trunk interface if neighboshow ring interface is set to trunk or desirable mode. (this is default)
- `dynamic desirable` \- actively tries to convert interface to trunk. Will do so if neighbor is set to trunk, desirable, or auto mode.
- `trunk` \- puts interface in permanent trunking mode.

### 3.5.3 Results of DTP Configuration
Best practice is to configure trunks statically whenever possible.

### 3.5.4 Verify DTP Mode
`show dtp interface *interface-id*`