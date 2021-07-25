### Spis treści
- [Chapter 5: Netowork security concepts](#chapter-5-netowork-security-concepts)
  - [5.1 Current state of cybersecurity](#51-current-state-of-cybersecurity)
  - [5.2 Threat actor tools](#52-threat-actor-tools)
  - [5.3 Malware](#53-malware)
  - [5.4 Common network attacks](#54-common-network-attacks)
  - [5.5 IP vulnerabilities and threats](#55-ip-vulnerabilities-and-threats)
  - [5.6 TCP and UDP vulnerabilities](#56-tcp-and-udp-vulnerabilities)
  - [5.7 IP services](#57-ip-services)
  - [5.8 Network security best practises](#58-network-security-best-practises)
  - [5.9 Cryptography](#59-cryptography)

# Chapter 5: Netowork security concepts

ENSA - Enterprise Networking, Security and Automation

## 5.1 Current state of cybersecurity

`Assets` - anything of value to the organization. It includes people, equipment, resources, and data.

`Vulnerability` - weakness in a system, or its design, that could be exploited by a threat.

`Threat` - potential danger to a company’s assets, data, or network functionality.

`Exploit` - mechanism that takes advantage of a vulnerability.

`Mitigation` - counter-measure that reduces the likelihood or severity of a potential threat or risk.

`Risk` - likelihood of a threat to exploit the vulnerability of an asset, with the aim of negatively affecting an organization. Risk is measured using the probability of the occurrence of an event and its consequences.

`Attack vector` - path by which a threat actor can gain access to a server, host, or network. 

>A DoS attack occurs when a network device or application is incapacitated and no longer capable of supporting requests from legitimate users.

Internal threats have the potential to cause greater damage than external threats because internal users have direct access to the building and its infrastructure devices. Employees may also have knowledge of the corporate network, its resources, and its confidential data.

Data loss or data exfiltration is when data is intentionally or unintentionally lost, stolen, or leaked to the outside world. The data loss can result in:
- brand damage and loss of reputation,
- loss of competitive advantage,
- loss of customers,
- loss of revenue,
- litigation/legal action resulting in fines and civil penalties,
- significant cost and effort to notify affected parties and recover from the breach

Common data loss vectors:
- email/social networking,
- unencrypted devices,
- cloud storage devices,
- removable media,
- hard copy,
- improper access control

Hacing term:
- script kiddies,
- vulnerability broker,
- hacktivists,
- cyber criminals,
- state-sponsored

>In 1985, attacks were not very sophisticated and required a lot of technical knowledge. As time passed, the sophistication of attack grew and the required technical knowledge diminished.

## 5.2 Threat actor tools

Penetration testing tool:
- password crackers,
- wireless hacking tools,
- network scanning and hacking tools,
- packet crafting tools,
- packet sniffers,
- rootkit detectors,
- fuzzers to search vulnerabilities,
- forensic tools,
- debuggers,
- hacking OS,
- encryption tools,
- vulnerability exploitation tools,
- vulnerability scanners

Attack types:
- eavesdropping, sniffing, snooping,
- data modification,
- IP address spoofing,
- password-based,
- denial of service,
- man-in-the-middle,
- compromised-key
- sniffer

## 5.3 Malware

Types of malware:
- viruses:
  - boot sector, file partition, file system,
  - firmware,
  - macro - MS Office,
  - program
  - script
- trojan horses:
  - remote-access,
  - data-sending,
  - destructive
  - proxy - source device to lauch attacks and perform other illegal activities
  - FTP
  - secuirty software disabler(antivirus, firewalls),
  - DoS,
  - keylogger
- adware,
- ransomware,
- rootkit - gain administrator account-level access to a computer,
- spyware,
- worm

Viruses can:
- alter, corrupt, delete files, or erase entire drives,
- cause computer booting issues, and corrupt applications,
- capture and send sensitive information to threat actors,
- access and use email accounts to spread,
- lay dormant until summoned by the threat actor.

Trojan horse is a program that looks useful but also carries malicious code. Trojan horses are often provided with free online programs such as computer games. Unsuspecting users download and install the game, along with the Trojan horse.

## 5.4 Common network attacks

Networks are susceptible to the following types of attacks:
- reconnaissance - gathering information, threat actors use reconnaissance (or recon) attacks to do unauthorized discovery and mapping of systems, services, or vulnerabilities,
  - perform an information query of a target,
  - initiate a ping sweep of the target network,
  - initiate a port scan of active IP addresses,
  - run vulnerability scanners,
  - run exploitation tools
- access,
  - password attack,
  - spoofing(IP, MAC, DHCP),
  - trust exploitation - use unauthorized privileges to gain access to a system, possibly compromising the targe,
  - port redirections - SSH tunneling,
  - man-in-the-middle attacks,
  - buffer overflow,
  - social engineering attack - attempts to manipulate individuals into performing actions or divulging confidential information. Types of social engineering attacks:
    - `pretexting` - threat actor pretends to need personal or financial data to confirm the identity of the recipient.
    - `phishing` - sends fraudulent email which is disguised as being from a legitimate, trusted source to trick the recipient into installing malware on their device, or to share personal or financial information.
    - `spear phishing` - creates a targeted phishing attack tailored for a specific individual or organization.
    - `spam` - junk mail, this is unsolicited email which often contains harmful links, malware, or deceptive content.
    - `something for something` - sometimes called “Quid pro quo”, this is when a threat actor requests personal information from a party in exchange for something such as a gift.
    - `baiting` - eaves a malware infected flash drive in a public location. A victim finds the drive and unsuspectingly inserts it into their laptop, unintentionally installing malware
    - `impersonation` - pretends to be someone they are not to gain the trust of a victim
    - `tailgating` - quickly follows an authorized person into a secure location to gain access to a secure area
    - `shoulder surfing` - inconspicuously looks over someone’s shoulder to steal their passwords or other information
    - `dumpster diving` - rummages through trash bins to discover confidential documents
- DoS/DDoS:
  - `overwhelming quatity of traffic` - threat actor sends an enormous quantity of data at a rate that the network, host, or application cannot handle. This causes transmission and response times to slow down. It can also crash a device or service,
  - `maliciously formatted packets` - sends a maliciously formatted packet to a host or application and the receiver is unable to handle it

Protecting against social engineering attacks:
- never give your username/password credentials to anyone,
- never leave your credentials where they can easily be found,
- never open emails from untrusted sources,
- never release information on social media sites,
- never re-use work related passwords,
- always lock or sign out of your computer when unattended,
- always report suspicious individuals,
- always destroy confidential information according to the organization policy

## 5.5 IP vulnerabilities and threats

>IP does not validate whether the source IP address contained in a packet actually came from that source. For this reason, threat actors can send packets using a spoofed source IP address. 

IP related attacks:
- ICMP - use to discover subnets and hosts, DoS flood, altering host routing tables
  - `ICMP echo request and echo reply` - used to perform host verification and DoS,
  - `ICMP unreachable` - network reconnaissance and scanning,
  - `ICMP mask reply` - map an internal IP network,
  - `ICMP redirects` - lure a target host into sending all traffic through a compromised device and create a MITM attack,
  - `ICMP router discovery` - inject bogus route entries into the routing table of a target host
- amplification and reflection - prevent legitimate users from accessing information or services using DoS and DDoS attacks(eg. Smurf attack)
  - `Amplification` – the threat actor forwards ICMP echo request messages to many hosts. These messages contain the source IP address of the victim(ex. NTP amplification),
  - `Reflection` – these hosts all reply to the spoofed IP address of the victim to overwhelm it(ex. DNS reflection),
- address spoofing - spoof the source IP address in an IP packet to perform blind spoofing or non-blind spoofing
  - `non-blind` - threat actor can see the traffic that is being sent between the host and the target. Non-blind spoofing determines the state of a firewall and sequence-number prediction. It can also hijack an authorized session.
  - `blind` - threat actor cannot see the traffic that is being sent between the host and the target
- MITM - position themselves between a source and destination to transparently monitor, capture, and control the communication. They could eavesdrop by inspecting captured packets, or alter packets and forward them to their original destination
- session hijacking - gain access to the physical network, and then use an MITM attack to hijack a session

>MAC address spoofing attacks are used when threat actors have access to the internal network.

## 5.6 TCP and UDP vulnerabilities

TCP Segment control bits:
- `URG` - urgent pointer field significant,
- `ACK` - acknowledgment field significant,
- `PSH` - push function,
- `RST` - reset the connection,
- `SYN` - synchronize sequence numbers,
- `FIN` - no more data from sender

TCP provides:
- `reliable delivery` - ack is a guarantee of delivery,
- `flow control` - ack of multiple segments,
- `stateful communication` - three way handshake

Three way handshake:
1. SYN,
2. SYN, ACK,
3. ACK

Terminating a TCP Connection:
1. FIN,
2. ACK,
3. FIN,
4. ACK

TCP attacks:
- discovery,
- TCP SYN Flood - exploits TCP Three-way handshake by sending TCP SYN sessions request packets with randomly spoofed source IP,
- TCP reset attack - sending TCP RST,
- TCP Session Hijacking - difficult to conduct, a threat actor takes over an already-authenticated host as it communicates with the target. The threat actor must spoof the IP address of one host, predict the next sequence number, and send an ACK to the other host. If successful, the threat actor could send, but not receive, data from the target device.

>TCP connection terminates when it receives an RST bit. This is an abrupt way to tear down the TCP connection and inform the receiving host to immediately stop using the TCP connection.

>UDP is a connectionless transport layer protocol. It has much lower overhead than TCP because it is not connection-oriented and does not offer the sophisticated retransmission, sequencing, and flow control mechanisms that provide reliability.

>The low overhead of UDP makes it very desirable for protocols that make simple request and reply transactions.

UDP attacks:
- UDP Flood

## 5.7 IP services

Hosts broadcast an ARP Request to other hosts on the segment to determine the MAC address of a host with a particular IP address.

>Any client can send an unsolicited ARP Reply called a “gratuitous ARP”(ping to victim). This is often done when a device first boots up to inform all other devices on the local network of the new device’s MAC address. When a host sends a gratuitous ARP, other hosts on the subnet store the MAC address and IP address contained in the gratuitous ARP in their ARP tables.

ARP attacks:
- ARP Cache Poisoning
  - `passive` - steal confidential information,
  - `active` - modify data in transit, or inject malicious data

DNS defines an automated service that matches resource names, with the required numeric network address, such as the IPv4 or IPv6 address. It includes the format for queries, responses, and data and uses resource records (RR) to identify the type of DNS response.

DNS attacks:
- DNS open resolver - answers queries from clients outside of its administrative domain(8.8.8.8)
  - `cache poisoning` - send spoofed, falsified record resource (RR) information to a DNS resolver to redirect users from legitimate sites to malicious sites.
  - `amplification and reflection` - DoS or DDoS attacks on DNS open resolvers to increase the volume of attacks and to hide the true source of an attack. Threat actors send DNS messages to the open resolvers using the IP address of a target host. These attacks are possible because the open resolver will respond to queries from anyone asking a question.
  - `resource utilization` - DoS attack that consumes the resources of the DNS open resolvers. This DoS attack consumes all the available resources to negatively affect the operations of the DNS open resolver. The impact of this DoS attack may require the DNS open resolver to be rebooted or services to be stopped and restarted.
- DNS stealth
  - `fast flux` - threat actors use this technique to hide their phishing and malware delivery sites behind a `quickly-changing` network of compromised DNS hosts. The DNS IP addresses are continuously changed within minutes. Botnets often employ Fast Flux techniques to effectively hide malicious servers from being detected.
  - `double IP flux` - threat actors use this technique to rapidly change the hostname to IP address mappings and to `also change the authoritative name server`. This increases the difficulty of identifying the source of the attack.
  - `Domain Generation Algorithms` - technique in malware to randomly generate domain names that can then be used as rendezvous points to their command and control (C&C) servers.
- DNS domain shadowing - gathering domain account credentials in order to silently create multiple sub-domains to be used during the attacks. These subdomains typically point to malicious servers without alerting the actual owner of the parent domain.
- DNS tunelling - place non-DNS traffic within DNS traffic. This method often circumvents security solutions when a threat actor wishes to communicate with bots inside a protected network, or exfiltrate data from the organization, such as a password database. 

DNS tunneling works for CnC commands sent to a botnet:
1. The command data is split into multiple encoded chunks.
2. Each chunk is placed into a lower level domain name label of the DNS query.
3. Because there is no response from the local or networked DNS for the query, the request is sent to the ISP’s recursive DNS servers.
4. The recursive DNS service will forward the query to the threat actor’s authoritative name server.
5. The process is repeated until all the queries containing the chunks of are sent.
6. When the threat actor’s authoritative name server receives the DNS queries from the infected devices, it sends responses for each DNS query, which contain the encapsulated, encoded CnC commands.
7. The malware on the compromised host recombines the chunks and executes the commands hidden within the DNS record.

>To stop DNS tunneling, the network administrator must use a filter that inspects DNS traffic.

DHCP attacks:
- `DHCP Spoofing` - occurs when a rouge DHCP server is connected to the network and provides false IP configuration parameters to legitimate clients. A rouge server can provide a variety of misleading information(wrong default gateway, DNS server, IP address),

## 5.8 Network security best practises

CIA triad:
- `confidentiality` – Only authorized individuals, entities, or processes can access sensitive information. It may require using cryptographic encryption algorithms such as AES to encrypt and decrypt data.
- `integrity` – Refers to protecting data from unauthorized alteration. It requires the use of cryptographic hashing algorithms such as SHA.
- `availability` – Authorized users must have uninterrupted access to important resources and data. It requires implementing redundant services, gateways, and links.

The defense-in-depth approach - layered approach, it reqiures a combination of networking devices and services working together

Security devices implemented to protect an organization against threats:
- VPN,
- ASA Firewall - enforce an access control policy between networks:
  - resistant to network attacks,
  - only transit points between corporate network and external networks because all traffic flows through the firewall,
  - enforce access policy,
  - `benefits`
    - prevent the exposure of sensitive hosts, resources and applications to untrusted users,
    - sanitize protocol flow, which prevents the exploitation of protocol flaws,
    - block mailicious data from servers and clients,
    - reduce security management complexity by off-loading most of the network access control to a few firewalls in the network
  - `limitations`
    - misconfigured firewall can have serious consequences for the network(SPOF),
    - the data from many applications cannot be passed through firewalls securely,
    - Users might proactively search for ways around the firewall to receive blocked material, which exposes the network to potential attack,
    - network performance can slow down,
    - unauthorized traffic can be tunneled or hidden so that it appears as legitimate traffic through the firewall.
- IPS,
- ESA/WSA - Email/Web Security Appliance,
- AAA Server

>IDS and IPS technologies detect patterns in network traffic using signatures. A signature is a set of rules that an IDS or IPS uses to detect malicious activity. Signatures can be used to detect severe breaches of security, to detect common network attacks, and to gather information. IDS and IPS technologies can detect atomic signature patterns (single-packet) or composite signature patterns (multi-packet).

>WSA can perform blacklisting of URLs, URL-filtering, malware scanning, URL categorization, web application filtering, and encryption and decryption of web traffic.

## 5.9 Cryptography

Elements of secure communications:
- `data integrity` - message was not altered(MD5, SHA)
- `origin authentication` - message is not a forgery and does actually come from whom it says. HMAC, uses an additional secret key as input to the hash function. Only the sender and the receiver know the secret key, and the output of the hash function now depends on the input data and the secret key.
- `data configdentiality` - only authorized users can read the message(symetric and asymetric encryption algorithms) 
- `data non-repudiation` - sender cannot repudile, or refute, the validity of a message sent

>Hashing is `vulnerable to man-in-the-middle attacks`(threat actor could intercept the message, change it, recalculate the hash, and append it to the message) and does not provide security to transmitted data. To provide integrity and origin authentication, something more is required

Protocols that use asymetric key agorithms include:
- `IKE` - Internet Key Exchange,
- `SSL` - Secure Socket Layer,
- `SSH` - Secure Shell,
- `PGP` - Pretty Good Privacy

>Because asymmetric algorithms are slow, they are typically `used in low-volume cryptographic mechanisms`, such as digital signatures and key exchange. However, the key management of asymmetric algorithms tends to be simpler than symmetric algorithms, because usually one of the two encryption or decryption keys can be made public.

Diffie-Hellman (`DH`) is an asymmetric mathematical algorithm where two computers generate an identical shared secret key without having communicated before. The new shared key is never actually exchanged between the sender and receiver. However, because both parties know it, the key can be used by an encryption algorithm to encrypt traffic between the two systems.

Examples of instances when DH is commonly used:
- data is exchanges using IPsec VPN,
- data is encrypted on the internet usign either SSL or TLS,
- SSH data is exchanged

---

<div>
<a href="srwe-chapter-13.md">Prev: SRWE 13</a>
</div>
<div align="right">
<a href="ensa-chapter-08.md">Next: ENSA 8</a>
</div>