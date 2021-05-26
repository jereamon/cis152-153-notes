Module 12 - WLAN Concepts

# 12.1 - Introduction to Wireless
## 12.1.1 - Benefits of Wireless
## 12.1.2 - Types of Wireless Networks
* WPAN - short-range network. 20 to 30ft using Bluetooth, ZigBee. Based on 802.15 standard and use 2.4Ghz frequency
* WLAN - cover up to ~300 feet. Based on 802.11 standard and use 2.4Ghz or 5Ghz frequency
* WMAN (metropolitan) - covers larger geographic area. Uses specific licensed frequencies
* WWAN (wide area) - national & global communications. Also use specific licensed frequencies.

## 12.1.3 - Wireless Technologies
* Bluetooth - IEEE 802.15 WPAN standard uses device pairing process & can communicate up to 300 ft. Two types of radios
	* Bluetooth Low Energy (BLE) - supports multiple network technologies including mesh topology.
	* Bluetooth Basic Rate/Enhanced Rate (BR/EDR) - supports point to point topologies. optimized for audio streaming.
* WiMAX - Worldwide Interoperability for Microwave Access - IEEE 802.16 WWAN standard range of 30 mi. alternative to broadband internet. Typically used in areas not connected to DSL or cable. Uses towers like cell phone towers.
* Cellular Broadband - 4G/5G 2 types of cellular networks GSM & CDMA. 
* Satellite Broadband - requires clear line of sight, expensive.

## 12.1.4 - 802.11 Standards
| IEEE Standard | Frequency | Description |
| --- | --- | --- |
| 802.11 | 2.4Ghz | 2Mbps |
| 802.11a | 5Ghz | 54Mbps, small area |
| 802.11b | 2.4Ghz | 11Mbps, bigger area |
| 802.11g | 2.4Ghz | 54Mbps |
| 802.11n | 2.4 & 5Ghz | 150-600Mbps, range of 230ft (70m), MIMO |
| 802.11ac | 5Ghz | 450Mbps - 1.3Gbps, MIMO, up to 8 antennas |
| 802.11ax | 2.4 & 5Ghz | 2019, will use 1 & 7 Ghz when those become available, WiFi gen 6 |


# 12.1.6 - Wireless Standards Organizations
* ITU - International Telecommunication Union - regulates allocation of radio frequency spectrum & satellite orbits through ITU-R
* IEEE - specifies how radio frequency is modulated to carry information
* Wi-Fi Alliance - global, non-profit, association of vendors aiming to improve interoperability of 802.11 based products


# 12.2 - WLAN Components
## 12.2.1 - Video - WLAN Components
## 12.2.2 - Wireless NICs
## 12.2.3 - Wireless Home Router
Serves as an:
* Access Point - provides 802.11a/b/g/n/ac wireless access
* Switch - provides a four port, full-duplex, 10/100/1000 ethernet switch
* Router - provides default gateway for connecting to other network infrastructures

## 12.2.4 - Wireless Access Points
they're cool

## 12.2.5 - AP Categories
* Autonomous APs - standalone device that must be configured via cmd line or gui. good when you don't need many.
* Controller-based APs - require no initial configuration. Often called lightweight APs. These scale well.

## 12.2.6 - Wireless Antennas
* Omnidirectional Antennas
* Directional Antennas
* MIMO Antennas - uses multiple antennas to increase bandwidth

# 12.3 - WLAN Operation
## 12.3.1 - Video - WLAN Operation
## 12.3.2 - 802.11 Wireless Topology Modes
* Ad hoc mode - when two devices connect wirelessly in a peer-to-peer manner without any APs or wireless routers. 802.11 refers to ad hoc network as independent basic service set (IBSS)
* Infrastructure Mode - when wireless clients connect to a wireless router or AP.
* Tethering - variation of ad hoc when a smartphone or tablet connected to a cell network creates a personal hotspot.

## 12.3.3 - BSS & ESS
* Basic Service Set (BSS) - consists of a single AP interconnecting all associated wireless clients. Coverage for BSS is called Basic Service Area (BSA). The MAC addr of the AP uniqely identifies each BSS, called the Basice Service set Identifier (BSSID) which is always only associated with one AP.
* Extended Service Set (ESS) - two or more BSSs joined through a common distribution system (DS) into an ESS. Each ESS is identified by an SSID and each BSS is identified by its BSSID. wireless clients can move seamlessly between BSAs and communicate with clients connected to other BSSs within the same ESS.

