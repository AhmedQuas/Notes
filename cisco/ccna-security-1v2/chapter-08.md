### Spis treści
- [Chapter 8: Implementing Virtual Private Network](#chapter-8-implementing-virtual-private-network)
  - [8.1 VPN Overview](#81-vpn-overview)
  - [8.2 IPsec VPN components and operation](#82-ipsec-vpn-components-and-operation)
  - [8.3 Implementing Site-to-Site IPsec VPNs with CLI](#83-implementing-site-to-site-ipsec-vpns-with-cli)
  - [8.3.1 ACL configuration](#831-acl-configuration)
  - [8.3.2 ISAKMP Policy](#832-isakmp-policy)

# Chapter 8: Implementing Virtual Private Network

>Organizations use virtual private networks (VPNs) to create an end-to-end private network connection over third-party networks. VPNs use a tunnel to enable remote users to access central site network resources. VPNs cannot guarantee that the information remains secure while traversing the tunnel - it is guaranteed by are appling  cryptography to VPNs to establish secure, end-to-end, private network connections.

>The IP Security (IPsec) protocol provides a framework for configuring secure VPNs. It is a reliable way to maintain communication privacy while streamlining operations, reducing costs, and allowing flexible network administration.

## 8.1 VPN Overview

>Instead of using a dedicated physical connection, a VPN uses virtual connections routed through the Internet from the organization to the remote site. The first VPNs were strictly IP tunnels that did not include authentication or encryption of the data(`GRE`).

VPN benefits:
- `cost savings` 
- `security` - provide the highest level of security avaiable, by using advanced encryption and authentication protocols
- `scalability` - adding new users don`t require making significant changes in infrastructure, VPN use Internet 
- `compatibility` - can be implemented using popular broadband technologies

Types of VPN:
- `remote-access` - is created when VPN information is not statically set up, but instead allows for dynamically changing connection information, which can be enabled and disabled when needed,
- `site-to-site` -  when devices on both sides of the VPN connection are aware of the VPN configuration in advance. The VPN remains static, and internal hosts have no knowledge that a VPN exists. Hosts send and receive normal TCP/IP traffic through a VPN gateway, which can be a router, firewall, Cisco VPN concentrator, or Cisco ASA. The VPN gateway is responsible for encapsulating and encrypting outbound traffic from a particular site and sending it through a VPN tunnel over the Internet to a peer VPN gateway at the other site

>Site-to-site VPN topologies continue to evolve. Examples of more complex VPN topologies include Multiprotocol Label Switching (`MPLS`) VPN, Dynamic Multipoint VPN (`DMVPN`) and Group Encrypted Transport VPN (`GETVPN`).

`Hairpinning` is a term used to describe a situation in which VPN traffic that enters an interface may also be routed out of that same interface. Hairpinning can also be used when remote-access VPNs must connect to the VPN terminating device at corporate headquarters before traffic is permitted to go to the Internet.

`Split tunneling` can be used if the corporate policy dictates that VPN traffic must be split between traffic destined for the corporate subnets (`trusted`) and traffic destined to the Internet (`untrusted`). The VPN software on the remote-access client splits the traffic. If traffic is destined for a corporate subnet, it is sent through the VPN tunnel. Otherwise, it is sent as unencrypted traffic (untrusted) to the Internet.

IPsec(IETF standard, RFC 2401- 2412) can protect virtually all traffic from Layer 4 through Layer 7. Using the IPsec framework, IPsec provides these essential security functions:
- confidentiality using encryption,
- integirty using hashing algorithms,
- authentication using `IKE` - Internet Key Exchange,
- secure key exchange using the `DH` - Diffie Hellman algorithm

>IPsec is not bound to any specific rules for secure communications. This flexibility of the framework allows IPsec to easily integrate new security technologies without updating the existing IPsec standards.


`SA`(Security Associations) is the basic building block of IPsec. When establishing a VPN link, the peers must share the same SA to negotiate key exchange parameters, establish a shared key, authenticate each other, and negotiate the encryption parameters.

## 8.2 IPsec VPN components and operation

IPsec framework:
- IPsec protocol:
  - `AH` - Authentication Header, uses IP protocol 51 and is appropriate only when confidentiality(data encryption) is not required or peritted. It provides data `authentication and integrity`. All text is transported unencrypted. The AH function is applied to the entire packet, except for any IP header fields that normally change in transit. Fields that normally change during transit are called mutable fields. For example, the Time to Live (TTL) field is considered mutable because routers modify this field. AH may not work if the environment uses NAT. AH = Hash(IP header + data + key). Anti-replay is supported.
  - `ESP` - Encapsultaion Security Protocol, uses IP protocol 50 and provides both `confidentiality authentication`. It encrypt the `payload` only. It can enforce `anti-replay` protection(works by keeping track of packet sequence numbers and using a sliding window on the destination end). With ESP authentication, the encrypted IP datagram and trailer, and the ESP header are included in the hashing process. Then, a new IP header is attached to the authenticated payload. The new IP address is used to route the packet through the Internet. When both authentication and encryption are selected, encryption is performed first(apid detection and rejection of replayed or bogus packets). To reiterate, ESP provides confidentiality with encryption and provides integrity with authentication.
  - `ESP + AH`
- confidentiality = encryption(if ESP was choosen as IPsec protocol)
  - `DES` - Data Encryption Standard, 56-bit
  - `3DES` - 3 times 56bit
  - `AES` - Advanced Encryption Standard, 128, 192, 256 bits
  - `SEAL` - Software-optimized Encryption Algorithm, stream cipher, 160 bits key
- integirty - ensures that data arrives unchanged at the destination using hash algorithm
  - `MD5` - Message-Digest 5
  - `SHA` - Secure Hash Algorithm
    - SHA1 - legacy,
    - SHA256 - recomended
- authentication, IKE to authenticate users and devices that can carry out communication independently
  - `PSK` - Pre-shared keys, value is entered into each peer manually. The PSK is combined with other information to form the authentication key. PSK do not scale well
  - `RSA` - Rivest, Shamir and Adleman, exchange of digital certificates authenticates the peers
- Diffie-Hellman, public key exchange method:
  - DH 1,2,5 use 768, 1024,1536 bits key siez. They are no longer recomended,
  - DH 14,15,16  use 2048, 3072, 4096. They are recomended for use until 2030
  - DH 19,20,21,24 use 256, 384, 512, 2048 and support `ECC`(Elliptical Curve Cryptography), which reduces the time neede to generate keys. DH 24 is preferred next generation encryption

NSA recomendation:
- AES 128 or 256-bit keys,
- SHA-2
- ECDSA 256, 384-bit prime moduli
- IKE with ECDH

>The discussion of IPsec has focused on IPv4. However, IPsec was initially established to provide security for IPv6 packets. In IPv4, AH and ESP are IP protocol headers. IPv6 uses the extension headers with a next-header value of 50 for ESP and 51 for AH.

ESP and AH can be applied to IP packets in 2 modes:
- `transport` - security is provided only for the transport layer of the OSI model and above. Transport mode protects the payload of the packet but leaves the original IP address in plaintext. The original IP address is used to route the packet through the Internet. ESP transport mode is used `between hosts`.
- `tunnel` - provides security for the complete original IP packet. The original IP packet is encrypted and then it is encapsulated in another IP packet. This is known as IP-in-IP encryption. The IP address on the outside IP packet is used to route the packet through the Internet. ESP tunnel mode is used between a `host and a security gateway, or between two security gateways`. AH transport mode provides authentication and integrity for the entire packet. It does not encrypt the data, but it is protected from modification. AH tunnel mode encapsulates the IP packet with an AH and a new IP header, and signs the entire packet for integrity and authentication.

The Internet Key Exchange (`IKE`) protocol is a key management protocol standard. IKE is used in conjunction with the IPsec standard. IKE automatically negotiates IPsec security associations and enables IPsec secure communications. IKE enhances IPsec by adding features and simplifies configuration for the IPsec standard.

IKE is a hybrid protocol that implements key exchange protocols inside the Internet Security Association Key Management Protocol (`ISAKMP`) framework. ISAKMP defines the message format, the mechanics of a key exchange protocol, and the negotiation process to build an SA for IPsec.

>Instead of transmitting keys directly across a network, IKE calculates shared keys based on the exchange of a series of data packets. This disables a third party from decrypting the keys even if the third party captured all of the exchanged data that was used to calculate the keys. IKE uses `UDP port 500` to exchange IKE information between the security gateways

IKE Phases:
1. Negotiate ISAKMP policy to create a tunnel(main or aggressive mode)
   - verify the peer indetity
   - negotiate a matching IKE SA policy - main goal of IKE Phase 1. This protect of exchange
   - DH key exchange
   - set up a secure tunnel to negotiate IKE Phase 2
2. Negotiate IPsec policy for sending secure traffic across the tunnel(quick mode)
   - negotiate IPsec SA(Secuirty Association) parameters
   - establish IPsec SAs and generate secuirty keys for inbound and outbound
   - priodically renegotiate IPsec SAs,
   - optionally perform an additional Diffie-Hellman exchange

In Phase 1, two IPsec peers perform the initial negotiation of SAs. The basic purpose of Phase 1 is to negotiate ISAKMP policy, authenticate the peers, and set up a secure tunnel between the peers. This tunnel will then be used in Phase 2 to negotiate the IPsec policy.

>The phrases IKE policy and ISAKMP policy are equivalent.

Phase 1 can be implemented in main mode or aggressive mode. When main mode is used, the identities of the two IKE peers are hidden. Aggressive mode takes less time than main mode to negotiate keys between peers. However, since the authentication hash is sent unencrypted before the tunnel is established, aggressive mode is vulnerable to brute-force attacks.

The purpose of IKE Phase 2 is to negotiate the IPsec security parameters that will be used to secure the IPsec tunnel. IKE Phase 2 is called quick mode and can only occur after IKE has established a secure tunnel in Phase 1.

Quick mode also renegotiates a new IPsec SA when the IPsec SA lifetime expires. Basically, quick mode refreshes the keying material that creates the shared secret key.

> IKE version 2 supports NAT detection during Phase 1 and NAT Traversal (NAT-T) during Phase 1. If both VPN devices are NAT-T capable, and if they detect that they are connecting to each other through a NAT device, NAT-T is auto detected and auto negotiated. NAT-T encapsulates ESP packets inside UDP and assigns both the Source and Destination ports as 4500. Now ESP packets can traverse NAT.

## 8.3 Implementing Site-to-Site IPsec VPNs with CLI

VPN configuration tasks:
1. Configure the ISAKMP policy for IKE Phase 1
2. Configure the IPsec Policy for IKE Phase 2
3. Configure a Crypto Map for the IPsec Policy
4. Apply the IPsec Policy
5. Verify the IPsec Tunnel is Operational

## 8.3.1 ACL configuration

```
R1(config)# access-list extended INBOUND
R1(config-ext-nacl)# permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
R1(config-ext-nacl)# permit icmp host 10.0.0.1 host 10.0.0.2
R1(config-ext-nacl)# permit udp host 10.0.0.1 host 10.0.0.2 eq isakmp
R1(config-ext-nacl)# permit esp host 10.0.0.1 host 10.0.0.2
R1(config-ext-nacl)# permit ahp host 10.0.0.1 host 10.0.0.2
R1(config-ext-nacl)# deny ip any any

R1(config-if)# ip access-group INBOUND in
```

>Ensure that the existing ACLs do not block traffic necessary for IPsec negotiations.

>IPsec only supports unicast traffic. To enable routing protocol traffic, the peers in a site-to-site IPsec VPN implementation would need to be configured with a Generic Routing Encapsulation (GRE) tunnel for the multicast traffic. GRE does not provide encryption.

## 8.3.2 ISAKMP Policy

Cisco router has seven default ISAKMP policies ranging from the most secure (policy 65507) to the least secure (policy 65514). If no other policy has been defined by the administrator, Router will attempt to use the most secure default policy.

```
R1# show crypto isakmp default policy
```

ISAKMP policy consist of five SAs to configure(HAGLE):
- `H`ash
- `A`uthentication
- `G`roup
- `L`ifetime
- `E`ncryption

```
R1(config)# crypto isakmp policy 1
R1(config-isakmp)# hash sha
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 24
R1(config-isakmp)# lifetime 3600
R1(config-isakmp)# encryption aes 256

R1# show crypto isakmp policy

R1(config)# crypto isakmp key cisco12345 address 10.0.0.1
```

Peers will attempt to negotiate using the policy with the lowest number (highest priority).

Define interesting traffic

```
R1(config)# access-list 101 permit ip 192.168.1.0 0.0.0.255 192.168.2.0 0.0.0.255
```

`Transform set` - is a set of encryption and hashing algorithms that will be used to transform the data sent through the IPsec tunnel

```
R1(config)# crypto ipsec transform-set SET_NAME ?

R1(config)# crypto ipsec transform-set SET_NAME esp-aes esp-sha-hmac
```

Configure cyrpto map which binds configuration of IPsec and apply it to interface

```
R1(config)# crypto map MAP 10 ipsec-isakmp
R1(config-crypto-map)# match address 101
R1(config-crypto-map)# set transform-set SET_NAME
R1(config-crypto-map)# set peer 10.0.0.1
R1(config-crypto-map)# set pfs group24
R1(config-crypto-map)# set secuirty-association lifetime seconds 900

R1(config-if)# crypto map MAP

R1# show crypto map
```

Test VPN configuration:
- send interesting traffic
- verify that tunnels have been established
    ```
    R1# show crypto isakmp sa
    R1# show crypto ipsec sa
    ```



---

<div>
<a href="chapter-07.md">Prev: Chapter 7</a>
</div>
<div align="right">
<a href="chapter-09.md">Next: Chapter 9</a>
</div>