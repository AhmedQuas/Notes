### Table of Contents
- [Cheetsheet: Cisco Security configuration commands (for CCNA Security)](#cheetsheet-cisco-security-configuration-commands-for-ccna-security)
  - [Basic router config](#basic-router-config)
  - [Secuiring the console, aux & vty lines](#secuiring-the-console-aux--vty-lines)
  - [SSH server configuration](#ssh-server-configuration)
  - [AAA](#aaa)
  - [AAAv2](#aaav2)
  - [IOS Image](#ios-image)
  - [SNMP](#snmp)
  - [NTP](#ntp)
  - [Syslog](#syslog)
  - [AutoSecure](#autosecure)
  - [Zone-Based Policy Firewalls](#zone-based-policy-firewalls)
  - [Basic switch config](#basic-switch-config)
  - [Secure STP, trunk & access ports](#secure-stp-trunk--access-ports)
  - [Port security](#port-security)
  - [DHCP](#dhcp)
  - [Inter VLAN routing](#inter-vlan-routing)
  - [DHCP Snooping](#dhcp-snooping)
  - [Site-to-site VPN](#site-to-site-vpn)
- [ASA](#asa)
  - [Basic config](#basic-config)
  - [PAT](#pat)
  - [Static NAT](#static-nat)
  - [ICMP inspection](#icmp-inspection)
  - [DHCP](#dhcp-1)
  - [AAA](#aaa-1)
  - [ACL](#acl)
  - [Show](#show)
- [Misc](#misc)

# Cheetsheet: Cisco Security configuration commands (for CCNA Security)

## Basic router config

```
Router(config)# hostname R1
R1(config)# no ip domain-lookup

R1(config)# interface s0/0/1
R1(config-if)# ip address 1.1.1.1 255.255.255.0
R1(config-if)# no shutdown

R1(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2 

R1(config)# router ospf 1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.3 area 0
R1(config-router)# passive-interface g0/1

R1(config)# key chain NetAcad
R1(config-keychain)# key 1
R1(config-keychain-key)# key-string CCNASkeystring
R1(config-keychain-key)#cryptographic-algorithm hmac-sha-256
R1(config-if)# ip ospf authentication key-chain NetAcad

R1# show ip route
R1# show ip ospf neighbor
R1# show ip ospf interface s0/0/0

R1(config)# login block-for 30 attempts 2 within 120
R1(config)# login on-failure log every 1
```

## Secuiring the console, aux & vty lines

```
R1(config)# security passwords min-length 10
R1(config)# enable algorithm-type scrypt secret cisco12345
R1(config)# service password-encryption
R1(config)# username user01 algorithm-type scrypt secret user01pass
R1(config)# banner motd $Unauthorized access strictly prohibited!$

R1(config)# line [console/aux] 0
R1(config-line)# password ciscocon
R1(config-line)# exec-timeout 5 0
R1(config-line)# login local
R1(config-line)# logging synchronous

R1(config)# line vty 0 4
R1(config-line)# password ciscovty
R1(config-line)# exec-timeout 5 0
R1(config-line)# transport input tssh
R1(config-line)# login local
R1(config-line)# privilege level 15
```

## SSH server configuration

```
R1(config)# ip domain-name ccnasecurity.com
R1(config)# username admin privilege 15 algorithm-type scrypt secret
cisco12345
R1(config)# crypto key generate rsa general-keys modulus 1024

R1(config)# ip ssh version
R1(config)# ip ssh time-out 90
R1(config)# ip ssh authentication-retries 2

R1(config)# show ip ssh
```

## AAA

```
R1(config)# aaa new-model

#Default login authentication/authorization method
R1(config)# aaa authentication login default local
R1(config)# aaa authorization exec default local

R1# enable view
Password: cisco12345
R1(config)# parser view admin1
R1(config-view)# secret admin1pass
R1(config-view)# commands exec include all show
R1(config-view)# commands exec include all config terminal
R1(config-view)# commands exec include all debug

R1# enable view admin1
Password:
R1# show parser view
```

## AAAv2

```
R1(config)# username Admin01 privilege 15 algorithm-type scrypt secret 
Admin01pass
R1(config)# aaa new-model
R1(config)# aaa authentication login default local-case none

R1(config)# aaa authentication login TELNET_LINES local
R1(config)# line vty 0 4
R1(config-line)# login authentication TELNET_LINES

R1(config)# aaa authentication login default group radius none
R1(config)# radius server CCNAS
R1(config-radius-server)# address ipv4 192.168.1.3 auth-port 1812 acct-port 1813
R1(config-radius-server)# key WinRadius
```

## IOS Image

```
R1(config)# secure boot-image
R1(config)# secure boot-config
R1# show secure bootset
R1# show flash:
```

## SNMP

```
R1(config)# ip access-list standard PERMIT-SNMP
R1(config-std-nacl)# permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)# exit

R1(config)# snmp-server view SNMP-RO iso included
R1(config)# snmp-server group SNMP-G1 v3 priv read SNMP-RO access PERMIT-SNMP
R1(config)# snmp-server user SNMP-Admin SNMP-G1 v3 auth sha Authpass priv aes
128 Encrypass
R1# show snmp group
R1# show snmp user
```

## NTP

```
R1# show clock
R1# clock set 20:12:00 Dec 17 2014

R1(config)# ntp authentication-key 1 md5 NTPpassword
R1(config)# ntp trusted-key 1
R1(config)# ntp authenticate
R1(config)# ntp master 1

R2(config)# ntp authentication-key 1 md5 NTPpassword
R2(config)# ntp trusted-key 1
R2(config)# ntp authenticate
R2(config)# ntp server 10.1.1.2
R2(config)# ntp update-calendar
```

## Syslog

```
R1(config)# service timestamps log datetime msec
R1(config)# logging host 192.168.1.3
R1(config)# logging trap [warnings]
R1# show logging
```

## AutoSecure

>By using a single command in CLI mode, the AutoSecure feature allows you to disable common IP services that can be exploited for network attacks. It can also enable IP services and features that can aid in the defense of a network when under attack.

```
R1# auto secure
```

## Zone-Based Policy Firewalls


```
R1(config)# zone security INSIDE 
R1(config)# zone security INTERNET 
```

Class map is used to match traffic which will be allowed to pass from INSIDE.

```
R1(config)# class-map type inspect match-any INSIDE_PROTOCOLS
R1(config-cmap)# match protocol tcp 
R1(config-cmap)# match protocol udp 
R1(config-cmap)# match protocol icmp
```

Policy map is used to bind class-map with action(inspect)

```
R1(config)# policy-map type inspect INSIDE_TO_INTERNET
R1(config-pmap)# class type inspect INSIDE_PROTOCOLS
R1(config-pmap-c)# inspect
```

The zone pair allows you to define a one-way firewall policy between two zones security.

```
R1(config)# zone-pair security INSIDE_TO_INTERNET source INSIDE destination 
INTERNET
```

Bind zone-pair with policy-map

```
R1(config)# zone-pair security INSIDE_TO_INTERNET
R1(config-sec-zone-pair)# service-policy type inspect INSIDE_TO_INTERNET
```

The last step is to bind interface with zone

```
R1(config)# interface G0/0/0
R1(config-if)# zone-member security INSIDE
```

```
R1# show zone-pair security
R1# show policy-map type inspect zone-pair
```

## Basic switch config

```
S1(config)# int vlan 1
S1(config-if)# adres ip 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown

S1(config)# no ip http server S1
S1(config)# no ip http secure-server

S1(config)# ip default gateway

S1(config)# enable algorithm-type scrypt secret cisco12345
```

## Secure STP, trunk & access ports

```
S1(config)# spanning-tree vlan 1 priority 0
S1# show spanning-tree
S1# show spanning-tree interface f0/6 detail
S1(config-if)# spanning-tree guard root
S1# show spanning-tree inconsistentports

S1(config)# interfejs f0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# switchport nonegotiate

S1# show interfaces trunk
S1# show interfaces f0/1 trunk
S1# show interfaces f0/1 switchport

S1(config)# interface f0/5
S1(config-if)# switchport mode access
S1(config-if)# spanning-tree portfast
S1(config-if)# spanning-tree bpduguard enable

S1(config)# spanning-tree loopguard default
```

## Port security

```
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security mac-address xxxx.xxxx.xxxx

S1(config)# interface f0/18
S1(config-if)# switchport mode access
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security maximum 3
S1(config-if)# switchport port-security violation shutdown
S1(config-if)# switchport port-security aging time 120
```

## DHCP

```
R1(config)# ip dhcp pool CCNAS
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.4
```

## Inter VLAN routing

```
R1(config)# interface g0/0/1
R1(config-if)# shutdown
R1(config-if)# no ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# int g0/0/1.1
R1(config-if)# encapsulation dot1q 1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
```

## DHCP Snooping

```
S1(config)# ip dhcp snooping
S1(config)# ip dhcp snooping information option
S1(config)# ip dhcp snooping vlan 1,20

S1(config)# interface f0/6
S1(config-if)# ip dhcp snooping limit rate 10

S1(config-if)# description connects to DHCP server
S1(config-if)# ip dhcp snooping trust

S1# show ip dhcp snooping
```

## Site-to-site VPN

>In Phase 1, IKE defines the key exchange method to be used for upload and validation IKE rules between partners. In Phase 2 of IKE, partners exchange and match IPsec policies to data traffic authentication and encryption.

```
R1(config)# crypto isakmp enable

R1(config)# crypto isakmp policy 10
R1(config-isakmp)# hash sha
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 14
R1(config-isakmp)# lifetime 3600
R1(config-isakmp)# encryption aes 256

R1# show crypto isakmp policy

R1(config)# crypto isakmp key cisco123 address PEER_IP_ADDRESS

R1(config)# crypto ipsec transform-set 50 esp-aes 256 esp-sha-hmac
R1(config)# crypto ipsec security-association lifetime seconds 1800

R1(config)# access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 
0.0.0.255

R1(config)# crypto map CMAP 10 ipsec-isakmp
R1(config-crypto-map)# match address 101
R1(config-crypto-map)# set peer PEER_IP_ADDRESS
R1(config-crypto-map)# set pfs group14
R1(config-crypto-map)# set transform-set 50
R1(config-crypto-map)# set security-association lifetime seconds 900

R1(config)# interface S0/0/0
R1(config-if)# crypto map CMAP

R1# show crypto ipsec transform-set
R1# show crypto isakmp sa
R1# show crypto map
```

# ASA

## Basic config

```
ASA(config)# configure factory-default
ASA# write erase

ASA(config)# hostname CCNAS-ASA
ASA(config)# domain-name ccnasecurity.com
ASA(config)# enable password PASSWORD
ASA(config)# clock set 08:09:00 april 19 2019

ASA(config)# interface g1/1 
ASA(config-if)# nameif outside 
ASA(config-if)# ip address 209.165.200.226 255.255.255.248 
ASA(config-if)# security-level 0 
ASA(config-if)# no shutdown

ASA(config)# http server enable
ASA(config)# http 192.168.1.0 255.255.255.0 inside
ASA(config)# route outside 0.0.0.0 0.0.0.0 209.165.200.225

ASA# show interface ip brief
ASA# show ip address
```

## PAT

```
ASA(config)# object network INSIDE-NET
ASA(config-network-object)# subnet 192.168.1.0 255.255.255.0
ASA(config-network-object)# nat (inside,outside) dynamic interface

ASA# show run object
ASA# show run nat
ASA# show xlate

ASA# clear nat counters
```

## Static NAT

```
ASA(config)# object network dmz-server
ASA(config-network-object)# host 192.168.2.3
ASA(config-network-object)# nat (dmz,outside) static 209.165.200.227
```

## ICMP inspection

```
ASA# show run | begin class
ASA# show run policy-map

ASA(config)# policy-map global_policy
ASA(config-pmap)# class inspection_default
ASA(config-pmap-c)# inspect icmp
```

## DHCP

```
ASA(config)# dhcpd address 192.168.1.5-192.168.1.36 inside
ASA(config)# dhcpd dns 209.165.201.2
ASA(config)# dhcpd option 3 ip 192.168.1.1
ASA(config)# dhcpd enable inside

ASA(config)# show run dhcpd
```

## AAA

```
ASA(config)# username admin password cisco12345
ASA(config)# aaa authentication ssh console LOCAL
ASA(config)# crypto key generate rsa modulus 1024

ASA(config)# ssh 192.168.1.0 255.255.255.0 inside
ASA(config)# ssh 172.16.3.3 255.255.255.255 outside
ASA(config)# ssh timeout 10
```

## ACL

```
ASA(config)# access-list OUTSIDE-DMZ permit ip any host 192.168.2.3
ASA(config)# access-group OUTSIDE-DMZ in interface outside
```

## Show

```
ASA# show running-config
ASA# show file system
ASA# show version
ASA# show flash

show interface ip brief
```

# Misc

```
R1(config)# ip scp server enable
R1(config)# show flash

S1(config)# interface range f0/2 - 4
S1(config-if-range)# shutdown

ASA# write mem / copy run star

R1(config)# license boot module module-name technology-package securityk9
R1# write
R1# reload
```