## 12.3.4 - 802.11 Frame Structure
802.11 frame format is similar to ethernet frame format except it contains more fields.

all 802.11 frames contain:
* Frame Control - identifies the type of wireless frame and contains subfields for protocol version, frame type, address type, power management, & security settings.
* Duration - typically used to indicate the remaining duration needed to receive the next frame transmission.

802.1 frames from wireless devices contain:
* Address 1 Receiver address - MAC address of the AP
* Address 2 Transmitter Address - MAC addr of the sender
* Addr 3 SA/DA/BSSID - MAC addr of the destination

802.11 frams from the AP contain:
* Addr 1 Receiver Addr - MAC addr of the sender
* Addr 2 Transmitter Addr - MAC addr of the AP
* Addr 3 SA/DA/BSSID - MAC addr of the wireless destination
* Sequence Control - info to control sequencing and fragmented frames.
* Addr 4 - usually missing. Only used in ad hoc mode.
* Payload - contains the data for transmission
* FCS - used for layer 2 error control

## 12.3.5 - CSMA/CA
WLANs are half duplex, shared media configurations. Shared media means that wireless clients can all transmit & receive on the same radio channel. Wireless clients cannot hear while they're sending.

They use CSMA/CA to avoid collisions. Clients do the following:
1. Listens to the channel to see if it's idle. No other traffic on the channel. Channel is called the carrier.
2. Sends a request to send (RTS) message to the AP to request dedicated access to the network.
3. Receives a clear to send (CTS) message from AP
4. if wireless client does not receive CTS it waits a random amount of time before retrying
5. after it receives CTS, it transmits the data
6. all transmissions are acknowledged. If client doesn't receive acknowledgment it assumes a collision has occurred and starts the process over.

## 12.3.6 - Wireless Client and AP Association
Wireless clients complete the following 3 stage process:
1. Discover a wireless AP
2. Authenticate with AP
3. Associate with AP

For successful associationi client and AP must aggree on specific parameters:
* SSID - in larger organizations each SSID may be mapped to one VLAN.
* Password - required from the wireless client to authenticate to the AP
* Network Mode - refers to the 802.11a/b/g/n/ac/ad WLAN standards. APs and router can operate in mixed modes.
* Security Mode - WEP, WPA, WPA2
* Channel Settings - refers to the frequency bands used to transmit wireless data.


## 12.3.7 - Passive and Active Discover Mode
* Passive Mode - AP openly advertises its service by periodically sending broadcast beacon frames containing the SSID, supported standards, and security settings. Primary purpose of the beacon is to allow wireless clients to learn which networks and APs are available.
* Active Mode - wireless client must know the name of the SSID. wireless client initiates the process by broadcasting a probe request frame on multiple channels. request includes SSID name and standards supported. APs, in turn, send a probe response that includes SSID, supported standards, & security settings.

# 12.4 - CAPWAP Operation
## 12.4.1 - Video - CAPWAP
## 12.4.2 - Introduction to CAPWAP
CAPWAP - Control and Provisioning of Wireless Access Points
WLC - Wireless Lan Controller

CAPWAP  is an IEEE standard that enables a WLC to manage multiple APs and WLANs. It is also responsible for the encapsulation and forwarding of WLAN client traffic between an AP and a WLC.

Based on LWAPP but adds additional security with Datagram Transport Layer Security (DTLS). It establishes tunnels on UDP ports and can operate over IPv4 or IPv6.

IPv4 & 6 both use UDP ports 5246 & 5247. 5246 is for CAPWAP control messages used by WLC to manage the AP. 5247 used by CAPWAP to encapsulate data packets traveling to and from wireless clients.

CAPWAP tunnels use different IP protocols in the packet header. IPv4 uses IP protocol 17, & IPv6 uses IP protocol 136.

## 12.4.3 - Split MAC Architecture
CAPWAP split MAC distributes functions normally performed by an AP and distributes them between two functional components.

