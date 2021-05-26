Module 9 - QoS Concepts

# 9.1 -- Network Transmission Quality
## 9.1.1 -- Video Tutorial - The Purpose of QoS
QoS lets you prioritize certain types of traffic over others.

QoS is needed at points in the network where there is congestion:
* Where there is aggregation <-- multiple communication lines going to a single device
* A speed mismatch between interfaces
* going from lan to wan

No QoS can result in Delay Jitter & Packet Loss

## 9.1.2 -- Prioritizing Traffic
when there is more traffic than the network can process devices queue (hold) packets in memory until resources are available.
If there are too many packets even for the queue they are dropped.

One solution is to classify the data into multiple queues.


## 9.1.3 -- Bandwidth, Congestion, Delay, and Jitter
There can be fixed delays & variable delays. A fixed delay is the specific amount of time a process takes.

Sources of delay:
| Delay | Description |
| --- | --- |
| Code delay | fixed time to compress data at the source before transmitting to the first internetworking device, usually a switch |
| Packetization Delay | fixed time to encapsulate a packet with all necessary header info |
| Queuing delay | variable amount of time a frame or packet waits to be transmitted on the link |
| Serialization delay | fixed amount of time to transmit a frame onto the wire |
| Propagation delay | variable amount of time for frame to travel between the source & destination |
| De-jitter delay | fixed time to buffer a flow of packets & send them out in evenly spaced intervals |


## 9.1.4 -- Packet Loss
Playout delay buffer -- compensates for jitter. It buffers incoming packets & plays them out in a steady stream.

When jitter is too large for the buffer out of range packets are discarded & dropouts are heard in the audio.


# 9.2 -- Traffic Characteristics
## 9.2.1 -- Video Tutorial - Traffic Characteristics
## 9.2.2 -- Network Traffic Trends
According to Cisco, by 2022 video will represent 82% of all internet traffic & mobile video will reach 60.9 exabytes per month by 2022.

## 9.2.3 -- Voice
Voice traffic is predictable & Smooth
* Voice traffic characteristics
	* Smooth
	* benign
	* drop sensitive
	* delay sensitive
	* UDP priority
* One-Way Requirements
	* latency <= 150ms
	* Jitter <= 30ms
	* Loss <= 1% Bandwidth 
	* Requires 30 - 128 Kbps


## 9.2.4 -- Video
UDP port 554 -- Real-Time Streaming Protocol (RSTP)
* Video Traffic Characteristics
	* bursty
	* greedy
	* drop sensitive
	* delay sensitive
	* UDP priority
* One-Way Requirements
	* Latency <= 200-400ms
	* Jitter <= 30-50ms
	* Loss <= .1-1%
	* Bandwidth 384 Kbps - 20 Mbps


## 9.2.5 -- Data
Data applications have no tolerance for data loss. Use UDP or TCP.
* Data Traffic Characteristics
	* smooth or bursty
	* benign or greedy
	* drop insensitive
	* delay insensitive
	* TCP retransmits

Factors for Data Delay:
| Factor | Mission Critical | Not Mission Critical |
| --- | --- | --- |
| interactive | prioritize the lowest delay of all data traffic & strive for a 1 to 2 second response time. | applications benefit from lower delay |
| not interactive | delay can vary greatly as long as minimum necessary bandwidth is supplied | gets leftover bandwidth after voice, video, & other data application needs are met. |


## 9.3 -- Queuing Algorithms
## 9.3.1 -- Video Tutorial - QoS Algorithms
## 9.3.2 -- Queuing Overview
lists the queuing algorithms we'll be focusing on. Detail below

## 9.3.3 -- First In First Out
FIFO -- no concept of priority or classes of traffic. First packet to arrive is the first packet sent out. fastest method of queuing. best for large links with little delay & congestion because packets will be dropped if there is congestion.

all interfaces except serial interfaces at E1 (2.048Mbps) and below use **FIFO by default**


## 9.3.4 -- Weighted Fair Queuing (WFQ)
WFQ -- applies priority to identified traffic & classifies it into conversations.
WFQ simultaneously schedules interactive traffic to the front of a queue & fairly shares the remaining bandwidth among high bandwidth flows. 

WFQ classifies traffic based on packet header addressing. (source & dest ip addrs, mac addrs, port numbers, protocol, & type of service (ToS) value)

