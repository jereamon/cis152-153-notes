Module 10 - Network Management

# 10.1 -- Device Discovery with CDP
## 10.1.1 -- CDP Overview
Cisco Discovery Protocol (CDP) -- Proprietary, layer 2, media & protocol independent, runs on all Cisco devices

## 10.1.2 -- Configure & Verify CDP
`show cdp` -- verify status of CDP & display info about it.
`cdp run` -- global config command to enable CDP globally
`no cdp run` -- to disable it globally
`cdp enable` -- from interface config to enable/disable cdp on an interface
`show cdp neighbors` -- displays info about cdp neighbors
`show cdp neighbors detail` -- shows more info including neighbor ip addr
`show cdp interface` -- displays the interfaces that are cdp enabled & their status


## 10.1.3 -- Discover Devices by Using CDP
`show cdp neighbors` will show:
* device identifiers -- host name of the neighbor device
* port identifer -- name of the local & remote port
* capabilities list -- shows whether device is router or switch (r or s)
* platform -- the hardware platform of the device


# 10.2 -- Device Discover with LLDP
## 10.2.1 -- LLDP Overview
Link Layer Discovery Protocol (LLDP) -- Vendor neutral, does same thing as CDP but not cisco specific.


## 10.2.2 -- Configure & Verify LLDP
`lldp run` -- global config command to enable lldp globally
`no lldp run` -- disable it globally

when configured on an interface lldp must be configured separately to transmit & receive:
`lldp transmit` -- interface config to transmit
`lldp receive`  -- interface config command to receive

`show lldp` -- lldp information


## 10.2.3 -- Discover Devices by Using LLDP
`show lldp neighbors` -- shows same info as `show cdp neighbors`
`show lldp neighbors detail` -- shows more detail than ^^



## 10.3 -- NTP
## 10.3.1 -- Time & Calendar Services
it is important that the time on all devices are synchronized.
You can set the time manually or use **Network Time Protocol (NTP)**

NTP uses UDP port 123, documented in RFC 1305

## 10.3.2 -- NTP Operation
NTP hierarchy different levels are called strata. Strata 0 clocks are the most accurate. The max hop count is 15 (starting from 0). A stratum 16 device is unsynchronized.

* Stratum 0 -- atomic & GPS clocks, most accurate, non-network high-precision timekeeping devices.
* Stratum 1 -- network devices directly connected to authoritative time sources.
* stratum 2 & below -- stratum 2 servers sync their time using NTP packets from stratum 1 servers. These may act as servers for stratum 3 devices.

## 10.3.3 -- Configure and Verify NTP
`show clock` -- shows the current time
`show clock detail` -- also shows the time source (user configured or NTP)

`ntp server ip-address` -- global config command sets the ntp server
`show ntp associations` -- shows the server providing the time & that server's stratum (st)
`show ntp status` -- shows the devices stratum and lots of other info

once ntp is set up on a local router that can be used as a server for our other network devices.



# 10.4 -- SNMP
## 10.4.1 -- Introduction to SNMP
SNMP -- application layer protocol. Polls the agent & queries the MIB for agents on UDP port 161. SNMP agents send 'traps' on UDP port 162.

SNMP system consists of 3 elements:
* SNMP Manager
	* part of a network management system (NMS)
	* uses **'get'** action to collect info from SNMP agent
	* uses **'set'** action to change config on an agent
	* agents forward info to a network manager using **'traps'**
* SNMP Agent (managed node)
* Management Information Base (MIB) -- stores data about the device it resides on

The agent & MIB reside on client devices that need managing.


## 10.4.2 -- SNMP Operation
Get & Set requests:
| Operation | Description |
| --- | --- |
| get-request | Retrieves a value from a specific variable |
| get-next-request | gets value from variable w/in a table. Doesn't need the exact variable name, performs sequential search to find variable from w/in a table. |
| get-bulk-request | get large blocks of data that would otherwise require transmission of many small blocks (ex: multiple row in a table). SNMPv2 & later only. |
| get-response | replies to the above requests |
| set-request | stores a value in a specific variable |


## 10.4.3 -- SNMP Agent Traps
NMS periodically polls SNMP agents for info. Since events could occur and not be noticed until the next poll agents are able to send traps to inform the NMS immediately of certain events.


## 10.4.4 -- SNMP Versions
* SNMPv1 -- full internet standard defined in RFC 1157. Legacy
* SNMPv2c:
	* defined in RFCs 1901 to 1908. 
	* uses a community-string-based Administrative Framework (defined below). 
	* noAuthNoPriv -- doesn't offer authentication or encryption
	* offers bulk retrieval mechanism & more detailed error message reporting, unlike SNMPv1
	* improved error handling, expanded error codes.
