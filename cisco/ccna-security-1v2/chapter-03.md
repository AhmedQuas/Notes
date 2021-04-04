### Spis treści
- [Chapter 3: Authentication, Authorization, and Accounting](#chapter-3-authentication-authorization-and-accounting)
  - [3.1 Purpose of the AAA](#31-purpose-of-the-aaa)
  - [3.2 Local AAA authnetication](#32-local-aaa-authnetication)
  - [3.3 Server-based authentication](#33-server-based-authentication)
  - [3.4 Server-based AAA authentication configuration](#34-server-based-aaa-authentication-configuration)
  - [3.5 Server-based AAA authorization & accounting](#35-server-based-aaa-authorization--accounting)

# Chapter 3: Authentication, Authorization, and Accounting

`ACS` - Cisco Secure Access Control System, is very scalable because all infrastructure devices access a central server. The Cisco Secure ACS solution is also fault tolerant because multiple servers can be configured. ACS is an identity and access control policy platform that enables enterprises to enforce compliance, enhance infrastructure security, and streamline their service operations.

`ISE` - Cisco Identify Services Engine, ensure that only the right people, with the right devices get the right access to the enterprise services(identity-based network access). ISE provides visibility into the users and devices that are accessing a network. This next-generation solution not only provides AAA, but also enforces security and access policies for endpoint devices connected to the organization’s switches and routers.

>Access to the LAN can be secured using IEEE 802.1X. 802.1X is a port-based access control and authentication protocol used to restrict unauthorized workstations from connecting to a LAN through publicly accessible switch port.

## 3.1 Purpose of the AAA

Types of authentication:
- login & password on console, vty lines & aux ports - anyone with the password can to the device. The easiest to implement, but it is also the weakest and least secure
- local database - user accounts configured localy on each device. No fallback authentication method.
- AAA - all devices refer to the same database of usernames and passwords from central server, more scalable solution

AAA components:
- `authentication` - users prove that they are who they say they are,
- `authorization` - determine which resources the user can access and which operations the user is allowed to perform,
- `accounting & auditing` - record what the user does. The collected data might include the start and stop connection times, executed commands, number of packets, and number of bytes. Types of accounting information:
  - network accounting,
  - connection accounting,
  - EXEC accounting,
  - system accounting,
  - command accounting

Authentication modes:
- `local AAA` - self-contained authentication, stores usernames & passwords locally inside device. This database is the same one required for establishing role-based CLI. Local AAA is ideal for small networks. Good solution for small network
- `server-based AAA` - central server that contains users & passowrds for all devices:
  - `RADIUS` - Remote Authentication Dial-In User Service,
  - `TACACS+` - Terminal Access Controller Access Control System

## 3.2 Local AAA authnetication

>The Local AAA Authentication method is similar to using the `login local` command with one exception. AAA also provides a way to configure backup methods of authentication.

```
R1(config)# username ADMIN algorithm-type scrypt secret PASSWORD
R1(config)# aaa new-model
R1(config)# aaa authentication login local-case
```

>The aaa authentication login command in the figure allows the ADMIN  user to log into the router via the console or vty terminal lines. The `default` keyword means that the authentication method applies to all lines, except those for which a specific line configuration overrides the default. The authentication is case-sensitive, indicated by the `local-case` keyword. This means that both the password and the username are case sensitive.

Login method types:
- enable,
- local,
- local-case,
- none,
- group radius,
- group tacacs+
- group_name
  
    ```
    R1(config)# aaa authentication login SSH-LOGIN local-case
    R1(config)# line vty 0 4
    R1(config)# login authentication SSH-LOGIN
    ```

Disable AAA

```
R1(config)# no aaa new-model
```

>To specify that a user can authenticate using the enable password, use the `enable` keyword. 

>To ensure that the authentication `succeeds even if all methods return an error`, specify `none` as the final method. 

>For security purposes, use the `none` keyword only when testing the AAA configuration. It should never be applied on a live network.

```
R1(config)# aaa local authentication attempts max-fail INT
```

>`aaa local authentication attempts max-fail` command locks the user account if the authentication fails. The locked out user account remains locked until it is manually cleared by an administrator using the `clear aaa local user lockout` privileged EXEC mode command.

```
R1# show aaa local user lockout
R1# show aaa user
R1# show aaa sessions

R1# debug aaa
R1# debug aaa authentication
R1# undebug all
R1# debug radius
R1# debug tacacs
```

## 3.3 Server-based authentication

Server-based aaa authentication can also work with external databases:
- `AD` - Active Directory,
- `LDAP` - Lightweight Directory Access Protocol

>These databases store user account information and passwords, allowing for central administration of user accounts. For increased redundancy, multiple servers can be implemented.

>The Cisco Secure Access Control System (`ACS`) is a centralized solution that ties together an enterprise’s network access policy and identity strategy.

Cisco Secure ACS features:
- distributed architecture for medium-sized & large-scale deployments,
- web-based GUI,
- Administrator authentication through AD & LDAP,
- automated reports sent through email,
- integrated advanced monitoring, reporting, and troubleshooting capabilities for excellent control and visibility using SNMP traps for Cisco Secure ACS health status,
- encrypted syslog,
- flexible and detailed device administration with full auditing & reporting

>The Cisco ACS family of products comprises highly scalable, high-performance access control servers. These can be leveraged to control administrator access and configuration for all network devices in a network supporting `RADIUS`, or `TACACS+`, or `both`.

<div align='center'>

|TACACS+|RADIUS|
|:---:|:---:|
|separates authentication and authorization|authentication & authorization are one process|
|cisco standard|RFC, open standard|
|49/tcp|1812,1645(authentication);1813,1646(accounting)/udp|
|encrypts all comunication|only password is encrypted|
|provides authorization of router commands on per-user or per-group basis|has no option to authorize router commands on per-user|
|limited accounting|extensive accounting|

</div>

---

>TACACS+ is considered the more secure protocol. This is because all TACACS+ protocol exchanges are encrypted, while RADIUS only encrypts the user’s password. RADIUS does not encrypt user names, accounting information, or any other information carried in the RADIUS message.

>Separating the AAA services provides flexibility in implementation because it is possible to use TACACS+ for authorization and accounting while using another method of authentication.

>RADIUS is widely used by VoIP service providers. It passes login credentials of a SIP endpoint, such as a broadband phone, to a SIP registrar using digest authentication, and then to a RADIUS server using RADIUS. RADIUS is also a common authentication protocol that is utilized by the 802.1X security standard.

>A next-generation AAA protocol alternative to RADIUS is the `DIAMETER AAA` protocol. DIAMETER is an IETF standard that uses a new transport protocol called Stream Control Transmission Protocol (SCTP) and TCP instead of UDP

`IAS` - Internet Authentication Service, the Microsoft implementation of a AAA server using RADIUS. However, starting with Windows Server 2008, IAS has since been renamed Network Policy Server (`NPS`).

## 3.4 Server-based AAA authentication configuration

Basic step to configure server-based authentication:
- enable AAA,
- specify the IP address of the ACS server,
- configure secret key,
- configure authentication to use either the RADIUS or TACACS+ server

Configuring TACACS+ server

```
R1(config)# aaa new-model
R1(config)# tacacs server SERVER_NAME
R1(config-server-tacacs)# address ipv4 192.168.1.201

# enhance TCP performance by maintaining a single TCP connection
R1(config-server-tacacs)# single-connection

# shared secret key
R1(config-server-tacacs)# key TACACS-PASSWORD-KEY
```

Configuring RADIUS server

```
R1(config)# aaa new-model
R1(config)# radius server SERVER_NAME
R1(config-radius-server)# address ipv4 192.168.1.201 auth-port 1812 acct-port 1813
R1(config-radius-server)# key RADIUS-PASSWORD-KEY
```

>Because RADIUS uses `UDP`, there is no equivalent `single-connection` keyword.

Configure authentication to use either the RADIUS or TACACS+ server

```
R1(config)# aaa authentication login default group tacacs+ group radius local-case
```

Authentication status = `PASS` or `FAIL`.

## 3.5 Server-based AAA authorization & accounting

```
R1(config)# aaa authorization (network | exec | command LEVEL) {default | list name} method1

R1(config)# aaa authorization network default group tacacs+
```

Types of commands or services:
- `network` - network services such as PPP,
- `exec` - starting an exec(shell),
- `commands` LEVEL - for exec(shell) commands

>When AAA authorization is not enabled, all users are allowed full access. After authentication is started, the default changes to allow no access. 

>This means that the `administrator must create a user with full access rights before authorization is enabled`. Failure to do so immediately locks the administrator out of the system the moment the aaa authorization command is entered. The only way to recover from this is to reboot the router. If this is a production router, rebooting might be unacceptable. `Be sure that at least one user always has full rights`.

Accounting triggers:
- `start-stop` - sends a "start" accounting notice at the beginning of a process and a "stop" accounting notice at the end of a process,
- `stop-only` - sends a "stop" accounting record for all cases including authentication failures,
- `none` - Disables accounting services on a line or interface

```
R1(config)# aaa accounting (network | exec | connection) {default | list name} {start-stop | stop-only | none}
```

>The IEEE 802.1X standard defines a port-based access control and authentication protocol that restricts unauthorized workstations from connecting to a LAN through publicly accessible switch ports. The authentication server authenticates each workstation that is connected to a switch port before making available any services offered by the switch or the LAN.

Roles in 802.1X port-based authentication:
- `supplicant` - client, workstation(device) that requests access to LAN, must be running 802.1X-compliant client software
- `authenticator` - switch, controls physical access to the network based on the authentication status of client. The switch uses a `RADIUS` software agent, which is responsible for encapsulating and de-encapsulating the `EAP`(Extensible Authentication Protocol) frames and interacting with the authentication server
  >The switch acts as an intermediary (proxy) between the client (supplicant) and the authentication server.
- `authentication server` - performs the actual authentication of the client

>Until the workstation is authenticated, 802.1X access control enables only Extensible Authentication Protocol over LAN (`EAPOL`), Cisco Discovery Protocol (`CDP`), and Spanning Tree Protocol (`STP`).

The encapsulation in 802.1X occurs as follows:
- `between the supplicant and the authenticator` - EAP data is encapsulated in EAPOL frames.
- `between the authenticator and the authentication server` - EAP data is encapsulated using RADIUS.

```
S1(config-if)# authentication port-control {auto | force-authorized | force-unauthorized}
```

States of dot1x port-control:
- `auto` - enables 802.1X port-based authentication and causes the port to begin in the unauthorized state, enabling only EAPOL, STP & CDP frames to be send and receive through the port
- `force-authorized` - default setting, the port sends and receives normal traffic without 802.1X-based authentication of the client
- `force-unauthorized` - causes the port to remain in the unauthorized state, ignoring all attempts by the client to authenticate. The switch cannot provide authentication services to the client through the port

802.1X configuration

```
S1(config)# aaa new-model
S1(config)# radius server CCNAS
S1(config-radius-server)# address ipv4 10.1.1.10 auth-port 1812 acct-port 1813
S1(config-radius-server)# key RADIUS_PASSWORD

S1(config)# aaa authentication dot1x default group radius
S1(config)# dot1x system-auth-control
S1(config)# int f0/1
S1(config-if)# switchport mode access
S1(config-if)# authentication port-control auto
S1(config-if)# dot1x pae authenticator
```

`PAE` - Port Access Entity, the interface acts only as an authenticator and will not respond to any messages meant for a supplicant.

---

<div>
<a href="chapter-02.md">Prev: Chapter 2</a>
</div>
<div align="right">
<a href="chapter-04.md">Next: Chapter 4</a>
</div>