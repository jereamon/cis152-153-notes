Module 3 - Networking Security Concepts

# 3.1 - Current state of cybersecurity
## 3.1.1 - Current State of Affairs
| Security Terms | Description |
| --- | --- | 
| Assets | an asset is anything of value to the organization, including people equipment, resources, and data. |
| Vulnerability | a weakness in a system, or its design, that could be exploited by a threat |
| Threat | a potential danger to a company's assets, data, or network functionality |
| Exploit | a mechanism that takes advantage of a vulnerability |
| Mitigation | a counter-measure that reduces the likelihood or severity of a potential threat or risk. network security involves multiple mitigation techniques. | 
| Risk | the likelihood of a threat to exploit the vulnerability of an asset, with the aim of negatively affecting an orgranization. measured using the probability of the occurrence of an event & its consequences |


## 3.1.2 - Vectors of Network Attacks
atack vectors can originate from inside or outside the corporate network (internal or external).

Internal user may, accidentally or intentionally:
* steal & copy confidential data
* compromise internal servers or network infrastructure
* disconnect a critical network connection & cause an outage
* connect an infected USB drive

internal threats have the potential to cause greater damage than external ones


## 3.1.3 - Data Loss
lost data can result in:
* brand damage/loss of reputation
* loss of competitive advantage
* loss of customers
* loss of revenue
* litigation/legal action
* significant cost & effort to notify affected parties

Common data loss vectors:
| Data Loss Vectors | Description |
| --- | --- | 
| Email/Social Networking | intercepted email or im messages |
| unencrypted devices | plain text data is easier to retrieve |
| cloud storage devices | access to cloud is compromised due to weak security |
| removable media | either via an employee transferring data they shouldn't or a usb drive with valuable info being lost. |
| hard copy | confidential data should be shredded when no longer needed |
| improper access control | compromised passwords can provide threat actor with easy access. |


# 3.2 - Threat Actors
## 3.2.1 - The Hacker
black hat - white hat - grey hat

## 3.2.2 - Evolution of Hackers
Modern Hacking Terms:
| Term | Description |
| script kiddies | inexperienced hackers running existing scripts, tools, & exploits to cause harm |
| vulnerability broker | usually gray hat hackers who attempt to discover exploits & report them to vendors |
| hacktivists | grey hat hacker publicly protesting organizations or governments |
| cyber criminals | black hat hackers either self employed or working for large cybercrime organizations |
| state-sponsored | either white or black hat hackers who steal government secrets, gather intelligence, & sabotage networks |

## 3.2.3 - Cyber Criminals
buy and sell private info & intellectual property, attack toolkits, zero day exploit code, botnet services, banking trojans, keyloggers, & more.

## 3.2.4 - Hacktivists
tend to rely on fairly basic freely available tools

## 3.2.5 - State-Sponsored Hackers
create advanced customized attack code, often using previously undiscovered software vulnerabilities.


# 3.3 - Threat Actor Tools
## 3.3.1 - Video - Threat Actor Tools
## 3.3.3 - Evolution of security tools
Pen testing Tools:
* password crackers - password recovery tools, either removes the original password, after bypassing data encryption, or outright discovers the password. 
	* John the ripper, ophcrack, l0phtCrack, THC Hydra, RainbowCrack, Medusa
* wireless hacking tools - used to detect wireless vulnerabilites.
	* Aircrack-ng, kismet, inSSIDer, KisMAC, Firesheep, NetStumblr
* network scanning and hacking tools - probe network devices, servers, and hosts for open TCP & UDP ports. 
	* Nmap, SuperScan, Angry IP Scanner, NetScanTools
* packet crafting tools - used to probe & test a firewall's robustness using specially crafted packets.
	* Hping, Scapy, Socat, Yersinia, Netcat, Nping, Nemesis
* packet sniffers - used to capture & analyze packets
	* Wireshark, Tcpdump, ettercap, Dsniff, EtherApe, Paros, Fiddler, Ratproxy, SSLstrip
