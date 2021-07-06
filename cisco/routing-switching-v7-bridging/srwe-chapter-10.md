### Spis treści
- [Chapter 1: LAN Security Concepts](#chapter-1-lan-security-concepts)
  - [1.1 Endpoint Security](#11-endpoint-security)
  - [1.2 Access Control](#12-access-control)
  - [1.3 Layer 2 security Threats](#13-layer-2-security-threats)
  - [1.4 MAC Address Table Attack](#14-mac-address-table-attack)
  - [1.5 LAN Attacks](#15-lan-attacks)

# Chapter 1: LAN Security Concepts

SRWE - Switching, Routing and Wireless Essentials Bridging Concept

## 1.1 Endpoint Security

Cyber attacks often involve one or more of the following:
- `DDoS` - Distributed Denial of Service, coordinated attack from many devices, called zombies, with the intention of degrading or halting public access to an organization’s website and resources,
- `Data Breach` – organization’s data servers or hosts are compromised to steal confidential information,
- `Malware` – organization’s hosts are infected with malicious software that cause a variety of problems. For example, ransomware such as WannaCry, encrypts the data on a host and locks access to it until a ransom is paid

Network security devices:
- `VPN enabled router` - provides a secure connection to remote users across a public network and into the enterprise network. VPN services can be integrated into the firewall,
- `NGFW` - Next-generation firewall, provides stateful packet inspection, application visibility and control, a next-generation intrusion prevention system (`NGIPS`), advanced malware protection (`AMP`), and URL filtering,
- `NAC` - Network Access Control, includes authentication, authorization, and accounting (AAA) services. In larger enterprises, these services might be incorporated into an appliance that can manage access policies across a wide variety of users and device types. The Cisco Identity Services Engine (ISE) is an example of a NAC device

Endpoints are hosts which commonly consist of:
- laptops, 
- desktops, 
- servers, 
- IP phones,
- employee-owned devices that are typically referred to as bring your own devices (`BYODs`)

Endpoint - traditional host-based security features:
- antivirus/antimalware,
- host-based firewalls,
- HIPS - host-based intrusion prevention systems
 
However, today endpoints are best protected by a combination of:
- NAC, 
- host-based Advanced Malware Protection(`AMP`) software,
- email security appliance (`ESA`),
- web security appliance (`WSA`)

>According to the Cisco’s Talos Intelligence Group, in June 2019, 85% of all email sent was spam. Phishing attacks are a particularly virulent form of spam.

Email Security Appliance (ESA) is a device that is designed to monitor Simple Mail Transfer Protocol (SMTP). The Cisco ESA is constantly updated by real-time feeds from the Cisco Talos, which detects and correlates threats and solutions by using a worldwide database monitoring system. This threat intelligence data is pulled by the Cisco ESA every three to five minutes. These are some of the functions of the Cisco ESA:
- block known threats,
- remediate against stealth malware that evaded initial detection,
- discard emails with bad links,
- block access to newly infected sites,
- encrypt content in outgoing email to prevent data loss

Cisco Web Security Appliance (WSA) mitigation technology for web-based threats. It combines advanced malware protection, application visibility and control, acceptable use policy controls, and reporting.

Cisco WSA provides complete control over how users access the internet. Certain features and applications, such as chat, messaging, video and audio, can be allowed, restricted with time and bandwidth limits, or blocked, according to the organization’s requirements. The WSA can perform blacklisting of URLs, URL-filtering, malware scanning, URL categorization, Web application filtering, and encryption and decryption of web traffic.

## 1.2 Access Control

Methods of access control:
- Local password:
  - Login & password,
  - SSH
- AAA - Authentication, Authorization, Accounting
  - `RADIUS` - Remote Authentication Dial-In User Service
  - `TACACS +` - Terminal Access Controller Access Control System
- 802.11x - port-based access control and authentication protocol. The authentication server authenticates each workstation that is connected to a switch port before making available any services offered by the switch or the LAN
  - Suplicant - client
  - Authenticator - switch
  - Authentication - auth server

`Authorization` - governs what users can and cannot do on the network after they are authenticated

`Accounting` - collects and reports usage data

Limitation of local database:
- must be configured locally on each device
- no fallback authentication method - password recovery

## 1.3 Layer 2 security Threats

> If Layer 2 is compromised, then all the layers above it are also affected.

Layer 2 attacks:
- MAC table - MAC address flooding attack,
- VLAN - VLAN Hopping, VLAN Double tagging, attack between common devices on same VLAN,
- DHCP - DHCP Starvation and DHCP Spoofing attack,
- ARP - ARP Spoofing, ARP poisoning attack,
- Adress Spoofing - MAC address and IP address spoofing attacks,
- STP attacks - Spanning Tree Protocol manipulation attacks

Layer 2 attack mitigation:
- `port security` - prevents many types of attacks including MAC address flooding attacks and DHCP starvation attacks,
- `DHCP Snooping` - prevents DHCP starvation and DHCP spoofing attacks,
- `DAI` - Dynamic ARP Inspection, prevents ARP spoofing and ARP poisoning attacks.
- `IPSG` - IP Source Guard, prevents MAC and IP address spoofing attacks

Management protocols(Syslog, SNMP, TFTP, telnet, FTP) should also be secure

Recomendation:
- always use secure variants of these protocols such as SSH, Secure Copy Protocol (SCP), Secure FTP (SFTP), and Secure Socket Layer/Transport Layer Security (SSL/TLS),
- consider using out-of-band management network to manage devices,
- use a dedicated management VLAN where nothing but management traffic resides,
- use ACLs to filter unwanted access.

## 1.4 MAC Address Table Attack

MAC address tables are stored in memory and are used to more efficiently forward frames. 

All MAC tables have a fixed size and consequently, a switch can run out of resources in which to store MAC addresses. 

MAC address flooding attacks take advantage of this limitation by bombarding the switch with fake source MAC addresses until the switch MAC address table is full.

When this occurs, the switch treats the frame as an unknown unicast and begins to flood all incoming traffic out all ports on the same VLAN without referencing the MAC table(like hub). This condition now allows a threat actor to capture all of the frames sent from one host to another on the local LAN or local VLAN.

`macof` - tool to make MAC address table attack

Another reason why these attack tools are dangerous is because they not only affect the local switch, they can also affect other connected Layer 2 switches. When the MAC address table of a switch is full, it starts flooding out all ports including those connected to other Layer 2 switches.

To mitigate MAC address table overflow attacks, network administrators must implement `port security`. Port security will only allow a specified number of source MAC addresses to be learned on the port. Port security is further discussed in another module.

## 1.5 LAN Attacks

VLAN Hopping attack:
- enables traffic from one VLAN to be seen by another VLAN `without` the aid of a router(Dynamic Auto, Dynami Desirable),
- a threat actor configures the host(act like a switch) to `spoof 802.1Q and DTP signaling` to trunk with the connectiong switch,
- the threat actor can send and receive traffic on any VLAN, effectively `hopping between VLANs`


VLAN Double-tagging attack:
- a threat actor may `embed a hidden 802.1Q tag` inside the frame that already has an 802.1Q tag
- this tag allows the frame to go to a VLAN that the `original 802.1Q tag did not specify`
- it only works when the attacker is connected to a port residing in the `same VLAN as the native VLAN` of the trunk port

DHCP servers dunamically provide IP configuration information including:
- IP address,
- subnet mask,
- default gateway,
- DNS servers,
- and more

DHCP uses `DORA`:
- `D`iscover
- `O`ffer
- `R`equest
- `A`cknowlege

DHCP attacks:
- DHCP Starvation attack
  - the goal of this attack is to create a DoS for connecting clients,
  - a threat actor created `DHCP discovery messages with bogus MAC addresses`
- DHCP Spoofing attack - a rouge DHCP server is connected to the network and provides false IP configuration parameters to legitime clients:
  - wrong default gateway,
  - wrong DNS server,
  - wrong IP address

>A VLAN double-tagging attack is unidirectional and works only when the attacker is connected to a port residing in the same VLAN as the native VLAN of the trunk port. The idea is that double tagging allows the attacker to send data to hosts or servers on a VLAN that otherwise would be blocked by some type of access control configuration. Presumably the return traffic will also be permitted, thus giving the attacker the ability to communicate with devices on the normally blocked VLAN.

VLAN attack mitigation:
- disable trunking on all access ports,
- disable auto trunking on trunk links so that trunks must be manually enabled,
- be sure that the native VLAN is only used for trunk links

ARP Spoofign & Poisoning Attack:
- a client can send an unsolicited ARP Reply called "gratuitous ARP",
- when a host sends a gratuitous ARP, other hosts on the subnet store the MAC address and IP address contained in the gratuitous ARP in their ARP tables,
- an attacker can send a gratuitous ARP message containing a spoofed MAC address to a switch and the switch would update its MAC table accordingly,
- `any host can claim to be the owner of any IP and MAC address combination they choose`

ARP MitM:
- dsniff,
- Cain & Abel,
- ettercap,
- Yersina

>IPv6 uses ICMPv6 Neighbor Discovery Protocol for Layer 2 address resolution. IPv6 includes strategies to mitigate Neighbor Advertisement spoofing, similar to the way IPv6 prevents a spoofed ARP Reply.

>When the target host sends traffic, the switch will correct the error, realigning the MAC address to the original port. To stop the switch from returning the port assignment to its correct state, the threat actor can create a program or script that will `constantly send frames` to the switch so that the switch maintains the incorrect or spoofed information. There is no security mechanism at Layer 2 that allows a switch to verify the source of MAC addresses, which is what makes it so vulnerable to spoofing.

STP attack:
- netowrk attackers can manipulate STP to spoof the root bridge and `change the topology of a network`
- attackers can make their hosts appear as `root bridges` and therfore `capture all traffic` for the immediate switched domain
- the BPDUs sent by the attacking host announce a lower bridge priority in an attempt to be elected as the root bridge 

CDP reconnaissance:
- CDP is a proprietary L2 link discovery protocol and it is `enabled by default`,
- CDP can automatically discover other CDP enabled devices and help auto-configure their connection,
- CDP information is sent out CDP enable ports in `periodic, unencrypted broadcasts`,
- CDP information includes the IP address of the device, IOS software version, platform, capabilities and the native VLAN

>To mitigate the exploitation of CDP, limit the use of CDP on devices or ports. For example, disable CDP on edge ports that connect to untrusted devices.

```
S1(config)# no cdp run

S1(config-if)# no cdp enable
```

LLDP - Link Layer Discovery Protocol - vendor neutral CDP

```
S1(config)# no lldp run

S1(config-if)# no lldp transmit
S1(config-if)# no lldp receive
```



---

<div align="right">
<a href="srwe-chapter-11.md">Next: SRWE 11</a>
</div>