WFQ is not supported with tunneling & encryption


## 9.3.5 -- Class-Based Weighted Fair Queuing (CBWFQ)
CBWFQ -- extends WFQ to provide user-defined traffic classes.

You define traffic classes based on match criteria including: protocols, ACLs, input interfaces, etc.

once a class is defined, it is assigned characteristics: bandwidth, weight, maximum packet limit, queue limit.

once more packets have arrived than the queue limit allows new packets are dropped.


## 9.3.6 -- Low Latency Queuing (LLQ)
LLQ -- brings strict priority queuing (PQ) to CBWFQ.
Strict PQ -- allows delay-sensitive packets to be sent before packets in other queues.


Cisco recommends only voice traffic be directed to the priority queue.


## 9.4 -- QoS Models
## 9.4.1 -- Video Tutorial - QoS Models
## 9.4.2 -- Selecting an Appropriate QoS Policy Model
3 models available:
* best effort
* integrated services (IntServ)
* differentiated services (DiffServ)

## 9.4.3 -- Best Effort
all packets are treated the same.
| Benefits | Drawbacks |
| --- | --- |
| most scalable model | no guarantee of delivery |
| scalability is only limited by bandwidth | packets arrive whenever they can if at all |
| no special QoS mechanisms required | no packets have preferential treatment |
| easiest & quickest model to deploy | critical data treated the same as non-critical |


## 9.4.4 -- Integrated Services
IntServ -- from RFC 1633, 2211, & 2212 developed in 1994.
uses a connection-oriented approach delivering end-to-end QoS that real time applications require.

^^ like hard QoS, where applications signal their QoS needs to the network

Resource Reservation Protocol (RSVP) -- used by IntServ to signal along the end-to-end path determining if there is sufficient bandwidth for the application.

| Benefits | Drawbacks |
| --- | --- |
| explicit end-to-end resource admission control | resource intensive |
| per request policy admission control | flow based approach is not scalable |
| signaling of dynamic port numbers | |


## Differentiated Services
DiffServ -- simple & scalable, described in RFCs 2474, 2597, 2598, 3246, 4594

uses a "soft QoS" approach, hosts don't signal QoS needs to the network.
Traffic is classified & routers provide the appropriate QoS for those classes.

Example difference between IntServ & DiffServ:
* DiffServ -- might group all TCP flow into a single class & allocate bandwidth for that class
* Intserv -- would allocate bandwidth for individual flows

| Benefits | Drawbacks |
| --- | --- |
| highly scalable | no absolute guarantee of service |
| provides many levels of quality | requires a complex set of mechanisms to work |


# 9.5 -- QoS Implementation Techniques
## 9.5.1 -- Video Tutorial - QoS Implementation Techniques
## 9.5.2 -- Avoiding Packet Loss
These approaches can prevent drops in sensitive applications:
* increase link capacity
* guarantee enough bandwidth & increase buffer space. WFQ, CBWFQ, & LLQ can guarantee bandwidth.
* drop lower priority packets before congestion occurs.

## 9.5.3 -- QoS Tools
3 categories of QoS tools to be described:
* Classification & marking tools
* congestion avoidance tools
* congestion management tools

## 9.5.4 -- Classification and Marking
only after traffic is marked can policies be applied to it. Marking a packet adds a value to its header.

classifying traffic at layers:
* 2 & 3 -- done using interfaces, ACLs, & class maps
* 4 to 7 -- using Network Based Application Recognition (NBAR)

### Traffic marking for QoS:
| QoS Tools | Layer | Marking Field | Width in Bits |
| --- | --- | --- | --- |
| Ethernet (802.1Q, 802.1p) | 2 | Class of Service (CoS) | 3 |
| 802.11 (Wi-Fi) | 2 | Wi-Fi Traffic Identifier (TID) | 3 |
| MPLS | 2 | Experimental (EXP) | 3 |
| IPv4 & IPv6 | 3 | IP Precedence (IPP) | 3 |
| IPv4 & IPv6 | 3 | Differentiated Services Code Point (DSCP) | 6 |


## 9.5.5 -- Marking at Layer 2
802.1Q -- IEEE standard, supports vlan tagging at layer 2 on ethernet networks. adds two fields to the frame after the source MAC addr field.