* rootkit detectors - detects installed rootkits
	* AIDE, netfilter, PF: OpenBSD Packet Filter	
* fuzzers to search vulnerabilities - used to discover a computer's security vulnerabilities
	* Skipfish, wapiti, W3af 
* forensic tools - used by white hat hackers to sniff out evidence existing in a computer
	* sleuth kit, helix, maltego, encase
* debuggers - used by black hats to reverse engineer binary files when writing exploits. Used by white hats to analyze malware.
	* GDB, WinDbg, IDA Pro, Immunity Debugger
* hacking operating systems - specially designed OS's with tools optimized for hacking
	* Kali Linux, Knoppix, BackBox Linux
* encryption tools - encode data to prevent unauthorized access
	* VeraCrypt, CipherShed, OpenSSH, OpenSSL, tor, OpenVPN, Stunnel
* vulnerability exploitation tools - identify whether a remote host is vulnerable to a security attack
	* metasploit, core impact, sqlmap, social engineer toolkit, netsparker
* vulnerability scanners - scan a network or system to identify open ports. also scan for known vulnerabilities and scan VMs, byod devices, & client databases
	* Nipper, Secunia PSI, Core Impact, Nessus v6, SAINT, Open VAS


## 3.3.4 - Attack Types
Attack Types:
* eavesdropping attack - sniffing/snooping, captures & listens to network traffic.
* data modification attack - if traffic is captured then the packets can be altered
* ip addr spoofing attack - threat actor crafts an IP packet appearing to originate from a valid addr
* pasword-based attack - uses a password to log in to a valid user account.
* denial-of-service attack - floods a computer or network with traffic until it is unusable
* man in the middle attack - occurs when threat actor is positioned between a source & destination making them able to monitor, capture, & control the communication.
* compromised key attack - once threat actor obtains a secret key they can become privvy to secured communication.
* sniffer attack - a device that can read, monitor, & capture network data exchanges, and read network packets.


# 3.4 - Malware
## 3.4.1 - Overview of Malware
end devices are particularly prone to malware attacks

## 3.4.2 - Viruses & Trojan Horses
### Viruses
Viruses require human action to propagate & infect other computers.
Example: virus infects when someone opens an email atachment, opens a file on USB drive, or downloads a file.

Viruses can:
* alter, corrupt, delete files, or erase entire drives
* cause computer booting issues & corrupt applications
* capture & send sensitive info to threat actors
* access and use email accounts to spread
* lay dormant until summoned by the threat actor

| Types of viruses | Description |
| --- | --- | 
| boot sector virus | attacks boot sector, file partition table, or file system |
| firmware virus | attacks device firmware |
| macro virus | uses the MS office or other appication's macro feature maliciously |
| program virus | inserts itself in another executable program | 
| script virus | attacks the OS interpreter which is used to execute scripts |

### Trojan Horses
| Type of Trojan Horse | Description |
| --- | --- |
| remote-access | enables unauthorized remote access |
| data sending | provides threat actor with sensitive data, such as passwords |
| destructive | corrupts or deletes files |
| proxy | uses the victim's computer as the source to launch attacks & perform other illegal activities |
| FTP | enables unauthorized file transfer services |
| security software disabler | stops antivirus programs or firewalls from functioning |
| DoS | slows or halts network activity |
| keylogger | attempts to steal confidential information |

## 3.4.3 - Other Types of Malware
| Malware | Description |
|--- | --- |
| Adware | usually distributed by downloading online software. displays unsolicited ads |
| Ransomware | denies user access to files by encrypting them. demands ransom for decryption |
| rootkit | used to gain admin level access. difficult to detect because they can alter firewall, antivirus protection, system files, and OS commands to conceal their presence. |
| spyware | similar to adware, used to gather info about the user to send to threat actors. |
| worm | self replicating program, propogates automatically. uses the network to search for other victims with the same vulnerability. |


