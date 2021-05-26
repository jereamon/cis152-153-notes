Module 11 -- Network Design

# 11.1 -- Hierarchical Networks
## 11.1.1 -- Video - Three Layer Network Design
## 11.1.2 -- The need to scale the Network
Enterprise Networks must be able to:
* Support critical applications
* support converged network traffiic
* support diverse business needs
* provide centralized administrative control


## 11.1.3 -- Borderless Switched Networks
Cisco Borderless Network -- Allows for networks that can connect anyone, anywhere, anytime, on any device; securely reliably, & seamlessly. Provides 2 primary sets of services: network services & user endpoint services.


## 11.1.4 -- Hierarchy in the Borderless Switched Network
Borderless Switched Network design guidelines & principles:
* Hierarchical -- the design facilitates understanding the role of each device at every tier, simplifies deployment, operation, & management, & reduces fault domains at every tier.
* Modularity -- The design allows seamless network expansion & integrated service enablement on an on-demand basis.
* Resiliency -- the design satisfies user expectations for keeping the network always on.
* Flexibility -- the design allows intelligent traffic load sharing by using all network resources.

3 critical layers within both 2 & 3 tiered designs:
* access layer
* distribution layer
* core layer

in 2 tier the distribution & core layers are combined. In 3 tier design they are separate (2 layers of switches rather than 1)


## 11.1.5 -- Access, Distribution, and Core Layer Functions
* access layer -- represents network edge, where traffic enters or exits the campus network. Primary function is to provide access to the user.
* Distribution Layer -- interfaces between access layer & core layer. Provides many functions including:
	* aggregating large scale wiring closet networks
	* aggregating Layer 2 broadcast domains & layer 3 routing boundaries
	* Providing intelligent switching, routing, & network access policy functions to access the rest of the network.
	* Providing high availability -- redundant distribution layer switches to the end user & equal cost paths to the core.
	* Providing differentiated services to various classes of applications at the edge of the network.
*  Core Layer -- The network backbone. Aggregator for all distribution layer devices & ties the campus together with the rest of the network. Primary purpose is to provide fault isolation & high speed backbone connectivity.

## 11.1.6 -- Three-Tier & Two-Tier Examples
* 3 tier -- core & distribution layers are separate. Recommendation is to build an extended star physical topology network from a centralized building to all other buildings on the campus.
* 2 tier -- combined core & distribution layers. Used on smaller campus locations w/fewer users, or sites consisting of a single building. AKA - collapsed core network.

## 11.1.7 -- Role of Switched Networks
Not long ago flat Layer 2 switched networks were the norm. They relied on ethernet & many hub repeaters to propagate lan traffic.



# 11.2 -- Scalable Networks
## 11.2.1 -- Design for Scalability
Scalability -- a network that can grow without losing availability & reliability.

Design strategy recommendations:
* use expandable, modular equipment, or clustered easilsy upgradable devices.
* design a hierarchical network including easily added, upgraded, or modified modules that won't affect the design of othher areas of the network.
* create an IPv4 & 6 hierarchical addressing strategy. Allow room for growth without having to re-address.
* choose routers & multilayer switches to limit broadcasts & filter unwanted trafffic.

Advanced network design requiremets:
* redunant links -- ensure critical devices have a backup connection
* multiple links -- where possible implement multiple links between devices with EtherChannel or equal cost load balancing (increases bandwidth).
* Scalable routing protocol -- use a scalable routing protocol & use features to isolate routing updates & minimize routing table size (OSPF areas)
* Wireless connectivity -- implement wireless connectivity. Its cool.


## 11.2.2 -- Plan for Redundancy
* minimize possibility of single point of failure by installing duplicate equipment & providing failover services.
* implement redundant paths -- alternate physical paths for data to traverse if one device goes down. Rely on STP to avoid layer 2 loops or just use layer 3 devices!


## 11.2.3 -- Reduce Failure Domain Size
failure domain -- the area of a network affected when a critical device or service experiences problems.

Limit the size of your failure domains!!
The closer the device is to end devices typically the smaller its failure domain will be.
Example:
* a switch connected to 3 hosts, failure domain affects the switch & those hosts.
* a router connected to 3 switches & 9 collective hosts affects all the switches & hosts.

**Usually least expensive** to control failure domain size in the distribution layer.

Switch Block Deployment -- routers or multilayer switches deployed in pairs with access layer switches diivided evenly between them. each switch block acts independently of the others creating small failure domains.


## 11.2.4 -- Increase Bandwidth
Link aggregation such as EtherChannel -- can increase bandwidth without requiring new devices.