802.1Q also includes 802.1p for QoS prioritization.
802.1p -- Priority (PRI) field uses the 1st 3 bits in the Tag Control Information (TCI) field. 3 bits means 8 possible levels of priority.

Ethernet CoS Values:
| CoS Value | CoS Binary Value | Description |
| --- | --- | --- |
| 0 | 000 | Best-Effort Data |
| 1 | 001 | Medium-Priority Data |
| 2 | 010 | High-Priority Data |
| 3 | 011 | Call Signaling |
| 4 | 100 | Videoconferencing |
| 5 | 101 | Voice bearer (voice traffic) | 
| 6 | 110 | reserved |
| 7 | 111 | reserved |


## 9.5.6 -- Marking at Layer 3 
IPv4 & 6 have an 8-bit field in their headers to mark packets.
IPv4 has Type of Service (ToS) field
IPv6 has Traffic Class field


## 9.5.7 -- ToS & Traffic Class fields
The ToS (IPv4) & Traffic Class (IPv6) fields are 8 bits.
Previously, RFC 791 specified only 3 bits for marking. 
RFC 2474 superceded this & specifies 6 bits for marking with the final 2 being used for Explicit Congestion Notification (ECN). 

The 6 bits are called Differentiated Services Code Point (DSCP) -- 6 bits offers 64 levels

ECN bits allow ECN aware routers to inform downstream routers of congestion in the packet flow.


## 9.2.8 -- DSCP Values
the 64 DSCP values have 3 categories:
* Best effort -- default for IP packets. DSCP value is 0. no QoS plan is implemented, if there is congestion, they are dropped.
* Expedited Forwarding (EF) -- RFC 3246 defines EF as the DSCP decimal value of 46 (101110). Cisco recommends only voice traffic is marked with EF.
* Assured Forwarding (AF) -- RFC 2597 defines AF to use the 5 most significant DSCP bits to indicate queues & drop preference.


AF values are described like AFxy (AF42 for example)
The 4 represents the class (uses the first 3 bits):
	* 4 classes, class 4 is best queue, 1 is worst. (only 4 classes even though 3 bits offer 7 possibilities)
The 2 represents the drop value (uses the next 2 bits):
	* 3 drop values, low (01), medium (10), & high (11). 
* the 6th, final bit is always 0

so, AF42 as binary would be:
100100 <-- class 4 (3 bits), medium drop (next 2 bits), final zero bit.

AF13 would be:
001110 <-- class 1, high drop, final zero bit

AF31 would be:
011 | 01 | 0 <-- class 3, low drop, final zero bit
011010


## 9.5.9 -- Class Selector Bits
CS bits -- first three bits in the DSCP field. They map directly to the 3 bits of the CoS (layer 2) field & IPP field to maintain compatibility with 802.1p.


## 9.5.10 -- Trust Boundaries
traffic should be marked as close to its source as possible. The **trust boundary** is where the QoS markings on a packet are trusted as they enter an enterprise network.

* Trusted endpoints can mark application traffic w/either Layer 2 CoS and/or Layer 3 DSCP values. These include: ip phones, WAPs, ip conferencing stations, etc...
* Secure endpoints can have marked traffic at layer 2
* traffic can also be marked at layer 3 switches/routers


## 9.5.11 -- Congestion Avoidance
When traffic gets above a minimum threshold a percentage of the traffic begins to be dropped. If traffic climbs above a maximum threshold all traffic is dropped.

Cisco IOS QoS includes weighted random early detection (WRED) -- Allows TCP traffic to throttle back before buffers are exhausted. This can help avoid tail drops & maximize network use & TCP-based application performance.

There's no congestion avoidance for UDP traffic.


## 9.5.12 -- Shaping & Policing
Cisco IOS QoS provides these:
* Shaping -- an **outbound** concept, excess packets are retained in a queue to be released on a schedule resulting in smooth packet output rate. Requires sufficient memory!
* Policing -- applied to **inbound** traffic. Excess traffic over a defined thresthold is dropped. Commonly used by service provider to enforce a contracted customer information rate (CIR)


## 9.5.13 -- QoS Policy Guidelines
if any devices in the path are using a different QoS policy the whole QoS policy is impacted.

Guidelines:
* enable queing at every device in the path
* classify & mark traffic close to the source
* shape & police traffic flows as close to source as possible