# 3.5 - Common Network Attacks
## 3.5.1 - Overview of Network Attacks
## 3.5.2 - Video - Reconnaissance Attacks
## 3.5.3 - Reconnaissance Attacks
information gathering. 
| Technique | Description |
| --- | --- |
| perform an information query of a target | OSINT |
| initiate a ping sweep of the target network | helps determine which IP addrs are active |
| initiate a port scan of IP addrs | check which ports are in use (nmap, superscan, angry ip scanner, netscan tools) |
| run vulnerability scanners | nipper, secuna PSI, core impact, nessus v6, SAINT, open VAS |
| Run exploitation tools | metasploit, core impact, sqlmap, social engineer toolkit, netsparker |


## 3.5.4 - Video - Access & Social Engineering Attacks
## 3.5.5 - Access Attacks
exploit known vulnerabilities in authentication services, ftp services, web services

* Password attacks - attempt to gain passwords
* Spoofing attacks - pose as another device by falsifying data
* trust exploitation attack - use unauthorized privileges to gain access to a system
* port redirection attack - uses compromised system as base for attacks, then accesses more restricted systems from there
* man in the middle attack - threat actor is positioned between two legitimate entities intercepting all data
* buffer overflow attack - overwhelms buffer memory with unexpected values rendering the system inoperable resulting in a DoS attack.

## 3.5.6 - Social Engineering Attacks
manipulate individuals into performing actions or divulging confidential info

| Social engineering attack | description |
| --- | --- |
| pretexting | pretend to need personal or financial data to confirm the identity of the recipient |
| phishing | fraudulent email disguised to look legitimate |
| spear phishing | targeted phishing attack |
| spam | junk mail with malicious content |
| something for something | 'quid pro quo', request personal info in exchange for a gift |
| baiting | leaves jump drive in public space |
| impersonation | pretend to be someone else |
| tailgating | follow in an authorized person |
| shoulder surfing | |
| dumpster diving | |


## 3.5.9 - DoS & DDoS Attacks
creates interruption in services to users. 2 major causes:
* overwhelming quantity of traffic
* maliciously formatted packets - designed to crash the receiving device

# 3.6 - IP Vulnerabilities & Threats
## 3.6.1 Video - Common IP & ICMP Attacks
## 3.6.2 - IPv4 & IPv6
ip does not validate the source IP addr in packets. this allows threat actors to spoof their ip addr

| IP attack techniques | Description |
| ICMP Attacks | threat actor uses pings to discover network info, generate DoS flood attacks, & alter host routing tables |
| amplification & reflection attacks | DoS & DDoS attacks |
| address spoofing attacks | spoofed source IP to perform blind spoofing or non-blind spoofing (discussed in 3.6.6)|
| man-in-the-middle (MITM) | eavesdrop on intercepted traffic |
| session hijacking | gain access to physical network & then use MITM to hijack a session |

## 3.6.3 - ICMP Attacks
used for reconnaissance & scanning attacks.
can discover which hosts are active, identify host OS, & determine the state of a firewall.

Networks should have strict ICMP access control lists.
| ICMP messages used by hackers | description |
| --- | --- |
| ICMP echo request & reply | host verification & DoS attacks |
| ICMP unreachable | used to perform network reconaissance & scanning attacks |
| ICMP mask reply | used to map internal IP network |
| ICMP redirects | lure target host into sending all traffic through compromised device (MITM) |
| ICMP router discovery | injects bogus route entries into routing table |


## 3.6.5 - Amplification & Reflection Attacks
1. Amplification - ICMP echo requests are forwarded to many hosts all containing the source IP addr of the victim.
2. Reflection - all those hosts reply to the victim overwhelming it

## 3.6.6 - Address Spoofing Attacks
threat actor creates packets with false source IP addr allowing them to pose as another legitimate user & potentially gain access to otherwise inaccessible data or circumvent security configurations.

