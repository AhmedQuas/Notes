### Spis treści
- [Chapter 3: WLAN Concepts](#chapter-3-wlan-concepts)
  - [3.1 Benefits of Wireless](#31-benefits-of-wireless)
  - [3.2 WLAN Components](#32-wlan-components)
  - [3.3 WLAN Operation](#33-wlan-operation)
  - [3.4 CAPWAP Operation](#34-capwap-operation)
  - [3.5 Channel Management](#35-channel-management)
  - [3.6 WLAN Threats](#36-wlan-threats)
  - [3.7 Secure WLANs](#37-secure-wlans)

# Chapter 3: WLAN Concepts

SRWE - Switching, Routing and Wireless Essentials Bridging Concept

## 3.1 Benefits of Wireless

- network that supports people who are on the move,
- makes mobility possible within the home and business environments

Types of Wireless networks(based on IEEE):
- `WPAN` - Wireless Personal Area Networks - low powered transmitters for a short range network(6-9 meters). Mainly based on 802.15 standard and 2.4 GHz(Bluetooth and ZigBee)
- `WLAN` - Wireless LAN, covers medium-sized netowrk up to 100 meters, based on 802.11(2.4 GHz and 5 GHz)
- `WMAN` - Wireless MANs, cover larger geographics area(mereopolitan city, specific district). WMANs use specific licensed frequencies
- `WWAN` - Wireless WANs, use transmitters to provide coverage over an extensive geographic area, suitable for national and global communications, also use specific licensed frequencies

>Wireless technology uses the unlicensed radio spectrum to send and receive data. The unlicensed spectrum is accessible to anyone who has a wireless router and wireless technology in the device they are using.

Bluetooth - 802.15 WPAN standard that uses a device-pairing process to communicate over distance up to 100m. It can be found in smart home devices, audio connections, automobiles, and other devices that require a short distance connection. Types of BT radios:
- `BLE` - Bluetooth Low Energy - supports multiple network technologies including mesh topology to large scale network devices,
- `BR/EDR` - Bluetooth Basic Rate/Enhanced Rate - this supports point to point topologies and is optimized for audio streaming

Table with 802.11 standards

Wireless radio frequencies bands:
- 2.4 GHz (`UHF`) – 802.11b/g/n/ax
- 5 GHz (`SHF`) – 802.11a/n/ac/ax

Wireless standards organizations:
- `ITU` - International Telecomunication Union regulates the allocation of the radio frequency spectrum and satelite orbits. It prepared some attenuation calculations
- `IEEE` - specified how a radio frequency is modulated to carry information(IEEE 802 LAN/MAN, 802.3, 802.11)
- `Wi-Fi Alliance` - association of vendors whose objective is to improve the interoperability of products that are based on the 802.11 standard by certyfying vendors to conformance to industry norms

## 3.2 WLAN Components

WLAN Components:
- `Wireless NIC`,
- `Wireless router` - typically serves as an:
  - access point,
  - switch,
  - router
- `Wi-Fi range extenders` - easy to set up and configure,
- `Wireless AP`(Access Points),
  - `autonomous APs` - standalone device, useful in situations where only a couple of APs are required in the organization. This type of AP operate independent of other APs and each AP would require manual configuration and management. This would become overwhelming if many APs were needed,
  - `controller-based APs` - often called lightweight APs (LAPs). It use the Lightweight Access Point Protocol (LWAPP) to communicate with a WLAN controller (WLC). Controller-based APs are useful in situations where many APs are required in the network. As more APs are added, each AP is automatically configured and managed by the WLC,
- `Wireless antennas`
  - `omnidirectional antenanas` - provide 360-degree coverage(office, house, conference room, and outside areas),
  - `directional antenna` - focus the radio signal in a given direction, this provides a stronger signal strength in one direction and reduced strength in all other directions(Yagi and parabolic dish antennas),
  - `MIMO antennas` - use multiple(ex up to 8) antennas to increase avaiable bandwidth for IEEE 802.11n/ac/ax wireless networks,


>Many wireless devices you are familiar with do not have visible antennas. They are embedded inside smartphones, laptops, and wireless home routers.

>`LAG` - Link Aggregation Protocol, it is something like EtherChannel. It provides redundancy and load-balancing. All the ports on the switch that are connected to the WLC need to be trunking and configured with EtherChannel on. However, LAG does not operate exactly like EtherChannel. The WLC does not support Port Aggregation Protocol (PaGP) or Link Aggregation Control Protocol (LACP).

## 3.3 WLAN Operation

Wireless topology modes:
- `infrastructure` - a wireless router or AP connects wireless clients to a wired distribution system,
- `ad hoc` - two devices connect wireless in a peer-to-peer(P2P) manner. A wireless router or AP is not present(Wi-Fi Direct, Bluetooth),
- `tethering` - a variant of ad hoc, where a smartphone or tablet with cellular data access is used as a personal hotspot. Quick wireless access

`BSS` - Basic Service Set, consists of a single AP interconnecting all associated wireless clients

`BSSID` - BSS Identifier, MAC address of AP

`BSA` - Basic Service Area 

`ESS` - Extended Service Set, multiple BSS, which are connected to signle distribution system. It provides roaming between BSS

>BSSID is the formal name of the BSS and is always associated with only one AP.

>The IEEE 802.11 standard refers to an ad hoc network as an independent basic service set (`IBSS`).

802.11 Frame Structure

>WLANs are `half-duplex`, shared media configurations. Half-duplex means that only one client can transmit or receive at any given moment.

`CSMA/CA` - Carrier(channel) Sense Multiple Access Collision Avoidance:
- Listen 
- RTS - Ready to send message
- CTS - Clear to send, access is ready for you, send data
- Transmit
- Ack 

>For wireless devices to communicate over a network, they must first associate with an AP or wireless router.

Wireless Client and AP Association
- discover AP,
- authenticate,
- associate
  - SSID,
  - password,
  - network mode - some of them can operate in mixed mode meaning that they can simultaneously support clients connecting via multiple standards,
  - security mode - WEP, WPA, WPA2, WPA3,
  - channel settings - frequency bands

Passive discovery mode - listen the beacon send by APs, default mode,
Active discovery mode - Wireless Client send probe request

## 3.4 CAPWAP Operation

`CAPWAP` - Control and Provisioning of Wireless Access Points, protocols that enables a WLC to manage multiple APs and WLANs. It is responsible for encapsulation and forwarding of WLAN client traffic. It adds additional security with `DTLS`(Datagram Transport Security Layer) on 5246/udp between AP and WLC.

`CAPWAP` is based on LWAPP but adds additional security with `DTLS`.

Split MAC Architecture

CAPWAP split MAC concept does all of the functions normally performed by individual APs and distributes them between two functional components:
- AP MAC functions,
- WLC MAC functions

<div align='center'>

| AP MAC  | WLC MAC  |
|:---:      |:---:       |
| Beacons and probe responses | Authentication|
| Packet acknowledgements and retransmissions| Association and re-association of roaming clients |
| Frame queueing and packet prioritization | Frame translation to other protocols|
| MAC layer data encryption and decryption |Termination of 802.11 traffic on a wired interface |

</div>

>`DTLS` is a protocol which provides security between the AP and the WLC. It allows them to communicate using encryption and prevents eavesdropping or tampering. DTLS is enabled by default to secure the CAPWAP control channel but is disabled by default for the data channel

FlexConnect - wireless soultion for branch office and remote office deployments. It lets you configure and control access points in a branch office from the corporate office through a WAN link, without deploying a controller in each office. Modes of operation for the FlexConnect:
- Connected mode - AP has CAPWAP connectivity with its WLC,
- Stanalone mode - WLC is unreachable. In this mode, a FlexConnect AP can assume some of the WLC functions such as switching client data traffic locally and performing client authentication locally

## 3.5 Channel Management

A common practice is for frequencies to be allocated as ranges. Such ranges are then split into smaller ranges called channels.

Frequency channel saturation techniques:
- `DSSS` - Direct-Sequence Spread Spectrum, spread a signal over a larger frequency band. A properly configured receiver can reverse the DSSS modulation and re-construct the original signal. DSSS is used by 802.11b devices to avoid interference from other devices using the same 2.4 GHz frequency,
- `FHSS` - Frequency-Hopping Spread Spectrum, it transmits radio signals by rapidly switching a carrier signal among many frequency channels. With the FHSS, the sender and receiver must be synchronized to “know” which channel to jump to. This channel hopping process allows for a more efficient usage of the channels, decreasing channel congestion. FHSS was used by the original 802.11 standard. Walkie-talkies and 900 MHz cordless phones also use FHSS, and Bluetooth uses a variation of FHSS.
- `OFDM` - Orthogonal Frequency-Division Multiplexing, this is a subset of frequency division multiplexing in which a single channel uses multiple sub-channels on adjacent frequencies. Sub-channels in an OFDM system are precisely orthogonal to one another which allow the sub-channels to overlap without interfering. OFDM is used by a number of communication systems including 802.11a/g/n/ac. The new 802.11ax uses a variation of OFDM called Orthogonal frequency-division multiaccess (`OFDMA`).

>Each channel is allotted 22 MHz bandwidth and is separated from the next channel by 5 MHz. The 802.11b standard identifies 11 channels for North America, as shown in the figure (13 in Europe and 14 in Japan).

>For the 5GHz standards 802.11a/n/ac, there are 24 channels. The 5GHz band is divided into three sections. Each channel is separated from the next channel by 20 MHz.

WLAN deployment recomendations:
- if APs are to use existing wiring or if there are locations where APs cannot be placed, note these locations on the map,
- note all potential sources of interference which can include microwave ovens, wireless video cameras, fluorescent lights, motion detectors, or any other device that uses the 2.4 GHz range,
- position APs above obstructions,
- position APs vertically near the ceiling in the center of each coverage area, if possible,
- position APs in locations where users are expected to be. For example, conference rooms are typically a better location for APs than a hallway,
- if an IEEE 802.11 network has been configured for mixed mode, the wireless clients may experience slower than normal speeds in order to support the older wireless standards.

## 3.6 WLAN Threats

WLAN is open to anyone withon range of an AP and the appropriate credentials to associate to it.

WLAN threats:
- `interception of data` - wireless data should be enrypted,
- `wireless intruders` - unauthorized users attempting to access network resources, can be deterred through effective authentication techniques,
- `DoS attacks` - access to WLAN services can be compromised,
  - improperly configured devices,
  - a malicious user intentionally interfering with wireless communication,
  - accidential interference - microwave, bluetooth
- `rogue APs` - unauthorized APs installed(in corporate network without explicit authorization) by well-intentioned user or for mailicious purposes, used to launch MitM attack

>A popular wireless MITM attack is called the “`evil twin AP`” attack, where an attacker introduces a rogue AP and configures it with the same SSID as a legitimate AP, as shown in the figure. Locations offering free Wi-Fi, such as airports, cafes, and restaurants, are particularly popular spots for this type of attack due to the open authentication.

## 3.7 Secure WLANs

- SSIDs are easily discovered even if APs do not broadcast them,
- MAC addresses can be spoofed

>The best way to secure a wireless network is to use authentication and encryption systems.

Security recomendation:
- SSID Cloaking - SSID Broadcast, SSID beacon frame disabled,
- MAC Address Filtering - MAC address can be spoofed,
- WPA2 authentication
- encryption

Authnetication methods:
- open - no password required, free access to internet in public places,
- shared:
  - WEP- the original 802.11 specification designed to secure the data using the Rivest Cipher 4 (RC4) encryption method with a static key. However, the key never changes when exchanging packets. This makes it easy to hack. WEP is no longer recommended and should never be used.
  - WPA - Wi-Fi Protected Access, a Wi-Fi Alliance standard that uses WEP, but secures the data with the much stronger Temporal Key Integrity Protocol (TKIP) encryption algorithm. TKIP changes the key for each packet, making it much more difficult to hack.
  - WPA2 - current industry standard for securing wireless networks. It uses the Advanced Encryption Standard (AES) for encryption. WPA2 authetication methods:
    - Personal - PSK, pre-shared key,
    - Enterprise - RADUIS(AAA)
      - 1812, 1645/udp - authentication,
      - 1813, 1646/udp - accounting,
      - 802.1X authentication which uses `EAP`(Extensible Authentication Protocol) for authentication
  - WPA3 - use latest security methods, disallow outdated legacy protocols ad require the use of `PMF`(Protected Management Frames)

Encryption methods:
- WEP - RC4,
- WPA - Temporal Key Integrity Protocol(TKIP), it provides support for legacy WLAN equipment by addressing the original flaws associated with the 802.11 WEP encryption method. It makes use of WEP, but encrypts the Layer 2 payload using TKIP, and carries out a Message Integrity Check (MIC) in the encrypted packet to ensure the message has not been altered,
- WPA2 - Advanced Encryption Standard(AES), uses the Counter Cipher Mode with Block Chaining Message Authentication Code Protocol (CCMP) that allows destination hosts to recognize if the encrypted and non-encrypted bits have been altered,
- WPA3
  - Personal - use SAE(Simultaneous Authentication Equals) based on 802.11-2016 so PSK is never exposed, so it is not vulnerable to WPA2 brute force, 
  - Enterprise - use 802.1X/EAP authentication, use `CNSA`(Commercial National Security Algorithm)
  - Open networks - still do not use any authentication, but they use `OWE`(Opportunistic Wireless Encryption) to encrypt all wireless traffic
  - IoT onboarding - use DPP(Device Provisioning Protocol)

>Each headless device has a hardcoded public key. The key is typically stamped on the outside of the device or its packaging as a Quick Response (QR) code. The network administrator can scan the QR code and quickly onboard the device. Although not strictly part of the WPA3 standard, DPP will replace WPS over time.




---

<div>
<a href="srwe-chapter-11.md">Next: SRWE 11</a>
</div>
<div align="right">
<a href="srwe-chapter-13.md">Next: SRWE 13</a>
</div>