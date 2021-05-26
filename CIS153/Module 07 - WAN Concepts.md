Module 7 - WAN Concepts

## 7.1 - Purpose of WANs

## 7.1.2 - Private and Public WANs

WANs may be built by a variety of organizations:

- one that wants to connect users in different locations
- an ISP that wants to connect customers to the internet
- an ISP or telecommunications that wants to interconnect ISPs

A private WAN is one dedicated to a single customer.
this provides:
Private WAN infrastructure includes:
\* dedicated point-to-point leased lines, PSTN, ISDN, Ethernet WAN (metroE), ATM, or Frame Relay

- guaranteed service level
- consistent bandwidth
- security

## 7.1.3 - WAN Topologies

physical topologies for WANs are complex. There are too many physical connections to identify.

WAN topologies are described as a logical topology.

WANs logical topology designs:

- Point-to-Point -- often involve dedicated, leased-line connections.
- Hub-and-Spoke -- all communication from 'spoke' routers goes through one 'hub' router.
- Dual-Homed - provides redundancy, two hub routers are dual-homed & redundantly attached to each spoke router
- Fully Meshed - most fault tolerant. every site connected to every other
- Partially Meshed - connects many but not all sites
    large networks are usually a combination of ^^ those topologies

## 7.1.4 -- Carrier Connections

SLA - Service Level Agreement -- agreement with the service provider, it outlines the expected services relating to the reliability & availability of the connection.

the service provider may not be the actual carrier. The carrier owns & maintains the physical connection & equipment between the provider & the customer.

- Single-Carrier WAN Connection -- the organization connects to only one service provider. With this setup the carrier connection & service provider are both single points of failure
- Dual-Carrier WAN Connection -- the organization negotiates SLAs with two different service providers. Each provider should use a different carrier. More expensive but more redundancy.

## 7.1.5 - Evolving Networks

Description of a company network as it grows in size:

- Small network -- ~15 employees, uses a single LAN connected to a wireless router, internet connection via DSL supplied by the local telephone service provider, IT requirements fullfilled by contracting services from the DSL provider.
- Campus Network -- big enough for several building floors. Now needs a Campus Area Network (CAN). several interconnected LANs to segment various departments, several dedicated servers, a firewall securing internet access, and an in-house IT staff.
- Branch Network -- additional branches are added. Now a Metropolitan Area Network (MAN) is required to interconnect sites within the same city. As well, private dedicated lines are used to connect the central office with branch offices in nearby cities. Those branches may use a WAN to connect or internet services. the IT team must address security issues associated with the internet.
- Distributed Network -- offices distributed globally. Lots of use of teleworking & virtual teams using web based applications. Site-to-site & VPNs let the company use the internet to connect securely.

# 7.2 - WAN Operations

## 7.2.1 - WAN Standards

Modern WAN standards are defined & managed by recognized authorities, including:

- TIA/EIA -- Telecommunications Industry Association and Electronic Industries Alliance
- ISO -- International Organization for Standarization
- IEEE -- Institute of Electrical & Electronics Engineers

## 7.2.2 - WANs in the OSI Model

most WAN standards focus on layer 1 & 2, physical & data link layers.

Layer 1 protocols describe the electrical, mechanical, & operational components needed to transmit bits over a WAN

- Synchronous Digital Hierarchy (SDH)
- Synchronous Optical Networking (SONET)
- Dense Wavelength Division Multiplexing (DWDM)

SDH & SONET provide essentially the same services & their transmission capacity can be increased by using DWDM technology.

Layer 2 Protocols include:

- Broadband (DSL, Cable)
- Wireless
- Ethernet WAN (metro ethernet)
- Multiprotocol Label Switching (MPLS)
- Point-to-Point Protocol (PPP) (less used)
- High-level Data Link Control (HDLC) (less used)
- Frame Relay (legacy)
- Asynchronous Transfer Mode (legacy)

## 7.2.3 - Common WAN Terminology

