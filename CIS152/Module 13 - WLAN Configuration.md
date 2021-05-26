Module 13 - WLAN Configuration

# 13.1 - Remote Site WLAN Configuration
## 13.1.1 - Video - Configure a Wireless Network
## 13.1.2 - The Wireless Router
Wireless routers typically provde WLAN Security, DHCP Services, integrated Name Address Translation (NAT), and quality of service (QoS), & other features.


## 13.1.3 - Log in to the Wireless Router
## 13.1.4 - Basic Network Setup
1. log in to the router from a web browser
2. change the default admin password
3. log in with new password
4. change default DHCP IPv4 addresses
5. renew the IP addr
6. log into the router with new IP addr

## 13.1.5 - Basic Wireless Setup
1. view the WLAN defaults
2. change the network mode
3. configure the SSID 
4. configure the security mode
5. configure the passphrase

## 13.1.6 - Configure a Wireless Mesh Network
## 13.1.7 - NAT for IPv4
## 13.1.8 - Quality of Service
these settings may be under **Bandwidth Control** or something similar

## 13.1.9 - Port Forwarding
port triggering lets you forward data to a computer only when a designated port range is used to make an outbound request.


# 13.2 - Configure a Basic WLAN on the WLC
## 13.2.1 - Video
## 13.2.2 - WLC Topology
Controller based APs (aka Lightweight APs) requre no initial configuration and use the Lightweight Access Point Protocol (LWAPP) to communicate with a WLAN Controller (WLC).

## 13.2.3 - Log in to the WLC
Just like logging into a router

## 13.2.4 - View AP Information
Once logged in click **Access Points** to view overall picture of the AP's system

## 13.2.5 - Advanced Settings
click advanced settings tab...

## 12.2.6 - Configure a WLAN
Wireless LAN Controllers have ports and interfaces.
Ports are sockets for physical connections while Interfaces are virtual, created in software and similar to VLAN interfaces.

Each interface that will carry traffic from a WLAN is configured on the WLC as a different VLAN.

The Cisco 3504 WLC can support 150 APs and 4096 VLANs but only has 5 physical ports.

Basic WLAN configuration steps:
1. Create the WLAN
2. Apply and Enable the WLAN - after wlan is created it must be enabled before it can be accessed.
3. Select the Interface - you must select the interface that will carry the WLAN traffic.
4. Secure the WLAN - WPA2/802.1X
5. Verify the WLAN is Operational - click *WLANs* in the menu to view the new WLAN.
6. Monitor the WLAN - *Monitor* tab
7. View Wireless Client Details - *clients* tab

# 13.3 - Configure a WPA2 Enterprise WLAN on the WLC
## 13.3.2 - SNMP & RADIUS
SNMP - Simple Network Management Protocol
RADIUS - Remote Authentication Dial-In User Service

**The steps** in the next couple sections should all be followed from the WLC management interface.

## 13.3.3 - Configure SNMP Server Information
1. click MANAGEMENT
2. click SNMP
3. click Trap Receivers
4. click New
5. enter the SNMP Community name and the IP address for the SNMP server
6. click Apply

## 13.3.4 - Configure RADIUS Server Information
1. click SECURITY
2. click RADIUS
3. click Authentication
4. click New
5. enter the IP address for PC-A (the pc running the RADIUS server) and the shared secret. Shared secret is the password used between WLC & RADIUS server, not for users.
6. click Apply

after clicking apply the list of configured RADIUS Authentication Servers refreshes with the new server listed.

## 13.3.5 - Video - Configure a VLAN for a New WLAN
## 13.3.6 - Topology with VLAN 5 Addressing
Each WLAN configured on the WLC needs its own virtual interface.

## 13.3.7 - Configure a New Interface
1. Create a new interface - click CONTROLLER > interfaces > New
2. Configure the VLAN name & ID - click Apply after entering the info
3. Configure the port and interface address - the edit page will show. Configure the physical Port number, then configure the VLAN 5 interface addressing.
4. Configure the DHCP server address
5. click Apply & click OK
6. click Interfaces (under Controller) and verify the interfaces

## 13.3.8 - Video - Configure a DHCP Scope
DHCP Scope - similar to DHCP pool. Allows us to provide management addresses to management devices using DHCP.

## 13.3.9 - Configure a DHCP Scope
1. Create a new DHCP scope - in Controller, click Internal DHCP Server > DHCP Scope > New
2. Name the DHCP scope & click Apply
3. Verify the new DHCP Scope - you should be returned to the DHCP Scopes page. Click the scope name to configure it.
4. Configure & enable the new DHCP scope - set the network settings and ensure that Status is enabled. Click Apply
5. Verify the enable DHCP scope - check the address pool on the DHCP Scopes page

## 13.3.10 - Video - Configure a WPA2 Enterprise WLAN
## 13.3.11 - Configure a WPA2 Enterprise WLAN
Newly created WLANs on the WLC will use WPA2 with AES by default. And 802.1X as the default key management protocol used to communicate with the RADIUS server.

1. Create a new WLAN - click WLANs tab > then click Go (next to create new)
2. Configure the WLAN name & SSID - fill in the WLAN info & click Apply
3. Enable the WLAN for VLAN 5 - change the status to enabled, choose VLAN 5 from Interface/Interface Group(G) dropdown, click apply, & then OK
4. Verify AES & 802.1X defaults - click Security tab to view default config for the new WLAN
5. Configure the RADIUS server - still under security tab click AAA servers select the RADIUS server we previously configured & click apply
6. Verify that the new WLAN is available - click Back or the WLANs submenu on the left. 


# 13.4 - Troubleshoot WLAN Issues
## 13.4.1 - Troubleshooting Approaches
| Step | Title | Description |
| --- | --- | --- |
| 1 | Identify the Problem | a conversation with the user is often helpful |
| 2 | Establish a theory of probable causes | try to establish a theory of probable causes |
| 3 | Test the theory to determine cause | Test your theories to determine which caused the problem |
| 4 | Establish a plan of action to resolve the problem & implement the solution | yeah. do what the title says |
| 5 | Verify full system functionality & implement preventative measures | |
| 6 | Document findings, actions, & outcomes | Important for future reference |


## 13.4.2 - Wireless Client not connecting
If no connectivity check:
* confirm network config on the pc using *ipconfig*
* confirm that the device can connect to the wired network
* if necessary reload drivers, maybe try a different NIC
* if client's wireless NIC is working check the security mode & encryption settings on the client

If client is connected but the connection is performing poorly check:
* how far from the AP is the PC?
* check the channel settings on the wireless client
* check for presence of other devices in the area that may be interferring

## 13.4.3 - Troubleshooting when the Network is Slow
* Upgrade your wireless clients - older 802.11b, 802.11g, and even 802.11n devices can slow the entire WLAN
* Split the Traffic - split traffic between 2.4Ghz & 5Ghz bands

## 13.4.4 - Updating Firmware