* non-blind spoofing - threat actor can see traffic being sent between host & target. Used to inspect reply packets from the target. Can determine the firewall state & hijack authorized sessions
* blind spoofing - used in DoS attacks, threat actor cannot see traffic between host & target


# 3.7 - TCP & UDP Vulnerabilities
## 3.7.1 - TCP Segment Header
6 control bits of TCP segment:
* URG - urgent pointer field significant
* ACK - acknowledgement field significant
* PSH - push function
* RST - reset the connection
* SYN - synchronize sequence numbers
* FIN - no more data from sender

## 3.7.2 - TCP Services
Tcp Provides:
* reliable delivery - uses ACKs to guarantee delivery. data is retransmitted if an ACK is not received in a timely manner.
* Flow control - requiring ACKs can cause delays so TCP uses flow control to mitigate this. multiple segments can be acknowledged with a single ACK segment.
* Stateful communication - TCP requires 3 way handshake to have ocurred before connection is open.

TCP connection established in 3 steps:
1. initiating client requests client-to-server communication session
2. server acknowledges this & requests a server-to-client communication session
3. initiating client acknowledges the server-to-client comm session

## 3.7.3 - TCP Attacks
* TCP SYN Flood Attack - exploits the 3 way handshake
	* threat actor sends loads of TCP SYN session request packets w/randomly spoofed IP addr. the target receives these and is overwhelmed with half-open TCP connections.
* TCP Reset Attack - can terminate TCP comms between two hosts
	* TCP typically terminates in a civilized manner with a four-way exchange of FIN & ACK segments from each endpoint
	* Threat actor could send spoofed packet containing TCP RST to one or both endpoints causing an **uncivilized** termination
* TCP Session Hijacking - threat actor takes over an already-authenticated host as it communicates with the target. Threat actor must spoof the IP addr of one host, predict the next sequence number, & send an ACK to the other host.

## 3.7.4 - UDP Segment Header & Operation
UDP is a connectionless transport layer protocol. Much lower overhead than TCP

## 3.7.5 - UDP Attacks
UDP doesn't use encryption be default.

* UDP Flood Attacks - tools like UDP unicorn or Low Orbit Ion Cannon send a flood of UDP packets, from a spoofed host, to a server. The tools look specifically for closed ports, causing the server to reply with ICMP port unreachable messages creating lots of network traffic.

# 3.8 - IP Services
## 3.8.1 - ARP Vulnerabilities
any client can send an unsolicited ARP reply called a "gratuitous ARP". When this is sent, other hosts store the MAC addr & IP addr contained in the gratuitous arp in their ARP tables.

this feature means any host can claim to own any IP or MAC addr


## 3.8.2 - Arp Cache Poisoning
threat actor sends gratuitous arp claiming to be an IP addr it is not while leaving its real MAC addr so that traffic destined for a certain IP addr is sent to the threat actor first (MITM)

## 3.8.4 - DNS Attacks
dns attacks include:
* DNS Open resolver attacks - publicly open DNS servers (GoogleDNS 8.8.8.8) are known as open resolvers. They are vulnerable to:
	* DNS cache poisoning attacks - spoofed resource record information is sent to DNS resolver to redirect users from legit sites to malicious ones.
	* DNS amplification & relfection - threat actor uses DoS or DDoS to increase volume of attacks & hide the true source. Threat actors send DNS messages to open resolvers using IP addr of a target host.
	* DNS resource utilization - DoS attack consuming all of an open resolver's resources. 
* DNS stealth attacks - used to hide identity
	* Fast Flux - used to hide phishing & malware delivery sites behind quickly-changing network of compromised DNS hosts.
	* Double IP flux - rapidly change hostname to IP addr mappings & change the authoritative name server.
	* Domain Generation Algorithms - used in malware to randomly generate domain names to be used as rendezvous points to their command & control servers.
