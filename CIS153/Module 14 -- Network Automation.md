Module 14 -- Network Automation

# 14.1 -- Automation Overview
## 14.1.1 -- Video - Automation Everywhere
## 14.1.2 -- The Increase in Automation
Many benefits of automation:
machines can work longer, are more consistent, lots of data can be collected, robots can be used in dangerous conditions, smart devices can reduce energy usage.


# 14.2 -- Data Formats
## 14.2.1 -- Video - Data Formats
## 14.2.2 -- The Data Formats Concept
Common Data formats:
* JavaScript Object Notation (JSON)
* eXtensible Markup Language (XML)
* YAML Ain't Markup Language (YAML)


## 14.2.3 -- Data Format Rules
## 14.2.4 -- Compare Data Formats
YAML uses indentation to format data


## 14.2.5 -- JSON Data Format
JSON is easy to parse and very popular.

## 14.2.6 -- JSON Syntax Rules
* Hierarchical structure & contains nested values
* {} to hold objects, [] to hold arrays
* data is written as key/value pairs


* Keys must be strings within double quotes ""
* keys & values are separated by a colon
* multiple key/value pairs w/in an object are separted by commas
* **whitespace is NOT significant**

## 14.2.7 -- YAML Data format
* considered a superset of JSON.
* key value pairs are separated by colon, without quotes
* a hyphen separates each element in a list


## 14.2.8 -- XML Data Format
* Like HTML
* data is enclosed in a set of tags <tag>data</tag>
* doesn't use any predefined tags or document structure


# 14.3 -- APIs
## 14.3.1 -- Video - APIs
## 14.3.2 -- The API Concept
Application Programmin Interface (API) -- a set of rules describing how one application can interact with another and the instructions to allow the interaction to occur.


## 14.3.3 -- An API Example
## 14.3.4 -- Open, Internal, and Partner APIs
* OpenAPIs (Public APIs) -- publicly available and can be used with no restrictions. Many require the user obtain a free api key or token.
* Internal or Private APIs -- used by an organization or company for internal use only. 
* Partner APIs -- used between a company and its business partners or contractors. 


## 14.3.5 -- Types of Web Service APIs
4 Types of web service APIs:
* Simple Object Access Protocol (SOAP)
	* Microsoft
	* slow to parse, complex, rigid
* Representational State Transfer (REST)
	* less verbose than SOAP, easier to use.
	* Most widely used. accounts for over 80% of all API types used.
* eXtensible Markup Language-Remote Procedure Call (XML-RPC)
* JavaScript Object Notation-Remote Procedure Call (JSON-RPC)

| Characteristic | SOAP | REST | XML-RPC | JSON-RPC |
| --- | --- | --- | --- | --- |
| Data Format | XML | JSON, XML, YAML, and others | XML | JSON |
| First Released | 1998 | 2000 | 1998 | 2005 |
| strengths | well established | flexible formatting, most widely used | well established, simplicity | simplicity |



# 14.4 -- REST
## 14.4.1 -- Video -- REST
## 14.4.2 -- REST & RESTful API
REST API works on top of the HTTP protocol. It defines a set of functions developers can use to perform requests & receive responses via HTTP.

An API can be considered RESTful if it has the following features:
* Client-Server -- client handles the front end data & server handles the back end.
* Stateless -- No client data is stored on the server between requests. The session state is stored on the client.
* Cacheable -- Clients can cache responses to improve performance.


## 14.4.3 -- RESTful Implementation
RESTful web service is a collection of resources implemented using HTTP with 4 defined aspects:
* the base URI for the web service. Ex: http://example.com/resources
* the data format supported by the web service. Often JSON, YAML, or XML.
* the set of operations supported by the web service using HTTP methods
* the API must be hypertext driven

RESTful APIs use common HTTP methods:
| HTTP Method | RESTful operation |
| --- | --- |
| POST | Create |
| GET | Read |
| PUT/PATCH | Update |
| DELETE | Delete |


## 14.4.4 -- URI, URN, & URL
URI -- a string of characters identifying a specific network resource

A URI has 2 specializations:
* Uniform Resource Name (URN) -- Identifies only the namespace of the resource without reference to the protocol.
* Uniform Resource Locator (URL) -- Defines the network location of a resource. HTTP or HTTPS URLs are typically used with web browsers. Other protocols (FTP, SFTP, SSH, & others) can use a URL. Might look like: sftp://sftp.example.com

So if you have a URI of:
* https://www.example.com/author/book.html#page155
The URL is
* https://www.example.com/author/book.html
And the URN is:
* URN = www.example.com/author/book.html

Parts of a URI:
* Protocol/scheme -- HTTPS, FTP, mailto, NNTP, etc.
* hostname -- www.example.com
* path & filename -- /author/book.html
* fragment -- #page155


## 14.4.5 -- Anatomy of a RESTful Request
Parts of an API request include:
* API server -- the URL for the server that answers REST requests
* Resources -- specifies the API that is being requested
* Query -- specifies the data format & info the client is requesting. Can include:
	* Format -- usually JSON, could be YAML or XML
	* Key -- authorization key
	* Parameters -- for example with mapquest directions API parameters include the origination & destination cities.