| WAN Term | Description |
| --- | --- |
| Data Terminal Equipment (DTE) | the device that connects the subscriber LANs to the WAN communication device. Passes data from a customer network or host computer for transmission over the WAN. (usually a router) |
| Data Communications Equipment (DCE) | aka data circuit-terminating equipment. The device used to communicate with the provider, puts data on the local loop. (modem) |
| Customer Premises Equipment (CPE) | the DTE & DCE devices (ie, router, modem, etc) on the enterprise edge |
| Point-of-Presence (POP) | where the subscriber connects to the service provider network |
| Demarcation Point | a physical location that officially separates the CPE from service provider equipment. typically a cabling junction box. |
| Local Loop (or last mile) | the actual copper or fiber cable that connects the CPE to the CO of the service provider |
| Central Office (CO) | local service provider facility or building that connects CPE to the provider network |
| Toll Network | includes backhaul, long-haul, all-digital, fiber-optic communication lines, switches, routers & other equip. inside the WAN provider network |
| Backhaul Network | connect multiple access nodes of the service provider network. can span over municipalities, countries, & regions |
| Backbone Network | large, high-capactiy, used to interconnect service provider networks & create a redundant network. (also called Tier-1 providers) |

## 7.2.4 - WAN Devices

| WAN Device | Description |
| --- | --- |
| Voiceband Modem | dial-up modem, leacy device, converted digital signals into analog voice frequencies, used telephone lines |
| DSL & Cable Modems | broadband modems, high speed, digital, connect to DTE router using ethernet. DSL modems using telephone lines, Cable modems use coaxial lines. Operate similarly to voiceband modem but w/higher broadband frequencies & transmission speeds |
| CSU/DSU | digital leased lines require a CSU & a DSU. they connect a digital device to a digital line. Can be a separate device (modem) or an interface on a router. CSU provides termination for digital signal, DSU converts line frames into frames the LAN can interpret & vice versa. |
| Optical converter | optical fiber converter. connect fiber-optic media to copper media |
| Wireless router or Access Point | used to wirelessly connect to WAN |
| WAN Core devices | WAN backbone consists of multiple high-speed routers & layer 3 switches. |

## 7.2.5 - Serial Communication

serial communication transmits bits sequentially over a single channel. almost all network communication occurs this way.

Parallel communications simultaneously transmit several bits using multiple wires.

Parallel communication is prone to synchronization issues, especially as distance increases. It is not a viable WAN communication method because of this length restriction (less than 8 meters \[26 feet\]).

## 7.2.6 - Circuit-Switched Communication

a circuit switched network establishes a dedicated circuit (channel) between endpoints before the users can communicate.

During transmission over a circuit switched network, all communication uses the same path. The entire fixed capacity allcoated to the circuit is available for the duration of the communication. This can lead to inefficiencies making circuit switching not suitable for data communication.

2 most common circuit switched WAN technologies:

- PSTN -- public switched telephone network
- ISDN -- integrated services digital network (legacy)

## 7.2.7 -- Packet-Switched Communication

packet switching -- most common, breaks traffic down into packets. doesn't require a circuit to be established. Less expensive & more flexible but is susceptible to delays (latency) & variability of delay (jitter).

common packet-switched WAN technologies include:

- Ethernet WAN (Metro Ethernet)
- Multiprotocol Label Switching (MPLS)
- Frame Relay (LEGACY)
- Asynchronous Transfer Mode (ATM) (LEGACY)

## 7.2.8 -- SDH, SONET, and DWDM

2 optical fiber OSI layer 1 standards for service providers:

- SDH - Synchronoush Digital Hierarchy -- global standard for data transportation over fiber
- SONET - Synchronous Optical Networking -- North American standard that provides the same services as SDH

SDH/SONET define how to transfer multiple data, voice, & video comms over optical fiber using lasers or LEDs over great distances. Both are used on ring network topology which contains reundant fiber paths allowing traffic to flow in both directions.

DWDM - Dense Wavelength Division Multiplexing -- newer tech. increases data carrying capacity of SDH/SONET by multiplexing using different wavelengths of light.
DWDM features include:

- supports SONET/SDH
- can multiplex more than 80 different channels of data (i.e, wavelengths) onto a single fiber.
- each channel can cary 10Gbps multiplexed signal
- it assigns incoming optical signals to specific wavelengths of light

# 7.3 -- Traditional WAN Connectivity

## 7.3.1 -- Traditional WAN Connectivity Options

when LANs came about in the 1980s & organizations saw the need to interconnect with other locations they connected their networks to the local loop of a service provider using dedicated lines or by using switched services from a service provider.

Traditional WAN connectivity options:

- Dedicated
    - Leased Line
        - T1/T3
        - E1/E3
- Switched
    - circuit switched
        - PSTN
        - ISDN
    - Packet switched
        - frame relay
        - ATM

## 7.3.2 -- Common WAN Terminology

old stuff here.
point-to-point links provide a pre-established WAN comms path from customer to the provider network. They were leased from a provider & were called "leased lines"

