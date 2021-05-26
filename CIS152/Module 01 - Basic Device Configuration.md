Module 1 - Basic Device Configuration

# Module 1 - Basic Device Configuration
## 1.1
### 1.1.1 Switch Boot Sequence

Switch five step boot sequence:

1.  loads a POST that check CPU subsystem, tests CPU, DRAM, and file system.
2.  Loads boot loader software.
3.  Boot loader initializes CPU registers.
4.  Boot loader initializes flash file system.
5.  Booter loader locate and loads the default IOS.

### 1.1.2 Boot system command

Once boot is successful IOS initializes the interfaces using startup-config. startup-config file is called **config.text**

`boot system` command from global config to set BOOT environment variable. ex:  
`boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin1`

`show boot` to see current IOS boot file

### 1.1.3 Switch LED Indicators

- **SYST** \- system LED - if off then no power, if green then all good, if amber then receiving power but not functioning properly.
- **RPS** \- Redundant Power System - if off then RPS is off, if green RPS is ready, if blinking green RPS connected but unavailable, if amber RPS in standby, blinking amber internal power failed & RPS providing power.
- **STAT** \- Port Status - if green a link is present, blinking green there is activity on port, alternating green-amber = link fault, amber port is blocked (usual for 30 seconds after power on), blinking amber port blocked.
- **DUPLX** \- Port Duplex - if off = half-duplex mode, if green = full-duplex.
- **SPEED** \- Port Speed - if off = 10Mbps, if green = 100Mbps, blinking green = 1000Mbps.
- **PoE** \- lots of options. Green is good, amber not so good.

### 1.1.4 Recovering from system crash

boot loader provides access to a switch even if OS cannot be used. Must connect via console connection.

1.  Physically connect PC to switch.
2.  unplug switch's power.
3.  plug in switch and hold down **Mode** button.
4.  Hold Mode button until System led turns amber then solid green.
5.  the `switch:` prompt should appear on the PC.

in boot prompt use `set` to view path of BOOT env variable, and `flash_init` to initialize flash file system.  
`dir flash:` to view dirs and files in flash.  
`BOOT=flash` to change BOOT env variable.  
use `set` again to verify BOOT var,  
then `boot` to load the new IOS.

### 1.1.5 Switch Management Access

switch must have SVI configured with IPv4 or IPv6 address to be remotely accessible.

### 1.1.6 Switch SVI Config Example

from global config enter svi config mode: `interface vlan 99`,  
conf IPv4 addr: `ip address 172.17.99.11 255.255.255.0`,  
conf IPv6 addr: `ipv6 address 2001:db8:acad:99::11/64`,  
enable interface: `no shutdown`, `end`, `copy running-config startup-config`

then, from global config set default gateway:  
`ip default-gateway 172.17.99.1`, exit and save config again.

then verify configuration via:  
`show ip interface brief` and `show ipv6 interface brief`

\

## 1.2 Configure Switch Ports

### 1.2.1 Duplex Communication

### 1.2.2 Configure Switch Ports at the Physical Layer

global config --> chosen interface config mode --> `duplex full` --\> `speed 100`  
Will set the duplex mode and the speed of that interface. (speed and duplex can be set to **auto** as well)

### 1.2.3 Auto MDIX

with this enabled it doesn't matter if you use straight through, crossover, or rollover cables.  
enter interface config mode then --> `mdix auto` to enable auto-mdix

### 1.2.4 Switch Verification commands

Useful commands to verify config:

> `show interfaces [interface id]` \- shows interface status and config  
> `show startup-config`  
> `show running-config`  
> `show flash` \- shows info about flash file system  
> `show version` \- shows hardware & software status  
> `show history` \- shows history of entered commands  
> `show ip interface [interface-id]` \- shows ip info about an interface
> 
> > `show ipv6 interface`

> `show mac-address-table`
> 
> > `show mac address-table`

### 1.2.5 Verify Switch Port Configuration

`show running-config` & `show interfaces [interface id]`

### 1.2.6 Network Access Layer Issues

`show interfaces` \- pay attention to the line and data link protocol status (first line, should show up and up)  
this also shows a count of errors during run time. Common errors:

- input errors - total number of errors, a count of all errors
    - runts - packets discarded because they are too small, (<64 Bytes)
    - giants - packets discarded because they are too big, (>1518 Bytes)
    - CRC - is generated when calculated checksum doesn't match received checksum
- output errors - sum of all errors that prevented final transmission of datagrams
    - collisions - number of retransmitted messages because of ethernet collision
    - late collisions - collisions that occurred after 512 bits of the frame were transmitted

\

## 1.3 Secure Remote Access

### 1.3.1 Telnet Operation

Telnet uses TCP port 23  
unsecure! Use ssh instead.

### 1.3.2 SSH Operation

SSH uses TCP port 22  
Secure! Encrypted

### 1.3.3 Verify the Switch Supports SSH

Switch must be running an IOS version that includes cryptographic features.  
use `show version` to check, if IOS filename includes **k9** it supports cryptography.

### 1.3.4 Configure SSH

1.  `show ip ssh` \- verifies ssh support (command will be unrecognized if it doesn't)
2.  `ip domain-name cisco.com` \- configure the ip domain
3.  `crypto key generate rsa` \- generate rsa key pairs
4.  `username admin secret ccna` \- configure user authentication
5.  1.  `line vty 0 15`
    2.  `transport input ssh`
    3.  `login local`
    
    > to remove vty line password: `no password`
    
    4.  `exit` \- these four commands configure the vty lines

### 1.3.5 Verify SSH is operational

test by trying to login via putty or the terminal

\

## 1.4 Basic Router Confiuration
### 1.4.1 Basic Router Settings
initial router setup:
* `hostname R1` - set the router name
* `enable secret class` - set password for privileged exec mode
* `line console 0` - enter line console config mode
* `password cisco` - set console password
* `login` - require the previously set password
* `exit`
* `line vty 0 4` - enter vty line config mode
* `password cisco`
* `login`
* `exit`
* `service password-encryption`
* `banner motd $DONT ACCESS THIS$`

### 1.4.4 Configure Router Interfaces
* `interface gigabitethernet 0/0/0`
* `ip address 192.168.10.1`
* `ipv6 address 2001:db8:acad:1::1/64`
* `description Link to LAN 1`
* `no shutdown` - turns on the interface

### 1.4.6 IPv4 Loopback Interfaces
`interface loopback [number]` - creates a new loopback interface
then set the ip addr for that loopback interface - `ip address [addr]`

\

## 1.5 Verify Directly Connected Networks
### 1.5.1 Interface Verification Commands
* `show ip interface brief`
* `show running-config interface *interface-id*`
* `show ip route` && `show ipv6 route` - shows routing tables

### 1.5.2 Verify Interface Status
`show ip interface brief` && `show ipv6 interface brief`

###1.5.3 Verify IPv6 Link Local & Multicast Addrs
`show ipv6 interface gigabitethernet 0/0/0`

### 1.5.4 Verify Interface Configuration
`show running-config interface gigabitethernet 0/0/0`

### 1.5.6 Filter Show Command Output
use a **|** after a show command to add filters
* `section` - shows entire section that starts with the filtering expression, EX: `show running-config | section line vty`
* `include` - includes all output lines that match the expression
* `exclude` - opposite of include
* `begin` - shows lines from a certain point starting with the filtering expression

### 1.5.8 Command History Feature
use up arrow to see previously entered commands
`terminal history size 200` - sets the remembered number of commands to 200
`show history` - shows all remembered commands