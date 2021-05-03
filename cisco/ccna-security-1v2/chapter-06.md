### Spis treści
- [Chapter 6: Securing the Local Area Network](#chapter-6-securing-the-local-area-network)
  - [6.1 Introducing Endpoint Security](#61-introducing-endpoint-security)
  - [6.2 Controlling Network Access](#62-controlling-network-access)
  - [6.3 Layer 2 Security Considerations](#63-layer-2-security-considerations)
    - [6.3.1 CAM table attacks](#631-cam-table-attacks)
    - [6.3.2 VLAN attacks](#632-vlan-attacks)
    - [6.3.3 DHCP attacks](#633-dhcp-attacks)
    - [6.3.4 ARP attacks](#634-arp-attacks)
    - [6.3.5 Address spoofing attacks](#635-address-spoofing-attacks)
    - [6.3.6 STP attacks](#636-stp-attacks)

# Chapter 6: Securing the Local Area Network

## 6.1 Introducing Endpoint Security

>A secure network is only as strong as its weakest link. For this reason, in addition to securing the network edge, it is also important to secure the end devices that reside on the network.

>Many attacks can, and do, originate from inside the network.

LAN elements to secure:
- `endpoints` - hosts commonly consist of laptops, desktops, servers and IP phones which are susceptible to malware-related attacks
- `network infrastructure`
  - switches,
  - wireless devices,
  - IP telephony devices

Network perimeter security:
- hardened ISR,
- ASA firewall,
- IPS appliance
- AAA ACS server

Layer 2 attacks:
- MAC address table overflow,
- spoofing attacks,
- DHCP related attacks,
- LAN storm attacks,
- STP manipulation attacks,
- VLAN attacks

Endpoint protection(traditional):
- `anitivirus/antimalware software` - used to detect and mitigate viruses and malware
- `host-based firewall` - restrict incoming and outgoing connections to those initiated by that host only
- `host-based IPS` - monitor and report on the system configuration, application activity, log analysis, event correlation, integrity checking, policy enforcement, rootkit detection and alerting

>Larger organizations now require protection before, during, and after an attack.

Endpoint protection(in the borderless network):
- `anitivirus/antimalware software`
- `SPAM filtering` - filter spam emails before they reach the endpoint
- `URL filtering` - block websites before they reach the endpoint
- `Blacklisting` - identify websites with bad reputation
- `DLP` - Data Loss Prevention, type of software used to prevent sensitive information from being lost or stolen

Modern endpoint security solutions:
- `AMP` - Antimalware Protection, endpoint protection from viruses and malware
  - File reputation
  - File sandboxing - analyze to understand true file behavior
  - File retrospection - continue to analyze files for changing threat
  - access the collective security intelligence of the Talos:
    - `SIO` - Cisco Security Intelligence Operation
    - `VRT` - Sourcefire Vulnerability Research Team
- `ESA` - Email Security Appliance, SPAM filtering, IronPort appliance as virtual, cloud and hybrid solutions. It provide:
  - fast, comprehensive email protection, spam blocking, advanced malware protection
  - flexible cloud, virtual and physical deployment options
  - outbound message control through in device DLP and email encryption
  - updated by real-time feeds from Cisco Talos every 3-5 minutes
- `WSA` - Web Security Appliance, blacklisting, IronPort appliance
  - Talos Secuirty Intelligence
  - Cisco Web Usage Controls - URL filtering with dynamic content analysis to mitigate compliance, liability and productivity risks
  - Advanced Malware Protection(AMP)
  - Data Loss Prevention(DLP)
- `NAC` - Network Admission Control, permits only authorized and compliant systems to connect to the network
- `CWS` - Cloud Web Security
  - easy to integrate
  - granular web use policies can be set and enforces across the entire environment
  - real-time threat intelligence
  - centralized management and reporting

Hardware and software encryption of local data - encyption protects the confidential data from unathorized access(lost or stole device)

BitLocker, TrueCrypt, VeraCrypt for Windows.

MAC OSX natively provide encryption options.

Strong encryption algorithm = AES 256-bit

AMP helps:
- before
  - discover
  - enforce
  - harden
- during
  - detect
  - block
  - defend
- after
  - scope
  - contain
  - remediate

AMP is avaiable in a variety of formats:
- for Endpoints,
- for Networks - integrated into dedicated Cisco ASA & FirePOWER
- for Content Security - integrated feature in Cisco Cloud Web Security for Cisco ESA & WSA

## 6.2 Controlling Network Access

Cisco NAC - Network Admission Control is to allow only authorized and compliant systems, whether managed or unmanaged, to access the network.

>Cisco NAC is also designed to enforce network security policy. NAC helps maintain network stability by providing authentication, authorization, and posture assessment (evaluating an incoming device against the policies of the network). NAC also quarantines noncompliant systems and manages the remediation of noncompliant systems.

Cisco NAC use case:
- recongnize users, their devices and their roles in the network,
- evaluate whether machines are compliant with security policies,
- enforce secuirty policies by blocking, isolating and repairing noncompliant machines,
- provice easy and secure guest access,
- simplify non-authenticating device access
- audit and report who is on the network

Categories of Cisco NAC products:
- `NAC framework` - use existing Cisco network infrastructure and third-party software to enforce security policy compliance on all endpoints
- `Cisco NAC appliance` - incorporates NAC functions into an appliance and provides a solution to control network access. Ideal for organizations that need simplified and integrated tracking of OS.

>NACs are evolving away from basic security protection to more sophisticated `endpoint visibility, access, and security (EVAS)` controls. Unlike older NAC technologies, EVAS use more granular information to enforce access policies, such as data about user role, location, business process considerations, and risk management.

NAC components:
- `NAM` - NAC Manager, policy & management center
- `NAS` - NAC Server, assesses and enforces security policy compliance in a applicance-based NAC deployment environment 
- `NAA` - NAC Agent, optional lightweight agent running on an enpoint device. It performs deep inspection of the device's security profile by analyzing registry setting , services and files
- `NAC Guest Server` - provides guest policy enforcement
- `NAC Profiler` - enables dynamic discovery, identification and monitoring of all netowrk-attached endpoints. It consist of NAC Profiler Collector and NAC Profiler Server
  - simplify deployment of Cisco NAC by automating device identification and authentication
  - facilitate deployment and management of the Cisco ACS 802.1X based infrastructure
  - gather endpoint device profiling information and maintain a real-time, contextual inventory of networked devices
  - monitor and manage devices behaviour anomalies(port swapping, MAC address spoofing and profile changes)
  - secure all company-owned endpints, including non-authenticating devices such as printers and IP phones

>These are two additional TrustSec Policy enforcement tools:
>- Cisco NAC guest server - Manages guest network access, including provisioning, notification, management, and reporting of all guest user accounts and network activities.
>- Cisco NAC profiler - Helps to deploy policy-based access control by providing discovery, profiling, policy-based placement, and post-connection monitoring of all endpoint devices.

Sponsor = administrator

## 6.3 Layer 2 Security Considerations

>If Layer 2 is compromised, then all layers above it are also affected. Then all of the security implemented on the layers above would be useless. Layer 2 is considered to be that weakest link.

>The first step in mitigating attacks on the Layer 2 infrastructure is to understand the underlying operation of Layer 2 and the threats posed by the Layer 2 infrastructure.

Layer 2 attacks:
- `CAM table attacks` 
  - MAC address flooding = CAM table overflow
- `VLAN attacks`
  - VLAN hopping,
  - VLAN double-tagging
- `DHCP attacks`
  - DHCP starvation,
  - DHCP spoofing
- `ARP attacks`
  - ARP spoofing,
  - ARP poisoning
- `Address spoofing attacks`
  - MAC address spoofing,
  - IP address spoofing
- `STP attacks` - spanning tree manipulation attack

Following strategies are recomended:
- always use secure variants of these protocols such as SSH, SCP and SSL,
- consider using out-of-band(OOB) management,
- use a dedicated management VLAN where nothing but management traffic resides
- use ACLs to filter unwanted access

Mitigate Layer 2 attacks:
- `port security`, prevents:
  - CAM table overflow,
  - DHCP starvation
- `DHCP snooping`, prevents:
  - DHCP spoofing,
  - DHCP starvation
- `DAI` - Dynamic ARP Inspection, prevents:
  - ARP spoofing,
  - ARP poisoning
- `IPSG` - IP Source Guard, prevents:
  - IP address spoofing
  - MAC address spoofing

### 6.3.1 CAM table attacks

>To make forwarding decisions, a Layer 2 LAN switch builds a table of MAC addresses that is stored in its Content Addressable Memory (CAM), simply known as MAC address table.

>The CAM table binds and stores MAC addresses and associated VLAN parameters that are connected to the physical switch ports.

>If the destination MAC address is in the CAM table, the switch forwards the frame accordingly. However, if the destination MAC address is not in the CAM table, the switch will flood the frame out of all ports except for the frame’s port of ingress. This is called `an unknown unicast flood`.

```
S1# show mac-address-table
```

>All CAM tables have a fixed size and consequently, a switch can run out of resources in which to store MAC addresses. CAM table overflow `attacks` (also called MAC address overflow attacks) `take advantage of this limitation by bombarding the switch with fake source MAC addresses until the switch MAC address table is full`.

>The switch, in essence, acts as a hub. As a result, the attacker can capture all of the frames sent from one host to another.

Tools:
- macof -i eth0

Mitigation:
- port security - limit number of learned MAC address per port

```
S1(config)# interface f0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# switchport maximum {1}
S1(config-if)# switchport violation 
S1(config-if)# switchport sticky
S1(config-if)# switchport aging time {10}
S1(config-if)# switchport aging type {absolute|inactivity}

S1# show port-security interface f0/1
```

Port security violations:
- `protect` - least secure, packets with unknown source address are dropped without any notification
- `restrict` - dropped with notification
- `shutdown` - port in error-disabled state

Port security aging can be used to set the aging time for static and dynamic secure addresses on a port. Two types of aging are supported per port:
- `absolute` - the secure addresses on the port are deleted after the specified aging time
- `inactivity` - the secure addresses on the port are deleted `only if they are inactive for the specified aging time`.

IP phone & 

>When the port is connected to a Cisco IP phone, the IP phone requires up to two MAC addresses. The IP phone address is learned on the voice VLAN and might also be learned on the access VLAN. Connecting a PC to the IP phone requires an additional MAC address.

The MAC address notification feature sends SNMP traps to the network management station (NMS) whenever a new MAC address is added to, or an old address is deleted from the forwarding tables.

Use the `mac address-table notification` global configuration command to enable the MAC address notification feature on a switch.

### 6.3.2 VLAN attacks

VLAN hopping attack enables traffic from one VLAN to be seen by another VLAN without the aid of a router. In a basic VLAN hopping attack, the attacker `takes advantage of the automatic trunking port feature` enabled by default on most switch ports

The network attacker configures a host to spoof a switch to use `802.1Q` signaling and Cisco-proprietary Dynamic Trunking Protocol (`DTP`) signaling to trunk with the connecting switch.

If successful and the switch establishes a trunk link with the host, then the attacker `can access all the VLANS` on the switch and hop (i.e., send and receive) traffic on all the VLANs.

A VLAN hopping attack can be launched in one of two ways:
- spoofing DTP messages from the attacking host to cause the switch to enter trunking mode,
- introducing a rogue switch and enabling trunking

Another type of VLAN hopping attack is a `double-tagging` (or double-encapsulated) `attack`. This attack takes advantage of the way hardware on most switches operates.

Mitigation:
- disable `DTP`(Dynamic Trunking Protocol, auto trunking) and manually assign switch role
- set `native vlan` to be sth other than vlan 1
- diasble unused ports
    ```
    S1(config-if)# switchport mode access

    S1(config-if)# switchport mode trunk
    S1(config-if)# switchport nonegotiate
    S1(config-if)# switchport trunk native vlan 99
    
    S1(config-if)# shutdown
    ```

- use `PVLAN`(Private VLAN) - ensures that there is no exchange of unicast, broadcast or multicast traffic between PVLAN edge ports(protected ports). All data traffic passing between protected ports must be forwarded through a Layer 3 device.(Hotel, other companies)
    ```
    S1(config)# switchport protected

    S1# show interfaces f0/1
    ```

Types of PVLAN ports:
- promiscuous - can talk to everyone
- isolated - can only talk to promiscuous ports
- community - can talk to other ports in the same community and with promiscous ports

### 6.3.3 DHCP attacks

DHCP spoofing attack occurs when a rogue DHCP server is connected to the network and provides false IP configuration parameters to legitimate clients. A rogue server can provide a variety of misleading information:
- wrong default gateway
- wrong dns server
- wrong IP address

DHCP starvation - lease all avaiable IP addresses in scope

Mitigation:
- port security,
- DHCP snooping
  - trusted - DHCP servers
  - untrusted port
- DAI
- IPSG

```
S1(config)# ip dhcp snooping

S1(config-if)# ip dhcp snooping trust

S1(config-if)# ip dhcp snooping limit rate 5
S1(config)# ip dhcp snooping vlan 5,10,15
S1# show ip dhcp snooping
S1# show ip dhcp binding
S1# show ip dhcp snooping database
```

>To provide more information about the actual client that generated the DHCP request, enable DHCP option 82 with the ip dhcp snooping information option global configuration command. This adds the switch port identifier into the DHCP request.

>DHCP snooping is also required by Dynamic ARP Inspection (DAI).

### 6.3.4 ARP attacks

Host broadcasts an ARP Request to other hosts to determine the MAC address of a host with a particular IP address.

`ARP spoofing` - an attacker can send a gratuitous ARP message containing a spoofed MAC address to a switch, and the switch. In a typical attack, a malicious user can send unsolicited ARP Replies to other hosts on the subnet with the MAC Address of the attacker and the IP address of the default gateway.

`ARP poisoning` attack is when an attacker uses ARP spoofing to redirect traffic. ARP poisoning leads to various man-in-the-middle attacks, posing a serious security threat to the network.

`DAI` - Dynamic Address Inspection, intercepts all ARP Requests and all replies on the untrusted ports. Each intercepted packet is verified for valid IP-to-MAC binding.

>It is generally advisable to configure all access switch ports as untrusted and to configure all uplink ports that are connected to other switches as trusted.

>DAI requires the DHCP snooping table to operate.

DAI configuration check:
- destination MAC
- source MAC
- IP address - invalid and unexpected

```
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan 10
S1(config)# ip arp inspection vlan 10
S1(config)# ip arp inspection validate src-mac dst-mac
S1(config)# int f0/1
S1(config-if)# ip dhcp snooping trust
S1(config-if)# ip arp inspection trust
```

>Entering multiple `ip arp inspection validate` commands overwrite the previous command. To include more than one validation method, enter them on the same command line

### 6.3.5 Address spoofing attacks

`IPSG`(IP Source Guard) operates just like DAI, but it looks at every packet, not just the ARP packets. Like DAI, IPSG also requires that DHCP snooping be enabled.

IPSG Features:
- is deployed on untrusted Layer 2 access and trunk ports,
- dynamically maintains per-port VLAN ACLs (PVACL) based on IP-to-MAC-to-switch-port bindings

>Initially, all IP traffic on the port is blocked, except for DHCP packets that are captured by the DHCP snooping process. A PVACL is installed on the port when a client receives a valid IP address from the DHCP server or when a static IP source binding is configured by the user.

Untrusted port, possible levels of IP traffic security filtering:
- source IP address filter,
- source IP and MAC address filter

```
S1(config)# int f0/1
S1(config-if)# ip verify source

S1(config)# show ip verify source
```

### 6.3.6 STP attacks

`STP` - Spanning Tree Protocol ensures that redundant physical links are loop-free. It ensures that there is only one logical path between all destinations on the network by intentionally blocking redundant paths that could cause a loop.

Blocked ports still exchange `BPDU`(BID & root ID) frames which are used by STP to prevent loops by dynamically blocking redundant paths or unblocking them when there is a change in the network. BPDU are send out every two seconds.

The BID contains:
- bridge priority value(default 32768), 
- the MAC address of the sending switch, 
- optional extended system ID - identifies the VLAN
 
The lowest BID value is determined by the combination of these three fields.

`RSTP` - Rapid STP

`MSTP` - Multiple Spanning Tree Protocol

>The latest IEEE documentation on spanning tree, IEEE-802-1D-2004, says "STP has now been superseded by the RSTP".

`The switch with the lowest BID`(Bridge ID) automatically becomes the `root bridge` for the spanning tree algorithm calculations.

>The path costs are calculated using port cost values associated with port speeds for each switch port along a given path. The sum of the port cost values determines the overall path cost to the root bridge. If there is more than one path to `choose from`, spanning tree algorithm chooses the path with `the lowest path cost`.

STP port roles:
- `root` - switch port closest to the root bridge(on ther switches)
- `designated` - non-root ports that are still permitted to forward traffic on the network
- `alternate` - backup ports are in blocking state to prevent loops

The root bridge serves as a reference point for all spanning tree calculations to determine which redundant paths to block.

>Priority is the initial deciding factor when electing a root bridge. If the priorities of all the switches are the same, the device with the lowest MAC address becomes the root bridge.

>To conduct an `STP manipulation attack`, the attacking host broadcasts STP configuration and topology change BPDUs to force spanning-tree recalculations

Mitigation:
- `PortFast` - immediately brings an interface configured as an access or trunk port to the forwarding state from a blocking state, bypassing the listening and learning states. Apply to `all end-user ports`

  ```
  S1(config)# spanning-tree portfast default

  S1(config-if)# spanning-tree portfast

  S1# show running-config interface f0/1
  ```
- `BPDU Guard` - immediately error disables a port that receives a BPDU. Apply to `all end-user ports`

  ```
  S1(config)# spanning-tree portfast bpduguard default

  S1(config-if)# spanning-tree bpduguard enable

  S1# show spanning-tree summary {totals}
  ```

  >Always enable BPDU Guard on all PortFast-enabled ports.
- `Root Guard` - limits the switch ports out of which the root bridge may be negotiated. Apply to `all ports which should not become root ports`

  ```
  S1(config-if)# spanning-tree guard root

  S1# show spanning-tree inconsistent ports
  ```
- `Loop Guard` - prevents alternate or root ports from becoming designated ports because of a failure that leads to a unidirectional link. Apply to `all ports that are or can become non-designated`

  ```
  S1(config)# spanning-tree loopguard default
  
  S1(config-if)# spanning-tree guard loop
  ```

  >If for some reason one direction traffic flow fails, this creates a unidirectional link which can result in a Layer 2 loop.

Tools to carry out L2 attacks:
- `macof` - CAM flood attacks
- `Gobbler` - DHCP Starvation
- `dsniff`, `Cain & Abel`, `ettercap`, `Yersina` - ARP spoofing & poisoning
- `Yersina` - STP, CDP, DTP, DHCP, HSRP 

---

<div>
<a href="chapter-05.md">Prev: Chapter 5</a>
</div>
<div align="right">
<a href="chapter-07.md">Next: Chapter 7</a>
</div>