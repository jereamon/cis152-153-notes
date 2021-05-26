Module 2 - Switching Concepts

# 2.1 Frame Forwarding
## 2.1.1 Switching in Networking
2 terms:
* ingress - port where a frame enters a device
* egress - port used when frames leave a device

## 2.1.2 Switch MAC Address Table
MAC table is stored in **CAM (Content Addressable Memory)**
CAM = special memory used for high speed searching

## 2.1.3 Switch Learn and Forward Method
2 steps performed on every frame that enters a switch:
1. Learn - add MAC address if it isn't in the table, refresh the timer on that entry if it is.
2. Forward - send the frame out the port associated with its destination MAC. (If MAC is unknown forward out all ports [unknown unicast])

## 2.1.5 Switching Forwarding Methods
## 2.1.6 Store and Forward Switching
Does:
* Error checking - recieves entire frame and compares to the frame check sequence (FCS).
* Automatic Buffering - ingress port buffering provides flexibility for a range of ethernet speeds.

## 2.1.7 Cut-Through Switching
* may forward invalid frames since there is no error check
* low latency
* can't deal with different ethernet port speeds
* **fragment free** modified form of cut-through that provides better error checking

\

# 2.2 Collision and Broadcast Domains
## 2.2.1 Collision Domains
collision domains - network segements that share bandwidth.
if a switch is operating in half-duplex each segment would be it's own collision domain.

## 2.2.2 Broadcast Domains
broadcast domain - a collection of interconnected switches. routers segment broadcast domains and collision domains.

## 2.2.3 Alleviate Network Congestion
full duplex is required for 1 Gbps & higher ethernet speeds.
Fast port speeds, fast internal switching, large frame buffers, & high port density on switches reduce network congestion.
*high port density* lowers overall cost since fewer switches are needed.