## 11.2.5 -- Expand the Access Layer
Wireless connectivity -- allows for many connected devices increasing flexibility, reducing costs, & giving the ability to adapt & grow.


## 11.2.6 -- Tune Routing Protocols
Ensure protocols like OSPF are tuned (configured) to function to the best of their ability.
Use multiple goddamn OSPF areas on large networks!


# 11.3 -- Switch Hardware
## 11.3.1 -- Switch Platforms
There are many switch platforms to choose from:
* Campus LAN Switches -- include core, distribution, access, & compact switches ranging from fanless 8 fixed port switches to 13 blade switches which support hundreds of ports. Platforms include:
	* Cisco 2960, 3560, 3650, 3850, 4500, 6500, & 6800 series
* Cloud Managed Switches -- Cisco Meraki cloud-managed access switches enable virtual stacking of switches. Thousands of switch ports are monitored & configured over the web.
* Data Center Switches -- Cisco Nexus Series - promotes scalability & operational continuity
* Service Provider Switches -- 2 categories:
	* Aggregation Switches -- carrier grade ethernet switches which aggregate traffic at the edge of a network.
	* Ethernet access switches -- feature application intelligence, unified services, virtualization, integrated security, & simplfied managment.
* Virtual Networking -- Cisco Nexus virtual networking switch platforms provide secure multi-tenant services by adding virtualization intelligence technology to the data center network.


## 11.3.2 -- Switch Form Factors
* fixed configuration switches -- can't add modules to these.
* modular configuration switches -- chassis accept field-replaceable line cards.
* Stackable configuration switches -- special cables allow stacked switches to operate as one large switch.
* Thickness -- expressed in number of rack units. Fixed configuration switches are all one rack units (1U, 1.75in, 44.45mm) in height.

## 11.3.3 -- Port Density
port density -- number of ports available on a single switch.


## 11.3.4 -- Forwarding Rates
Forwarding rates -- the processing capabilities of a switch based on how much data it can process per second.
Wire speed -- the data rate each ethernet port on a switch is capable of attaining. (100Mbps, 1Gbps, etc..)

So a 48-port gigabit switch operating at full wire speed generates 48 Gbps of traffic. If it only supports a forwarding rate of 32 Gbps it cannot run at full wire speed across all ports simultaneously.


## 11.3.5 -- Power over Ethernet
Switches that support PoE are expensive.
* PoE switch ports look just like any other port. Check the model of the switch to see if PoE is supported.
* IP phones often accept PoE
* WAPs do too
* Catalyst 2960-C & 3560-C support PoE pass-through. PoE devices connected to the switch can be powered & the switch itself can draw power from certain upstream switches.


## 11.3.6 -- Multilayer Switching
Multilayer switches -- typically deployed in core & distribution layers. Builds a routing table & can forward IP packets nearly as fast as Layer 2 forwarding.

There is a trend toward all switches being layer 3 switches.

Catalyst 2960 supports Lyaer 3 switching. Prior to IOS version 15.x they only supported 1 SVI interface. Now, after 15.x they support multiple active SVIs.


## 11.3.7 -- Business Considerations for Switch Selection
Common Considerations:
* cost -- depends on number & speed of interfaces, features, & expansion capability.
* port density
* power -- PoE or no
* Reliability
* Port Speed
* Frame buffers -- how many frames the switch can store during congestion.
* scalability


# 11.4 -- Router Hardware
## 11.4.1 -- Router Requirements
In addition to routing, routers:
* limit broadcasts to the lan
* interconnect geographically separated locations
* group users logically by application or department
* provide enhance security by filtering traffic


## 11.4.2 -- Cisco Routers
Categories of Cisco routers:
* Branch Routers -- optimize branch services & deliver optimal aplication experience accross branch & WAN infrastructures. Designed for 24x7x365 uptime. Include: Cisco ISR 4000
* Network Edge Routers -- unite campus, data center, & branch networks with high-performance, highly secure, & reliable services. Include: Cisco ASR 9000
* Service Provider Routers -- deliver end-to-end scalable solutions & subscriber aware services. Include: Cisco NCS 6000
* Industrial -- provide enterprise class features in rugged & harsh environments. Include: Cisco 1100


## 11.4.3 -- Router Form Factors
* 900 Series -- small branch office router. Combines WAN, switching, security, & advanced connectivity options. Fanless.
* ASR 9000 & 1000 series -- big. provide density, resiliency, and programmability for scalable network edge.
* 5500 series -- designed to efficiently scale between large data centers & large enterprise networks.
* cisco 800 -- compact & designed for harsh environments.

routers can also be fixed configuration or modular like switches.