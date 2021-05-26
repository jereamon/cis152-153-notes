Module 13 -- Network Virtualization

# 13.1 -- Cloud Computing
## 13.1.1 -- Video - Cloud & Virtualization
## 13.1.2 -- Cloud Overview
Large number of computers connected through a network that can be located anywhere.

Virtualization is heavily relied upon.

"pay as you go" model lets organizations treat computing and storage more as a utility rather than investing in infrastructure


## 13.1.3 -- Cloud Services
3 Main Cloud Computing Services defined by NIST:
* SaaS -- cloud provider is responsible for access to applications & services, user doesn't manage any of the cloud services except for limited user-specific application settings.
* PaaS -- cloud provider is responsible for providing development tools. Users are typically programmers & may have control over cloud provider's application hosting environment.
* IaaS -- cloud provider gives access to network equipment, virtualized network services, & supporting network infrastructure. User can deploy OSs, applications, etc.

ITaaS - IT as a Service -- support for each of ^^ those services. This can extend network capability without needing to invest in new infrastructure, train anyone, or license new software.


## 13.1.4 -- Cloud Models
4 Primary cloud models:
* Public Clouds    -- apps & services made available to the general population. Either free or pay per use model.
* Private Clouds   -- Apps & services intended for a specific organization. May be setup using their private network, or managed by an outside org with strict access security.
* Hybrid Clouds    -- Made up of 2 or more clouds. Each part remains separate but is connected using a single architecture.
* Community Clouds -- Created for exclusive use by a specific community. 


## 13.1.5 -- Cloud Computing versus Data Center
* Data Center     -- a data storage & processing facility run by an in house IT department or leased offsite.
* Cloud Computing -- an off premise service offering on demand access to a shared pool of configurable computing resources.


# 13.2 -- Virtualization
## 13.2.1 -- Cloud Computing & Virtualization
Virtualization separates the OS from the hardware.


## 13.2.2 -- Dedicated Servers
Enterprise servers used to consist of a server OS, installed on specific hardware.

1 major issue with this is that when a component fails the server becomes unavailable. Another, is that dedicated servers were underused, sitting idle too often.


## 13.2.3 -- Server Virtualization
Multiple OSs can exist on a single hardware platform, taking advantage of idle resources & consolidating the number of required servers.

Redundancy is built into virtualization to ensure no single point of failure.
If hypervisor fails the VM could be started on another, or the VM can run on two hypervisors concurrently, copying RAM & CPU instructions between them.


## 13.2.4 -- Advantages of Virtualization
* Reduced Cost
	* Less equipment required
	* less energy consumed
	* less space required
* Easier prototyping -- self contained labs, operating on isolated networks are easy to spin up & delete
* Faster server provisioning -- creating virtual servers is far faster than provisioning physical ones
* increased server uptime -- more redundancy, better fault tolerance.
* improved disaster recovery -- advanced business continuity solutions. Recovery sites don't have to have identical hardware as the production environment. Failover can be tested in advance.
* Legacy Support -- virtualization ca extend the life of OSs & applications


## 13.2.5 -- Abstraction Layers
Computer systems consist of the following abstraction layers:
* Services
* OS
* Firmware
* Hardware

At each layer programming code is used to interface between the layer below & above

A hypervisor is installed between the firmware & OS


## 13.2.6 -- Type 2 Hypervisors
Type 2 Hypervisor -- Software that creates & runs VM instances.
AKA hosted hypervisor -- run on top of the existing OS.

Popular type 2 hypervisors include:
* Virtual PC
* VMware Workstation
* Oracle VM VirtualBox
* VMware Fusion
* Mac OS X Parallels


# 13.3 -- Virtual Network Infrastructure
## 13.3.1 -- Type 1 Hypervisors
Type 1 Hypervisor is installed directly on the hardware.usually found on enterprise servers & data center networking devices.
^^ have direct access to hardware resources making htme more efficient than hosted architectures (type 2 hypervisors)


## 13.3.2 -- Installing a VM on a Hypervisor
Type 1 Hypervisors require a management console. Management console lets you maanage multiple servers using the same hypervisor & can automatically consolidate servers & power them off and on.

Management console provides recovery from hardware failure, VMs are automatically moved if this occurs.


## 13.3.3 -- The Complexity of Network Virtualization
Server virtualization hides server resources which can create problems if the data center is using traditional network architecture.
One Example: VLANs used by VMs must be assigned the same switch port as the physical server. VMs are movable, & the network admin must be able to add, drop, & change network resources & profiles, a manual & time-consuming task with traditional network switches.

East west traffic   -- between virtual servers in the same data center.
North South Traffic -- between distribution & core layer, typically is traffic destined for offsite locations.


Network infrastructure also benefits from virtualization.
Segment each network device into multiple virtual devices. Examples: subinterfaces, virtual interfaces, VLANs, routing tables.

Virtual routing & forwarding (VRF)


