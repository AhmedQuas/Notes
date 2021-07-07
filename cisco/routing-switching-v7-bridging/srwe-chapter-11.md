### Spis treści
- [Chapter 2: Switch Security Configuration](#chapter-2-switch-security-configuration)
  - [2.1 Port Security](#21-port-security)
  - [2.2 Mitgate VLAN attacks](#22-mitgate-vlan-attacks)
  - [2.3 Mitigate DHCP attacks](#23-mitigate-dhcp-attacks)
  - [2.4 Mitigate ARP attacks](#24-mitigate-arp-attacks)
  - [2.5 Mitigate STP attacks](#25-mitigate-stp-attacks)

# Chapter 2: Switch Security Configuration

SRWE - Switching, Routing and Wireless Essentials Bridging Concept

## 2.1 Port Security

>Layer 2 devices are considered to be the weakest link in a company’s security infrastructure.

Recomendation:

`1.Disable all unused ports`

```
S1(config)# int range f0/1-10
S1(config-if-range)# shutdown
```

`2.Enable port security` - limits the number of valid MAC addresses allowed on a port. It prevent MAC address table overflow attack. Port security port states:
  - secure-down - no devices attached and no violation has occured
  - secure-up - device connected to the port and automatically added the device MAC as secure MAC
  - secure-shutdown - port in error-disabled mode

Types of violation:
- `protect` - least secure, drops packets with unknown MAC address
- `restrict` - drop packets with unknown source address until you remove a sufficient number of secure MAC addresses to drop below the maximum value or increase the maximum value, increase violation counter, generate syslog message
- `shutdown` - default, turn port into error-disabled state(shudown && no shutdown), send syslog message, increment violation counter

```
S1(config)# int f0/10
S1(config-if)# switchport mode access
S1(config-if)# switchport port-sercurity
S1(config-if)# switchport port-security MAX value
S1(config-if)# switchport port-security MAC_ADDR IP_ADDR
S1(config-if)# switchport port-security aging { static | time TIME | type { absolute | inactivity } }
S1(config-if)# switchport port-security violation { protect | restrict | shutdown }

S1# show port-security
S1# show port-security address
S1# show port-security interface f0/10
S1# show run interface f0/10
```

Methods of learning MAC addresses:
- static
- dynamic - if the switch is rebooted, the port will have to re-learn
- sticky - MAC address saved in NVRAM


## 2.2 Mitgate VLAN attacks

VLAN Hopping can be alunched on one of three ways:
- spoofing DTP from the attacking host to cause the switch to enter trunking mode - the attacker can send traffic tagged with the target VLAN, and the switch then delivers the packets to the destination,
- introduce a rogue switch and enable trunking - the attacker can then access all the VLANs on the victim switch from the rogue switch,
- double-tagging(double-encapsulated) - this attack takes advantage of the way hardware on most switches operate

Recomendation:
- disable DTP(auto trunking) negotiations on non-trunking ports by using the `switchport mode access `interface command,
- disable unused ports,
- manually enable the trunk link on a trunking port by using `switchport mode trunk`,
- disable DTP on trunking ports by using `switchport nonegotiate`,
- set native vlan to vlan other than 1 by using `switchport trunk native vlan VLAN_ID`

## 2.3 Mitigate DHCP attacks

DHCP Snooping - does not rely on source MAC addresses. Instead, DHCP snooping determines whether DHCP messages are from an administratively configured trusted or untrusted source. It then filters DHCP messages and rate-limits DHCP traffic from untrusted sources.

All interfaces are treated as untrusted by default.

>A DHCP table is built that includes the source MAC address of a device on an untrusted port and the IP address assigned by the DHCP server to that device. The MAC address and IP address are bound together. Therefore, this table is called the `DHCP snooping binding table`.

```
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping VLAN_1,VLAN_2

S1(config-if)# ip dhcp snooping trust

S1(config-if)# ip dhcp snooping limit rate LIMIT_PER_SEC

S1# show ip dhcp snooping
S1# show ip dhcp snooping binding
```

>DHCP snooping is also required by Dynamic ARP Inspection (DAI)

## 2.4 Mitigate ARP attacks

Dynamic ARP inspection (`DAI`) requires DHCP snooping and helps prevent ARP attacks by:
- not relaying invalid or gratuitous ARP Requests out to other ports in the same VLAN,
- intercepting all ARP Requests and Replies on untrusted ports,
- verifying each intercepted packet for a valid IP-to-MAC binding,
- dropping and logging ARP Requests coming from invalid sources to prevent ARP poisoning,
- error-disabling the interface if the configured DAI number of ARP packets is exceeded

DAI configuration:
- enable DHCP snooping globally,
- enable DHCP snooping on selected VLANs,
- enable DAI on selected VLANs,
- configure trusted interfaces for DHCP snooping and ARP inspection

```
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping vlan VLAN_ID
S1(config)# ip arp inspection vlan VLAN_ID
S1(config)# interface fa0/10
S1(config-if)# ip dhcp snooping trust
S1(config-if)# ip arp inspection trust

ip arp inspection validate { src-mac | dst-mac | ip }
```

ARP validation types:
- `destination MAC` - Checks the destination MAC address in the Ethernet header against the target MAC address in the ARP body.
- `source MAC` - Checks the source MAC address in the Ethernet header against the sender MAC address in the ARP body.
- `IP address` - Checks the ARP body for invalid and unexpected IP addresses including addresses 0.0.0.0, 255.255.255.255, and all IP multicast addresses.

## 2.5 Mitigate STP attacks

`PortFast` - immediately brings an interface configured as an access port to the forwarding state from a blocking state, bypassing the listening and learning states. Apply to all end-user ports. PortFast should only be configured on ports attached to end devices

```
S1(config)# spanning-tree portfast default

S1(config)# interface fa0/10
S1(config-if)# switchport mode access
S1(config-if)# spanning-tree portfast

show running-config | begin span
show spanning-tree summary
show running-config interface
show spanning-tree interface f0/10 detail
```

`BPDU Guard` - immediately error disables a port that receives a BPDU. Like PortFast, BPDU guard should only be configured on interfaces attached to end device

>Even though PortFast is enabled, the interface will still listen for BPDUs. Unexpected BPDUs might be accidental, or part of an unauthorized attempt to add a switch to the network.

>If any BPDUs are received on a BPDU Guard enabled port, that port is put into error-disabled state. This means the port is shut down and must be manually re-enabled or automatically recovered through the `errdisable recovery cause bpduguard` global command.

```
S1(config)# spanning-tree portfast bpduguard default

S1(config)# interface fa0/1
S1(config-if)# spanning-tree bpduguard enable

S1# show spanning-tree summary
```

---

<div>
<a href="srwe-chapter-10.md">Next: SRWE 10</a>
</div>
<div align="right">
<a href="srwe-chapter-12.md">Next: SRWE 12</a>
</div>