* DNS domain shadowing attacks - gathers domain account credentials to silently create multiple sub domains to be used during attacks.
* DNS tunneling attacks - described in next section


## 3.8.5 - DNS Tunneling
occurs when threat actors place non-DNS traffic within DNS traffic to circumvent security solutions, to communicate with bots in a protected network, or to exfiltrate data from an organization.

DNS tunneling for CnC commands sent to a botnet:
1. command data split into multiple encoded chunks
2. each chunk is placed in a lower level domain name label of the DNS query
3. the request will end up bein sent to the ISP's recursive DNS servers
4. those servers forward the query to the threat actor's authoritative name server
5. this is repeated until all queries containing the chunks are sent
6. when the threat actor's name server receives the DNS queries it sends responses with encapsulated, encoded CnC commands
7. the malware on compromised hosts recombines the chunks & executes the commands hidden in the DNS record

**To stop DNS tunneling** administrators must use a filter that inspect DNS traffic

## 3.8.6 - DHCP
## 3.8.7 - DHCP Attacks
DHCP Spoofing Attack - rogue dhcp server connected to network
* wrong default gateway - may go undetected while threat actor intercepts data
* wrong DNS server - users are pointed to a malicious website
* wrong IP address - invalid IP addr or default gateway IP are provided resulting in DoS attack


# 3.9 - Network Security Best Practices
## 3.9.1 - Confidentiality, Integrity, and Availability
## 3.9.2 - Defense-in-Depth Approach
layered approach. A combination of networking devices & services working together
Security devices include:
* VPN - router provides a secure encrypted tunnel to access corporate resources
* ASA Firewall - provides powerful stateful firewall servies
* IPS - intrusion prevention system monitors incoming & outgoing traffic
* ESA/WSA - email security appliance & web security appliance
* AAA Server - secure database of who is authorized to access & manage network devices

## 3.9.3 - Firewalls
all firewalls share some common properties:
* resistant to network attacks
* all traffic flows through firewalls
* enforce the access control policy

Several firewall benefits:
* prevent exposure to untrusted users
* sanitize protocol flow
* block malicious data
* reduce security management complexity

firewall limitations:
* misconfigured firewall can fuck everything up
* data from many applications can't be passed through firewall securely
* users may look for ways around firewall to receive blocked material
* can slow network performance
* unauthorized traffic can be tunneled or hidden

## 3.9.4 - IPS
IPS is more scalable than IDS
can come in several different forms:
* router configured with Cisco IOS IPS software
* dedicated device
* network module installed in an adaptive security appliance (ASA), switch, or router

IDS & IPS detect network traffic patterns using signatures. 


## 3.9.5 - Content Security Appliances
* Cisco Email Security Appliance (ESA) - special device designed to monitor SMTP. Cisco ESA is constantly updated by real-time feeds from Cisco Talos. threat intelligence data is pulled every 3-5 minutes.
* Cisco Web Security Appliance (WSA) - combines malware protection, application visibility & control, acceptable use policy controls, & reporting. Provides complete control over how users access the internet.


# 3.10 - Cryptography
## 3.10.1 - Video - Cryptography

## 3.10.2 - Securing Communications
four elements of secure communications:
* Data Integrity - message wasn't altered. ensured by hashing the data & comparing hashes
* Origin Authentication - guarantees message is not a forgery & comes from where it says it does. ensured by protocols like Hash Message Authentication Code (HMAC)
* Data Confidentiality - only authorized users can read the message. implemented using symmetric & asymmetric encryption algorithms
* Data Non-Repudiation - sender cannot repudiate, or refute, the validity of a sent message.

the trend is toward all communication being encrypted


## 3.10.3 - Data Integrity
data is hashed and the hash is sent with the plaintext message. if the receiving device hashes the message and the hashes are different the message aint legit!

