### Spis treści
- [Chapter 10: DHCP](#chapter-10-dhcp)
  - [10.1 Dynamic Host Configuration Protocol v4](#101-dynamic-host-configuration-protocol-v4)
    - [10.1.1 Podstawowa konfiguracja serwera DHCP](#1011-podstawowa-konfiguracja-serwera-dhcp)
    - [10.1.2 Rozwiązywanie problemów z konfiguracją DHCPv4](#1012-rozwiązywanie-problemów-z-konfiguracją-dhcpv4)
  - [10.2 Dynamic Host Configuration Protocol v6](#102-dynamic-host-configuration-protocol-v6)
    - [10.2.1 Bezstanowy serwer DHCP v6](#1021-bezstanowy-serwer-dhcp-v6)
    - [10.2.2 Stanowy serwer DHCP v6](#1022-stanowy-serwer-dhcp-v6)
    - [10.2.3 DHCPv6 Relay Agent](#1023-dhcpv6-relay-agent)
    - [10.2.4 Rozwiązywanie problemów z DHCPv6](#1024-rozwiązywanie-problemów-z-dhcpv6)

# Chapter 10: DHCP

## 10.1 Dynamic Host Configuration Protocol v4

Zadaniem serwera DHCP jest przypisanie adresu IP i innych informacji dotyczacych konfiguracji sieci: brama domyślna, adres serwera DNS i wiele innych.

Sposoby alokacji adresów przez serwer DHCP:
- `alokacja statyczna` - administrator przypisuje ręcznie adres IP dla klienta, a DHCP przekazuje ten adres do urządzenia np. serwery, drukarki
- `alokacja automatyczna` - usługa DHCP przypisuje statycznie adres na stałe do urządzenia, nie występuje tutaj dzierżawa
- `alokacja dynamiczna` - adresy są przydzielane z puli na określony czas, nazywany czasem dzierżawy. Jest to najczęściej używany typ alokacji adresów DHCP. Gdy okres dzierżawy dobiega końca, adres klienta trafia z powrotem do puli, a klient musi ponownie zarządać adresu, zazwyczaj otrzymuje ten sam adres.

Proces uzyskania adresu IP za pomocą DHCP:
- `DHCPDISCOVER` - wysyłany przez hosta w celu wyszukania serwerów DHCP w sieci
- `DHCPOFFER` - serwer po otrzymaniu DHCPDISCOVER rezerwuje adres dla klienta. Sam DHCPOFFER jest wysyłany w trybie unicast
- `DHCPREQUEST` - służy do powiadomienia o akceptacji parametrów zaoferowanych przez serwer
- `DHCPACK` - po otrzymaniu DHCPREQUEST serwer wysyła ping na, adres aby upewnić się czy nie jest on używany. Klient po otrzymaniu DHCPACK wykonuje zapytanie ARP, aby upewnić się, że adres nie jest zajęty. Jeśli nie uzyska odpowiedzi to adres traktuje jak swój.

Proces odnowienia dzierżawy:
- `DHCPREQUEST`
- `DHCPACK`

>Komunikaty DHCPv4 wysyłane z klienta do serwera używają portu źródłowego UDP `68` oraz portu docelowego UDP `67`.

Format komunikatu DHCP:
- `Op(eration) Code` - określa ogólny typ komunikatu:
  - `1` - żądanie
  - `2` - odpowiedź 
- `Hardware Type` - typ sprzętu używanego w sieci:
  - 1 - Ethernet
  - 15 - Frame Relay
  - 20 - łącze szeregowe
  
  Podobne pole jest w komunikatach ARP.
- `Hardware Address Length` - długość używanego adresu sprzętowego np. MAC
- `Hops` - tak jak TTL, tylko tutaj jest zwykle ustawione na 0
- `Transaction Identifier` - pomaga w dopasowaniu żądania do odpowiedzi w sytuacji gdy serwer nie będzie w stanie zidentyfikować adresu sprzętowego
- `Seconds` - liczba sekund, która upłynęła od momentu gdy klient rozpoczął próbę uzyskania dzierżawy lub jej odnowienia
- `Flags` - gdy klient nie zna swojego adresu IP, to może ustawić jedną z flag, która oznacza wysłanie odpowiedzi jako komunikat rozgłoszeniowy
- `Your IP Address` - używany w momencie odnowienia dzierżawy przez klienta, w przeciwnym razie pole jest ustawione na 0
- `Server IP Address` - ustawiany przez serwer adres IP serwera, którego klient powinien użyć do następnego etapu 
- `Gateway IP Address` - służy do routowania komunikatów DHCP, umożliwia on wymianę żądań i odpowiedzi między klientem a sewerem znajdującym się w różnych sieciach
- `Client Hardware Address` - zawiera adres MAC klienta
- `Server Name` - opcjonalnie pseudonim lub nazwa DNS, dla komunikatów DHCPOFFER lub DHCPACK
- `Boot Filename` - opcjonalnie przez klienta(DHCPDISCOVER) może być żądany określony plik rozruchowy, DHCPOFFER zwarca określony katalog i plik rozruchowy
- `DHCP Options` - zawiera opcje i parametry wymagane do postawowego funkcjonowania DHCP, ma zmienną długość. Może być używane przez klienta oraz serwer

>Ponieważ klient nie ma możliwości uzyskania informacji do jakiej podsieci należy, to musi wysłać komunikat rozgłoszeniowy DHCPDISCOVER IPv4 (dlatego docelowy adres IPv4 to `255.255.255.255`). Klient nie ma jeszcze skonfigurowanego adresu IPv4, więc używa `0.0.0.0` jako źródłowego adresu IPv4.

### 10.1.1 Podstawowa konfiguracja serwera DHCP

Kroki standardowej konfiguracji serwera DHCP:
- wykluczenie adresów IPv4 z puli
  >Wśród wykluczonych adresów powinny znajdować się adresy przypisane do routerów, serwerów, drukarek i innych urządzeń, które zostały skonfigurowane ręcznie.
  ```
  R1 (config)# ip dhcp excluded-address 192.168.10.1 192.168.10.9
  ```
- konfigurowanie puli adresów
  ```
  R1 (config)# ip dhcp pool LAN-1
  R1 (dhcp-config)# network 192.168.10.0 255.255.255.0
  R1 (dhcp-config)# default-router 192.168.10.1
  ```
- konfigurowanie parametrów opcjonalnych
  ```
  R1 (dhcp-config)# dns-serwer 8.8.8.8
  R1 (dhcp-config)# domain-name example.com
  ```

>Usługa DHCPv4 w Cisco IOS jest domyślnie włączona. Aby wyłączyć usługę DHCP, użyj polecenia `no service dhcp` w trybie konfiguracji globalnej.

Weryfikowanie poprawności konfiguracji DHCP

Weryfikacja obecnej konfiguracji serwera DHCP
```
R1# show running-config | section dhcp
```

Wyświetlenie wszystkich powiązanych adresów IPv4 z adresami MAC wykonanych przez usługę DHCP
```
R1# show ip dhcp binding
```

Sprawdzenie listy komunikatów odebranych i wysłanych
```
R1# show ip dhcp server statistics
```

Weryfikacja konfiguracji stosu TCP/IP u klienta

```
C:\ ipconfig /all
```

Zwolnienie posiadanego adresu

```
C:\ ipconfig /release
```

Odnowienie adresu IP

```
C:\ ipconfig /renew
```

`Przekazywanie DHCP` - sytuacja w której serwer DHCP nie znajduje się w tej samej sieci co klienci. Problem pojawia się na routerze bo nie wie co zrobić z taki rozgłoszeniem. Rozwiązaniem jest tutaj konfiguracja adresu pomocniczego, `helper address`. W tym momencie router może być nazwany agentem przekazywania DHCPv4, `relay agent`.

```
R1 (config)# interface g0/0
R1 (config-if)# ip helper-address 192.168.11.6
```

>Gdy router zostaje skonfigurowany jako agent przekazywania DHCPv4, to akceptuje wszystkie rozgłoszenia DHCPv4, a następnie przesyła je, już jako pakiety unicast.

IP helper address przekazuje również poniższe usługi UDP:
- 37 - time
- 49 - TACACS
- 53 - DNS
- 67 - klient DHCP/BOOTP
- 68 - serwer DHCP/BOOTP
- 69 - TFTP
- 137 - NetBIOS name service
- 138 - NetBIOS datagram service

Konfigurowanie routera jako klienta DHCP

```
R1 (config)# interface g0/0
R1 (config-if)# ip address dhcp
R1 (config-if)# no shutdown
```

Sprawdzenie czy adres IP został rzeczywiście przypisany

```
R1 # show ip interface g0/1
```

>Routery Cisco wykorzystują specjalny zestaw funkcji systemu Cisco IOS (Easy IP), jako opcjonalnego, w pełni funkcjonalnego serwera DHCP.

### 10.1.2 Rozwiązywanie problemów z konfiguracją DHCPv4

Rozwiązywanie problemów z konfiguracją DHCPv4:
- `usuń konflikty adresów IPv4`
  >Jeżeli zostanie wykryty konflikt adresów, to dany adres jest usuwany z puli i nie jest przypisywany aż do momentu rozwiązania konfliktu przez administratora
  
  ```
  R1 # show ip dhcp conflict
  ```

-  `sprawdzenie połączenia fizycznego` - bramy domyślnej
  ```
  R1 # show interface g0/0
  ```
- `testowanie połączenia za pomocą statycznego adresu IP`
- `sprawdzenie konfiguracji portów przełącznika` - przyładowo interfejs wyłączony, inne vlany lub coś podobnego
- `testowanie działania DHCP w tej samej podsieci lub w tej samej sieci VLAN` - sprawdzenie czy usługa działa dla innych klientów

Rozwiązywanie problemów na routerze przekazującym żądania DHCP:
- sprawdzenie czy na poprawnym interfejsie jest skonfigurowny helper address
  ```
  R1 # show running-config | section interface GigabitEthernet0/0
  ```
- sprawdzenie czy usługa nie jest wyłączona
  ```
  R1 # show running-config | include no service dhcp
  ```

W celu sprawdzenia czy pakiety docierają do routera, można włączyć `tryb debugowania zawerający listę ACL przepuszczającą tylko ruch DHCP`

```
R1 (config)# access-list 100 permit udp any any eq 67
R1 (config)# access-list 100 permit udp any any eq 68
R1# debug ip packte 100
```

Raportowanie zdarzeń na serwerze DHCP

```
R1 # debug ip dhcp server events
```

## 10.2 Dynamic Host Configuration Protocol v6

Adresy globalne unicast IPv6 można skonfigurować ręcznie lub dynamicznie. Dynamicznie może być skonfigurowany za pomocą następujących metod:
- `SLAAC` - bezstanowa automatyczna konfiguracja adresu, wykorzystanie ICMPv6 Router Solicitaion i Advertisement do zapewnienia adresacji i pozostałych informacji dotyczących konfiguracji
- `Stateful DHCPv6` - połączeniowy protokół dynamiczniej konfiguracji hostów dla IPv6
- `Stateless DHCPv6` - wykorzystanie komunikatu RA i DHCPv6

>Bezstanowość usługi oznacza, że nie ma serwera, który obsługuje informacje o adresach sieciowych. Oznacza to, że nie ma serwera, który wie które adresy IPv6 są zajęte.

Działanie SLAAC:
1. Host chcący uzyskać adres sieciowy wysyła na adres multicastowy routerów `FF02::2` komunikat RS
2. Router odpowiada komunikatem RA, który zawiera prefix i długość prefixu tej sieci. Komunikat jest skierowany do wszyskich węzłów łącza lokalnego `FF02::1`, a źródłowy routera to jego link-local
3. Z otrzymanych danych host tworzy własny globalny adres unicast IPv6, musi wygenerować 64 bitowy identyfikator interfejsu `IID`. Może on być utworzony za pomocą:
   - `EUI-64`
   - `generowany losowo`
4. Host w celu upewnienia się, że nie ma tego adresu w sieci, za pomocą ICMPv6 Neighbour Solicitation, który zawiera jego własny adres. Jeśli nikt nie odpowie na ten komunikat to znaczy, że jest on unikalny. Tą operację nazywa się również `DAD` - Duplicate Address Detection.

Ustawienie obsługi opcji SLAAC

```
Router (config-if)# no ipv6 nd managed-config-flag
Router (config-if)# no ipv6 nd other-config-flag
```

Ustawienie obsługi bezstanowego DHCPv6 (RA+DHCPv6)

>Bezstanowy serwer DHCPv6 dostarcza klientom jedynie parametrów konfiguracyjnych, a nie adresów IPv6.

```
Router (config-if)# ipv6 nd other-config-flag
```

Ustawienie obsługi połączeniowego protokołu DHCPv6(tylko DHCPv6)

```
Router (config-if)# ipv6 nd managed-config-flag
```

Komunikacja w DHCPv6

>Komunikaty DHCPv6 od serwera do klienta używają portu docelowego `UDP 546`. Komunikaty DHCPv6 od klienta do serwera używają portu docelowego `UDP 547`.

1. Lokalizacja serwerów DHCPv6 - `DHCPv6 SOLICIT` dla multicast serwerów DHCP `FF02::1:2`
2. `DHCPv6 ADVERTISE` - w ten sposób serwer DHCP informuje klienta, że serwer świadczy usługi DHCPv6
3. Klient wysyła:
  - `DHCPv6 REQUEST` - stanowy DHCPv6
  - `INFORMATION-REQUEST` - bezstanowy DHCPv6
4. `DHCP REPLY`

### 10.2.1 Bezstanowy serwer DHCP v6

Konfiguracja:
- włączenie routingu IPv6, wymagane do wysyłania komunikatów ICMPv6 RA
  ```
  R1 (config)# ipv6 unicast-routing
  ```
- konfigurowanie puli DHCPv6
  ```
  R1 (config)# ipv6 dhcp pool LAN-1
  ```
- parametrów puli
  ```
  R1 (config-dhcpv6)# dns-server 8.8.8.8
  R1 (config-dhcpv6)# domain-name ccna.lab
  ```
- interfejsu
  ```
  R1 (config)# interface g0/0
  R1 (config-if)# ipv6 dhcp server LAN-1
  R1 (config-if)# ipv6 nd other-config-flag
  ```

>Zazwyczaj `bezstanowym klientem DHCPv6` jest urządzenie takie jak: komputer, tablet, urządzenie mobilne lub kamera internetowa.

Konfigurowanie routera jako bezstanowego klienta DHCPv6

```
R1 (config)# interface g0/1
R1 (config-if)# ipv6 enable
R1 (config-if)# ipv6 address autoconfig
```

1. Włączenie ipv6 na interfejsie
2. Włączenie automatycznej konfiguracji ipv6 przy użyciu SLAAC

Weryfikowanie bezstanowego serwera DHCPv6
```
R1 # show ipv6 dhcp pool
R1 # show running-config
R1 # debug ipv6 dhcp detail
```

Weryfikowanie bezstanowego klienta DHCPv6
```
R1 # show ipv6 interface
R1 # debug ipv6 dhcp detail
```

`"Stateless address autoconfig enabled"`

### 10.2.2 Stanowy serwer DHCP v6

Konfiguracja:
- włączenie routingu IPv6, wymagane do wysyłania komunikatów ICMPv6 RA
  ```
  R1 (config)# ipv6 unicast-routing
  ```
- konfigurowanie puli DHCPv6
  ```
  R1 (config)# ipv6 dhcp pool LAN-1
  ```
- parametrów puli
  ```
  R1 (config-dhcpv6)# address prefix
  R1 (config-dhcpv6)# dns-server 8.8.8.8
  R1 (config-dhcpv6)# domain-name ccna.lab
  ```
- interfejsu
  ```
  R1 (config)# interface g0/0
  R1 (config-if)# ipv6 dhcp server LAN-1
  R1 (config-if)# ipv6 nd managed-config-flag
  ```

Konfigurowanie klienta odbywa się na tej samej zasadzie jak w przypadku bezstanowego serwera DHCPv6.

Weryfikowanie poprawności konfiguracji opiera się na tych samych poleceniach jak w przypadku bezstanowego serwera DHCPv6. Ze względu, że jest to stanowy DHCP to możemy uzyskać informację o przypisanych adresach ipv6

```
R1# show ipv6 dhcp LAN-1
R1# show ipv6 dhcp binding
```
>Komunikaty DHCPv6 od klientów są wysyłane na adres multicast IPv6 `FF02::1:2`. Adres ten nazywa się `All_DHCPv6_Relay_Agents_and_Servers`

### 10.2.3 DHCPv6 Relay Agent

Polecenie to jest konfigurowane na `interfejsie od strony klienta DHCPv6 i zawiera adres serwera DHCPv6` jako adres docelowy.

```
R1 (config)# interface g0/0
R1 (config-if)# ipv6 dhcp relay destination 2001:db8:cafe::5
```

```
R1# show ipv6 dhcp interface g0/0
```

### 10.2.4 Rozwiązywanie problemów z DHCPv6

1. Usuń konflikty adresów
   ```
   R1# show ipv6 dhcp conflict
   ```
2. Sprawdzenie metody alokacji - weryfikacja poprawności przypisania flag management - `M` i other - `O`
3. Testowanie komunikacji przy użyciu statycznego adresu IPv6
4. Sprawdzanie konfiguracji portów przełącznika
5. Testowanie działania DHCPv6 w tej samej podsieci lub sieci VLAN

```
R1# debug ipv6 dhcp detail
```

---

<div>
<a href="chapter-09.md">Prev: Chapter 9</a>
</div>
<div align="right">
<a href="chapter-11.md">Next: Chapter 11</a>
</div>