* SNMPv3:
	* an interoperable standards based protocol. 
	* Defined in RFCs 2273 to 2275. 
	* Provides for security models & security levels (defined below)
	* Provides secure access by authenticating & encrypting packets. 
	* message integrity reporting, authentication of source, & encryption of messages.


community string -- defines the community of managers able to access the MIB of an agent.


security model -- authentication strategy for a user & their group.
security level -- permitted level of security within a security model.
^^ these two determine which security mechanism is used when handling an SNMP packet.


SNMPv3 offers:
* noAuthNoPriv -- authentication uses a username, no password, no encryption.
* authNoPriv -- authentication based on HMAC-MD5 or HMAC-SHA algorithms. no encryption.
* authPriv -- same auth as ^^. Encryption via DES or AES.


## 10.4.6 -- Community Strings
Community strings -- used by SNMPv1 & v2c, are plaintext passwords which authenticate access to MIB objects

2 types of community strings:
* Read-only (ro) -- provides access to MIB variables but doesn't allow them to be changed.
* Read-write (rw) -- these provide read & write access to all MIB variables


## 10.4.7 -- MIB Object ID
Object ID (OID) -- MIB defines each variable as an OID.

The MIB is organized based on RFC standards as a hierarchy of OIDs.
the MIB tree includes variables common to many networking devices & vendor specific ones.

OID can be described in words or numbers.


## 10.4.8 -- SNMP Polling Scenario
Using SNMP we can observe CPU utilization over a period of time to create a baseline for expected usage.

To get this polling data use the:
`snmpget` utility

This utility can manually retrieve real time data, or have the NMS run a report over a period of time.

Example usage:
`snmpget -v2c -c community 10.250.250.14 .1.3.6.1.4.1.9.2.1.58.0`
The above includes:
* -v2c -- the version of SNMP
* -c community -- the SNMP password, a community string
* 10.250.250.14 -- the ip addr of the monitored device
* 1.3.6.1.4.1.9.2.1.58.0 -- the fucking OID of MID variable. wtf.

The response to that command would be a five minute average of CPU use percentage represented as an integer.


## 10.4.9 -- SNMP Object Navigator
Working with long MIB variable names (OIDs) can be problematic... No shit.
Typically a GUI is used...



# 10.5 -- Syslog
## 10.5.1 -- Intro to Syslog
syslog -- most common protocol for accessing system log messages. uses UDP port 514.

syslog describes a standard & the protocol developed for that standard. Protocol was originally developed for UNIX in 1980s but was first documented as RFC 3164 by IETF in 2001.

syslog logging service provides 3 primary functions:
* ability to gather logging info
* ability to select the type of logging info that is captured
* ability to specify the destinations of captured syslog messages

## 10.5.2 -- Syslog Operation
popular destinations to have syslog messages sent:
* logging buffer (RAM inside a switch or router)
* console line
* terminal line
* syslog server

You can remotely monitor system messages by viewing logs on a syslog server, or by accessing the device via ssh, telnet, or through the console port.


## 10.5.3 -- Syslog Message Format
smaller numerical level means a message is more critical:
| Severity Name | Severity Level | Description | Definition |
| --- | --- | --- | --- |
| Emergency | 0 | System is unusable | LOG_EMERG |
| Alert | 1 | immediate action needed | LOG_ALERT |
| Critical | 2 | critical conditions exist | LOG_CRIT |
| Error | 3 | error conditions exist | LOG_ERR |
| Warning | 4 | warning conditions exist | LOG_WARNING |
| Notification | 5 | normal but significant | LOG_NOTICE |
| Informational | 6 | informational messages only | LOG_NFO |
| Debugging | 7 | debugging messages | LOG_DEBUG |


* Warning level 4 through 0 -- error messages about software or hardware malfunctions. They mean functionality of the device is affected.
* Notification level 5 -- normal, but significant events. Ex: interface up or down transitions, system restart messages.
* Informational Level 6 -- normal information message. Doesn't affect device functionality. 
* Debugging Level 7 -- messages are output from debug commands


## 10.5.4 -- Syslog Facilities
syslog facilities -- service identifiers that identify & categorize system state data for error & event reporting.

newer cisco switches & routers support **24 facility options** **categorized into 12 facility types**

