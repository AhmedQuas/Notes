### Spis treści
- [Chapter 5: Implementing Intrusion Prevention](#chapter-5-implementing-intrusion-prevention)
  - [5.1 IPS technologies](#51-ips-technologies)
  - [5.2 IPS signatures](#52-ips-signatures)
  - [5.3 Implement IPS](#53-implement-ips)

# Chapter 5: Implementing Intrusion Prevention

## 5.1 IPS technologies

>A zero-day attack, is a computer attack that tries to exploit software vulnerabilities that are unknown or undisclosed by the software vendor. The term zero-hour describes the moment when the exploit is discovered. During the time it takes the software vendor to develop and release a patch, the network is vulnerable to these exploits.

`IDS` - Intrusion Detection Systems, devices implemented to passively monitor the traffic on a network. This device analyzes the copied traffic, it works offline:
- passively,
- traffic must be mirrored in order to reach IDS,
- can not stop malicious packets
- does not negatively affect the packet flow of the forwarded traffic(latency, jitter, sensor failures)
- requires assistance from other device(router, firewall) to respond to an attack

`IPS` - Intrusion Prevention System, device which is build upon IDS technology, implemented in inline mode - all traffic must flow through IPS, so this device can stop malicious packets. IPS use detection technologies such as:
- signature-based,
- profile-based,
- protocol-analysis-based

The biggest difference between IDS and IPS is that an IPS responds immediately and does not allow any malicious traffic to pass, whereas an IDS allows malicious traffic to pass before it is addressed.

Forms of IPS/IDS sensors:
- router configured with Cisco IOS IPS software,
- device specifically designed to provide dedicated IDS or IPS services,
- network module installed in an adaptive security appliance (ASA), switch, or router

Common characteristics of IDS & IPS:
- both technologies are deployed as sensors,
- both technologies use signatures to detect patterns of misuse in network traffic
- bothe can detect atomic patterns(single-packet) or composite patterns(multi-packet)

>A `signature` is a set of rules that an IDS or IPS uses to detect malicious activity.

>`Stream normalization` is a technique used to reconstruct the data stream when the attack occurs over multiple data segments. 

IPS sensors are inline so, they can use stream normalization.

Kinds of IPS:
- `HIPS` - host-based IPS, used to monitor and protect OS and critical system processes that are specific to that host. 
  
  >With detailed knowledge of the operating system, HIPS can monitor abnormal activity and prevent the host from executing commands that do not match typical behavior. This suspicious or malicious behavior might include unauthorized registry updates, changes to the system directory, executing installation programs, and activities that cause buffer overflows.

  >HIPS can be thought of as a combination of antivirus software, antimalware software, and firewall. 

- `NIPS` - network-based IPS, can be implemented using a dedicated or non-dedicated IPS device. It is used to monitor network traffic.
  
  Advantages:
  - is cost-effective,
  - not visible on the network,
  - OS indepemdent,
  - lower level network events seen

  Disadvantages:
  - cannot examine encrypted traffic,
  - cannot determine whether an attack was successful

IPS hardware includes mainly three components:
- NIC,
- Processor,
- Memory

>For larger volumes of traffic, Cisco IPS sensors can be implemented using `standalone` appliances or as modules added to network devices.

Factors that affect the IPS sensor selection and deployment:
- amount of network traffic,
- network topology,
- security budget,
- avaiable security staff to manage IPS

Modes of deployment:
- inline - inline interface pair mode, placed directly into traffic flow and makes packet-forwarding rates slower by adding latency

  >Inline mode allows the sensor to stop attacks by dropping malicious traffic before it reaches the intended target, thus providing a protective service.

- offline - promiscuous, passive mode, sensor analyzes a copy of monitored traffic

  >The response actions implemented by promiscuous sensor devices are post-event responses and often require assistance from other networking devices

>The `packet analyzer` (also known as a packet sniffer or traffic sniffer) is typically software that captures packets entering and exiting the network interface card (NIC). It is not always possible or desirable to have the packet analyzer on the device that is being monitored. Sometimes it is better on a separate station designated to capture the packets.

Traffic sniffing techniques:
- `hub` - when this device receives an Ethernet frame, the bits received on one port are sent out all other ports except the port that the frame came in on,
- `port mirroring` - feature that allows a switch to make a duplicate copy of an incoming Ethernet frame, and then send it out a port with a packet analyzer attached for capture. The original frame is forwarded in the usual manner,

`SPAN` - Switched Port Analyzer, feature that sends copies of the frame entering a port, out another port on the same switch.

>SPAN terminology includes several specific items:
>- `ingress traffic` - traffic that enters the switch,
>- `egress traffic` - traffic that leaves the switch,
>- `source` (SPAN) `port` - port that is monitored with use of the SPAN feature,
>- `destination` (SPAN) `port` - port that `monitors` source ports, usually where a packet analyzer or IDS is connected. The destination port is no longer a normal switch port. Only monitored traffic passes through that port.

Cisco SPAN configuration

```
S1(config)# monitor session NUMBER source [interface | vlan]
S1(config)# monitor session NUMBER destination [interface | vlan]

S1# show monitor
```

>Remote SPAN (`RSPAN`) can be used when the packet analyzer or IDS is on a different switch than the traffic being monitored. RSPAN extends SPAN by enabling remote monitoring of multiple switches across the network. The traffic for each RSPAN session is carried over a user-specified `RSPAN VLAN` that is dedicated (for that RSPAN session) in all participating switches.

## 5.2 IPS signatures

Signature is a set of rules that an IDS and an IPS use to detect typical intrusion activity. These signatures uniquely identify specific worms, viruses, protocol anomalies, or malicious traffic. A sensor takes action when it matches a signature with a data flow, such as logging the event or sending an alarm to the IDS or IPS management software.

Signatures attributes:
1. `type`:
    - atomic - single packet
    - composite - stateful, multi-packet
    
    >The length of time that the signatures must maintain state is known as the event horizon. 
    
    >Configuring the length of the event horizon is a trade-off between consuming system resources and being able to detect an attack that occurs over an extended period of time.

    `SME` - signature micro-engines used to categorize common signatures in groups. 

    Types of micro-engines:
    - `atomic` - examine simple packets(ICMP, UDP)
    - `service` - examine the many services that are attackerd
    - `string` - use regular expression-based patterns to detect intrusions
    - `multi-string` - supports flexible pattern matching and Trend Labs signatures
    - `other` - handles miscellaneous signatures

2. `trigger` - alarm, anything that can reliably signal an intrusion or security policy violation. Types of signature triggers:
   - `pattern-based detection`:
     - advantages: 
       - easy configuration,
       - fewer false positives,
       - good signature design
     - disadvantages:
       - no detection of unknow signatures,
       - initially a lot of false positives
       - signatures must be created, updated and tuned
   - `anomaly-based detection` - profile-based detection. The administrator simply defines a profile for normal activity. Any activity that deviates from this profile is considered abnormal and triggers a signature action
     - advantages:
       - simple and reliable,
       - customized policies
     - disadvantages:
       - generic output,
       - policy must be created
        
      >Another consideration is that the administrator must guarantee that the network is free of attack traffic during the learning phase.
   - `policy-based detection` - behavior-based detection, similar to pattern-based detection. However, instead of trying to define specific patterns, the administrator defines behaviors that are suspicious based on historical analysis
     - advantages:
       - easy configuration,
       - can detect unknown attacks
     - disadvantages:
       - difficult to profile typical activity in large networks,
       - traffic profile must be constant
   - `honey pot-based detection` - are rarely used in production environments. Antivirus and other security vendors tend to use them for research.
     - advantages:
       - window to view attacks,
       - distract and confuse attacker,
       - slow down and avert attackers,
       - collect information about attacks
     - disadvantages:
       - dedicated honey pot server,
       - honey pot server must not be trusted

3. `action`
   - `generate an alert` - produce verbose alert,
   - `log the activity` - log attacker and victim packets. Logging the actions or packets that are seen so that they can be analyzed later in more detail. The IPS device begins logging the traffic from the attacker’s IP address for a specified period of time or number of bytes, 
   - `drop or prevent the activity`
     - deny attacker inline - terminates current and future packets for a specified period of time
     - deny connection inline - terminates current TCP flow
     - deny packet inline - terminates only this packet

    >By dropping traffic for a connection or host, the IPS conserves resources without having to analyze each packet separately.

   - `reset a TCP connection` - basic action that can be used to terminate TCP connections by generating a packet for the connection with the TCP RST flag set,
   - `block future activity` - most IPS devices have the capability to block future traffic by having the IPS device update the ACLs on one of the infrastructure devices. The ACL stops traffic from an attacking system without requiring the IPS to consume resources analyzing the traffic. After a configured period of time, the IPS device removes the ACL
   - `allow the activity` - an administrator can define exceptions to configured signatures.

    >Configuring exceptions enables administrators to take a more restrictive approach to security because they can first deny everything and then allow only the activities that are needed

    >Suppose vulnerability scanning routine. This scanning causes the IPS to trigger various alerts. These are the same alerts that the IPS generates if an attacker scans the network. By allowing the alerts from the approved IT scanning host, an administrator can protect the network from intrusive scans while eliminating the false positives generated by the routine IT-approved scanning.

>Network security threats are occurring more frequently and spreading more quickly. As new threats are identified, new signatures must be created and uploaded to an IPS. To make this process easier, all signatures are contained in a signature file and uploaded to an IPS on a regular basis.

>To protect a network, the signature file must be updated regularly.

>Just as virus checkers must constantly update their virus database, network administrators must be vigilant and regularly update the IPS signature file

Alarm types:
- `false positive` alarm is an expected but undesired result. A false positive occurs when an intrusion system generates an alarm after processing normal user traffic that should not have triggered an alarm
- `false negative` is when an intrusion system fails to generate an alarm after processing attack traffic that the intrusion system is configured to detect. It is imperative that the intrusion system does not generate false negatives because that means that known attacks are not being detected
- `true positive` alarm describes a situation in which an intrusion system generates an alarm in response to known attack traffic
- `true negative` describes a situation in which normal network traffic does not generate an alarm

Types of alerts:
- `atomic` - alert is generated every time a signature triggers
- `summary` - single alert that indicates multiple occurrences of the same signature from the same source address or port. Additional instances of the same activity or duplicate alerts are counted until the end of the signature's summary interval.

Factors to consider when planning a monitoring strategy:
- `management method`,
  - individually,
  - centrally
- `event correlation` - process of correlating attacks and other events that are happening simultaneously at different points across a network(NTP & time stamps). Another factor that facilitates event correlation is deploying a centralized monitoring facility on a network.
- `security staff`,
- `incident response plan`

>Only by monitoring the security events on a network can an administrator accurately identify the attacks and security policy violations that are occurring.

GUI-based IPS device managers available:
- Cisco Configuration Professional,
- Cisco IPS Manager Express (IME),
- Cisco Security Manager

`SDEE` - Secure Device Event Exchange, protocol developed to improve communication of events generated by security devices.

```
%IPS-4-SIGNATURE:Sig:1107 Subsig:0 Sev:2 RFC1918 address [192.168.121.1:137 ->192.168.121.255:137]
```

IPS best practises:
1. Balance the need to upgrade sensors with the latest signature packs against the momentary downtime during which the network becomes vulnerable to attack.
2. Update signature packs automatically when setting up a large deployment of sensors, rather than manually upgrading each sensor. This gives security operations personnel more time to analyze events.
3. Download new signature packs to a secure server within the management network. Use another IPS to protect this server from attack by an outside party.
4. Place signature packs on a dedicated SFTP server within the management network. If a signature update is not available, a custom signature can be created to detect and mitigate a specific attack.
5. Configure the SFTP server to allow read-only access to the files within the directory on which the signature packs are placed.
6. Configure the sensors to regularly check the SFTP server for new signature packs. Stagger the time of day for each sensor to check the SFTP server for new signature packs, perhaps through a predetermined change window. This prevents multiple sensors from overwhelming the SFTP server by asking for the same file at the same time.
7. Keep the signature levels that are supported on the management console synchronized with the signature packs on the sensors.

`Cisco SensorBase Network` - centralized Cisco threat database which contains real-time, detailed information about known threats on the Internet.

Goals of global correlation:
- dealing intelligently with alerts to improve effectiveness,
- improving protection against know malicious sites,
- sharing telemetry data with SensorBase Network to improve visibility of laerts and sensor actions on a global scale,
- simplifying configuration settings,
- automatic handling of security information uploads and downloads

>The IPS sensor can be configured to participate in the global correlation updates and in sending telemetry data. Both services can also be turned off.

>Sensors installed at customer sites can also enable network participation, in which they send data to the SensorBase Network. This allows the SensorBase Network to collect nearly real-time data from sensors around the world. Communication between sensors and the SensorBase Network involves an HTTPS request and response over TCP/IP.

>Cisco SensorBase Network provides information to the IPS sensor about `IP addresses with a reputation`.

>SensorBase Network is part of a larger, back-end security ecosystem, known as the Cisco Security Intelligence Operation(`SIO`).

>The purpose of Cisco SIO is to detect threat activity, research and analyze threats, and provide real-time updates and best practices to keep organizations informed and protected. Cisco S

>SIO consists of three elements:
>- threat intelligence from the Cisco SensorBase Network
>- threat Operations Center, which is the combination of automated and human processing and analysis
>- automated and best practices content that is pushed to network elements in the form of dynamic updates

SensorBase Network participation gathers the following data:
- signature ID, version
- attacker IP address and port,
- MSS,
- victim IP address and port,
- TCP options string,
- reputation score,
- risk rating

>A reputation is based on a commonly held opinion. Reputations can be tarnished when there is a reason that causes others to become distrustful or suspicious. In security it applies to IP addresses, mail servers URLs and other entities. 

## 5.3 Implement IPS 

Steps to implement IPS in IOS:
1. Download the IOS IPS files.
   - IOS-Sxxx-CLI.pkg - the latest signature package
   - realm-cisco.pub.key.txt - the public crypto key used by IOS IPS
2. Create an IOS IPS configuration directory in flash.

    ```
    R1# mdkir DIR_NAME
    R1# rename OLD_NAME NEW_NAME
    R1# dir flash:
    ```

3. Configure an IOS IPS crypto key - copy commands from realm-cisco.pub.key.txt
4. Enable IOS IPS.
   - identify the IPS rule name and specify the location

    >An `optional extended or standard ACL` can be configured to filter the scanned traffic. All traffic that is permitted by the ACL is subject to inspection by the IPS. Traffic that is denied by the ACL is not inspected by the IPS.

    ```
    R1(config)# ip ips name IPS {list}
    R1(config)# ip ips config location flash:DIR_NAME
    ```

    - enable SDEE and logging event notification

    ```
    R1(config)# ip http
    R1(config)# ip ips notify sdee
    R1(config)# ip ips notify log
    ```

    - configure the signature category

    >Retiring a signature means that IOS IPS does not compile that signature into memory for scanning. Un-retiring a signature instructs IOS IPS to compile the signature into memory and use it to scan traffic. 

    ```
    R1(config)# ip ips signature-category
    R1(config-ips-category)# category all
    R1(config-ips-category-action)# retired true
    R1(config-ips-category-action)# exit
    R1(config-ips-category)# category IPS basic
    R1(config-ips-category-action)# retired false
    ```
    >CAUTION: Do not unretire the all category. The all signature category contains all signatures in a signature release. The IOS IPS cannot compile and use all the signatures at one time because it will run out of memory.

    - apply an IPS rule on interface

    ```
    R1(config)# interface g0/0
    R1(config-if)# ip ips IPS in

    R1(config)# interface g0/1
    R1(config-if)# ip ips IPS in
    R1(config-if)# ip ips IPS out
    ```

5. Load the IOS IPS signature package to the router.

    ```
    R1# copy tftp://IP_ADDR/SIG_PKG_FILE.pkg idconf
    R1# show ip ips signature count
    ```

    >The `idconf` parameter instructs the router that and IDConf configuration file is being copied.

>The IOS release 12.4(15)T4 or later, uses the newer 5.x format signature files.

>Only registered customers can download the package files and key.

If the key is configured incorrectly, the key must be removed and then reconfigured.

```
%IPS-3-INVALID_DIGITAL_SIGNATURE: Invalid Digital Signature found (key not found)

R1(config)# no crypto key pubkey-chain rsa
R1(config)# no named-key realm-cisco.pub signature

R1(config)# copy & paste
R1(config)# show run
```

Retire single signature

```
R1(config)# ip ips signature-definition
R1(config-ips-digdef)# signature 6130 10
R1(config-ips-digdef)# status
R1(config-ips-digdef-status)# retired true
```

>Some unretired signatures, either unretired as an individual signature or within an unretired category, might not compile because of insufficient memory, invalid parameters, or if the signature is obsolete.

Change signature actions

```
R1(config-sigdef-sig)# event-action ACTION
```

Event-action parameters:
- deny-attacker-inline,
- deny-connection-inline,
- deny-packet-inline,
- produce-alert,
- reset-tcp-connection

Action can also be changed for the whole signature category.

Verify IOS IPS

```
R1# show ip ips
R1# show ip ips all
R1# show ip ips configuration
R1# show ip ips interfaces
R1# show ip ips signatures {detail}
R1# show ip ips statistics {reset}
```

>Use the `clear ip ips configuration` command to disable IPS, remove all IPS configuration entries, and release dynamic resources. The `clear ip ips statistics` command resets statistics on packets analyzed, and alarms sent.

Reporting IPS alert

Syslog reporting

```
R1(config)# logging 192.168.1.50
R1(config)# ip ips notify {log|sdee}
R1(config)# logging on
```

>The `log` keyword sends messages in syslog format. The sdee keyword sends messages in SDEE format.

SDEE reporting

```
R1(config)# ip http server
R1(config)# ip http secure-server
R1(config)# ip ips notify sdee
R1(config)# ip sdee events 500
```

>SDEE uses a pull mechanism. With a pull mechanism, requests come from the network management application and the IDS or IPS router responds. SDEE is the standard format for vendor devices to communicate events to a network management application.

Clear the SDEE events of buffer

```
R1(config)# clear ip ips sdee {events|subscription}
```

Modify the SDEE buffer size

```
R1(config)# ip sdee events
```

---

<div>
<a href="chapter-04.md">Prev: Chapter 4</a>
</div>
<div align="right">
<a href="chapter-06.md">Next: Chapter 6</a>
</div>