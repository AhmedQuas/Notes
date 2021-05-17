### Spis treści
- [Chapter 9: Implementing the Cisco Adaptive Secuirty Appliance](#chapter-9-implementing-the-cisco-adaptive-secuirty-appliance)
  - [9.1 Introduction to the ASA](#91-introduction-to-the-asa)
  - [9.2 ASA Firewall Configuration](#92-asa-firewall-configuration)
    - [9.2.1 Configuring Management Settings and Services](#921-configuring-management-settings-and-services)
    - [9.2.2 Object groups](#922-object-groups)
    - [9.2.3 ACLS](#923-acls)
    - [9.2.4 NAT](#924-nat)
    - [9.2.5 AAA](#925-aaa)
    - [9.2.6 Modular Policy Framework](#926-modular-policy-framework)

# Chapter 9: Implementing the Cisco Adaptive Secuirty Appliance

Cisco provides two firewall solutions: 
- the firewall-enabled ISR,
- the Cisco Adaptive Security Appliance (`ASA`)

## 9.1 Introduction to the ASA

The Cisco ASA 5500 series is a primary component of the Cisco Secure Borderless Network. The Cisco ASA 5500 series helps organizations provide secure, high performance connectivity and protect critical assets by integrating the following:
- proven firewall technology,
- comprehensive, highly effective intrusion prevention system (IPS) with Cisco Global Correlation and guaranteed coverage,
- high-performance VPNs and always-on remote-access,
- failover feature for fault tolerance

ASA firewall models:
- ASA 5505/Secuirty Plus - up to 150 Mbps, the default DRAM memory is 256 MB (upgradable to 512 MB) and the default internal flash memory is 128 MB
- ASA 5506-X/Security Plus - 750 Mbps, the default DRAM memory is 4 GB, it also includes 8 GB of flash. Unlike the ASA 5505, the ASA 5506-X does not use switchports. All interfaces are routed and require IP addresses
- ASA 5512-X/Security Plus - 1 Gbps
- ASA 5515-X - 1.2 Gbps
- ASA 5525-X - 2 Gbps
- ASA 5545-X - 3 Gbps
- ASA 5555-X - 4 Gbps
- ASA 5585-X SSP10 - 4 Gbps
- ASA 5585-X SSP20 - 10 Gbps
- ASA 5585-X SSP40 - 20 Gbps
- ASA 5585-X SSP60 - 40 Gbps
- ASA Service Module - 20 Gbps

`X` - FirePOWER Services devices

>New line of next-generation firewalls (NGFW) which combines their proven firewall technology with Sourcefire advanced threat and malware detection capabilities.

>Cisco Adaptive Security Virtual Appliance (`ASAv`) brings the power of ASA appliances to the virtual domain.

The ASA software features:
- basic:
  - firewall, 
  - VPN concentrator, 
  - intrusion prevention functionality
- advanced:
  - `virtualization` - single ASA can be partitioned into multiple virtual devices. Each virtual device is called a security context. Each context is an independent device, with its own security policy, interfaces, and administrators. Many features are supported in multiple context modes, including routing tables, firewall features, IPS, and management. Some features are not supported, including VPN and dynamic routing protocols.
  - `high availability with failover` - two identical ASAs can be paired into an active / standby failover configuration to provide device redundancy. Both platforms must be identical in software, licensing, memory, and interfaces, including the Security Services Module (`SSM`)
  - `identity firewall` - ASA provides optional, granular access control based on an association of IP addresses to Windows Active Directory login information
  - `threat control and containment services` - dvanced IPS features can only be provided by integrating special hardware modules with the ASA architecture. IPS capability is available using the Advanced Inspection and Prevention (`AIP`) modules

>The ASA is a dedicated firewall appliance. By default, it treats a defined inside interface as the trusted network and any defined outside interfaces as untrusted networks.

ASA firewall modes of operation:
- `router mode` - two or more interfaces separate Layer 3 networks. Routed mode supports multiple interfaces. Each interface is on a different subnet and requires an IP address on that subnet. The ASA applies policy to flows as they transit the firewall
- `transparent mode` - ASA functions like a Layer 2 device and is not considered a router hop(bump in the wire or a stealth firewall). This mode is useful to simplify a network configuration, or when the existing IP addressing cannot be altered. However, the drawbacks include no support for dynamic routing protocols, VPNs, QoS, or DHCP Relay

>Each interface has an associated security level(trust level). These security levels enable the ASA to implement security policies. 

>Each operational interface must have a name and a security level from 0 (lowest) to 100 (highest) assigned.

>When traffic moves from an interface with a higher security level to an interface with a lower security level, it is considered `outbound traffic`. Conversely, traffic moving from an interface with a lower security level to an interface with a higher security level is considered `inbound traffic`.

Security Level Control:
- `network access` - implicit permit from a higher security interface to a lower secuirty interface(outbound)
- `inspection engines` - some application inspection engines are dependent on the secuirty level. When interfaces have the `same security level`, the ASA inspects traffic in either direction
- `application firewall` - HTTP(S) and FTP filtering applies only for outbound connections, from higher level to a lower level

## 9.2 ASA Firewall Configuration

### 9.2.1 Configuring Management Settings and Services

ASA CLI recognizes:
- abbreviation of commands and keywords,
- use of the Tab key to complete a partial command,
- use of the help key (?) after a command to view additional syntax

| IOS Router Command  | Equivalent ASA Command  |
|:---:        |:---:      |
|enable secret PASSWORD | enable password PASSWORD |
|line vty 0 4; password PASSWORD; login| passwd PASSWORD |
| ip route | route IF_NAME |
| show ip interfaces brief | show interfaces ip brief |
| show ip route | show route |
| show vlan | show switch vlan |
| show ip nat translations | show xlate |
| copy running-config startup-config | write \[memory\] |
| erase startup-config | write erase |

```
ASA# help write
```

Restore ASA to its facory default configuration

```
ASA(config)# configure factory-default
```

Run ASA interactive setup initialization wizard & erase configuration

```
ASA(config)# write erase
ASA(config)# reload
```

Enter global configuration mode

```
ASA> enable
Password: <blank>
ASA# clock set 12:00:00 1 April 2021
ASA# configure terminal
ASA(config)#
```

Configure basic settings

```
ASA(config)# hostname ASA
ASA(config)# domain-name ccnasecurity.com
ASA(config)# enable password PASSWORD
ASA(config)# banner motd ---------------
ASA(config)# banner motd Auth Acc only
ASA(config)# banner motd ---------------
```

Enable AES password encryption

```
ASA(config)# key config-key password-encryption cisco123
ASA(config)# password encryption aes

ASA(config)# show password encryption
```

Configuring logical VLAN interfaces & assign layer 2 ports to VLANs

```
ASA(config)# interface vlan 1
ASA(config-if)# nameif inside
ASA(config-if)# security-level 100
ASA(config-if)# ip address 192.168.1.1 255.255.255.0

ASA(config)# interface g0/0
ASA(config-if)# switchport access vlan 1
ASA(config-if)# no shutdown

ASA# show switch vlan
ASA# show interface [ip brief] 
ASA# show ip address
```

IP address of an interface can be configured using one of the following options:
- `manually`
- `DHCP`
- `PPPoE`

Configurea a default static route

```
ASA(config)# route outside 0.0.0.0 0.0.0.0 209.165.200.225

ASA(config)# show route | begin Gateway
```

Configuring Telnet access 

```
ASA(config)# password
ASA(config)# telnet 192.168.1.10 255.255.255.255 inside
ASA(config)# telnet timeout 3

ASA(config)# show run telnet
```

Configuring SSH access

```
ASA(config)# username ADMIN password PASSWORD
ASA(config)# aaa authentication ssh console LOCAL
ASA(config)# crypto key generate rsa modulus 2048
ASA(config)# ssh 192.168.1.10 255.255.255.255 inside
ASA(config)# ssh 192.168.1.20 255.255.255.255 inside
ASA(config)# ssh version 2

ASA(config)# show ssh
```

Configuring NTP

```
ASA(config)# ntp authenticate
ASA(config)# ntp trusted-key 1
ASA(config)# ntp authentication-key md5 cisco123
ASA(config)# ntp server 192.168.1.254

ASA(config)# show ntp status
ASA(config)# show ntp associations
```

Configuring DHCP Services

```
ASA(config)# dhcpd address 192.168.1.10-192.168.1.30 inside
ASA(config)# dhcpd lease 1800

ASA(config)# show dhcpd state
ASA(config)# show dhcpd binding
ASA(config)# show dhcpd statistics

ASA(config)# clear dhcpd binding
ASA(config)# clear dhcpd statistics
```

### 9.2.2 Object groups

Objects are created and used by the ASA in place of an inline IP address in any given configuration. An object can be defined with: 
- particular IP address,
- entire subnet,
- range of addresses,
- protocol, 
- specific port,
- range of ports. 
 
>The object can then be re-used in several configurations. The advantage is that when an object is modified, the change is automatically applied to all rules that use the specified object.

Types of objects that can be created:
- network object
  - host
  - subnet
  - range
- service object
  - source or destination port

>A network object name can contain only one IP address and mask pair. Therefore, there can only be one statement in the network object. Entering a second IP address/mask pair replaces the existing configuration.

Configuring a Network Object

>Network objects `can be defined using one of the three methods` listed below.

```
ASA(config)# object network EXAMPLE-1
ASA(config-network-object)# host 192.168.1.10
ASA(config-network-object)# subnet 192.168.1.0 255.255.255.0
ASA(config-network-object)# range 192.168.1.10 192.168.1.20

ASA(config)# show running-config object

ASA(config)# no object network EXAMPLE-1
ASA(config)# clear config object network
```

Configuring a Service Object

```
ASA(config)# object service EXAMPLE-2
ASA(config-service-object)# service tcp destination eq ftp
ASA(config-service-object)# service tcp [destination\source] [eq\lt\gt\neq\range] www

ASA(config)# show running-config object service
```

>Objects can be grouped together to create an `object group`. By grouping like objects together, an object group can be used in an access control entry (`ACE`) instead of having to enter an ACE for each object separately.

Types of Objects Groups:
- `network` - network-based object group specifies a list of IP host, subnet or network address
- `service` - used to group TCP, UDP ports into an object. The ASA enables the creation of a service object group that can contain a mix od TCP, UDP, ICMP services and any protocol such as ESP & GRE 
- `security` - used in features that support Cisco TrustSec by including the group in extended ACL, which in turn can be used in an access rule
- `ICMP-type`
- `user` - locally created, as well as imported Active Directory user groups can be defined for use in features that support the identity firewall

The following guidelines and limitations apply to object groups:
- objects and object groups share the same name space,
- object groups must have unique names,
- an object group cannot be removed or emptied if it is used in a command,
- the ASA does not support IPv6 nested object groups

Network object group configuration

```
ASA(config)# object-group network ADMIN-HOSTS
ASA(config-network-object-group)# description Admin hosts
ASA(config-network-object-group)# network object host 192.168.10.12
ASA(config-network-object-group)# group-objects OTHER_OBJECT_GROUP

ASA(config)# show run object-group

ASA(config)# clear configure object-group
```

### 9.2.3 ACLS

>ASA ACLs differ from IOS ACLs in that they use a network mask (e.g., 255.255.255.0) instead of a wildcard mask (e.g. 0.0.0.255). Also most ASA ACLs are named instead of numbered.

Types of ASA ACL filtering:
- `through-traffic filtering` - traffic that is passing through the security appliance from one interface to another
- `to-the-box-traffic filtering` - management access rule, traffic that terminates on the ASA

>By default, security levels apply access control without an ACL configured. Traffic from a less secure interface is blocked from accessing more secure interfaces.

>To allow connectivity between interfaces with the same security levels, the `same-security-traffic permit inter-interface` global configuration mode command is require

Supported by ASA types of access list:
- extended - most common,
- standard - used to identify the destination IP addresses,
- EtherType - can be configured only if the security appliance is running in transparent mode,
- WebType - used in a configuration that supports filtering for clientless SSL VPN.
- IPv6 - used to determine which IPv6 traffic to block and which traffic to forward at router interfaces.

```
ASA# help access-list
ASA(config)# access-list NAME extended permit ip src dst
ASA(config)# access-group NAME {in|out} interface IF_NAME

ASA(config)# access-list NAME extended permit ip object-group EXAMPLE1 object-group EXAMPLE2 object-group SERVICE1

ASA# show access-list
ASA# show run access-list

ASA(config)# clear configure access-list NAME
```

### 9.2.4 NAT

Cisco ASA supports the following common types of NAT:
- dynamic NAT - many-to-many
- dynamic PAT - many-to-one
- static NAT - one-to-one
- policy NAT - policy-based NAT

Types of NAT deployments:
- inside NAT - host from higher-security interface has traffic destined for a lower-security interface and ASA translates the internal host address into a global address
- outside NAT - this method may be useful to make an enterprise host located in the outside of the internal network appears as one from a know internal IP address
- bidirectional NAT

>These three types of NAT are referred to as network object NAT because the configuration requires network objects to be configured.

Dynamic NAT configuration 

```
ASA(config)# object network PUBLIC
ASA(config-network-object)# range 209.165.200.240 209.165.200.248

ASA(config)# object network DYNAMIC_NAT
ASA(config-network-object)# subnet 192.168.1.0 255.255.255.224
ASA(config-network-object)# nat (inside,outside) dynamic PUBLIC

ASA# show xlate
ASA# show nat [detail]
ASA# clear nat counters
```

Enable return traffic(secuirty-level issue)

```
ASA(config)# policy-map GLOBAL_POLICY
ASA(config-pmap)# class inspection_default
ASA(config-cmap)# access-list ICMPACL extended permit icmp any any
ASA(config)# access-group ICMPACL in interface outside
```

PAT configuration

```
ASA(config)# object network INSIDE_NET_PAT
ASA(config-network-object)# subnet 192.168.1.0 255.255.255.0
ASA(config-network-object)# nat (inside,outside) dynamic interface
```

Static NAT configuration

```
ASA(config)# object network DMZ_SERVER
ASA(config-network-object)# host 192.168.2.10
ASA(config-network-object)# nat (dmz,outside) static 209.165.200.227

ASA(config)# access-list OUTSIDE-DMZ extended permit ip any host 192.168.2.10
ASA(config)# access-group OUTSIDE-DMZ in interface outside

ASA(config)# policy-map GLOBAL_POLICY
ASA(config-pmap)# class inspection_default
ASA(config-cmap)# access-list ICMPACL extended permit icmp any any
ASA(config)# access-group ICMPACL in interface dmz
```

### 9.2.5 AAA

Local AAA uses a local database for authentication. This method stores usernames and passwords locally on the ASA, and users authenticate against the local database. Local AAA is ideal for small networks that do not need a dedicated AAA server.

>Unlike the ISR, ASA devices do not support local authentication without using AAA.

Simple AAA TACACS+ server configuration

```
ASA(config)# username Admin password cisco1234 privilege 15
ASA(config)# show run username

ASA(config)# clear config username

ASA(config)# aaa server TACACS-SRV protocol tacacs+
ASA(config-aaa-server-group)# aaa-server TACACS-SRV (dmz) host 192.168.2.10
ASA(config)# show run aaa-server

ASA(config)# clear config aaa-server
```

```
ASA(config)# aaa authentication http console TACACS-SRV LOCAL
ASA(config)# show run aaa

ASA(config)# clear config aaa
ASA(config)# show running-config username
```

### 9.2.6 Modular Policy Framework

>A Modular Policy Framework (`MPF`) configuration defines a set of rules for applying firewall features, such as traffic inspection and QoS, to the traffic that traverses the ASA. MPF allows granular classification of traffic flows, to apply different advanced policies to different flows.

Steps to configure MPF on an ASA:
1. Configure extended ACLs to identify granular traffic that can be specifically referenced in the class map
2. Configure `class map` to identify L3/4 traffic
3. Configure `policy map` to apply actions to those class maps
4. Configure a `service policy` to attach the policy map to an interface

```
ASA(config)# access-list TCP permit tcp any any
ASA(config)# class-map ALL_TCP
ASA(config-cmap)# description "This matches all TCP traffic"
ASA(config-cmap)# match access-list TCP

ASA# show run class-map
ASA# clear configure class-map
```

>Policy maps are used to bind class maps with actions.

```
ASA(config)# policy map POLICY_MAP
ASA(config-pmap)# class ALL_TCP 
ASA(config-pmap-c)# inspect PROTOCOL
ASA(config-pmap-c)# [inspect/set connection/police]
ASA(config)# show running-config policy-map

ASA(config)# service-policy POLICY_MAP global
ASA(config)# show service-policy
ASA(config)# show running-config service-policy

ASA# clear configure service-policy
ASA# clear service-policy
```

>The ASA also automatically defines a default Layer 3/4 class map identified in the configuration by `class-map inspection_default`. Identified in this class map is the match default-inspection-traffic which matches the default ports for all inspections.

---

<div>
<a href="chapter-08.md">Prev: Chapter 8</a>
</div>
<div align="right">
<a href="chapter-10.md">Next: Chapter 10</a>
</div>