## 3.10.4 - Hash Functions
3 well known hash functions:
* MD5 with 128 bit digest - 1 way hash function, produces 128 bit hashed message. **Legacy! don't use this**
* SHA-1 - similar to MD5, created 160 bit hashed message. slightly slower than MD5. **DONT USE THIS**
* SHA-2 - includes SHA-224, SHA-256, SHA-384, SHA-512. 256,384,& 512 are next gen baddass hashers

hashing is vulnerable to **MITM** attacks since they could discern the hashing algorithm, change the message & include the new hash.


## 3.10.5 - Origin Authentication
use a keyed-hash message authentication code (HMAC) to add authentication
* HMAC Hashing Algorithm - is calculated using cryptographic algorithm combining hash function with a secret key. only the sender & receiver know the secret key. this defeats MITM attacks.

## 3.10.6 - Data Confidentiality
**Symmetric encryption ensures data confidentiality**
* symmetric encryption - each party must know pre-shared key. uses short key lengths (40-256bits) & same key encrypts & decrypts data. Faster than assymetric. commonly used for buld data (VPN traffic).
* asymmetric algorithms - long keys (512-4096bits), different key to encrypt & decrypt, takes longer than symmetric, commonly used for quick data transactions (HTTPS)

## 3.10.7 - Symmetric Encryption
alice & bob would have identical keys to a single padlock. the keys were exchanged prior to any secret messages

| Symmetric Encryption Algorithms | Description |
| --- | --- |
| Data Encryption Standard (DES) | legacy algorithm. can be used in stream cipher mode, usually used in block mode w/64bit block size. **a stream cipher encrypts 1 bit at a time** |
| 3DES | newer version of DES, repeats DES process 3 times. been in the field for 35 years! considered very trustworthy when using keys with very short lifetimes |
| AES | secure & more efficient than 3DES. popular & recommended. 9 key & block length combos using variable length of 128-, 192-, 256-bit key & 128,192, or 256bit long data block |
| Software-Optimized Encryption Algorithm (SEAL) | faster than DES, 3DES, & AES. uses 160 bit encryption key. lower impact on CPU |
| Rivest ciphers (RC) series algorithms | developed by Ron Rivest. RC4 is most prevalent version. RC4 is a **stream cipher** used in SSL & TLS |


## 3.10.8 - Asymmetric Encryption
since neither party has a shared secret key very long key lenghts must be used.
key lengths shorter than 1024 bits are considered unreliable.

examples of protocols using Asymmetric key algorithms:
* Internet Key Exchange (IKE) - fundamental component of IPsec VPNs
* Secure Socket Layer (SSL) - now implemented as IETF standard Transport Layer Security (TLS)
* Secure Shell (SSH)
* Pretty Good Privacy (PGP) - often used to increase email security

Common asymmetric encryption algorithms:
| algorithm | key length | description |
| --- | --- | --- |
| Diffie Hellman (DH) | 512,1024,2048,3072,4096 | relies on the assumption that it is easy to raise a number to a certain power & difficult to compute which power was used given the number & the outcome |
| Digital Signature Standard (DSS) & Digital Signature Algorithm (DSA) | 512-1024 | based on EIGamal signature scheme. creation speed is similar to RSA but verification is 10 to 40 times slower |
| Rivest, Shamir, & Adleman (RSA) | 512-2048bit | based on the current difficulty of factoring very large numbers. 1st algorithm to be suitable for signing & encryption. widely used in ecommerce |
| EIGamal | 512-1024 | based on Diffie-Hellman key aggreement. encrypted message becomes very big, almost 2x size of original. should only be used for small messages |
| Elliptical curve techniques | 160 | can be used to adapt many cryptographic algorithms including Diffie-Hellman & EIGamal. its keys can be much smaller |


## 3.10.9 - Diffie-Hellman
commonly used for:
* data exchange using an IPsec VPN
* data encrypted on the internet using either SSL or TLS
* SSH data exchange
