Module 8 - VPN & IPsec Concepts

# 8.1 -- VPN Technology
## 8.1.1 -- Virtual Private Networks
The first types of VPNs were only IP tunnels. They didn't include authentication or encryption of the data.

ASA - Adaptive Security Appliance

Generic Routing Encapsulation (GRE) -- a tunneling protocol developed by Cisco. Didn't include encryption services


## 8.1.2 -- VPN Benefits
modern VPNs support encryption such as IPsec & SSL.

VPN benefits include:
* Cost Savings -- organizations can reduce their connectivity costs while increasing bandwidth
* Security -- VPNs provide the highest level of security available via advance authentication & encryption protocols
* Scalability -- its easy to add users without adding significant infrastructure when you're using the internet
* Compatability -- VPNs are easily implemented accross a wide variety of WAN link options. 


## 8.1.3 -- Site-to-Site & Remote-Access VPNs
* Site-to-Site -- created when a VPN terminating device (VPN gateway) is preconfigured with info to establish a secure tunnel. Internal hosts have no knowledge that a VPN is being used.
* Remote-Access VPN -- dynamically created to establish a secure connection between a client & VPN terminating device


## 8.1.4 -- Enterprise & Service Provider VPNs
There are many available options to secure enterprise traffic.
VPNs can be managed & deployed as:
* Enterprise Managed VPNs:
	* Site-to-Site VPNs
		* IPsec VPN
		* GRE over IPsec
		* Cisco Dynamic Multipoint VPN (DMVPN)
		* IPsec virtual tunnel interface (VTI)
	* Remote Access VPNs
		* client-based IPsec VPN connection
		* clientless SSL connection
* Service Provider Managed VPNs:
	* Layer 2 MPLS
	* Layer 3 MPLS
	* Legacy Solutions:
		* Frame Relay
		* ATM

# 8.2 -- Types of VPNs
## 8.2.1 -- Remote-Access VPNs
enabled dynamically by the user when required. Can be created using either IPsec or SSL. The remote user must initiate a remote access VPN connection.

2 ways the remote user can initiate a remote access VPN connection:
* Clientless VPN connection -- connection is secured using a web browser SSL connection. SSL is most commonly used with HTTPS, IAM, & POP3. HTTPS is HTTP using an SSL tunnel. The SSL tunnel is established & then HTTP data is exchanged.
* Client-based VPN connection -- VPN client software (ex: Cisco AnyConnect Secure Mobility client) must be installed on the remote user's device. User's initiate the connection using the client & authenticate to the destination VPN gateway. VPN client software encrypts the traffic using IPsec or SSL & forwards it to the VPN gateway.


## 8.2.2 -- SSL VPNs
TLS is the newer version of SSL. The VPN client uses TLS when negotiating an SSL VPN connection with the VPN gateway.

SSL uses public key infrastructure & digital certificates for authentication. Both IPsec & SSL VPN technologies offer the same access but IPsec is superior. 

SSL however is better if support & ease of deployment are primary issues.

IPsec & SSL comparison:
| Feature | IPsec | SSL |
| --- | --- | --- |
| Applications supported | **Extensive** - all IP-based applications supported | **Limited** - only web based applications & file sharing are supported | 
| Authentication Strength | **Strong** - two-way authentication w/shared keys or digital certificates | **Moderate** - uses 1 way or 2 way authentication | 
| Encryption Strenth | **Strong** - key lengths from 56 - 256 bits | **Moderate to Strong** - key lengths from 40 - 256 bits |
| Connection Complexity | **Medium** - requires a preinstalled VPN client | **Low** - only requires a web browser |
| Connection Option | **Limited** - only specific devices w/specific configurations can connect | **Extensive** - any device w/web browser can connect |

## 8.2.3 -- Site to Site VPNs
used to connect networks across another, untrusted network.
End hosts send & receive normal unencrypted TCP/IP traffic through a VPN terminating device (VPN gateway).

VPN gateway could be a router or firewall. 
Cisco Adaptive Security Appliance (ASA) conbines firewall, VPN concentrator, & IPS into one software image.


## 8.2.4 -- GRE over IPsec
Generic Routing Encapsulation -- non-secure site-to-site VPN tunneling protocol. can encapsulate various network layer protocols & supports multicast & broadcast traffic but does not support encryption.

a standard IPsec VPN can only create secure tunnels for unicast traffic & therefore routing protocols cannot exchange routing info over an IPsec VPN.

To solve this ^^ we encapsulate a GRE packet containing routing protocol traffic in an IPsec packet.

Terms for this:
* Passenger protocol -- the original packet to be encapsulated by GRE. could be ipv4 or 6 or whatever.
* Carrier Protocol -- in this example GRE is the carrier protocol
* Transport Protocol -- the protocol that is used to forward the packet, ipv4 or 6

## 8.2.5 -- Dynamic Multipoint VPNs
Site-to-site VPNs & GRE over IPsec are not adequate when an enterprise has many sites to connect to because each site requires a static configuration to all other sites, or a central site.

Dynamic Multipoint VPN (DMVPN) -- Cisco solution for building multiple VPNs in an easy, dynamic, & scalable way. Relies on IPsec. Uses hub & spoke configuration to establish full mesh topology.


All sites are configured with Multipoint Generic Routing Encapsulation (mGRE) which allows a single GRE interface to dynamically support multiple IPsec tunnels. No additional configuration is required when a new site requires a secure connection.

