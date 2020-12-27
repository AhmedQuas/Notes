### Spis treści
- [Chapter 5: Translacja adresów sieciowych dla IPv4](#chapter-5-translacja-adresów-sieciowych-dla-ipv4)
  - [5.1 Działanie NAT](#51-działanie-nat)

# Chapter 5: Translacja adresów sieciowych dla IPv4

## 5.1 Działanie NAT

<div align='center'>

Prywatne adresy RFC 1918:

|Klasa|Zakres adresów|Przedrostek CIDR
|:---:|:---:|:---:|
|A  |10.0.0.0 - 10.255.255.255|10.0.0.0/8|
|B  |172.16.0.0 - 172.31.255.255|172.16.0.0/12|
|B  |192.168.0.0 - 192.168.255.255|192.168.0.0/16|

</div>

<a href="../routing-switching-2v5/chapter-11.md">Poprzedni rodział o NAT dla IPv4</a>

PAT dokonuje z powodzeniem translacji innych pakietów niż te zawierające segmenty TCP i UDP. W przypadku komunikatów IMCP, PAT wykorzystuje pole Query ID.

>Całkowita liczba adresów wewnętrznych, które mogą być przetłumaczone na jeden adres zewnętrzny, może teoretycznie wynosić nawet 65536. W rzeczywistości do jednego adresu IP może zostać przypisanych około 4000 adresów wewnętrznych.

>Powszechnie uznaje się, że NAT dostarcza pewnego poziomu zabezpieczenia, ponieważ hosty zewnętrzne nie mogą bezpośrednio zainicjować komunikacji z hostami za NAT. Nie powinno się jednak mylić NAT z zaporą sieciową. Jak omówiono w dokumencie RFC4864 (rozdział 2.2), akt translacji sam w sobie nie stanowi mechanizmu bezpieczeństwa. Funkcja stanowej filtracji może dać ten sam poziom zabezpieczenia bez konieczności realizowania funkcji translacji

Statyczny NAT

```
R1(config)# ip nat inside source static 192.168.1.250 209.165.201.250
R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

Dynamiczny NAT

```
R1(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 pool NAT-POOL1
R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

PAT wykorzystujący określoną pulą adresów

```
R1(config)# ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 pool NAT-POOL1 OVERLOAD
R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

PAT wykorzystujący pojedynczy adres

```
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# ip nat inside source list 1 interface s0/0/0 OVERLOAD
R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

Port Forwarding

```
R1(config)# ip nat inside source static tcp 192.168.1.254 80 209.165.200.225 8080
R1(config)# interface s0/0/0
R1(config-if)# ip nat {inside|outside}
```

---

<div>
<a href="chapter-04.md">Prev: Chapter 4</a>
</div>
<div align="right">
<a href="chapter-06.md">Next: Chapter 6</a>
</div>