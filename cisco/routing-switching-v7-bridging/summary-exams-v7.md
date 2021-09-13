`LLC`:
- it places information in the frame allowing multiple Layer 3(IPv4 and IPv6) protocols to use the same network interface and media,
- places information in the frame that identifies which network layer protocol is being used for the frame,
- adds layer 2 control information to network protocol data
- logical link control is implemented in software
- the data link layer uses LLC to communicate with the upper layers of the protocol suite. 

`MAC`:
- implements a trailer to detect transmission errors,
- controls the NIC responsible for sending and receiving data on physical medium,
- integrates various physical technologies,
- provides synchronization between source and target nodes,
- provides a mechanism to allow multiple devices to communicate on the physical medium

IPv4 packet:
- version - contains 4 bit value set to 0100
- protocol - used to identify the next level protocol
- differentiated services - used to determine the priority of each packet
 
IPv6 packet:
- flow label - inform router to maintain the same path for the packets in the same conversation 

setup mode - no config file in NVRAM.

ARP messages have a type field of `0x806`. This informs the receiving NIC that the data portion of the frame needs to be passed to the ARP process.

Differentiated services - IPv4 packet header that contains an 8 bi binary value used to determine the priority of each packet

Ping response codes:
- 0 - network unreachable,
- 1 - host unreachable,
- 2 - beyond scope of source address,
- 3 - address unreachable,
- 4 - protocol unreachable

IPv6:
- unicast,
- multicast,
- anycast

IPv6 Hop Limit => time exceeded

Send mail => DNS + SMTP

DHCP Discover message:
- the destiantion IP address is 255.255.255.255,
- the message comes from a client seeking an IP address,
- all hosts receive the message, but only a DHCP server replies

Packet size * Packet Number + 1

A structured network troubleshooting approach should include these steps in sequence:
1. Identify the problem.
2. Establish a theory of probable causes.
3. Test the theory to determine cause.
4. Establish a plan of action to resolve the issue.
5. Verify full system functionality and implement preventive measures.
6. Document findings, actions, and outcomes.

WhatAreyouwaiting4 - passphrase

show ipv6 interface - verify all of the IPv6 interface address on router

show running-config - verify that IPv6 is enabled

website address - nslookup 

Cisco IOS ping characters:
- `!` - completed successfuly, 
- `.` - indicate connection error,
- `U` - indicated that a router along the path may not have had a route to the destination address

history:
- set the command history buffer size,
- recall previously entered commands

auto-mdix - show controllers

set IOS image file - BOOT env variable, boot system

terminal monitor - to send debug messages to console or vty

minimum ethernet frame size => 64 bytes

native vlan:
- the native traffic vlan will be untagged across the trunk link,
- the native provides a common identifier to both ends of a trunk

What happens to switch ports after the vlan to which they are assigned is deleted? => the ports stop communicating with the attached devices

Exit command - returns to previous configuration mode

SVI inter-VLAN routing:
- switching packets is faster with SVI,
- there is no need for a connectio to a router

IEEE 802.11Q standard VLAN tag field:
- `type` - a value for the tag protocol ID value,
- `VLAN ID` - a VLAN number,
- `user priority` - a value that supports level or service implementation,
- `Cannonical Format Identifier` - an identifier that enables Token Ring frames to be carried across Ethernet links

DTP modes:
- `dynamic desirable` - actively attempts to convert the link to a trunk,
- `dynamic auto` - passively waits for the neighbor to initiate trunking,
- `nonegotiate` - requires manual configuration of trunking or nontrunking,
- `trunk` - permanent trunking mode

Routed port on a L3 Switch is not assigned to a VLAN

Initially the management VLAN is default VLAN

DTP configuration
- dynamic auto - access
- dynamic desirable - trunk
- trunk
- access

STP port roles:
- root - port which is near the root bridge,
- designated - other than root,
- alternate & backup - blocking state,
- shutdown, disabled

BID - Bridge ID(lower = better):
- bridge priority(default 32786),
- extended system ID,
- interface MAC address 

MSTP - IEEE PVSTP+

PAgP - Cisco protocol, modes:
- on
- desirable - active negotiation
- auto - passive negotiation

LACP - IEEE standard:
- on
- active
- passive