Spoke sites can obtain info about other spoke sites & create virtual spoke-to-spoke tunnels with them.


## 8.2.6 -- IPsec Virtual Tunnel Interface
IPsec Virtual Tunnel Interface (VTI) also simplifies multi site VPN access.
IPsec VTI configurations are applied to a virtual interface rather than a static mapping on a physical interface.

can send & recieve IP unicast & multicast encrypted traffic.


## 8.2.7 -- Service Provider MPLS VPNs
Traditional WAN solutions were inherently secure because customers cannot see each other's traffic.

MPLS is used now by service providers in their core network & is secure in this same way.

MPLS can provide managed VPN solutions making it the service provider's responsibility to secure traffic.

2 types of MPLS VPN solutions:
* Layer 3 MPLS VPN -- service provider establishes a peering between the customer's routers & provider's routers & then customer routes receieved by the provider are redistributes through MPLS network to customer's remote locations.
* Layer 2 MPLS VPN -- service provider deploys a Virtual Private LAN Service (VPLS) which emulate an Ethernet multiaccess LAN segment over the MPLS network. No routing involved. 


# 8.3 -- IPsec
## 8.3.1 -- Video - IPsec Concepts
Internet was not designed with security in mind. it wasn't necessary. 

## 8.3.2 -- IPsec Technologies
IPsec is an IETF standard (RFC 2401-2412).  it can protect traffic on layers 4 - 7

IPsec security framework provides these essential security functions:
* Confidentiality -- encryption algorithms prevent cybercriminals from reading package contents
* Integrity -- hashing algorithms ensure packets have not been altered
* Origin Authentication -- Internet Key Exchange (IKE) protocol authenticates source & destination. May use pre-shared keys, digital certificates, or RSA certificates
* Diffie-Hellman -- secure key exchange using various groups of DH algorithm

| IPsec function | Description |
| --- | --- |
| IPsec Protocol | Authentication Header (AH) or Encapsulation Security Protocol (ESP). AH authenticates Layer 3 packet while ESP encrypts it. AH + ESP not often used because it won't work with NAT |
| Confidentiality | Data Encryption Standard (DES), Triple DES (3DES), AES, or Software-Optimized Encryption Algorithm (SEAL). no encryption is an option too. |
| Integrity | MD5 or SHA |
| Authentication | Internet Key Exchange (IKE). This could use username & password, one-time password, biometrics, pre-shared keys, and digital certificates using RSA algorithm |
| Diffie-Hellman | used for public key exchange. Sever different groups to choose from: DH14, 15, 16 & DH 19, 20, 21 and 24. |


## 8.3.3 -- IPsec Protocol Encapsulation
This is the first building block in the IPsec framework. The choice made here determines which other building blocks are available.

* AH -- appropriate only when confidentiality is not required or permitted. Provides authentication & ensures data integrity. all text is transported unencrypted
* ESP -- provides confidentiality & authentication. Encrypts IP packets & authenticates the inner IP packet & ESP header.


## 8.3.4 -- Confidentiality
achieved by encrypting data.

* DES -- should be avoided. uses 56bit key
* 3DES -- uses 3 independent 56-bit encryption keys per 64 bit block. DES is computationally taxing & no longer considered secure
* AES -- most recommended symmetric encryption algorithm. stronger than DES & more computationally efficicent than 3DES. Offers keys of 128, 192, & 256 bits.
* SEAL -- a stream cipher. data is encrypted continuously, not as blocks. Uses 160 bit key, considered very secure.

## 8.3.5 -- Integrity
hashed message authentication code (HMAC) -- guarantees integrity of the message using a has value.

Cisco now rates SHA-1 as legacy. Recommends SHA-256 at least.
* Message Digest 5 (MD5) -- 128 bit shared key. no longer secure
* Secure Hash Algorithm -- uses 160 bit secret key. SHA-256 or higher are considered secure

## 8.3.6 -- Authentication
* pre-shared secret key (PSK) -- a value entered into each peer manually. Combined with other info to form authentication key. Easy to configure manually, but do not scale well. 
* RSA -- uses digital certificates. local device derives a hash & encrypts it with its private key. This encrypted hash is sent to the remote end & used as a signature. The remote end decrypts the hash using the public key and if the decrypted hash matches the recomputed hash, the signature is genuine.

## 8.3.7 -- Secure Key Exchange with Diffie-Hellman
DH provides a way for 2 peer to establish a shared secret key only they know, even though they're communicating over an insecure channel.

* DH groups 1, 2, & 5 should no longer be used. They support key size of 768, 1024, & 1536 bits respectively
* DH groups 14, 15, 16 use key sizes 2048, 3072, & 4096 bits respectively. Recommended for use until 2030
* DH groups 19, 20, 21, & 24 use key sizes 256, 384, 521, & 2048 bits & support Elliptical Curve Cryptography (ECC) which reduces time needed to generate keys. DH 24 is the preferred next gen encryption

The DH group chosen must have enough bits to protect the IPsec keys during negotiation. DH 1 can suppor DES & 3DES but not AES. If the encryption or authentication algorithms use 128-bit keys, use group 14, 19, 20 or 24. If they use 256-bit key or higher use group 21 or 24.


## 8.3.8 -- Video - IPsec Transport & Tunnel Mode