Keys let API providers:
* authenticate the source of the request
* limit the number of people using the API
* limit the number of requests per user
* better capture & track the data being requested
* gather info about people using the API



# 14.5 -- Configuration Management Tools
## 14.5.1 -- Video - Configuration Management Tools
## 14.5.2 -- Traditional Network Configuration
Configuring everything manually via CLI is stupid.
SNMP is better and widely available but cannot serve as an automation tool.

APIs can be used for network deployment and management automation.


## 14.5.3 -- Network Automation
We're moving to a world where admins deploy and manage hundreds, thousands, even ten of thousands of network devices with the help of software.

Protocols and technologies helping with this include:
REST, Ansible, Puppet, Chef, Python, JSON, XML, etc..


## 14.5.4 -- Configuration Management Tools
These make use of the RESTful API to automate tasks & can scale across thousands of devices.
They maintain the characteristics of a system or network for consistency.

Admins benefit from automating:
* software and version control
* device attribute: names, addressing, and security
* protocol configurations
* ACL configurations


Configuration Tools typically include automation & orchestration.
* Automation -- when a tool automatically performs a task
* Orchestration -- the process of how all automated activities need to happen. The order they happen in & which must happen before others are begun.


Tools to make configuration management easier include:
* Ansible, Chef, Puppet, SaltStack


## 14.5.5 -- Compare Ansible, Chef, Puppet, and SaltStack
| Characteristic | Ansible | Chef | Puppet | SaltStack |
| --- | --- | --- | --- | --- |
| programming language | Python + YAML | Ruby | Ruby | Python |
| agent-based or agentless? | agentless | agent-based | supports both | supports both |
| How are devices managed? | any device can be 'controller' | chef master | Puppet Master | Salt Master |
| what is created by this tool? | Playbook | cookbook | Manifest | Pillar |

Agent-based config is "pull-based" -- the agent on the managed device periodically connects with the master for its config info.
Agentless config is "push-based" -- config script is run on the master, the master connects to the device & executes the script.


# 14.6 -- IBN & Cisco DNA Center
## 14.6.2 -- Intent-Based Networking Overview
IBN -- emerging industry model for next get of networking. Builds on Software-Defined Networking (SDN) turning hardware centric, manual approach into one that is software centric & automated.

Business objectives for the network are expressed as **intent**
IBN captures business intent & translated it into network policies which can be automated & applied consistently across the network.


IBN has 3 essential functions:
* Translation -- lets admins express expected networking behavior that will best support business intent
* Activation -- captured intent is interpreted into policies. Activation function installs those policies into the physical & virtual network infrastructure using automation.
* Assurance -- maintains a continuous validation & verification loop


## 14.6.3 -- Network Infrastructure as Fabric
fabric -- describes an overlay that represents the logical topology used to virtually connect to devices.

overlay -- limits the number of devices an admin must program & provides services & alternate forwarding methods.

underlay network -- physical topology. Includes all hardware required to meet business objectives. Responsible for simple forwarding tasks


## 14.6.4 -- Cisco Digital Network Architecture (DNA)
Cisco implements the fabric using Cisco DNA. 
Cisco DNA is constantly learning and adapting to support business needs.

Some Cisco DNA products & solutions:
| Solution | Description | Benefits |
| --- | --- | --- |
| SD-Access | First intent based enterprise networking solution built using Cisco DNA. Uses single network fabric across LAN and WAN. Segments user, device, & application traffic & automates user-access policies. | Enables rapid network access for any user or device to any application without compromising security |
| SD-WAN | Uses secure, cloud-delivered architecture to centrally managed WAN connections. Simplifies & accelerates delivery of secure, flexible & rich WAN services to connect data centers, branches, campuses, & colocation facilities. | Delivers better user experience for applications on premise or in the cloud. Greater agility & cost savings through easier deployments & transport independence. |
| Cisco DNA Assurance | Used to troubleshoot & increase IT productivity. Applies advanced analytics & machine learning. Provides real-time notification for network conditions requiring attention. | Lets you identify root causes. Cisco DNA Center -- easy to use, single dashboard. |
| Cisco DNA Security | Used to provide visibility, uses the network as a sensor for real-time analysis & intelligence. Provides granular control to enforce policy & contain threats. | Reduce risk & protect your organization against threats. 360 degree visibility through real-time analytics. Lower complexity with end-to-end security. |


## 14.6.5 -- Cisco DNA Center
Cisco DNA Center -- foundational controller and analytics platform at the heart of Cisco DNA. A network management & command center for provisioning & configuring network devices.

5 main areas:
* Design -- model your entire network. Physical and virtual
* Policy -- policies automate & simplify network management.
* Provision -- provide new services to users w/ease, speed, & security.
* Assurance -- proactive monitoring & insights from the network, devices, & applications to predict problems faster & ensure policy config changes achieve business intent. 
* Platform -- use APIs to integrate with your preferred IT systems to create end-to-end solutions & add support for mult-vendor devices.


## 14.6.6 -- Video - DNA Center Overview & Platform APIs