| AP MAC Functions | WLC MAC Functions |
| --- | --- |
| Beacons and probe responses | Authentication |
| Packet acknowledgements and retransmissions | association and re-association of roaming clients |
| frame queing and packet prioritization | frame translation to other protocols |
| MAC layer data encryption & decryption | termination of 802.11 traffic on a wired interface |

## 12.4.4 - DTLS Encryption
DTLS provides security between AP and WLC.
Enabled by default on CAPWAP control channel
disabled by default on data channel

CAPWAP management trafic is encrypted by default to provide control plane privacy and prevent man in the middle attacks.

Data encryption is optional and requires a DTLS license to be installed on the WLC prior to being enabled on an AP.

## 12.4.5 - FlexConnect APs
A wireless solution for branch office and remote office deployments. Allows you to configure & control APs in a branch office from corporate office through a WAN link, without deploying a controller in each office.

Two modes:
* Connected Mode - WLC is reachable, FlexConnect AP has CAPWAP conectivity with its WLC & can send traffic through the CAPWAP tunnel. WLC performs all its CAPWAP functions.
* Standalone Mode - WLC is unreachable, FlexConnect has lost or failed to establish CAPWAP connectivity with its WLC. FlexConnect AP can assume some of the WLC functions: switching client data traffic locally & performing client authentication locally.


# 12.5 - Channel Management
## 12.5.1 - Frequency Channel Saturation
* Direct-Sequence Spread Spectrum (DSSS) - a modulation technique designed to make it more difficult for enemies to intercept or jam a signal. Spreads the signal over a wider frequency which hides the discernable peak of the signal. used by 802.11b to avoid interference at 2.4Ghz
* Frequency-Hopping Spread Spectrum (FHSS) - Rapidly switches carrier signal among many frequency channels. Sender & receiver must be synchronized. allows for more efficient used of channels. Used by original 802.11 standard, walkie talkies, 900Mhz cordless phones, & bluetooth uses a variation.
* Orthogonal Frequency-Division Multiplexing (OFDM) - subset of frequency division multiplexing in which a single subchannel uses multiple sub-channels on adjacent frequencies. Used by 802.11a/g/n/ac, 802.11ax uses a variation called Orthogonal frequency-division multiaccess (OFDMA).

## 12.5.2 - Channel Selection
802.11b/g/n operate in 2.4 - 2.5 Ghz spectrum. 2.4Ghz band is subdivided into channels w/each allotted 22Mhz bandwidth & separated from the next channel by 5Mhz. 

802.11b defines 11 channels for North America, 13 for europe, & 14 in Japan.

Best practice for APs is to have them use non-overlapping channels. Most do this automatically.

For 5Ghz standards (802.11a/n/ac) there are 24 channels. 5Ghz band is divided into 3 sections, w/each channel separated by 20Mhz. Channels mostly do not interefere with eachother.


## 12.5.3 - Plan a WLAN Deployment
Approximate circular coverage area of APs is important.
Additional recommendations:
* note if APs will use exisiting wiring or if there are locations they cannot be placed.
* note all potential sources of intereference in 2.4Ghz range (microwaves, wireless video cameras, fluorescent lights, motion detectors)
* position APs above obstructions
* position APs vertically near the ceiling in the center of each coverage area, if possible
* position APs where users will be
* if IEEE 802.11 network has been configured for mixed mode users may experience slower than normal speeds in order to support old standards.


# 12.6 - WLAN Threats
## 12.6.1 - Video
## 12.6.2 - Wireless Security Overview
Potential wireless network threats:
* Interception of data - wireless data should be encrypted to prevent eavesdroppers
* Wireless Intruders - deter unauthorized users through effective authentication techniques
* Denial of Service (DoS) Attacks - 
* Rogue APs - unauthorized APs installed on the network

## 12.6.3 - DoS Attacks
Wireless DoS attack can be the result of:
* Improperly configured devices - configuration errors can disable the WLAN
* Malicious user intentional interfering - 
* Accidental Interference - microwaves

## 12.6.4 - Rogue Access Points
An unauthorized AP connected to the network. May be malicious or benign, either way we probably don't want it.

can be used to capture MAC addresses, data packets, gain access to network resources, or launch a man in the middle attack. 

WLCs must be configured with rogue AP policies to prevent this.


