Module 5 - ACLs for IPv4 Configuration

# 5.1 - Configure Standard IPv4 ACLs
## 5.1.1 - Create an ACL
Complex ACL configuration suggestions:
* use a text editor & write out the specifics of the policy to be implemented
* add the IOS config commands to accomplish those tasks
* include remarks to document the ACL
* copy & paste the commands onto the device
* always thoroughly test an ACL to ensure that it correctly applies the desired policy


## 5.1.2 - Numbered Standard IPv4 ACL Syntax
to create a standard numbered ACL use (from global config):
`access-list access-list-number {deny | permit | remark text} source [source-wildcard] [log]`

Example:
`access-list 20 deny host 192.168.10.10`
`access-list 20 permit 192.168.10.0 0.0.0.255`
`access-list 10 remark ACE permits ONLY host 192.168.10.10 to the internet`

& `no access-list access-list-number` <-- to remove a numbered standard ACL

Standard ACL syntax:
| Parameter | Description |
| --- | --- |
| access-list-number | decimal number of the ACL. Standard ACL number range is 1-99 or 1300-1999 |
| deny | denies access if the condition is matched |
| permit | permits access if condition is matched | 
| remark *text* | (optional) adds text entry for documentation. (max 100 chars) |
| source | identifies source network or host addr to filter. use **any** to specify all networks. use **host** *ip-addr* or just enter the ip-addr to specify a host |
| source-wildcard | 32 bit wildcard mask that is applied to the source. **if omitted** 0.0.0.0 default mask is assumed |
| log | (optional) This should only be implemented for troubleshooting or security reasons. It will generate & send info message each time an ACE is matched. Message includes ACL number, matched condition(permit/deny), source addr, & number of packets. Generated for the first matched packet. |

## 5.1.3 - Named Standard IPv4 ACL Syntax
Named ACLs are better
Use this from global config:
`ip access-list standard access-list-name`
^^^ this will change the prompt to named standard ACL config mode (`R1(config-std-nacl)#`)
Then **add** ACEs with syntax like:
`permit host 192.168.10.10`
`permit 192.168.10.0 0.0.0.255` etc.

use help (`?`) to see named acl options

to remove a named standard ACL:
`no ip access-list standard access-list-name`

ACL names are alphanumeric, **case sensitive**, & must be unique.

## 5.1.4 - Apply a Standard IPv4 ACL
once an ACL is configured it must be linked to an interface or feature.
from interface config use:
`ip access-group {access-list-number | access-list-name} {in | out}`
to bind an ACL (in | out is inbound or outbound)

remove a bound ACL using:
`no ip access-group name-or-number`


## 5.1.5 - Numbered Standard IPv4 ACL Example
to review ACL configuration use:
`show run | section access list`


## 5.1.6 - Named Standard IPv4 ACL Example



# 5.2 - Modify IPv4 ACLs
## 5.2.1 - Two Methods to Modify an ACL
ACls may require a bit of trial & error to achieve the desired filtering result

## 5.2.2 - Text Editor Method
ACLs with multiple ACEs should be created in a text editor first & then pasted into the router interface.

## 5.2.3 - Sequence Numbers Method
Sequence numbers are automatically assigned when an ACE is entered
See these numbers with: `show access-lists`

go into ACL config mode with:
`ip access-list standard name-or-number`
and remove an ACE by its sequence number with:
`no sequence-number` (`no 10`)
Then set the ace by sequence number with:
`10 deny host 192.168.10.10` (or whatever it is supposed to be)

Verify your changes by again running:
`show access-lists`


## 5.2.4 - Modify a Named ACL Example
named ACLs can also use sequence numbers to modify ACEs

go into ACL config mode with:
`ip access-list standard ACL-NAME`

then make the edits just like in 5.2.3


## 5.2.5 - ACL Statistics
`show access-lists` - shows statistics for each statement that has been matched (match count)

`clear access-list counters` - will reset these counters


# 5.3 - Secure VTY Ports with a Standard IPv4 ACL
## 5.3.1 - The access-class Command
ACLs can also be used to secure remote admin access using the vty lines
To do this:
* create an ACL identifying which admin hosts should be allowed access
* apply that ACL to incoming traffic on vty lines

from vty line config use:
`access-class {access-list-number | access-list-name} { in|out }` to apply an ACL
this will pretty much always be an *in* filter

a couple vty line ACL considerations:
* name or numbered ACLs can be applied to vty lines
* all vty lines should have identical restrictions

## 5.3.2 - Secure VTY Access Example
vty config refresher (from gloal config):
`line vty 0 4` --> `login local` --> `transport input telnet-or-ssh`