In `forwarding` & `learning` port states a switch learn MAC addresses and process BPDUs in PVST.

EtherChanell load-balancing methods:
- source IP to destination IP
- source MAC to destination MAC

ICMPv6
flags(MO)=>(01) -> look for other configuration parameters(DNS) on DHCPv6 servers

(MO)=>(11)=>stateful DHCPv6

flags:
- m - managed address config
- o - other 

>RA messages that are sent from routers that are configured as `stateful DHCPv6 servers` have the M flag set to 1 with the command `ipv6 nd managed-config-flag`, whereas `stateless DHCPv6 servers` are indicated by setting the O flag to 1 with the `ipv6 nd other-config-flag` command.

The devices involved in the 802.1X authentication process are as follows:
- The `supplicant`, which is the client that is requesting network access
- The `authenticator`, which is the switch that the client is connecting to and that is actually controlling physical network access
- The `authentication server`, which performs the actual authentication

Port security default => shutdown

```
S1(config)# spanning-tree bpduguard default
S1(config-if)# spanning-tree bpduguard enable
```

Cisco provides solutions to help mitigate Layer 2 attacks including these:
- `IP Source Guard` (IPSG) – prevents MAC and IP address spoofing attacks
- `Dynamic ARP Inspection` (DAI) – prevents ARP spoofing and ARP poisoning attacks
- `DHCP Snooping` – prevents DHCP starvation and SHCP spoofing attacks
- `Port Security` – prevents many types of attacks including MAC table overflow attacks and DHCP starvation attacks

Fully specified static route = the IP address of the next-hop neighbor + he interface ID exit interface 

Lowest administrative distance = directly connected network

TTL = 0 => IMCP Time Exceeded

[IPv6 multicast addresses](https://menandmice.com/blog/ipv6-reference-multicast)

show ip int brief

status => up
protocol => down

Interface is turned on, but no cable is connected

IPv6 address = global routing prefix + subnet ID + interface ID

S* => default route, gw of last resort

type => used by LLC to identify the Layer 3 protocol

Bridge priority + VLAN ID => 65 535

Etherchannel on L3 Switch

DHCPv6:
- RA
- Solicit
- Advertise
- Request
- Response

Active mode => do not send beacons

`authentication` - proves that users are who the say they are

`authorization` - determines what resources users can access or the operations they are allowed to perform

`accounting` - calculates how much a user must pay for remote access to a device

Port security violation:
- shutdown - default
- protect - drop packet & do not log event
- restrict - drop packet & log syslog event

Cisco endpoint solutions:
- Web Security Applicance,
- Email Security Applicance,
- NAC Appliance

Network devices operate in two planes:
- `the data plane` - forwards traffic
- `the control plane` - maintains L2 & L3 forwarding mechanisms using the CPU

Cloud computing is used to separate the application or service from hardware. Vritualization separates the operating system from the hardware

`IBN` - Intent-based networking

`SDN` - Software defined networking

Chef, Puppet(Manifest) => Ruby
Ansible, SaltStack(Pilliar) => Python
RESTCONF is a network managemt protocol

The routing table pre-populates the FIB on Cisco devices that use CEF to process packets

FF02::1 - all nodes
FF02::2 - solicited-node

`Unique local` - FC00::/7, IPv6 address type, provides communication between subnets and cannot route on the internet

In the 5GHz band, we have channels ranging from 36 up to 165, and in the 6 GHz band, we have Wi-Fi channels ranging from 1-233. Both frequencies allow for channel width from 20 MHz-160 MHz).

Late colision - occurs after the first 64 bytes of the frame is received

Preambule is a 7 byte field in the Ethernet frame which helps to receiver to know that it is an actual data coming(Ethernet Frame)

Cisco Unified Wireless Network solution WLANs support 4 levels of QoS:
- Platinum/Voice,
- Gold/Video,
- Silver/Best Effort(default),
- Bronze/Background

Useful links:
- [CCNA v7 exam organization](https://csproidea.pl/egzamin-ccna-v7-wszystko-co-musisz-wiedziec/)
- [Similar Course Content](https://itexamanswers.net/ccna-1-v7-exam-answers-introduction-to-networks-v7-0-itn.html)
- [Test example](https://www.itexams.com/exam/200-301)