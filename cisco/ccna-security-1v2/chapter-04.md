### Spis treści
- [Chapter 4: Implementing Firewall Technologies](#chapter-4-implementing-firewall-technologies)
  - [4.1 Access Control Lists](#41-access-control-lists)
    - [4.1.1 Antispoofing](#411-antispoofing)
    - [4.1.2 Permitting necessary traffic through a Firewall](#412-permitting-necessary-traffic-through-a-firewall)
    - [4.1.3 Mitigating ICMP abuse](#413-mitigating-icmp-abuse)
    - [4.1.4 Mitigating SNMP exploits](#414-mitigating-snmp-exploits)
    - [4.1.5 IPv6 ACLs](#415-ipv6-acls)
  - [4.2 Firewall technologies](#42-firewall-technologies)
  - [4.3 Zone-based policy firewalls](#43-zone-based-policy-firewalls)

# Chapter 4: Implementing Firewall Technologies

## 4.1 Access Control Lists

<a href="../routing-switching-2v5/chapter-09.md">Previous chapter about ACL in Cisco CCNA Routing & Switching</a>

>ACLs can be defined for Layers 2, 3, 4, and 7 of OSI model.

>IPv6 ACLs must use a name.

>Standard ACLs match packets by examining the source IP address field in the IP header of that packet. These ACLs are used to filter packets based solely on Layer 3 source information.

>Extended ACLs match packets based on Layer 3 and Layer 4 source and destination information. Layer 4 can include TCP and UDP port information. Extended ACLs give greater flexibility and control over network access than standard ACLs.

>Instead of using a number, a name can be used to configure an ACL. Named ACLs must be specified as either standard or extended.

Standard ACL

```
R1(config)# ip access-list standard {1-99 | 1300-1999}
R1(config-std-nacl)# {permit | dney | remark} SOURCE_ADDRESS
```

Extended ACL

```
R1(config)# ip access-list extended {101-199 | 2000-2699}
R1(config-ext-nacl)# {permit | dney | remark} protocol SOURCE_ADDRESS DEST_ADDRESS
```

Named ACL

```
R1(config)# ip access-list {standard | extended} ACL_NAME
```

Apply ACL on an interface

```
R1(config-if)# ip access group ACL {in|out}
```

Apply ACL on the VTY lines

```
R1(config-line)# access-class ACL in
```

>Applying ACLs to interfaces and lines is just one of the many possible uses. ACLs are also an integral part of other security configurations, such as zone-based firewalls, intrusion prevention systems, and virtual private networks.

>Logging has been enabled with the `log` parameter. Log messages are generated on the first packet match and then at five minute intervals after that first packet match. 

>Enabling the log parameter on a Cisco router or switch `seriously affects the performance` of that device. The log parameter should only be used when the network is under attack, and an administrator is trying to determine who the attacker is.

ACL configuration guidelines:
1. Create an ACL globally and then apply it
2. Ensure the last statement is an implicit `deny any` or `deny any any`
3. Remember that statement order is important because `ACLs are processed top-down`. As soon as a statement is matched the ACL is exited
4. Ensure `the most specific statements are at the top` of the list
5. Remember that only one ACL is allowed per:
   - interface,
   - protocol - ip or ipv6,
   - per direction
6. Remember that new statement for an existing ACL are addres to the bottom of the ACL by default
7. Remember that `router generated packerts are not filtered` by outbound ACLs
8. Place `standard ACLs as close to the destination` as possible
9. Place `extended ACLs as close to the source` as possible

Editing ACLs

```
R1(config)# ip access-list extended 101
R1(config-ext-nacl)# no 10
R1(config-ext-nacl)# 5 deny tcp any any eq telnet
R1(config-ext-nacl)# 10 deny udp any any
```

>The `sequence number does not dictate processing order` in a standard ACL. However, it can be used as an identifier for deleting a specific entry.

### 4.1.1 Antispoofing

> IP address spoofing overrides the normal packet creation process by inserting a custom IP header with a different source IP address. Attackers can hide their identity by spoofing the source IP address.

>There are many well-known classes of IP addresses that should never be seen as source IP addresses for traffic entering an organization’s network.

Interface attached to the Internet and should never accept inbound packets from the following addresses:
- all zeros addresses,
- broadcast addresses,
- local host addresses - 127.0.0.0/8,
- reserved private addresses - RFC 1918,
- ip multicast addresses range - 224.0.0.0/4

In inbound LAN interface should only allow with source address from that network(ex. 192.168.1.0/24).

Inbound on WAN interface
```
R1(config)# access-list 150 deny ip host 0.0.0.0 any
R1(config)# access-list 150 deny ip 10.0.0.0 0.255.255.255 any
R1(config)# access-list 150 deny ip 127.0.0.0 0.255.255.255 any
R1(config)# access-list 150 deny ip 172.16.0.0 0.15.255.255 any
R1(config)# access-list 150 deny ip 192.168.0.0 0.0.255.255 any
R1(config)# access-list 150 deny ip 224.0.0.0 15.255.255.255 any
R1(config)# access-list 150 deny ip host 255.255.255.255 any
R1(config)# access-list 150 permit ip any any
```

Inbound on LAN interface
```
R1(config)# access-list 105 permit ip 192.168.1.0 0.0.0.255 any
```

### 4.1.2 Permitting necessary traffic through a Firewall

An effective strategy for mitigating attacks is to explicitly permit only certain types of traffic through a firewall:
- DNS,
- SMTP,
- FTP,
- SSH,
- syslog

In scenario presented below we have:
- 192.168.20.2 - host which is DNS, SMTP & FTP server
- 10.0.1.1 - Router WAN interface which permits ssh, syslog & snmptrap

Inbound on WAN interface
```
R1(config)# access-list 180 permit udp any host 192.168.20.2 eq domain
R1(config)# access-list 180 permit udp any host 192.168.20.2 eq smtp
R1(config)# access-list 180 permit udp any host 192.168.20.2 eq ftp
R1(config)# access-list 180 permit tcp host 200.5.5.5 host 10.0.1.1 eq 22
R1(config)# access-list 180 permit tcp host 200.5.5.5 host 10.0.1.1 eq syslog
R1(config)# access-list 180 permit tcp host 200.5.5.5 host 10.0.1.1 eq snmptrap
```

### 4.1.3 Mitigating ICMP abuse

ICMP messages recomended to be allowed into internal network:
- echo reply,
- source quench - request that the sender decrease the traffic rate of messages
- unreachable

ICMP messages recomended to be allowed exit internal network:
- echo,
- parameter problem - informs the host of packet header problems,
- packet too big - enables packet MTU discovery,
- source quench - throttles down traffic when necessary

Inbound on WAN interface
```
R1(config)# access-list 112 permit icmp any any echo-reply
R1(config)# access-list 112 permit icmp any any source-quench
R1(config)# access-list 112 permit icmp any any unreachable
R1(config)# access-list 112 deny icmp any any
R1(config)# access-list 112 permit ip any any
```

Inbound on LAN interface
```
R1(config)# access-list 114 permit icmp 192.168.1.0 0.0.0.255 any echo
R1(config)# access-list 114 permit icmp 192.168.1.0 0.0.0.255 any parameter-problem
R1(config)# access-list 114 permit icmp 192.168.1.0 0.0.0.255 any packet-too-big
R1(config)# access-list 114 permit icmp 192.168.1.0 0.0.0.255 any source-quench
R1(config)# access-list 114 deny icmp any any
R1(config)# access-list 114 permit ip any any
```

### 4.1.4 Mitigating SNMP exploits

The most effective means of exploitation prevention is to disable the SNMP server on IOS devices for which it is not required.

```
R1(config)# no snmp-server
```

### 4.1.5 IPv6 ACLs

IPv4 was designed without several modern-day network requirements:
- security - IPsec,
- device roaming - mobile IP,
- quality of service - resource reservation protocol, RSVP
- address scalability - DHCP, NAT, CIDR, VLSM 

> IPv4 will not disappear overnight. IPv4 will coexist with IPv6 and then gradually be replaced by IPv6. This creates potential security holes. An example of a security concern is attackers leveraging IPv4 to exploit IPv6 in dual stack.

>Attackers can accomplish stealth attacks that result in trust exploitation by using dual stacked hosts, rogue Neighbor Discovery Protocol (NDP) messages, and tunneling techniques.

>Teredo tunneling, for example, is an IPv6 transition technology that provides automatic IPv6 address assignment when IPv4/IPv6 hosts are located behind IPv4 network address translation (NAT) devices. It accomplishes this by embedding the IPv6 packets inside IPv4 UDP packets.

>This mitigation strategy should include filtering at the edge using various techniques, such as IPv6 ACLs.

>All IPv6 ACLs must be configured with a name.

```
R1(config)# ipv6 access-list ACL_NAME
R1(config-ipv6-acl)#
R1(config-ipv6-acl)# permit any any nd-na
R1(config-ipv6-acl)# permit any any nd-ns

R1(config-if)#ipv6 traffic-filter
```

>However, it is a good practice to document the implicit statements by explicitly configuring them.

## 4.2 Firewall technologies

Firewall properties:
- firewalls are resistant to attacks,
- firewalls are the only transit point between networks because all traffic flows through the firewall,
- firewall enforce the access control policy

>In `1988`, Digital Equipment Corporation (`DEC`) created the first network firewall in the form of a packet filter firewall. These early firewalls inspected packets to see if they matched sets of rules, and then chose to forward or drop the packets accordingly.

>This type of packet filtering, known as `stateless` filtering, occurs regardless of whether a packet is part of an existing flow of data.

>In 1989, AT&T Bell Laboratories developed the first stateful firewall. Stateful firewalls evaluate the state of connections in data flows to filter packets. The stateful firewall is able to determine if a packet belongs to an existing flow of data.

>The original firewalls were not standalone devices. They were routers or servers with software features added to provide firewall functionality.

A firewall is a system or group of systems that enforces an access control policy between networks, it can include options, such as a packet filtering router, a switch with two VLANs, and multiple hosts with firewall software.

Benefits of using firewall:
- prevent the exposure of sensitive hosts, resources, and applications to untrusted users,
- sanitize protocol flow, which prevents the exploitation of protocol flaws,
- block malicious data from servers and clients,
- reduce security management complexity by off-loading most of the network access control to a few firewalls in the network.

Firewall limitation:
- a misconfigured firewall can have serious consequences for the network, such as becoming a single point of failure,
- the data from many applications cannot be passed over firewalls securely,
- users might proactively search for ways around the firewall to receive blocked material, which exposes the network to potential attack,
- network performance can slow down,
- unauthorized traffic can be tunneled or hidden as legitimate traffic through the firewall(ex. Teredo).

Firewall types:
- `packet filtering firewall` - stateless, typicaly router with the capability to filter some packet content such as Layer 3 and sometimes Layer 4. 
  
  Advantages:
  - implement simple permit or deny rule set,
  - have low impact on network performance,
  - easy to implement and are supported by most routers,
  - provide initial degree of security at the network layer,
  - performs almost all the tasks of a high-end firewall at much lower cost

  Disadvantages:
  - susceptible to IP spoofing.Hackers can send arbitrary packets that meet ACL criteria and pass through the filter,
  - have problems with filtering fragmented packets,
  - sometimes filters use complex ACLs, which could be difficult to implement and maintain
  - cannot dynamically filter certain services - sessions that use dynamic port negotiations are difficult to filter without opening access to a whole range of ports,
  - stateless - each packet is examineg individually rather than in the context of the state of a connection

- `statefull` - the most common firewall technologies in use. They monitors the state of connections(`state tables`), whether the connection is in an initiation, data transfer or termination state. The firewall looks at the TCP header for: 
  - SYN - synchronize, 
  - RST - reset, 
  - ACK - acknowledgment, 
  - FIN - finish,
  - other control codes to determine the state of the connection

  Benefits:
  - often used as a primary means of defense by filtering unwanted, unnecessary or undesirable traffic,
  - strengthen packet filtering by providing more stringent control over security,
  - improve performance over packet filters or proxy servers,
  - defend against spoofing and DoS attacks by determining whether packets belong to an existing connection or are from an unauthorized source,
  - provide more log information than a packet filtering firewall

  Limitations:
  - cannot prevent Application Layer attacks because they do not examine the actual contents of the HTTP connection,
  - stateless protocols such as UDP and ICMP do not generate connection information for a state table, and, therefore, do not garner as much support for filtering,
  - it is difficult to track connections that use dynamic port negotiation. Some applications open multiple connections,
  - stateful firewalls do not support user authentication

- `application gateway firewall` - proxy firewall, filters information at L3,4,5 & 7 of the OSI model. Most of the firewall control & filtering is done in software

Other types of firewall:
- `host-based firewall` - host with firewall software running on it
- `transparent firewall` - filters IP traffic between a pair of bridging interfaces
- `hybrid firewall` - a combination of the various firewall types - an application inspection firewall combines a stateful firewall with an application gateway firewall.

`NGFW` - Next Generation Firewalls - goes beyond the stateful firewall

Properties of NGFW:
- granular identification, visibility, and control of behaviors within applications,
- restricting web and web application use based on the reputation of the site,
- proactive protection against Internet threats,
- enforcement of policies based on the user, device, role, application type, and threat profile,
- performance of NAT, VPN, and stateful protocol inspection (SPI),
- use of an integrated intrusion prevention system (IPS)

Cisco ASA Next-Generation Firewall - Cisco ASA with FirePOWER services with advanced malware protection. It is designed to provide defense across the entire attack continuum, which includes `before, during, and after attacks`.

Classic firewall - context-based access control (`CBAC`), statefull firewall feature which provides:
- traffic filtering,
- traffic inspection,
- intrusion detection,
- generation of audits and alerts,
- examine supported connections for embedded NAT and Port Address Translation (PAT)

>Classic Firewall creates temporary openings in the ACL to allow returning traffic. These entries are created as inspected traffic leaves the network and are removed when the connection terminates or the idle timeout period for the connection is reached.

>Classic Firewall can also be configured to inspect traffic in two directions: in and out. This is useful when protecting two parts of a network, in which both sides initiate certain connections and allow the returning traffic to reach its source.

Firewalls in network design:
- `inside`(private network, trusted) `and outside`(public network, untrusted) `networks`,
- `DMZ` - Demilitarized Zone, has typically 3 interfaces:
  - inside
  - outside
  - DMZ

  >Traffic originating from the DMZ network and traveling to the private network is usually blocked.
  
  >Traffic originating from the DMZ network and traveling to the public network is selectively permitted based on service requirements.
  
  >Traffic originating from the public network and traveling toward the DMZ is selectively permitted and inspected. This type of traffic is typically email, DNS, HTTP, or HTTPS traffic. Return traffic from the DMZ to the public network is dynamically permitted.

- `ZPF` - Zone-based policy firewalls. Zone is a group of one or more interfaces that have similar functions or features.

  >By default, the traffic between interfaces in the same zone is not subject to any policy and passes freely. However, all zone-to-zone traffic is blocked. In order to permit traffic between zones, a policy allowing or inspecting traffic must be configured.

  >The self zone is the router itself and includes all the router interface IP addresses. Policy configurations that include the self zone would apply to traffic destined to and sourced from the router. By default, there is no policy for this type of traffic

- `Layered defense` - uses different types of firewalls that are combined in layers to add depth to the security of an organization. It can consist of:
  1. Communication security,
  2. Endpoint security,
  3. Perimeter security,
  4. Network Core security

Firewall best practises:
1. Position firewalls at security boundaries.
2. Firewalls are a critical part of network security, but it is unwise to rely exclusively on a firewall for security.
3. Deny all traffic by default. Permit only services that are nedded.
4. Ensure that physical access to the firewall is controlled.
5. Regulary monitor firewall logs.
6. Practise change management for firewall configuration changes.
7. Remember that firewalls primarily protect from technical attacks originating from the outside.

## 4.3 Zone-based policy firewalls

Configuration models for Cisco IOS Firewall:
- `Classic Firewall` - The traditional configuration model in which firewall policy is applied on interfaces.
- `ZPF` - The new configuration mode in which interfaces are assigned to security zones, and firewall policy is applied to traffic moving between the zones

Benefits of a ZPF:
- it is not dependent on ACLs,
- the router security posture is to block unless explicity allowed,
- policies are easy to read, troubleshoot and document

`C3PL` - Cisco Common Classification Policy Language, structured method to create traffic policies based on events, conditions, and actions. Provides scalability by having one policy affects any given traffic, instead of needing multiple ACLs and inspection actions.

>When deciding whether to implement Classic Firewall or a ZPF, it is important to note that `both configuration models can be enabled concurrently on a router`. However, the models cannot be combined on a single interface.

ZPF design:
1. Determine the zones.
2. Establish policies between zones.
3. Design the physical infrastructure.
4. Identify subsets within zones and merge traffic requirements.

ZPF actions:
- `inspect` - performs stateful packets inspection,
- `drop` - similar to deny statement in ACL. A log option is avaiable to log the rejected packets
- `pass` - similar to permit in ACL. The pass action does not track the state of connections or sessions within the traffic.

Rules for transit traffic:
- if neither interface is a zone member, then the resulting action is to pass the traffic,
- if both interfaces are members of the same zone, then the resulting action is to pass the traffic,
- if one interface is a zone member, but the other is not, then the resulting action is to drop the traffic regardless of whether a zone-pair exists,
- if both interfaces belong to the same zone-pair and a policy exists, then the resulting action is inspect, allow, or drop as defined by the policy.

Self Zone traffic rules:
- if the router is the source or the destination, then all traffic is permitted. The only exception is if the source and destination are a zone-pair with a specific service-policy. In that case, the policy is applied to all traffic.

ZPF configuration steps:
1. Create the `zones`.
   - What interfaces should be included in the zones?
   - What will be the name for each zone?
   - What traffic is necessary between the zones and in which direction?
2. Identify traffic with a `class-map`. A class is a way of identifying a set of packets based on its contents using “match” conditions:
   - `match-any` - packet must meet one of the criteria  
   - `match-all` - packet must meet all of the match criteria
3. Define an action wthi `policy-map`.
4. Identify a zone pair and match it to a policy-map.
5. Assign zones to the appropriate interfaces.

```
R1(config)# zone security ZONE_NAME
R1(config-sec-zone)#

R1(config)# class type inspect {match-any | match-all} CLASS_MAP_TRAFFIC_NAME
R1(config-cmap)# match protocol http
R1(config-cmap)# match ANOTHER_CLASS_MAP
R1(config-cmap)# match access-group ACL_NUM_NAME

R1(config)# policy-map type inspect P_MAP_NAME
R1(config-pmap)# class type inspect CLASS_MAP_TRAFFIC_NAME
R1(config-pmap-c)# inspect

R1(config)# zone-pair security Z_PAIR_NAME source Z_NAME_1 destination Z_NAME_2
R1(config-sec-zone-pair)# service-policy type inspect P_MAP_NAME

R1(config)# int g0/0
R1(config-if)# zone-member security Z_NAME_1 

R1(config)# int s0/0/0
R1(config-if)# zone-member security Z_NAME_2

R1# show run | begin class-map
R1# show policy-map type inspect zone-pair sessions
R1# show class-map type inspect
R1# show zone security
```

`class class-default` will drop all other traffic that is not a member of defined class-map.

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>