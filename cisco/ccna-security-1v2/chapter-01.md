### Spis treści
- [Chapter 1: Modern Network Security Threats](#chapter-1-modern-network-security-threats)
  - [1.1 Securing Networks](#11-securing-networks)
  - [1.2 Network Topology Overview](#12-network-topology-overview)
  - [1.3 Network Threats](#13-network-threats)
  - [1.4 Mitigating Threats](#14-mitigating-threats)

# Chapter 1: Modern Network Security Threats

>Network security involves protocols, technologies, devices, tools, and techniques to secure data and mitigate threats. Network security solutions emerged in the 1960s but did not mature into a comprehensive set of solutions for modern networks until the 2000s. Network security is largely driven by the effort to stay one step ahead of ill-intentioned hackers. Network security professionals attempt to prevent potential attacks while minimizing the effects of real-time attacks

## 1.1 Securing Networks

Network sercurity policy - created to provide a framework for employees to follow during their day-to-day work

>Norse Dark Intelligence maintains an interactive display of `current network attacks on honeypot servers`.

`SIO` - Cisco Security Intelligence Operations, tool that provide alerts to network professionals regarding current network. It is a Cloud-based service that connects global threat information, reputation-based services, and sophisticated analysis, to Cisco network security devices.

`Vulnerability` - weakness or flaw in the network/system. Vulnerability can be exploited by an attacker to access confidential data or negatively impact a network. Examples of vulnerabilities: unsecure network protocols, configuration errors or weak security policies

`Threat` - potential for a vulnerability to turn into a network attack. Theats include malware, exploits and more

`Mitigation` - reducing the severity of the vulnerability

`Risk` - potential of threat to exploit the vulnerability. Risk is measured using the probability of the consequence of an event and its consequence

`Attack Vector` - path by which an attacker can gain access to server, host or network

Vectors of network attacks:
- `externat threat`
- `internat threat` - employee
  - steal/copy confidentional data,
  - compromise internal devices,
  - disconnect a critical network connection,
  - connect an infected USB

>Internal threats also have the potential to cause greater damage than external threats because internal users have direct access to the building and its infrastructure devices.

>A DoS attack occurs when a network is incapacitated and no longer capable of supporting requests from legitimate users.

`Data - the most valuable organization's asset`

The data loss can result in:
- brand damage and loss of reputation,
- loss of competitive advantage,
- loss of customers,
- loss of revenue,
- litigation/legal action resulting in fines and civil penalties,
- significant cost and effort to notify affected parties and recover from the breach

Common data loss vectors:
- `email/social networking`,
- `unencrypted` (stolen) `devices`,
- `cloud storage devices` - data can be lost if access to the cloud is compromised due to weak security settings
- `removable media` - unauthorized transfer of data to USB drive, or stole of USB drive with valuabe company data
- `hard copy` - data should be disposed of thoroughly. Confidential data should be shredded when no longer required
- `improper access control` - stolen passwords or weak passwords can provide easy access to confidential data

`DLP` - Data Loss Prevention

## 1.2 Network Topology Overview

>The main focus of this course is on securing Campus Area Networks (CANs). Campus Area Networks consists of interconnected LANs within a limited geographic area.

Campus network can consist of given security devices:
- `VPN` - ensure data confidentiality and integrity from authenticated sources
- `ASA Firewall` - Adaptive Security Appliance, perform stateful packet filtering (from outside in campus net)
- `ESA/WSA` - provide advanced threat defense, application visibility and control, reporting, control email and web traffic 
  - `ESA` - Email Security Appliance,
  - `WSA` - Web Security Appliance
- `IPS` - Intrusion Prevention System, device continuously monitors incoming and outgoing network traffic for malicious activity. It logs information about the activity and attempts to block and report it
- `AAA Server` - Authentication, authorization and accounting, it authenticates users, authorizes what they are allowed to do, and tracks what they are doing
- `Layer 3 Switches` - provide redundant trunk connections to L2 switches. L3 can implement security features, such as:
  - ACL
  - DHCP snooping
  - DAI - Dynamic ARP Inspection
  - IP source guard
- `Layer 2 Switches` - connect user ports to network. L2 can adopt security features such as:
  - port security,
  - DHCP snooping,
  - 802.1X user authentication
- `Hosts` - endpoints are secured using antivirus, antimalware software, host based IPS, 802.1X authentication features
  
SOHO - Small Office and Home Office NetworkL
- `Wireless Router` - consumer-grade wireless router provides integrated firewall features and secure wireless connections 
- `L2 Switch` - connect user-facing ports to LAN
- `Wireless Host`

WAN - Wide Area Networks:
- main site,
- branch site,
- regional site,
- SOHO site,
- mobile worker

Data Center Networks:
- outside perimeter security:
  - on-premise secuirty officers,
  - fences and gates,
  - continuous video surveillance,
  - security breach alarms,
  - gas-based fire suppression systems
  - redundant `HVAC` - Heating, Ventilation, and Air Conditioning
- inside perimeter security:
  - continuous video surveillance
  - electronic motion detectors,
  - security traps(mantrap),
  - biometrics access and exit sensors

>Cloud computing separates the application from the hardware. Virtualization separates the OS from the hardware.

VM specific threats:
- `hyperjacking` - hijack a VM hypervisor(VM controlling software)
- `instant on activation` - when offline VM is brought online it may have outdated security policies
- `antivirus storms` - happens when all VMs attempt to download antivirus data files at the same time

Secure Date Center:
- `secure segmentation` - ASA and Cisco Nexus(Virtual Security Gateway)
- `threat defense` - ASA and IPS use threat intelligence, passive OS fingerprinting, reputation and contextual analysis
- `visibility` - Cisco Security Manager helps to simplify operations and compliance reporting

`MDM` - Mobile Device Management, features secure, monitor, and manage mobile devices, including corporate-owned devices and employee-owned devices. Functions performed by MDM:
- data encryption
- pin enforcement
- data wipe
- DLP - Data Loss Prevention
- Jailbreak/Root Detection

## 1.3 Network Threats

Types of hacker:
- white hat hacker
- grey hat hacker
- black hat hacker

Modern hacker titles:
- `script kiddies` - inexperienced hackers that are using scirpts, tools, and exploits, to cause harm
- `vulnerability broker` - grey hat hacker that discover exploits and report them to vendors
- `hacktivists` - do not hack for profit, they hack for attention. They are usually politically or socially motivated cyber attackers who use the power of the Internet to promote their message.
- `cyber criminals` - black hat hackers with the motive to make money using any means necessary.
- `state-sponsored` - government-funded and guided attackers, ordered to launch operations that vary from cyber espionage to intellectual property theft.

>Over the years, attack tools have become more sophisticated, and highly automated, `requiring less technical knowledge` to use them than in the past.

Penetration testing tools:
- password crackers
- wireless hacking tools
- network scanning and hacking tools
- packet crafting tools
- packet sniffers
- rootkit detectors
- fuzzers to search vulnerablities
- forensic tools
- debuggers
- hacking operating systems
- encryption tools
- vulnerability exploitation tools
- vulnerability scanners

Categories of Attack Tools:
- Eavesdropping,
- Data Modification,
- IP Address Spoofing,
- Password-Based,
- Denial-of-Service,
- Man-in-the-Middle,
- Compromised-Key,
- Sniffer

Types of malware:
- `virus` - malicious software which executes a specific unwanted, and often harmful fucntion on a computer
- `worms` - executes arbitrary code and installs copiers of itself in the memory of the infected computer. The main purpose of worm is to automatically replicate itself and spread across the netowrk from system to system. Most worm attack consist of three concepts:
  - enabling vulnerability,
  - propagation mechanism,
  - payload
- `trojan horse` - non-self-repicating type of malware. It often contains malicious code that is designed to look like something else, such as legitime application or file. Classification of trojan horses:
  - `RAT` - Remote Access Trojan, enables unauthorized remote access
  - `Data-sending` - provides the attacker with sensitive data
  - `Destructive` - corrupts or deletes files
  - `Proxy` - use victim's computer as the source device to launch attacks and perform other illegal activities 
  - `FTP`,
  - `Security software disabler`
  - `DoS`
- `ransomware` - denies access to the infected computer system. The ransomware then demands a paid ransom for the restriction to be removed
- `spyware` - used to gather information about a user and send the information to another entity, without the user’s consent
- `adware` -  displays annoying pop-ups to generate revenue for its author
- `scareware` - includes scam software which uses social engineering to shock or induce anxiety by creating the perception of a threat
- `phishing` - attempts to convince people to divulge sensitive information
- `rootkits` - installed on a compromised system. After it is installed, it continues to hide its intrusion and maintain privileged access to the hacker.

Major types of network attack:
- `reconnaissance` - information gathering
  - informaion query of a target - Google, whois,
  - ping sweep fo the target network,
  - port scan of active IP addresses,
  - vulnerability scanning - Nipper, Secuna PSI, Core Impact, Nessus v6, SAINT, and Open VAS,
  - exploitation tools
- `access` - exploit known vulnerabilities in authentication services
  - password attack
  - trust exploitation
  - port redirection
  - Man-in-the-Middle
  - Buffer overflow
  - IP, MAC, DHCP Spoofing
  - `Social Engineering`
    - `Pretexting` - is when a hacker calls an individual and lies to them in an attempt to gain access to privileged data
    - `Phishing` - is when a malicious party sends a fraudulent email disguised as being from a legitimate, trusted source.
    - `Spear phishing` - targeted phishing attack tailored for a specific individual or organization
    - `Spam` - email to trick a user to click an infected link or download an infected file
    - `Tailgating` - is when a hacker quickly follows an authorized person into a secure location
    - `Something fot Something(Quid pro quo)` - hacker requests personal information from a party in exchange for something like a free gift
    - `Baiting` - is when a hacker leaves a malware-infected physical device, such as a USB flash drive in a public location such as a corporate washroom
- `DoS`
  - Maliciously Formatted Packets
    - Ping of Death
    - Smurf attack - ip direct-broadcast
    - TCP SYN Flood attck
  - Overwhelming Quantity of Traffic
- `DDoS`

`Botnet` - a network of infected machines. The compromised computers are called zombie computers, and they are controlled by handler systems.

## 1.4 Mitigating Threats

Network Security Professionals:
- `CIO` - Chief Information Officer
- `CISO` - Chief Information Security Officer
- `SecOps` - Security Operations Manager
- `CSO` - Chief Security Officer
- Security Manager
- Network Security Engineer

Network Security Organizations:
- `CERT` - Computer Emergency Response Team
- `SANS` - SysAdmin, Audit, Network, Security Institute
- `MITRE` - CVE
- `FIRST` - Forum of Incident Response and Security Teams
- `InfoSysSec` - Information Systems Security
- `(ISC)^2` - International Informational Systems Security Certification Consortium
- `MS-ISAC` - Multi-State Information Sharing & Analysis Center

Components of secure information:
- `Confidentiality` - hidding data using encryption 
- `Integrity` - use hashing algorithms to ensure that data is unaltered
- `Availability` - assures that data ins accessible

Domains of network security:
- risk assesment,
- secuirty policy - formal statement of the rules by which people that are given access to the technology and information assets of an organization, must abide. A security policy is a "living document", meaning that the document is regularly updated as technology, business, and employee requirements change.
- organization of information security,
- asset management,
- human resource security,
- physical and environmental security,
- communications and operations management,
- information systems acquisition,
- access control,
- information security incident management,
- business continuity management,
- compliance

Security Onion
Security Artichoke(karczoch, Borderless Network)

>In the 1990s, network security became an integral part of everyday operations.

Cisco SecureX product Families:
- Secure Edge and Branch
- Secure Email and Web
- Secure Mobility
- Secure Access
- Secure Data Center and Virtualization

Cisco SecureX Architecture:
- Scanning Engines
- Delivery Mechanisms
- Security Intelligence Operations
- Policy Management Consoles
- Next-Generation Endpoints

Context-aware policy parameters:
- person's identity,
- application in use,
- type of device being used for access,
- location,
- time of access

>Mitigation techniques are often referred to in the security community as “countermeasures”.

Mitigating malware:
- antivirus software

Worms mitigation:
- `Containment` - limiting the spread of worm infection to areasof the network that are already affected
- `Inoculation` - runs parallel to or subsquent to the containment phase. During this phase all uninfected systems are patched with the appropriate vendor patch
- `Quarantine` - tracking down and identifying infected machines within contained areas and disconnecting, blocking ro removing them
- `Treatment` - active disinfecting infected systems

Reconnaissance mitigation:
- implement authentication to ensure proper access
- use encryption
- use anti-sniffer tools to detect packet sniffers
- implement switched infrastructure
- use firewall and IPS

>`Principle of minimum trust` - systems should not use one another unnecessarily. For example, if an organization has a trusted server that is used by untrusted devices, such as web servers, the trusted server should not trust the untrusted devices unconditionally.

Access attack mitigation:
- use strong passwords,
- disable accounts after a specified number of unsuccessful logins has occurred,
- don't use a password based on a dictionary word,
- use principle of minimal trust,
- use encryption,
- educate employees about risks of social engineering
- review logs, bandwidth utilization and process loads

Mitigation DoS:
- use network utilization graph
- implement anti-spoofing techniques:
  - DHCP snooping,
  - IP Source Guard,
  - `DAI` - Dynamic ARP Inspection
  - ACL

NFP Framework - Network Foundation Protection, provides comprehensive guidelines for protecting the network infrastructure. It consist of three functional areas:
- `control plane` - responsible for routing data correctly, it includes: 
  - ARP message exchange,
  - OSPF routing advertisements
  
  Securing techniques:
  - routing protocol authentication,
  - CoPP - Control Plane Policing, Cisco IoS feature designed to allow users to control the flow of traffic that is handled by the route processor of a network
  - AutoSecure - can lock down management and forwarding plane

- `management plane` - responsible for managing network elements, it includes: 
  - Telnet, SSH,
  - TFTP, FTP,
  - SNMP, syslog, NetFlow,
  - TACACS+, RADIUS,
  - AAA,
  - NTP

  Securitng techniques:
  - login & password policy,
  - present legal notification,
  - ensure the confidentiality of data,
  - RBAC - Role-based access control,
  - authorize actions,
  - enable management access reporting
- `data plane` - forwarding plane is responsible for forwarding data

  Securing techniques:
  - blocking unwanted traffic or users,
  - reducing the chance of DoS attacks,
  - mitigating spoofing attacks,
  - providing bandwidth control,
  - classyfying traffic to protect the Management and Control planes

`OOB` - out-of-band management

`uRPF` - Unicast Reverse Path Forwarding

---

<div align="right">
<a href="chapter-02.md">Next: Chapter 2</a>
</div>