## 12.6.5 - Man-in-the-Middle-Attack
popular attack is called "evil twin AP". where attacker introduces a rogue AP and gives it the same SSID as the legitimate AP.


# 12.7 - Secure WLANs
## 12.7.1 - Video
## 12.7.2 - SSID Cloaking and MAC Address Filtering
* SSID Cloaking - routers won't broadcast SSID requiring clients to mannually configure it.
* MAC Address Filtering - Clients can be manually permitted or denied based on their MAC address.

## 12.7.3 - 802.11 Original Authentication Methods
Two types of autehntication the original 802.11 introduced:
* Open System Authentication - any client can easily connect (no password, nada), it is up to the client to provide security (VPN).
* Shared key authentication - provides WEP, WPA, WPA2, WPA3 to authenticate & encrypt data.

## 12.7.4 - Shared Key Authentication Methods
Four shared key authentication techniques available:

| Method | Description |
| --- | --- |
| Wired Equivalent Privacy (WEP) | original 802.11 spec designed using Rivest Cipher 4 (RC4) with a static key. Key never changes when exchanging packets & is therefore easily hackable. DON'T USE. |
| Wi-Fi Protected Access (WPA) | uses WEP, but uses stronger Temporal Key Integrity Protocol (TKIP) encryption. TKIP changes the key for each packet. Harder to hack. |
| WPA2 | current industry standard. Uses Advanced Encryption Standard (AES) |
| WPA3 | next gen. WPA3 enabled devices use latest security methods, disallow legacy protocols, require use of Protected Management Frames (PMF), not yet readily available | 


## 12.7.5 - Authenticating a Home User
Home routers typically have 2 choices: WPA & WPA2. 2 WPA2 authentication methods:
* Personal - intended for home or small office networks, users authenticate using pre-shared key (PSK)
* Enterprise - intended for enterprise networks, requires Remote Authentication Dial-In user Service (RADIUS) server. Device must be authenticated by RADIUS server & then users must authenticate using 802.1X standard, which uses Extensible Authentication Protocol (EAP)

## 12.7.6 - Encryption Methods
* Temporal Key Integrity Protocol (TKIP) - used by WPA, provides support for legacy WLAN equipment by addressing flaws associated with 802.11 WEP encryption method. Makes use of WEP but encrypts the layer 2 payload using TKIP, & carries a Message Integrity Check (MIC) to ensure the message has not been altered.
* Advanced Encryption Standard (AES) - used by WPA2, preferred because it is far stronger. Uses Counter Cipher Mode with Block Chaining Message Authentication Code Protol (CCMP) that allows destination hosts to tell if the encrypted & non-encrypted bits have been altered.

## 12.7.7 - Authentication in the Enterprise
Enterprise security requires an Authentication, Authorization, and Accounting (AAA) RADIUS server
* RADIUS Server Ip address - the reachable address of the RADIUS server
* UDP port numbers - officially assigned UDP ports 1812 for RADIUS authentication, and 1813 for RADIUS accounting. Can also operate using UDP ports 1645 and 1646.
* Shared key - used to authenticate the AP with RADIUS server

## 12.7.8 - WPA3
WPA2 is no longer secure but WPA3 devices aren't readily available. If it is available, it is recommended.
WPA3 includes 4 features:
* WPA3-Personal - WPA3 thwarts brute force PSK guessing by using Simultaneous Authentication of Equals (SAE), a feature specified in 802.11-2016. PSK is never exposed.
* WPA3-Enterprise - still uses 802.1X/EAP authentication but requires use of a 192-bit cryptographic suite & eliminates the mixing of security protocols for previous 802.11 standards. Adheres to Commerical National Security Algorithm (CNSA) Suite commonly used in high security Wi-Fi networks.
* Open Networks - WPA2 open networks send user traffic in unauthenticated, clear text. WPA2 open networks do not require authentication but do use Opportunistic Wireless Encryption (OWE) to encrypt all traffic.
* IoT Onboarding - WPA2 used WPS to quickly onboard devices. This is not recommended. Device Provisioning Protocol (DPP) was designed to facilitate easy connection of headless IoT devices. each headless device has a hardcoded public key, typically stamped on the outside of the device or package as a QR code. Code can be scanned to quickly onboard the device.