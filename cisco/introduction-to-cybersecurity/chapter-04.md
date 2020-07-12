### Spis treści
- [Chapter 4: Protecting the Organization](#chapter-4-protecting-the-organization)
  - [4.1 Firewalls](#41-firewalls)
  - [4.2 Behavior approach to cybersecurity](#42-behavior-approach-to-cybersecurity)
  - [4.3 Cisco approach to cybersecurity](#43-cisco-approach-to-cybersecurity)

# Chapter 4: Protecting the Organization

## 4.1 Firewalls

`Firewall` - software or hardware that is designed to control or filter, which communication is allowed in and out of device. Firewall can be installed on single computer and it is called `host-based firewall` or it can be a stand-alone network device that protects all host devices in network, called `network-based firewall`.

Common firewall types:
- `network layer` - filtering based on source and destiantion IP addresses
- `transport layer` - filtering based on source and destiantion ports and filtering based on connection states
- `application layer` - filtering based on application, program or service
- `context aware application` - filtering based on the user, device, role, application type and threat profile
- `proxy server` - based on filtering web content requests like URL, domain, media
- `reverse proxy server` - placed in front of web server to hide, protect, offlad and distribute access to web servers
- `network address translation (NAT) firewall` - hides or masquarades the private address of network hosts
- `host-based` - filtering applications, ports and system calls on single computer OS 

`Port scanning` - process of probing a host for open ports.

The most popular port scanner is Nmap. When we scan for open ports generally we can obtian one of three responses:
- `open` - accepted, host replied indicating a service is listening on the port
- `closed` - denied, not listening, host replied indicating that connections will be denied to that port
- `filtered` - dropped or blocked, host does not replay

>In networking, each application running on a device is assigned an identifier called a `port number`. This port number is used on both ends of the transmission so that the right data is passed to the correct application.

Security Applicances categories:
- `routers` - may have firewall and IPS capabilities, additional encryption and VPN tunelling
- `firewalls` - this devices has got router capabilities, advanced network manager and analytics
- `IPS` - Intrusin Prevention System, devices dedicated to intrusion prevention
- `VPN` - designed to secure, encrypted tunneling
- `malware/antivirus`
- `other security devices` - web and email security applicances, decryption devices, client access control servers, and sercurity management systems

`Zero-day attack` is when an attacker exploits a security flaw in software before vendor/creator can fix it.

Detecting attacks in real time:
- `real time scanning from edge to endpoint` - include actively scanning using firewall, IPS/IDS network devices, next generation malware detection with connection to global threat centers
- `DDoS attacks and real time response` - DDoS is extremely difficult to defend against because the attacks originate from thousands of zombie hosts, and it is very hard to decide which is legitimate traffic. Ability to detect and respond to DDoS attack in real-time is crucial

`AMP` - Advanced Malware Protection is client/server software deployed on host endpoints, as a standalone server, or on other network security devices.

>Cisco has an Advanced Malware Protection (`AMP`) Threat Grid that analyzes millions of files and correlates them against hundreds of millions of other analyzed malware artifacts. This provides a global view of malware attacks, campaigns, and their distribution.

Security Best Practises:
- perform risk assessment
- create security policy
- physical security measures - restrict physical access
- human resource security measures - background check
- perform and test backups
- maintain security patches and updates
- employ access controls
- regulary test incident response
- implement network monitoring, analytics and management tool
- implement network security devices
- imlement a comprehensive endpoint security solution
- educate users
- encrypt data

`NIST` - National Institute of Standards and Technology Computer Security Resource Center has the most helpful guidelines.

One of the most widely known and respected organization for cybersecurity training is the `SANS Institute`.

## 4.2 Behavior approach to cybersecurity

`Botnet` - group of host, called zombies infected by malware and controlled by attacker command and control server.

The kill chain - stages of an information systems attack, developed by Lockheed Martin.
It consist of fillowing stages:
1. `Reconnaissance` - gathering information about the target
2. `Weaponization` - creating an exploit and malicous payload to send to the target
3. `Delivery` - sending exploit by email or other method
4. `Exploitation` - the exploit is executed
5. `Installation` - malware and backdoors are installed on the target
6. `Command and Control` - remote control of the target is gained through a command and control channel or server
7. `Action` - performing mailcious action slike information theft, or executes additional attacks on other devices which belongs to the same network as compromised system. New round of kill chain for every newly attacked machine.

>`Behavior-based` security is a form of threat detection that does not rely on known malicious signatures, but instead uses informational context to detect anomalies in the network. Behavior-based detection involves capturing and analyzing the flow of communication between a user on the local network and a local, or remote destination. These communications, when captured and analyzed, reveal context and patterns of behavior which can be used to `detect anomalies`. Behavior-based detection can discover the presence of an attack by a `change from normal behavior`.

>`Honeypots` - is a behavior-based detection tool that first lures the attacker in by appealing to the attacker’s predicted pattern of malicious behavior, and then, when inside the honeypot, the network administrator can capture, log, and analyze the attacker’s behavior. This allows an administrator to gain more knowledge and build a better defense.

`NetFlow` - technology used to gather information about data flowing through a network, it shows you who and what devices are in your network. It also show when and hw users and devices accessed your network, it is an important component in behavior-based detection and analysis. Network intermediary devices sucha as routers, switches and firewalls equipped with NetFlow can report information about data entering, leaving and trvalling through the network.

>NetFlow is able to collect information on usage through many different characteristics of how data is moved through the network. By collecting information about network data flows, NetFlow is able to establish baseline behaviors on more than 90 different attributes.

## 4.3 Cisco approach to cybersecurity

`CSIRT` - Computer Security Incident Response Team, is a part of large organizations and it is responsible to receive, review and respond to computer security incidents. The primary mission of CSIRT is to help esure company, system and data preservation  by performing comprehensive investigations into computer incidents. They are also responsible for preventing security incidents so they provices threat assessment, mitigation, planning, incident trend analysis and security architecture review. 

`Security playbook` - collection of repeatable queries(reports, steps) against security event data sources that lead to incident detection and response.

Ideally the security playbook must accomplish the following actions:
- Detect malware infected machines
- Detect suspicious network activity
- Detect irregular authentication attempts
- Describe and understand inbound and outbound traffic
- Provide summary information including trends, statistics, and counts
- Provide usable and quick access to statistics and metrics
- Correlate events across all relevant data sources

Tools for incident prevention and detection:
- `SIEM` - Secuirty Information and Event Management system is software that collects and analyzes security alerts, logs and other real time and historical data from many devices on the network
- `DLP` - Data Leak Prevention Software is a software or hardware system designed to stop sensitive data from being stolen from or escaping a network. This system may focus on file access authorization, data exchange, coping, user activity monitoring.
- `IDS` - Intrusion Detection System, software or hardware that scans data against a database of rules or attack signatures, looking for malicous traffic. Detection of match will be logged and alert will be created to inform network administrator. IDS do not take any action so it does not prevent from happening
  >The scanning performed by the IDS slows down the network (known as latency). To prevent against network delay, an IDS is usually placed offline, separate from regular network traffic. Data is copied or mirrored by a switch and then forwarded to the IDS for offline detection.

- `IPS` - Intrusion Prevention System is similar to IDS but has the ability to block or deny traffic based on positive rule or signature match.

>One of the most well-known IPS/IDS systems is `Snort`.

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>