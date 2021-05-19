### Spis treści
- [Chapter 10: Advanced Cisco Adaptive Security Appliance](#chapter-10-advanced-cisco-adaptive-security-appliance)
  - [10.1 Introduction to the ASDM](#101-introduction-to-the-asdm)
  - [ASA VPN connection](#asa-vpn-connection)

# Chapter 10: Advanced Cisco Adaptive Security Appliance


## 10.1 Introduction to the ASDM

ASA can be configured using:
- CLI
- GUI - ASA Security Device Manage(`ASDM`)

ASA works with `SSL` to ensure secure communication. It also provides quick-configuration wizards and logging and monitoring functionality that is not available using the CLI.

Preparing ASA for ASDM

```
ASA(config)# configure factory-default
ASA(config)# write erase

ASA(config)# interface eth0/0
ASA(config-if)# ip address 192.168.1.1 255.255.255.0
ASA(config-if)# no shutdown

ASA(config)# http server enable
ASA(config)# http 192.168.1.3 255.255.255.255 inside

ASA(config)# clear configure http
```

Home page dashboard - provides a quick view of the operational status of ASA, it is divided into:
- device dashboard - status of interfaces, OS version, licensing information and performance related information,
- firewall dashboard - it provides security related information about traffic that passes through the ASA, such as connection statistics, dropped packets, scan, and SYN attack detection,
- intrusion prevention - appears only if an IPS module or card is installed, it displays status information about the IPS software,
- content security - appears only if a CSC-SSM module is installed in the ASA, it displays status information about the CSC-SSM software

ASDM page elements:
- menu bar
- toolbar
- device list button
- status bar

ASDM views:
- home
- configuration
  - `device setup` - this tab can be used to configure the hostname, passwords, system time, interface settings and routing
  - firewall
  - remote acccess VPN
  - site-to-site VPN
  - `device management` - this tab can be used to configure a variety of features including management access, users and AAA access, DHCP, and more. Specifically, this tab can be used to configure basic management features including legal notification and to create a master passphrase
- monitoring
  - interfaces
  - VPN
  - routing
  - properties
  - logging

Cisco ASDM offers several wizards to help simplify the configuration of the appliance:
- `startup wizard` - guides the administrator through the initial configuration of the ASA and helps to define basic settings,
- `VPN wizards` - allows an administrator to configure:
  - Site-to-site VPN,
  - AnyConnect VPN,
  - Clientless SSL VPN,
  - IPsec(IKEv1) remote access VPN
- `High Availability and Scalability wizard` - used to configure failover with high availability and VPN cluster load balancing
- `Unified Communication wizard` - used to configure the ASA to support the Cisco Unified Communications Proxy feature. It generates configuration settings including ACLs, NAT/PAT statements, self-signed certificates, TLS proxies, and application inspection 
- `ASDM Identity Certificate wizard` - when using current Java versions, the ASDM Launcher requires a trusted certificate. This wizard creates a self-signed identity certificate and configures the ASA to use it when establishing an SSL connection
- `packet capture wizard` - useful to configure and run captures for troubleshooting errors including validating a NAT policy
 

`Configuration > Device Setup > Device Name/Password` - to configure the ASA hostname, domain name, and enable password:

`Configuration > Device Management > Advanced > Master Passphrase` - to configure a master passphrase and encrypt all passwords(AES encryption)

`Configuration > Device Management > Management Access > Command Line (CLI) > Banner` - to configure legal notification(banner motd)

`Configuration > Device Setup > Interfaces` - to configure layer 3 interfaces

`Configuration > Device Setup > System Time > Clock` - to change the system time

`Configuration > Device Setup > Routing` - to change routing settings

`Configuration > Device Management > Management Access > ASDM/HTTPS/Telnet/SSH` - to configure management access for Telnet and SSH services

`Configuration > Device Management > DHCP > DHCP Server` - to enable and configure DHCP server services

`Configuration > Firewall > Objects > Network Objects/Groups` - to configure a network object or a network object group

`Configuration > Firewall > Objects > Service Objects/Groups` - to configure service objects, service object groups, ICMP object groups, or protocol object groups

`Configuration > Firewall > Access Rules` - to configure ACLs

`Configurations > Firewall > Objects > Network Objects/Groups` - dynamic, static NAT & PAT

`Configuration > Device Management > Users/AAA > User Accounts` - AAA

`Configuration > Firewall > Service Policy Rules` - service policy rules

## ASA VPN connection

Types of VPN:
- `Site-to-Site` - create a secure LAN-to-LAN connection,
- `Remote Access` - create a secure single-user-to-LAN connection

ASA VPN types:
- `Site-to-Site`
- `AnyConnect`
- `Clientless SSL`
- `IPsec(IKEv1) Remote Access`

`Wizards > VPN Wizards` - start point in configuring all types of VPN supported by ASA

Site-to-site configuration on ISR:
1. Configure the ISAKMP policy for IKE Phase 1
2. Configure the IPsec Policy for IKE Phase 2
3. Configure an ACL to define interesting traffic
4. Configure a crypto map for the IPsec policy
5. Apply the crypto map to the outgoing interface

Site-to-site configuration on ASA:
1. Launch the Site-to-Site VPN wizard
2. Identify the peer device in the Peer Device Identification window
3. Identify interesting traffic in the Traffic to Protect window
4. Secure the selected traffic in the Security window:
   - simple Configuration - Uses a pre-shared keyword to use when authenticating with the identified peer. It selects common IKE and ISAKMP security parameters to establish the tunnel
   - customized Configuration - Uses either a pre-shared key or a digital certificate to authenticate with the identified peer. The IKE and ISAKMP security parameters can also be specifically selected
5. Determine whether NAT should be exempted in the NAT Exempt window
6. Verify and commit the configuration.

`Configuration > Site-to-Site VPN > Connection Profiles` - verify and edit the site-to-site VPN

>When security is an issue, IPsec is the superior choice. If support and ease of deployment are the primary issues, consider SSL.

Internet Protocol Security(`IPsec`) VPN is a layer 3 VPN technology and is the conventional teleworker remote-access solution. However, it requires a VPN client such as `Cisco AnyConnect to be pre-installed on the host`. It supports all types of applications and provides superior encryption and authentication strength and overall security.

Secure Socket Layer(`SSL`) VPN is a layer 7 VPN technology created by Netscape in the mid 1990s that was designed to enable secure communications over the Internet using a web browser. SSL does not require any pre-installed VPN software but instead allows users to access web pages, services and files. With SSL, users can send and receive email and run TCP-based applications using a browser.

SSL/TLS session establishment:
1. Client and server negotiate authentication, encryption and key exchange settings
2. Server sends its certificate to client
3. Client sends its certificate to the server. A session key is calculated and the encryption algorithms are activated
4. Data transfer begins with exchange of session keys. The server sends a session ID to the client, which allows the server to track the session and quickly resume the session with the client, if needed

<div align='center'>

|  | IPsec | SSL |
|---        |:---:        |:---:        |
|Application supported| `Extensive` - all IP-based applications are supported | `Limited` - only web-based applications and file sharing are supported |
|Authentication strength| `Strong` - using two-way authentication with shared keys or digital certificates | `Moderate` - using one-way or two-way authentication |
|Encryption| `Strong` - with key lengths from 56 bits to 256 bits | `Moderate to strong` - with key lengths from 40 bits to 256 bits |
|Connection complexity| `Medium` - because it requires a VPN client pre-installed on the host | `Low` - it only requires a web browser on a host |
|Connection option| `Limited` - only specific devices with specific configurations can connect | `Extensive` - any device with a web browser can conncet |

</div>

>It is important to understand that IPsec and SSL VPNs are not mutually exclusive. Instead, they are complementary; both technologies solve different problems, and an organization may implement IPsec, SSL, or both, depending on the needs of its telecommuters.

`IKEv1` is implemented when connecting to older VPN clients such as the `Cisco VPN Client`. With IKEv1, only one encryption and authentication type can be configured per security policy.

`IKEv2` is implemented for newer VPN clients such as the `Cisco AnyConnect Secure Mobility Client`. With IKEv2, it is possible to configure multiple encryption and authentication types, and multiple integrity algorithms for a single policy.

`Clientless SSL VPN` deployment model enables corporations to have the additional flexibility of providing access to corporate resources even when the remote device is not corporately managed. It lets users establish a secure, remote-access VPN tunnel to the ASA using a web browser.

`Client-based SSL VPN` solution provides full tunnel SSL VPN connection but requires a VPN client application to be installed on the remote host(Cisco AnyConnect). he client-based SSL VPN deployment model provides authenticated users with LAN-like, full network access to corporate resources(MS Outlook, Telnet, SSH, X-Window)

>During the establishment phase, the AnyConnect client has the ability perform an endpoint posture assessment by identifying the operating system, antivirus, antispyware, and firewall software installed on the host prior to creating a remote access connection to the ASA.

Clientless SSL VPN:
- ASDM Assistant
- VPN Wizard

`Configurations > Remote Access VPN > Introduction > Clientless SSL VPN Remote Access (using Web Browser)`

Clientless SSL VPN:
1. Launch the Clientless SSL VPN wizard
2. Configure the SSL VPN interface
3. Configure user authentication
4. Create a group policy
5. Configure a bookmark list for clientless connections only
6. Verify and commit the configuration.

>A bookmark list is a set of URLs that is configured to be used in the clientless SSL VPN web portal.

`Configuration > Remote Access VPN > Clientless SSL VPN Access > Connection Profiles` - use to verify clientless SSL VPN 


SSL VPN AnyConnect:
- ASDM Assistant
- VPN Wizard

`Configurations > Remote-Access VPN > Introduction > SSL or IPsec(IKEv2) VPN Remote Access (using Cisco AnyConnect Client)`

To download Cisco AnyConnect we can make clientless SSL VPN and then we can download AnyConnect.

AnyConnect SSL VPN:
1. Launch the AnyConnect VPN Wizard
2. Configure a connection profile in the Connection Profile Identification window
3. Select the VPN protocols
4. Add the AnyConnect client images in the Client Images window
5. Configure the authentication methods in the Authentication Methods window
6. Create and assign the client IP address pool in the Client Address Management window
7. Specify the DNS-related information in the Network Name Resolution Servers window
8. Enable NAT exemption for VPN traffic in the NAT Exempt window
9. The AnyConnect Client Deployment window
10. Verify and commit the configuration

`Configurations > Remote Access VPN > Network (Client) Access > AnyConnect Connection Profiles` - verify AnyConnect

----

<div>
<a href="chapter-09.md">Prev: Chapter 9</a>
</div>
<div align="right">
<a href="chapter-11.md">Next: Chapter 11</a>
</div>