2 systems are used to define the digital capacity of a copper media serial link:

- T-carrier -- used in north america, provides T1 link supporting bandwidth up to 1.544Mps & T3 links up to 43.7Mbps
- E-Carrier -- used in Europe, E1 links supporting up to 2.048Mbps & E3 links up to 36.368Mbps

Advantages & disadvantages of leased lines:

| Advatages |     |
| --- | --- |
| Simplicity | point-to-point comm links require minimal expertise to install & maintain |
| Quality | point-to-point links offer high quality service. |
| availability | point-to-point comms prived permanent, dedicated capacity |
| Disadvantages |     |
| Cost | most expensive type of WAN access |
| limited flexibility | fixed capacity of leased lines seldom match what is actually needed |

## 7.3.3 -- Circuit-Switched Options

2 traditional circuit switched options:

- Public Service Telephone Network (PSTN) -- dialup WAN access. data travels on the lines as an analog signal & is modulated & demodulated by a voiceband modem converting it from digital to analog & vice versa. Limited to 56kbps
- Integrated Services Digital Network (ISDN) -- enables PSTN local loop to carry digital signals. provides data rates from 45kbps to 2.048Mbps

## 7.3.4 -- Packet-Switched Options

2 traditional (legacy) packet-switched connectivity options:

- Frame Relay -- simple layer 2 non-broadcast multi-access (NBMA) WAN technology. A single router interface can connect multiple sites using different PVCs. Supports data rates up to 4Mbps.
- Asynchronous Transfer Mode (ATM) -- capable of transferring voice, video, and data through private & public networks. Built on cell-based architecture rather than frame-based. ATM cells are fixed length of 53 bytes. 5 byte header & 48 bytes of payload. This large header mean ATM isn't as efficient as Frame Relay. An ATM line would need to be ~20% greater bandwidth to have the same throughput as a Frame Relay line.

# 7.4 -- Modern WAN Connectivity

## 7.4.1 -- Modern WANs

traditional WAN connectivity options have rapidly declined

## 7.4.2 -- Modern WAN Connectivity Options

- Dedicated Broadband -- dark fiber - many fiber cabling runs are not in use because new multiplexing technology makes then unnecessary. These can be leased or purchased from a supplier.
- Packet-Switched -- 2 options available
    - Ethernet WAN (metro ethernet) - provides fast bandwidth links, has replaced many tradition WAN options
    - Multi-protocol Label Switching (MPLS) - enables the WAN provider network to carry any protocol as payload data.
- Internet-based Broadband -- organizations commonly use the global internet infrastructure for WAN connectivity. Options include DSL, cable, wireless, & fiber

## 7.4.3 -- Ethernet WAN

Ethernet was originally not suitable as a WAN access technology, primarily because of the limited distance provided by copper media.

New ethernet standards using fiber have changed this.

- IEEE 1000BASE-LX -- fiber lengths of 5km
- IEEE 1000BASE-ZX -- fiber lengths of 70km

Ethernet WAN goes by many names:

- Metropolitan Ethernet (Metro E)
- Ethernet over MPLS (EoMPLS)
- Virtual Private LAN Service (VPLS)

**Benefits of Ethernet WAN**, Ethernet WAN:

- Reduced expenses & administration -- provides a switched, high-bandwidth layer 2 network. Eliminates need for conversions to other WAN technologies.
- Easy integration w/existing networks -- connects easily to existing Ethernet LANs
- Enhanced business productivity -- businesses can take advantage of applications that are difficult to implement on TDM or Frame Relay networks.

## 7.4.4 -- MPLS

Multiprotocol Label Switching (MPLS) -- a high performance service provider WAN routing technology. Lets clients interconnect without regard to access method or payload.

Supports many client access methods (ethernet, dsl, cable, frame relay)
can encapsulate all types of protocols (ipv4, ipv6, etc)

MPLS routers are **label switched routers** (LSRs). They attach labels to packets & those lables are used by other routers to forward traffic.

# 7.5 -- Internet-Based Connectivity

## 7.5.1 -- Internet-Based Connectivity Options

- Wired Options - use permanent cabling to provide consistent bandwidth & reduce error rates & latency
    - xDSL
    - Cable
    - Optical Fiber
- Wireless - less expensive to implement because they use radio waves rather than wired media. But, are affected by distance from radio towers, interference, weather.
    - Municipal Wi-Fi
    - Cellular
    - Satellite Internet
    - WiMAX