to apply an acl to vty lines see 5.3.1

## 5.3.3 - Verify the VTY Port is Secured
they connected via telnet and the permitted computer could connect while the other had its connection refused.


# 5.4 - Configure Extended IPv4 ACLs
## 5.4.1 - Extended ACLs
extended ACLs filter on more than just ip addr including source address, destination address, protocol (IP, TCP, UDP, ICMP), and port number

extended ACLs can be created as:
* `access-list access-list-number` -- numbered extended ACL
* `ip access-list extended access-list-name` -- named extended ACL

## 5.4.2 - Numbered Extended IPv4 ACL Syntax
Extended ACLs are created, configured, and then activated on an interface, just like standard ACLs

the actual syntax is a bit more complex though (from global config):
`access-list access-list-number {deny | permit | remark text} protocol source source-wildcard [operator {port}] destination destination-wildcard [operator {port}] [established] [log]`

**Examples**:
`access-list 100 permit tcp any any eq www` <--same (http)
`access-list 100 permit tcp any any eq 80` <--^ same (http)

`access-list 100 permit tcp any any eq 22` <-- ssh
`access-list 100 permit tcp any any eq 443` <-- https

and use:
`no access-list access-list-number` global config command to remove it

It **is not** necessary to use all the keywords when configuring an extended ACL
| Parameter | Description |
| --- | --- |
| access-list-number | decimal number of the ACL. Extended **number range** is 100-199 and 2000-2699 |
| deny | denies access if the condition is matched |
| permit | permits access if the condition is matched |
| remark *text* | (optional) adds text for documentation, max of 100 chars |
| protocol | commonly: ip, tcp, udp, & icmp. ip will match all ip protocols. |
| source | source network or host to filter. can use keywords *any* or *host* |
| source-wildcard | (optional) 32 bit wildcard mask to be applied to source |
| destination | identifies dest. network or host to filter. can use *any* or *host* keyword |
| destination-wildcard | 32 bit wildcard mask to be applied to the destination |
| operator | (optional) compares source & destination ports. operators include: lt, gt, eq, & neq (not equal) |
| port | (optional) decimal number or name of TCP or UDP port |
| established | (optional) for tcp protocol only. 1st gen firewall feature |
| log | (optional) generates & sends messages when ACEs are matched |



**apply extended ACL to interface** (from interface config):
`ip access-group {access-list-numer | access-list-name} {in | out}`
**remove ACL from interface**:
`no ip access-group`

**remove ACL from the router**:
`no access-list`


## 5.4.3 - Protocols & Ports
use:
`access-list 100 permit ?` <-- to view **protocols** that can be specified for an extended ACL

and

`access-list 100 permit tcp any any eq ?` <-- to view **port** names that can be specified for an extended ACL
**NOTE** - the above command will only show tcp ports. Change that to another protocol to view its ports.

## 5.4.4 - Protocols & Port Numbers Configuration Examples
see 5.4.2 for examples

## 5.4.5 - Apply a numbered Extended IPv4 ACL
from interface config:
`ip access-group 110 in`

## 5.4.6 - TCP Established Extended ACL
*established* keyword lets TCP function as a basic stateful firewall. Meaning traffic entering the network will only be allowed if it is in reply to previous outgoing traffic.

Configure an ACL with *established*:
`access-list 120 permit tcp any 192.168.10.0 0.0.0.255 established` <-- here, establised only allows response traffic that originates from the 192.168.10.0/24 network to return. More specifically, it is only permitted if the TCP segment has the ACK or RST flag bits set specifying that it belongs to an existing connection.

So my understanding:
`access-list 100 permit tcp any any established` <-- would allow incoming TCP segments from any network so long as they're part of an already established connection


## 5.4.7 - Named Extended IPv4 ACL Syntax
`ip access-list extended access-list-name` <-- from global config to create it

remember, names are alphanumeric, case sensitive, and must be unique


## 5.4.8 - Named Extended IPv4 ACL Example
## 5.4.9 - Edit Extended ACLs
`show access-lists` <-- to verify configuration


`ip access-list extended ACL-name` <-- to go to ACL config mode

**see section 5.2** for modifying ACLs

## 5.4.11 - Verify Extended ACLs
`show ip interface g0/0/0` <-- verify ACL is applied to an interface & what direction it was applied.

^^ filter the output of that down with:
`show ip interface g0/0/0 | include access list`

`show access-lists` - shows all ACLs and each of their ACEs

`show running-config` - can be used to validate what was configured. This also displays configured remarks.
^^^ **filter it down** with --> `show running-config | begin ip access-list`