common syslog facility message codes:
* IF -- was generated by an interface
* IP -- was generated by IP
* OSPF -- was generated by OSPF
* SYS -- was generated by the device OS
* IPSEC -- was generated by IP Security encryption protocol

syslog messages on cisco ios follow this format:
`%facility-severity-MNEMONIC: description`

Example:
`%LINK-3-UPDOWN: Interface Port-channel1, changed state to up`
^^ the facility is LINK, severity level is 3, mnemonic is UPDOWN


## 10.5.5 -- Configure Syslog Timestamp
log messages are not timestamped by default.

`service timestamps log datetime` -- global config command to enable syslog message timestamps



# 10.6 -- Router & Switch File Maintenance
## 10.6.1 -- Router File Systems
Cisco IOS File System (IFS)

`show file systems` -- lists the available file systems, total memory & free memory, type of file system, & its permissions.

Permissions -- read only (ro), write only (wo), read write (rw)
The file system with a * preceding it is the default file system.
The # symbol appended to a listing indicates it is bootable.

`dir` -- command displays contents of a directory.
`cd nvram:` -- changes to nvram directory
`pwd` -- present working directory command


## 10.6.2 -- Switch File Systems
`show file systems` -- same as on router


## 10.6.3 -- Use a Text File to Back up a Configuration
Tera Term can let you save a config file as a text file.
1. in tera term --> file --> log
2. choose the folder location & filename --> click save --> tera term: log window will open & capture all commands & output generated in the terminal window.
3. enter show running-config or show startup-config
4. click Close
5. open the file to verify its contents.


## 10.6.3 -- Use a Text File to Restore a Configuration
copy & paste into a device. IOS will execute each line.

Or, use tera term:
1. file --> Send file
2. locate file & click Open
3. tera term pastes the file into the device


## 10.6.5 -- Use TFTP to Back Up and Restore a Configuration
To save running or startup config to tftp server use either:
`copy running-config tftp` or
`copy startup-config tftp`

This will give some interactive prompts:
1. enter IP addr of the host where the config file will be stored
2. enter name for the file

To restore from a config file on tftp server use either:
`copy tftp running-config` or
`copy tftp startup-config`

Then the same steps as above:
1. enter IP addr of host where file is located
2. enter file name


## 10.6.6 -- USB Ports on a Cisco Router
some routers have usb ports. You can store things on flash drives & plug em in.

`dir usbflash0:` -- for example to see flash drive contents


## 10.6.7 -- Use USB to Back Up and Restore a Configuration
good idea to verify usb drive is there using:
`show file systems`

`copy run usbflash0:/` -- to copy a config file to the usb drive
the router will prompt you for a filename.

to restore from a flash drive:
`copy usbflash0:/file-name running-config`


## 10.6.8 -- Password Recovery Procedures
1. enter ROMMON mode
2. change the configuration register
3. copy the startup-config to the running-config
4. change the password
5. save the running-config as the new startup-config
6. reload the device

Terminal settings to access the device are:
* 9600 baud rate
* no parity
* 8 data bits
* 1 stop bit
* no flow control


## 10.6.9 -- Password Recovery Example
Follows the steps above
1. use the break sequence during boot up to enter ROMMON mode. (break sequence varies based on terminal emulator)
2. use `confreg 0x2142` to set the configuration register. The device will ignore the startup config file. Restart the device with `reset`
3. once loaded, copy the startup-config to the running-config
4. configure necessary passwords
5. `config-register 0x2102` to change the configuration register back & then save the running-config.
6. reload the device with `reload`.



# 10.7 -- IOS Image Management
## 10.7.1 -- video - Managing Cisco IOS Images
## 10.7.2 -- TFTP Servers as a Backup Location
keep back up Cisco IOS files on a tftp server


## 10.7.3 -- Backup IOS Image to TFTP Server Example
From the router:
1. ping the tftp server
2. verify the tftp server has sufficient disk space using `show flash0:` to determine the IOS image file size.
3. Copy the image to the server using `copy source-url destination-url` -- after issuing this command you will be prompted for the file name & ip addr of remote host.
	* example: `copy flash: tftp:`


## 10.7.4 -- Copy an IOS Image to a Device Example
1. ping the tftp server
2. verify the amount of free flash space using `show flash:`
3. copy to the router using `copy tftp: flash:`, you'll be prompted for remote host ip addr & filename.


## 10.7.5 -- The boot system Command
after copying a new IOS image to the router tell the router to boot from it using:
`boot system flash0:long-ass-new-image-filename.bin` -- from global config

`copy running-config startup-config` & `reload`

Once running use `show version` to verify the new image has loaded