## 7.5.2 -- DSL Technology

Digital Subscriber Line -- high-speed, always on connection. Uses existing telephone lines.

Plain Old Telephone System (POTS) -- frequency range used by voice-grade telephone service.

there are several **xDSL** varieties -- Assymetric DSL (ADSL) or Symmetric DSL (SDSL).

ADSL & ADSL2+ offer high downstream bandwidth than upload bandwidth.

Transfer rates depend on the length of the local loop, & type and condition of cabling. ADSL loop must be less than 5.46km (3.39miles)

## 7.5.3 -- DSL Connections

DSL connections are set up between a DSL modem & the DSL access multiplexer (DSLAM)

DSL modem converts ethernet signals to a DSL signal which is transmitted to a DSLAM at the provider location.

DSLAM is located at the Central Office (CO) of the provider. It concentrates connections from multiple DSL subscribers. It is often build into an aggregation router.

DSL is not a shared medium, each user has a separate direct connection to the DSLAM.

## 7.5.4 -- DSL & PPP

Point-to-Point protocol (PPP) -- layer 2 protocol, used to be used to establish connections over dial-up & ISDN networks.

PPP is still used for broadband DSL connections because:

- it can be used to authenticate the subscriber
- it can assign a public IPv4 address
- it provides link-quality management features

Ethernet links do not natively support PPP, thus, either a host runs a PPPoE client to obtain a public IP addr from the provider's PPPoE server, or a router with a PPPoE client is used.

If the host runs the PPPoE client only it can use the DSL connection.

## 7.5.5 -- Cable Technology

high speed, always on, uses coaxial cable.

modern systems offer internet access, cable tv, & telephone service

Data over Cable Service Interface Specification (DOCSIS) -- international standard for adding high speed bandwidth to an existing cable system

hybrid fiber-coaxial (HFC) networks -- deployed by cable operators to enable high speed data transmission to cable modems. This uses fiber & coaxial cable in different portions of the network.

an Optical Node converts optical to RF signal & optical signals travel long distances to a provider's headend, where they have a Cable Modem Termination System (CMTS)

**All subscribers share bandwidth**

## 7.5.6 -- Optical Fiber

Fiber to the x (FTTx) includes:

- Fiber to the Home (FTTH) - fiber reaches the boundary of the residence.
- Fiber to the Building (FTTB) - fiber reaches the boundary of the building, such as basement in a multi-dwelling unit
- Fiber to the Node/Neighborhood (FTTN) - cabling reaches an optical node that converts signals to a format acceptable for twisted pair or coaxial cable to the premise

## 7.5.7 -- Wireless Internet-Based Broadband

- Municipal Wi-Fi -- city wide high-speed internet. may be for city use only (police, fire department, etc.)
- Cellular --
    - 3G/4G/5G Wireless -- 4G supports up to 450Mbps download & 100Mbps upload. 5G supports 100Mbps to 10Gbps & beyond.
    - Long-Term Evolution (LTE) -- a newer & faster technology, part of 4G technology
- Satellite Internet -- typically used by rural users. upload speeds are typically 1/10 of download speeds. Download speeds range from 5Mbps to 25Mbps
- WiMAX -- Worldwide Interoperability for Microwave Access - IEEE 802.16 - new technology. provides high speed broadband with wireless access & provides broad coverage like a cell phone network. users must be within 30 miles of a tower.

## 7.5.8 -- VPN Technology

VPNs offer:

- cost savings
- security
- scalability
- compatability with new broadband technology

VPNs are commonly implemented as:

- Site-to-Site VPN -- VPN is configured on router. Clients are unaware of it.
- Remote Access -- user initiates remote access connection

## 7.5.9 -- ISP Connectivity Options

- Single Homed -- client connect to the ISP via one link. no redundancy, least expensive
- dual-homes -- connects to isp via two links. if both are operational traffic can be load balanced
- Multihomed -- client connects to two different ISPs
- Dual-multihomed -- client connects via two links each to two different ISPs. most expensive

## 7.5.10 -- Broadband Solution Comparison

Factors to consider when choosing a broadband solution:

- Cable - bandwidth is shared.
- DSL - limited bandwidth that is distance sensitive
- Fiber-to-the-Home - requires fiber installation directly to the home
- Cellular/Mobile - coverage is often an issue
- Municipal Wi-Fi - most municipalities do not have this
- Satellite - expensive & limited capacity. Typically used when no other option is available