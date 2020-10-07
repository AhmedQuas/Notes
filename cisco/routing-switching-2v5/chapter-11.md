### Spis treści
- [Chapter 11: Translacja adresów sieciowych dla IPv4](#chapter-11-translacja-adresów-sieciowych-dla-ipv4)
  - [11.1 Działanie NAT](#111-działanie-nat)
  - [11.2 Konfigurowanie NAT](#112-konfigurowanie-nat)
    - [11.2.1 Statyczny NAT](#1121-statyczny-nat)
    - [11.2.2 Dynamiczny NAT](#1122-dynamiczny-nat)
    - [11.2.3 PAT](#1123-pat)
    - [11.2.4 Port Forwarding](#1124-port-forwarding)
  - [11.3 NAT i IPv6](#113-nat-i-ipv6)
  - [11.4 Rozwiązywanie problemów z NAT](#114-rozwiązywanie-problemów-z-nat)

# Chapter 11: Translacja adresów sieciowych dla IPv4

## 11.1 Działanie NAT

`NAT` - Network Address Translation, jest to mechanizm pozwalający na tłumaczenia adresów prywatnych na adresy publiczne. Dzięki temu pozwala na zaoszczędzenie publicznych adresów IP. Ukrywanie adresów przed sieciami zewnętrznymi poprawia również bezpieczeństwo. NAT zazwyczaj działa na brzegu sieci szczątkowej, `stub network`, czyli takiej, do której prowadzi tylko jedna trasa.

>Połączenie z ISP także może posiadać adres prywatny lub publiczny współdzielony przez wielu klientów.

Terminologia związana z NAT:
- `adres wewnętrzny` - adres urządzenia podlegającego translacji z użyciem NAT
- `adres zewnętrzny` - adres urządzenia docelowego
- `adres lokalny` - dowolny adres dostępny po wewnętrznej stronie sieci
- `adres globalny` - adres obecny po zewnętrznej stronie sieci

Typy NAT:
- `static NAT` - przekształcenie jeden do jednego, szczególnie pożyteczny w serwerach WWW i innych urządzeniach, które muszą mieć niezmieniony, stały  adres
    >Statyczny NAT wymaga, aby dostępna pula adresów publicznych była wystarczająco duża do jednoczesnego obsłużenia wszystkich sesji użytkowników.
- `dynamic NAT` - przekształcenie wiele do wielu
    >Dynamiczny NAT przyznaje adresy publiczne obsługując żądania w kolejności zgłoszenia.
- `overloading NAT` - przekształecenie wiele do jednego, zwany także `PAT` - Port Address Translation. 
  >Powoduje przekształcanie wielu prywatnych adresów IPv4 na pojedynczy lub kilka publicznch adresów IPv4. Na takiej zasadzie działa większość routerów domowych. Najpowszechniejsza forma NAT.

  >Pakiety nadchodzące z sieci publicznej są routowane do celów w sieci prywatnej poprzez odwołania się do tabeli w routerze NAT. Tabela ta przechowuje powiązania par portów publicznych i prywatnych. Nazywa się to `śledzeniem połączenia`.

Zalety NAT:
- oszczędności adresów publicznych
- zwiększa elastyczność połączeń z siecią publiczną
- zapewnia spójność wewnętrznych schematów adresowania
- poprawia bezpieczeństwo sieci

Wady NAT:
- spadek wydajności
- utracenie połączeń end-to-end
- brak możliwości śledzenia pakietów
- komplikuje tunelowanie jak IPSec
- inicjowanie połączeń TCP może być zakłócone

## 11.2 Konfigurowanie NAT

Czyszczenie statystyk NAT

```
R1 # clear ip nat statistics
```

### 11.2.1 Statyczny NAT

```
R1 (config)# ip nat inside source static 192.168.10.6 209.165.201.5

R1 (config)# interface s0/0/0
R1 (config-if)# ip address && no shutdown command
R1 (config-if)# ip nat inside

R1 (config)# interface f0/0
R1 (config-if)# ip address && no shutdown command
R1 (config-if)# ip nat outside
```

>Zazwyczaj statycznej translacji używa się, kiedy klienci po zewnętrznej stronie sieci (Internetu) muszą się komunikować z serwerami umieszczonymi wewnątrz sieci.


Weryfikowanie konfiguracji

```
R1 # show ip nat translations
R1 # show ip nat statistics
R1 # show running-config
```

### 11.2.2 Dynamiczny NAT

Konfiguracja:
- definiowanie puli publicznych adresów
  ```
  R1 (config)# ip nat pool NAT-1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
  ```
- określnie, które adresy mają być przekształcane
  ```
  R1 (config)# ip access-list 1 permit 192.168.0.0 0.0.255.255
  ```
- skojarzenie puli z listą ACL
  ```
  R1 (config)# ip nat inside source list 1 pool NAT-1
  ```
- wskazanie intefejsu wewnętrznego
  ```
  R1 (config)# int s0/0/0
  R1 (config-if)# ip nat inside 
  ```
- wskazanie interfejsu zewnętrznego
  ```
  R1 (config)# int s0/1/0
  R1 (config-if)# ip nat outside 
  ```

Weryfikowanie konfiguracji

```
R1 # show ip nat translations verbose
R1 # show ip nat statistics

R1 # clear ip nat translations
```

### 11.2.3 PAT

>W rzeczywistości do jednego adresu IP może zostać przypisanych około 4000 adresów wewnętrznych.

Sposoby konfiguracji PAT:
- ISP przydziela `więcej niż jeden adres publiczny` IPv4 do danej organizacji,
- ISP przydziela `pojedynczy adres publiczny` IPv4, za pomocą którego organizacja ta łączy się z ISP.

Konfiguracja, gdy chcemy korzystać z kilku adresów publicznych wygląda podobnie do dynamicznego NAT, z tą różnicą, że momencie wiązania puli z listą ACL na końcu polecenia dodajemy `overload`.

Konfigurowanie PAT dla pojednyczego adresu publicznego:
- utworzenie ACL, która określi ruch poddawany translacji
  ```
  R1 (config)# ip access-list 1 permit 192.168.0.0 0.0.255.255
  ```

- skojarzenie ACL z interfejsem
  ```
  R1 (config)# ip nat source list 1 interface s0/0/0 overload
  ```

- określenie interfejsu wewnętrznego `ip nat inside`
- określenie interfejsu zewnętrznego `ip nat outside`

### 11.2.4 Port Forwarding

>Technika ta pozwala zewnętrznemu użytkownikowi na dotarcie do portu z prywatnym adresem IPv4 (wewnątrz sieci) z zewnątrz, przez router obsługujący NAT.

>Z powodów bezpieczeństwa, routery szerokopasmowe domyślnie nie pozwalają na przekierowywanie żadnych zapytań z sieci zewnętrznych do hostów wewnętrznych.

Konfiguracja:
- ustalenie statycznej translacji
  ```
  R1 (config)# ip nat inside source static tcp 192.168.10.5 80 209.165.200.225 8080
  ```
- określenie interfejsów wewnętrznych i zewnętrznych

## 11.3 NAT i IPv6

>Korzyść z NAT polega na podniesieniu poziomu bezpieczeństwa poprzez uniemożliwianie komputerom z publicznego Internetu dostępu do wewnętrznych hostów.

`ULA` - Unique Local Address, unikatowe lokalne adresy IPv6.

>Celem ULA jest `zapewnienie przestrzeni adresowej IPv6 dla komunikacji w obrębie danej sieci` (organizacji).

FC00 - FDFF

Prefix ULA - pierwsze 64 bity adresu:
- FC00::/7
- L - O lub 1
- ID globalne 40 bitów
- ID podsieci 16 bitów

Cechy ULA:
- pozwala na łączenie sieci, tworzenie prywatnych połączeń między nimi bez generowania konfliktów w adresacji
- niezależność od ISP i możliwość wykorzystania do komunikacji w obrębie sieci bez łączności z internetem
- nie są rutowane w Internecie

Jest jeszcze coś jak NAT64, ale to nie jest poruszane w tym kursie :(

## 11.4 Rozwiązywanie problemów z NAT

1. Na podstawie obecnej konfiguracji oceń jak oczekiwanie powinien pracować NAT
2. Sprawdzić czy istnieją właściwe translacje
  ```
  R1 # show ip nat translations
  ```
3. Wykorzystaj polecenia clear i debug do określenia, czy translacja NAT działa według oczekiwań
4. Przejrzyj operacje wykonywane na pakiecie i sprawdź, czy na routerach zapisane są poprawne informacje routingu wykorzystywane do przenoszenia pakietów
5. Sprawdzić ustawienia listy ACL

```
R1 # debug ip nat

R1 # debug ip nat detailed
```

---

<div>
<a href="chapter-10.md">Prev: Chapter 10</a>
</div>
<div align="right">
<a href="../routing-switching-3v5/chapter-01.md">Next: Chapter 3.1</a>
</div>