# 13.4 -- Software-Defined Networking
## 13.4.1 -- Video
Software Defined Networking (SDN) components:
* Open Network Foundation
	* OpenFlow -- A protocol that separates the control plane from the forwarding plane. Developed at Stanford to manage traffic between routers, switches, WAPs, & a controller. A basic element in building SDN solutions.
	* OpenStack -- Platform for cloud computing & providing IaaS. A virtualization & orchestration platform. Often used with Cisco ACI. Orchestration - the process of automating & provisioning network components (servers, storage, switches, routers, applications).
* other components
	* Interface to the Routing System (I2RS)
	* Transparent Interconnection of Lots of Links (TRILL)
	* Cisco FabricPath (FP)
	* IEEE 802.1aq Shortest Path Bridging (SPB)

Network Controllers -- SDN control plane device. Programmable point of automation. A hardware device. (OpenDaylight -- open source network controller)
* Cisco ACI -- cisco's approach to network controller. Involves:
	* ANP policy model
	* APIC-EM Controller
	* & programmable switches (Nexus 9000 series)


## 13.4.2 -- Control Plane & Data Plane
* Control plane -- Used to make forwarding decisions. Contains Layer 2 & 3 route forwarding mechanisms (neighbor tables, IPv4 & 6 routing tables, STP, ARP table). Info sent here is **processed by the CPU**
* Data Plane -- aka the forwarding plane. Used to forward traffic flows. Connects the various ports on a device. Takes info from control plane & forwards traffic out appropriate eggress interface. Processed by a special data plane processor, **not the CPU**
* Management Plane -- Admins use; SSH, TFTP, SFTP, HTTPS to access the management plane & configure the device. SNMP uses the management plane.


* Cisco Express forwarding (CEF) -- enables forwarding of packets w/out using the control plane. Control plane's routing table pre-populates the CEF Forwarding Information Base (FIB) table in the data plane. And control plane's ARP table pre-populates the adjacency table. The data plane forwards packets using the FIB & adjacency tables.
* SDN & Central Controller -- control plane is remove from each device and is performed by a centralized controller.


## 13.4.3 -- Network Virtualization Technologies
2 major architectures supporting network virtualization:
* Software-Defined Networking (SDN) -- see 13.4.1
* Cisco Application Centric Infrastructure (ACI) -- purpose built hardware solution for integrating cloud computing & data center management.


## 13.4.4 -- Traditional & SDN Architecture
SDN Controller (last line 13.4.2) -- logical entity that enables admins to manage & dictate how the data plane of switches & routers should handle network traffic.

SDN controller uses northbound APIs to communicate with upstream applications and soutbound APIs to define the behavior of the data planes on downstream switches & routers.

OpenFlow -- the original & widely implemented southbound API (see 13.4.1 as well)


# 13.5 -- Controllers
## 13.5.1 -- SDN Controller & Operations
SDN Controller -- defines the data flows between the centralized control plane & the data planes on individual switches & routers
^^ communicates with OpenFlow compatible switches using TLS

OpenFlow compatible switches have 3 table types:
* Flow Table -- matches incoming packets to a flow & specifies the functions to be performed on them. There may be multiple flow tables operating in a pipeline.
* Group table -- flow table may direct a flow to a group table which may trigger actions affecting one or more flows.
* Meter Table -- triggers variety of performance-related actions on a flow


## 13.5.2 -- Video - Cisco ACI
Cisco ACI -- hardware solution for integrating cloud computing & data center management.


## 13.5.3 -- Core Components of ACI
Cisco Application Centric Infratstructure -- ACI

3 Core Components:
* Application Network Profile (ANP) -- a collection of end-point groups (EPG), their connections, & the policies that define those connections.
* Application Policy Infrastructure controller (APIC) -- brains of the ACI architecture. A centralized software controller that manages & operates a scalable ACI clustered fabric. Designed for programmability & centralized management. Translated application policies into network programming.
* Cisco Nexus 9000 Series switches -- provide an application-aware switching fabric & work with the APIC to manage virtual & physical infrastructure.


## 13.5.4 -- Spine-Leaf Topology
Two tier Spine-leaf topology
* spine switches attach only to leaf switches & core switches
* leaf switches always attach to spines & never to eachother.

This way everything is only one hop from everything else.


## 13.5.5 -- SDN Types
To better understand APIC-EM (Enterprise Module) it is helpful to look at 3 types of SDN:
* Device-Based SDN -- devices are programmable by apps running on the device itself or a server in the network. Example: Cisco OnePK
* Controller-Based SDN -- uses centralized controller which has knowledge of all network devices. Example: Cisco OpenSDN Controller - a commercial distribution of OpenDaylight
* Policy-Based SDN -- similar to controller-based, but includes an additional policy layer operating at a higher level of abstraction. Uses build in applications to automate advanced configurations via guided workflow & GUI. Example: Cisco APIC-EM


## 13.5.6 -- APIC-EM Features
Includes:
* discovering & accessing device & host inventories
* viewing the topology
* tracing a path between endpoints
* setting policies


## 13.5.7 -- APIC-EM Path Trace
Lets an admin easily visualize traffic flows & discover conflicting, duplicate, or shadowed ACL entries.

It examines specific ACLs on the path between two end nodes displaying potential issues.