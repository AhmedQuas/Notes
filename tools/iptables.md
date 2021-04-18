### Iptables
- [Intro](#intro)
- [Practical examples](#practical-examples)
  - [Listing rules](#listing-rules)
  - [Deleting rules](#deleting-rules)
  - [Remove all rules in particular chain](#remove-all-rules-in-particular-chain)
  - [Insert rules at given position](#insert-rules-at-given-position)
  - [Replace rules](#replace-rules)
  - [Block single host - source IP](#block-single-host---source-ip)
  - [Block all tcp traffic](#block-all-tcp-traffic)
  - [Selecting interfaces](#selecting-interfaces)
  - [Custom chains](#custom-chains)
  - [Saving and restoring rules](#saving-and-restoring-rules)
  - [Logging packets](#logging-packets)
  - [Modules](#modules)
    - [tcp](#tcp)
    - [udp](#udp)
    - [multiport](#multiport)
    - [icmp](#icmp)
    - [conntrack](#conntrack)
    - [limit](#limit)
    - [recent](#recent)
    - [Owner](#owner)
- [Switch cheatsheet](#switch-cheatsheet)

# Intro

Iptables is a command-line interface to packet filtering functionality in netfilter. It is stateful firewall. Iptables need to be executed with root privileges

>There are two versions of the Internet Protocol — IPv4 and IPv6. These protocols have some differences and are handled differently in the kernel. Thus, iptables provides different commands for these protocols — iptables for IPv4 and ip6tables for IPv6.

It is organised in structure that have:
- `tables` - allows us to process packets in specific ways. Types of tables:
  - `filter` - default and most widely used, it is used to make decisions about whether a packet should be allowed to reach its destination
  - `mangle` - can be used when we want to alter packets headers in various ways(e.g changing TTL values)
  - `nat` - allows to route packets to different hosts on NAT by changing the src & dst address of packets
  - `raw` - allows to work with packets before kernel starts tracking its state(you can exempt certain packets from the state-tracking machinery)
  - `security` - used by SELinux to implement policies based on SELinux security contexts.
- `chains` - gives ability to inspect traffic at various points, checks if user defined rules match specific packets. Types of chains:
  - `PREROUTING` - rules in that chain apply to packets as they just arrive on the network interface(nat, mangle & filter tables)
  - `INPUT` - chain apply to packets just before they’re given to a local process(mangle & filter)
  - `OUTPUT` - apply to packets just after they’ve been produced by a proces(raw, mangle, nat & filter)
  - `FORWARD` - apply to any packets that are routed through the current host: NAT, forwarding and pass-through device(mangle & filter)
  - `POSTROUTING` - apply to packets as they just leave the network interface(nat & mangle)

    ![iptables packet flow](/img/iptablesPacketFlow.png)

- `targets` - when iptables finds a match, it jumps to this structure to performs the action associated with it. In case it does not find a match it performs `default policy` - default allow packets. Targets decide the fate of a packet:
  - `terminating` - the packet would not be matched against any other rules:
    - `ACCEPT` - accept the packet
    - `DROP` - drops packet, it would appear like the system did not even exist
    - `REJECT` - it sends TCP RST flag for TCP conections or ICMP destination host unreachable for UDP or ICMP connections
  - `non-terminating` - allow to match other rules even if a match was found:
    - `LOG` - when a packet is matched, it logs about it in the kernel logs
      - /var/log/syslog
      - /var/log/messages
    - `custom chains`

Check default chain policy

```
iptables -L | grep policy
```

Set default chain policy

```
iptables --policy INPUT ACCEPT
iptables -P INPUT ACCEPT
```

# Practical examples

## Listing rules

```
iptables {-t filter} -L --line-numbers
```

- `-n` - do not reverse DNS. Often, this is unnecessary and slows down the listing process.

## Deleting rules

Replace -A with -D switch

```
iptables -A INPUT -s 192.168.1.0/24 -j REJECT
iptables -D INPUT -s 192.168.1.0/24 -j REJECT
```

```
iptables -D INPUT 12
```

## Remove all rules in particular chain

```
iptables -F INPUT
```

## Insert rules at given position

```
iptables -I INPUT 1 -s 59.45.175.10 -j ACCEPT
```

## Replace rules

```
iptables -R INPUT 1 -s 59.45.175.10 -j ACCEPT
```

## Block single host - source IP

```
iptables -t filter -A INPUT -s 59.45.175.62 -j REJECT
```

>Filter table is used by default. So you can leave it out, which saves you some typing.

```
iptables -A INPUT -s 59.45.175.62 -j REJECT
```

## Block all tcp traffic

```
iptables -A INPUT -p tcp -j DROP
```

## Selecting interfaces

>For example, on a typical Nginx/PHP-FPM stack, Nginx communicates with PHP over localhost, which uses the loopback interface. Similarly, PHP may connect to a database server like Redis using the loopback interface. It’s useless to filter these kinds of traffic, so you can allow it. The loopback interface is typically named lo and you can add a rule like this at the top of the INPUT chain.

```
iptables -A INPUT -i lo -j ACCEPT 
```

## Custom chains

```
iptables -N ssh-rules
```

>Then, you can add the rules for the IPs in the new chain. since custom chains don’t have a default policy, make sure you end up doing something to the packet.

```
iptables -A ssh-rules -s 18.130.0.0/16 -j ACCEPT
iptables -A ssh-rules -s 18.11.0.0/16 -j ACCEPT
iptables -A ssh-rules -j DROP

iptables -A INPUT -p tcp -m tcp --dport 22 -j ssh-rules
```

Delete custom chain

```
iptables -A INPUT -p tcp -m tcp --tcp-flags FIN,SYN FIN,SYN -j LOG
iptables -A INPUT -p tcp -m tcp --tcp-flags FIN,SYN FIN,SYN -j DROP
```

`--log-prefix` - us it to search easily in syslog

```
iptables -A INPUT -p tcp -m tcp --tcp-flags FIN,SYN FIN,SYN -j LOG --log-prefix=iptables:
```

## Saving and restoring rules

>The workflow isn’t particularly nice. First, you have to first list the existing rules. Next, you need to figure out where a new rule should go, and then write a command to insert the rule.

```
iptables-save > iptables.rules
```

>Now, you can edit this file comfortably with a text editor. When you’re done, you can apply these rules

```
iptables-restore < iptables.rules
```

>Unfortunately, it turns out that iptables rules aren’t persistent — they’re lost when you reboot your system.

```
apt install iptables-persistent
```

>If your distribution doesn’t have a similar package, you can simply write a service file that loads iptables rules from a file when it starts up, and saves it when it stops.

## Logging packets

>You should first log the packet, and then drop it.

```
iptables 
```

## Modules

### tcp

`--dport` - check if tcp destination port is equal to given number

`--tcp-flags` - This switch takes in two arguments: a “mask” and a set of “compared flags”. The mask selects the flags that should be checked, while the “compared flags” selects the flags that should be set in the packet.

```
iptables -A INPUT -p tcp -m tcp --tcp-flags ALL FIN,PSH,URG -j DROP
```

>Invalid packet — a “new” connection that doesn’t begin with a SYN. Here, you simply need to check the FIN, RST, ACK and SYN flags; however only SYN should be set.

```
iptables -A INPUT -p tcp -m conntrack --ctstate NEW -m tcp ! --tcp-flags FIN,SYN,RST,ACK SYN -j DROP
```

### udp

### multiport

`--dports` - specify multiple ports

### icmp

`--icmp-type` - icmp number-style type

### conntrack

`--ctstate` - used to set state:
- `NEW` - first packet of connection
- `ESTABLISHED` - for packets that are part of an existing connection.
- `RELATED` - packets related to another ESTABLISHED connection.

    >An example of this is a FTP data connection — they’re “related” to the already “established” control connection.

- `INVALID` - packet doesn’t have a proper state.

    >This may be due to several reasons, such as the system running out of memory or due to some types of ICMP traffic.
- `UNTRACKED` - any packets exempted from connection tracking in the raw table with the NOTRACK target end up in this state
- `DNAT` - virtual state used to represent packets whose destination address was changed by rules in the nat table
- `SNAT` - this state represents packets whose source address was changed

>If you’ve tried blocking certain IPs on the INPUT chain, you might have noticed an interesting caveat — you can’t access the services hosted on those IPs either! You might think that rules in the INPUT chain are somehow affecting traffic on the OUTPUT chain, but that isn’t the case. The packets from your system do reach the server. However, the packets that the server sends to your system get rejected.

```
iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
```

>Keep in mind that you should first accept packets from established and related connections before using this rule! If you don’t, you’ll find out that you can’t use any internet based applications, becuase the responses coming in through the INPUT chain will be dropped.

### limit

`--limit`
`--limit-burst`

```
iptables -A INPUT -p icmp -m limit --limit 1/sec --limit-burst 1 -j ACCEP
```

### recent

Per-IP packet limits.

>Perhaps you’ve been facing some brute force attacks on your SSH server. Usually, attackers try to make many connections to speed up their attack.

```
iptables -A INPUT -p tcp -m tcp --dport 22 -m conntrack --ctstate NEW -m recent --set --name SSHLIMIT --rsource

iptables -A INPUT -p tcp -m tcp --dport 22 -m conntrack --ctstate NEW -m recent --set --name SSHLIMIT --update --seconds 180 --hitcount 5 --name SSH --rsource -j DROP
```

>We’ve set a name for the limit module by which it can keep track of things — in our case, it’s “SSHLIMIT”. The first line adds the source IP to the list that the recent module maintains. If the IP is already on this list, then the entry for this IP is updated. In the next line, we check whether the counter has hit the value of 5 in 180 seconds. If that’s indeed the case, we drop the packet. Thus, this allows 4 new SSH connections from an IP in 3 minutes.

`--mask` - to specify netmask

### Owner

>You’d like to block a particular website which as the IP 31.13.78.35 for your child. Assume that your child uses an account with the username bobby.

```
iptables -A OUTPUT -d 31.13.78.35 -m owner --uid-owner bobby -j DROP
```

>You can also use a numeric user ID or a range (such as 1000-1006) for the argument. Similarly, you can match packets of a group by using the `--gid-owner` packet.

# Switch cheatsheet

Rules:
- `-t` - specifies the table in which our rule would go into
- `-A` - append the list of existing rules in the INPUT chain
- `-s` - set the source IP
- `-d` - set the destination IP
- `-j` - jump to specified target, perform specified action
- `-i` - specifies interface
- `-o` - specifies exit interface in OUTPUT chain 

Protocols and modules:
- `-p` - specify protocol:
  - `tcp`,
  - `udp`,
  - `icmp` - icmpv6 in ip6tables
- `-m` - module to make additional checks:
  - `tcp`
    - --dport,
    - --tcp-flags
  - `udp`
  - `multiport`
    - --dports
  - `icmp`
    - --icmp-type 
  - `conntrack`
    - --ctstate
      - NEW,
      - ESTABLISHED,
      - RELATED,
      - INVALID,
      - UNTRACKED,
      - DNAT,
      - SNAT
  - `limit`
    - --limit
    - --limit-burst
  - `recent`
  

Rules mgmt:
- `-L` - list rules in given table,
- `-n` - do not reverse DNS during rules listing,
- `--line-numbers` - print line numbers during rules listening,
- `-D` - delete rule,
- `-F` - flush, remove all rules in given table,
- `-I` - insert rule,
- `-R` - replace rule

IP ranges:
- single IP - 192.168.1.10
- CIDR notation - 192.168.1.0/24
- Netmask - 192.168.1.0/255.255.255.0

Use `!` to negate conditions.

>A very common way to run node.js or Go web applications on a server is to place a server such as nginx in front of them. Once you’ve configured nginx to forward traffic to the app, there’s no need for it to be directly accessible.

```
iptables -A INPUT -p tcp -m multiport ! --dports 22,80,443 -j DROP
```

>However, you should first `accept packets from established and related connections` before using this rule! If you don’t, you’ll find out that you can’t use any TCP based applications. This is because legitimate TCP traffic would be dropped, too.

---

Those notes have been taken from articles:
- [An In-Depth Guide to iptables, the Linux Firewall](https://www.booleanworld.com/depth-guide-iptables-linux-firewall/)
- [The Beginner’s Guide to iptables, the Linux Firewall](https://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/)

Other useful articles:
- [Iptables Tutorial 1.2.2](https://www.frozentux.net/iptables-tutorial/iptables-tutorial.html)
- [Man iptables-extension](https://ipset.netfilter.org/iptables-extensions.man.html)
- [Netfilter](https://www.netfilter.org/documentation/HOWTO/netfilter-extensions-HOWTO-3.html)