### Table of Contents
- [Cheetsheet: Cisco configuration commands (for CCNA)](#cheetsheet-cisco-configuration-commands-for-ccna)
  - [Basic configuration](#basic-configuration)
    - [Hostname](#hostname)
    - [Priv exec password](#priv-exec-password)
    - [No ip domain lookup](#no-ip-domain-lookup)
    - [Console & vty password](#console--vty-password)
    - [Plain text password encryption](#plain-text-password-encryption)
    - [Banner MOTD](#banner-motd)
    - [Logging synchronus](#logging-synchronus)
    - [Saving config](#saving-config)
    - [Restore defaults](#restore-defaults)
    - [Clock](#clock)
    - [Blocking after x retries & set console log out timeout & password min-length](#blocking-after-x-retries--set-console-log-out-timeout--password-min-length)
    - [Disable CDP](#disable-cdp)
    - [Show switch command](#show-switch-command)
  - [Switch basic](#switch-basic)
    - [Port-security](#port-security)
    - [SVI - Switch Vlan Interface](#svi---switch-vlan-interface)
    - [Switch default gateway](#switch-default-gateway)
    - [STP](#stp)
      - [STP 802.1d](#stp-8021d)
      - [Per Vlan STP - PVST+](#per-vlan-stp---pvst)
      - [PortFast and BPDU guard](#portfast-and-bpdu-guard)
      - [Rapid PVST+](#rapid-pvst)
      - [Clear & show STP process](#clear--show-stp-process)
    - [Etherchannel](#etherchannel)
  - [Router basic](#router-basic)
    - [Enable IPv6 routing](#enable-ipv6-routing)
    - [FHRP](#fhrp)
      - [HSRP](#hsrp)
      - [GLBP](#glbp)
  - [SSH](#ssh)
  - [Vlan](#vlan)
    - [Create vlan and assign interfaces](#create-vlan-and-assign-interfaces)
    - [Trunk](#trunk)
    - [Vlan routing](#vlan-routing)
    - [Vlan\`s show commands](#vlans-show-commands)
  - [Static routing](#static-routing)
    - [Next hop route](#next-hop-route)
    - [Directly connected route](#directly-connected-route)
    - [Default route](#default-route)
    - [Alternate route](#alternate-route)
  - [Dynamic routing](#dynamic-routing)
    - [RIPv2](#ripv2)
    - [RIPng](#ripng)
    - [OSPFv2](#ospfv2)
    - [OSPFv3](#ospfv3)
  - [ACL - Access Control Lists](#acl---access-control-lists)
    - [Standard](#standard)
      - [Numered](#numered)
      - [Named](#named)
    - [Extended ACL](#extended-acl)
      - [Numered](#numered-1)
      - [Named](#named-1)
    - [Modifying ACL](#modifying-acl)
    - [ACL on VTY](#acl-on-vty)
    - [Show commands](#show-commands)
    - [Other](#other)
  - [DHCP](#dhcp)
    - [Server](#server)
    - [Helper address](#helper-address)
    - [Router as DHCP client](#router-as-dhcp-client)
    - [Show commands](#show-commands-1)
    - [Windows usefull commands](#windows-usefull-commands)
  - [NAT](#nat)
    - [Static](#static)
    - [Dynamic](#dynamic)
    - [Port Address Translation - using one IP](#port-address-translation---using-one-ip)
    - [Port Address Translation - using a pool of IP`s](#port-address-translation---using-a-pool-of-ips)
    - [Port Forwarding](#port-forwarding)
    - [Show](#show)

# Cheetsheet: Cisco configuration commands (for CCNA)

## Basic configuration

### Hostname

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname S1
S1(config)#
```

### Priv exec password

```
S1(config)# enable secret my-password
```

### No ip domain lookup

```
S1(config)# no ip domain-lookup
```

### Console & vty password

```
S1(config)# line console 0
S1(config-line)# password my-password
S1(config-line)# login

S1(config-line)# line vty 0 4
S1(config-line)# password my-password
S1(config-line)# login
```

### Plain text password encryption

```
S1(config)# service password encryption
```

### Banner MOTD

```
S1(config)# banner motd #Message of the day#
```

### Logging synchronus

```
S1(config)# line console 0
S1(config)# logging synchronous
```


### Saving config

```
S1# copy running-config startup-config
```

### Restore defaults

```
S1# erase startup-config
S1# delete vlan.dat
S1# reload
```

### Clock

```
S1# clock set 12:15:00 10 Mar 2020
S1# show clock
```

### Blocking after x retries & set console log out timeout & password min-length

```
R1(config)# line console 0 / line vty 0 4 
R1(config-line)# exec-timeout 5 0

R1(config)# login block-for 30 attempts 2 within 120

R1(config)# security passwords min-length 10
```

### Disable CDP

```
R1(config)# no cdp run
```

### Show switch command

```
S1# show interfaces f0/1
S1# show ip interface
S1# show ip interface brief
S1# show ip arp
S1# show mac-address-tabe
S1# show running config
S1# show vlan
S1# show access-lists
```

## Switch basic

### Port-security

```
S1(config)# interface f0/1
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# switchport maximum 1
S1(config-if)# switchport violation {protect|restrict|shutdown}
S1(config-if)# switchport mac-address {sticky|mac_addr}
```

### SVI - Switch Vlan Interface

```
S1(config)# interface vlan1 
S1(config-if)# ip address 192.168.1.10 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# description sample-text
```

### Switch default gateway

```
S1(config)# ip default-gateway 192.168.1.1
```

### STP

#### STP 802.1d

```
S1(config)# interface f0/1
S1(config-if)# spanning-tree cost 15
S1(config-if)# no spanning-tree cost
```

#### Per Vlan STP - PVST+

```
S1(config)# spanning-tree vlan 1 root {primary|secondary}
S1(config)# spanning-tree vlan 1 priority {4096*n + vlan_id}
```

#### PortFast and BPDU guard

On single interface

```
S1(config)# interface f0/1
S1(config-if)# spanning-tree portfast
S1(config-if)# spanning-tree bpduguard enable
```

Portfast global on all interfaces

```
S1(config)# spanning-tree portfast default
```

Portfast & bpdu guard global on all interfaces

```
S1(config)# spanning-tree portfast bpduguard default
```

#### Rapid PVST+

```
S1(config)# spanning-tree mode rapid-pvst
```

#### Clear & show STP process

```
S1# clear spanning-tree detected-protocols
S1# show spanning-tree
S1# show spanning-tree summary
S1# show spanning-tree vlan
S1# show spanning-tree vlan 10
```

### Etherchannel


```
S1(config)# interface range f0/1-2
S1(config-if-range) channel-group 1 mode {active|passive|on}

S1(config)# interface port-channel 1
S1(config-if)#

S1# show interface port-channel
S1# show etherchannel summary
S1# show etherchannel port-channel
S1# show interfaces etherchannel
```

## Router basic

### Enable IPv6 routing

```
R1(config)# ipv6 unicast-routing
```

### FHRP

#### HSRP

Wirtualny adres ip wprowadzamy jest na obu routerach, reszta jest opcjonalna
```
R1(config)# interface g0/1
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt

R1# show standby
R1(config-if)# no standby 1
```

#### GLBP

Na drugim routerze ustawamy wirtaulne IP i metodę balansowania ruchu
```
R1(config)# interface g0/1
R1(config-if)# glbp 1 ip 192.168.1.254
R1(config-if)# glbp 1 preempt
R1(config-if)# glbp 1 priority 150
R1(config-if)# glbp 1 load-balancing round-robin

R1# show glbp
R1(config-if)# no glbp 1
```

## SSH

```
Router(config)# hostaname R1
R1(config)# ip domain-name cisco.com

R1(config)# ip ssh version 2
R1(config)# ip ssh time-out 60
R1(config)# ip ssh authentication-retries 2

R1(config)# crypto key generate rsa general-keys modulus 1024

R1(config)# username Admin pribilege 15 secret my-password

R1(config)# line vty 0 4
R1(config)# transport input ssh
R1(config)# login local
```

## Vlan

### Create vlan and assign interfaces

```
S1(config)# vlan 10
S1(config-vlan)# name Servers

S1(config)# interface f0/1
S1(config-if)# switchport access vlan 10
```

### Trunk 

```
S1(config)# interface g0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# switchport trunk allowed vlan 10,20,99
```

### Vlan routing

```
R1(config)# interface g0/0
R1(config-if)# no shutdown

R1(config)# interface g0/0.10
R1(config-subif)# encapsulation dot1q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0

R1(config)# interface g0/0.99
R1(config-subif)# encapsulation dot1q 99 native
R1(config-subif)# ip address 192.168.99.1 255.255.255.0

R1# show ip route
R1# show vlans
```

### Vlan\`s show commands

```
S1(config)# show interface f0/2 switchport
S1(config)# show interface f0/1 trunk
S1(config)# show interfacs switchport
S1(config)# show vlan
```

## Static routing

### Next hop route

```
R1(config)# ip route 192.168.1.0 255.255.255.0 10.0.0.2
```

### Directly connected route

```
R1(config)# ip route 192.168.1.0 255.255.255.0 s0/0/0
```

### Default route

```
R1(config)# ip route 0.0.0.0 0.0.0.0 {ip_addr|interface}
```

### Alternate route

Example with administrative distanse eq 10
```
R1(config)# ip route -||- 10
```

## Dynamic routing

### RIPv2

```
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 192.168.1.0
```

Pass to othe router default route
```
R1(config-router)# default-information originate
```

Passive interface
```
R1(config-router)# passive interface g0/0
R1(config-router)# passive interface default -> for all int
```

### RIPng

```
R1(config)# ipv6 unicast-routing
R1(config)# interface s0/0/0
R1(config-if)# ipv6 rip RIP-AS enable
R1(config-if)# ipv6 rip RIP-AS default-information originate
```

### OSPFv2

```
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0

OR

R1(config-router)# network 192.168.1.1 0.0.0.0 area 0

R1(config-router)# passive-interface g0/0
R1(config-router)# default-information originate
```

OSPF path cost
```
R1(config)# interface s0/0/1
R1(config-if)# bandwidth 64

OR

R1(config-if)# ip ospf cost 1
```

OSPF message MD5 authentication

- global
  
  ```
  R1(config-router)# area 0 authentication message-digest
  
  R1(config-if)# ip ospf message-digest-key 1 md5 Cisco123
  ```

- on single interface
  
  ```
  R1(config-if)# ip ospf message-digest-key 1 md5 Cisco123
  R1(config-if)# ip ospf authentication message-digest
  ```

Multiarea OSPF

- network

  ```
  R1(config-router)# network 10.0.1.0 0.0.0.3 area {area-id}
  ```

- path summary
  
  ```
  R1(config-router)# area {area-id} range {ip_address} {mask}
  ```



Show commands

```
R1# show ip ospf neighbour
R1# show ip ospf database
R1# show ip ospf interface
R1# show ip ospf interface brief
R1# show ip ospf interface s0/0/0
R1# show ip protocols
R1# show ip ospf
R1# show ip route ospf
```

Other
```
R1# clear ip ospf process

R1(config-if)# ip ospf hello-interval 10
R1(config-if)# ip ospf dead-interval 30
```

### OSPFv3

```
R1(config)# ipv6 unicast-routing
R1(config)# ipv6 router ospf 10
R1(config-router)# router-id 1.1.1.1

R1(config)# interface s0/0/0
R1(config-if)# ipv6 ospf 10 area 0
```

Other
```
R1# clear ipv6 ospf process
```

## ACL - Access Control Lists

Generalną zasadą jest, że rozszerzone listy ACL umieszczamy jak najbliżej źródła, a standardowe ACL jak najbliżej celu

### Standard

#### Numered

```
R1(config)# access-list 1 {deny|permit} 192.168.0.1 0.0.0.255
R1(config)# access-list 2 deny host 192.168.0.1
R1(config)# access-list 3 permit any 192.168.0.1

R1(config)# interface g0/1
R1(config-if)# ip access group 1 {in|out} 
```

host = 0.0.0.0
any = 255.255.255.255

#### Named

```
R1(config)# ip access-list standard NO_ACCESS
R1(config-std-nacl)# deny host 192.168.11.10
R1(config-std-nacl)# permit any

R1(config)# interface g0/1
R1(config)# ip access-group NO_ACCESS out
```

### Extended ACL

#### Numered

```
R1(config)# ip access-list 101 permit tcp any any eq 23
```

#### Named

```
R1(config)# ip access-list extended NAME
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80

R1(config-ext-nacl)# permit tcp src_addr dst_addr eq 80
```

### Modifying ACL

```
R1(config)# ip access-list standard 1
R1(config-std-nacl)# no 10
R1(config-std-nacl)# 10 deny host 192.168.10.10
```

### ACL on VTY

```
R1(config)# line vty 0 4
R1(config-line)# access-class 21 in
```

### Show commands

```
R1# show access-lists
R1# show running-config
R1# show ip interface s0/0/0
```

### Other

```
R1# clear access-list counters 1
```

## DHCP

### Server

```
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.9
R1(config)# ip dhcp pool LAN-1
R1(config-dhcp-config)# network 192.168.1.0 255.255.255.0
R1(config-dhcp-config)# default-router 192.168.1.1
R1(config-dhcp-config)# dns-server 8.8.8.8
R1(config-dhcp-config)# domain-name ccna.lab
```

### Helper address

```
R1(config)# interface g0/1
R1(config-if)# ip helper address 192.168.11.6 
```

### Router as DHCP client

```
R1(config)# interface f0/1
R1(config-if)# no shutdown
R1(config-if)# ip address dhcp

OR

R1(config-if)# ip autoconfigure
```

### Show commands

```
R1# show ip dhcp conflict
R1# show interaface g0/1
```

### Windows usefull commands

```
C:\> ipconfig /release
C:\> ipconfig /renew
```

## NAT

### Static

```
R1(config)# ip nat inside source static 192.168.10.6 209.165.201.5
R1(config)# interface f0/1
R1(config-if)# ip address && no shutdown
R1(config-if)# ip nat inside

R1(config)# interface s0/0/1
R1(config-if)# ip address && no shutdown
R1(config-if)# ip nat inside
```

### Dynamic

```
R1(config)# ip nat pool NAT-1 209.156.200.226 209.156.200.240 netmask 255.255.255.224
R1(config)# ip access-list 1 permit 192.168.0.0 0.0.0.255 
R1(config)# ip nat insisde source list 1 pool NAT-1

R1(config)# interface g0/1
R1(config-if)# ip nat {inside|outside}
```

### Port Address Translation - using one IP

```
R1(config)# ip access-list 1 permit 192.168.0.0 0.0.255.255
R1(config)# ip nat source list 1 interface s0/0/0 overload

R1(config)# interface g0/1
R1(config-if)# ip nat {inside|outside}
```

### Port Address Translation - using a pool of IP`s

```
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 interface s0/0/0 overload

R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

### Port Forwarding

```
R1(config)# ip nat inside source static tcp 192.168.10.5 80 209.165.200.225 8080

R1(config)# interface g0/1
R1(config-if)# ip nat {inside|outside}
```

### Show

```
R1# show ip nat translations
R1# show ip nat statiscs
R1# show running-config
```
