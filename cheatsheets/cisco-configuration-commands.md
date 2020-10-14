### Table of Contents
- [Cheetsheet: Cisco configuration commands (for CCNA)](#cheetsheet-cisco-configuration-commands-for-ccna)
  - [Basic configuration](#basic-configuration)
    - [Hostname](#hostname)
    - [Priv exec password](#priv-exec-password)
    - [No ip domain lookup](#no-ip-domain-lookup)
    - [Console & vty password](#console--vty-password)
    - [Plain text password encryption](#plain-text-password-encryption)
    - [Banner MOTD](#banner-motd)
    - [Saving config](#saving-config)
    - [Restore defaults](#restore-defaults)
    - [Clock](#clock)
    - [SVI - Switch Vlan Interface](#svi---switch-vlan-interface)
    - [Switch default gateway](#switch-default-gateway)
    - [Blocking after x retries & set console log out timeout & password min-length](#blocking-after-x-retries--set-console-log-out-timeout--password-min-length)
    - [Disable CDP](#disable-cdp)
    - [Show switch command](#show-switch-command)
    - [Enable IPv6 routing](#enable-ipv6-routing)
  - [SSH](#ssh)
  - [Vlan](#vlan)
    - [Create vlan and assign interfaces](#create-vlan-and-assign-interfaces)
    - [Trunk](#trunk)
    - [Vlan routing](#vlan-routing)
    - [Vlan\`s show commands](#vlans-show-commands)
  - [Static routing](#static-routing)
    - [Next hop route](#next-hop-route)

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

### Enable IPv6 routing

```
R1(config)# ipv6 unicast-routing
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