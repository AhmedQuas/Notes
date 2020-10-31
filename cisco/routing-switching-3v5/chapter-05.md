### Spis treści
- [Chapter 5: Dostosowywanie i rozwiązywanie problemów z jednoobszarowym protokołem OSPF](#chapter-5-dostosowywanie-i-rozwiązywanie-problemów-z-jednoobszarowym-protokołem-ospf)
  - [5.1 Zaawansowana konfiguracja jednoobszarowego OSPF](#51-zaawansowana-konfiguracja-jednoobszarowego-ospf)
  - [5.2 Dostrajanie interfejsów OSPF](#52-dostrajanie-interfejsów-ospf)
  - [5.3 Zabezpieczanie OSPF](#53-zabezpieczanie-ospf)
  - [5.4 Rozwiazywanie problemów z jednoobszarowym OSPF](#54-rozwiazywanie-problemów-z-jednoobszarowym-ospf)

# Chapter 5: Dostosowywanie i rozwiązywanie problemów z jednoobszarowym protokołem OSPF

## 5.1 Zaawansowana konfiguracja jednoobszarowego OSPF

OSPF jest protokołem stanu łącza(link-state)

Cechy OSPF:
- bezklasowość,
- wydajność - aktualizacje routingu tylko w przypadku zmian w topologii,
- szybka zbieżność,
- skalowalność,
- bezpieczeństwo - uwierzytelnianie Message Digest (MD5), aktualizacje szyfrowane, współdzielone hasło

Unikanie stosowanie maski blankietowej, poprzez identyfikację konkretnego interfejsu maską złożoną z samych zer.

```
R1(config-router)# network 172.16.2.1 0.0.0.0 area 0
```

Typy sieci OSPF:
- `punkt - punkt` - typowe łącze WAN, połączenie dwóch routerów
- `rozgłoszeniowa wielodostępowa` - kilka routerów połączonych przez Ethernet(switch na środku)
- `wielodostępowa nierozgłoszeniowa`, NBMA - wiele routerów w sieci, która nie zezwala na ruch rozgłoszeniowy np Frame Relay
- `punkt - wielopunkt` - kilka routerów połączonych w topologii hub-and-spoke przez sieć NBMA. Często stosowane do łączenia oddziałów (spokes) z centralą (hub)
- `łącza wirtualne` - specjalna sieć OSPF stosowana do łączenia odległych obszarów OSPF do obszaru szkieletowego

>`Sieć wielodostępowa` to sieć z kilkoma urządzeniami podłączonymi do tego samego współdzielonego medium i współdzielącymi pasmo komunikacyjne. Ethernetowe sieci lokalne są najczęstszym przykładem wielodostępowej sieci rozgłoszeniowej. W sieciach rozgłoszeniowych wszystkie urządzenia znajdujące się w sieci widzą wszystkie ramki rozgłoszeniowe i komunikacji grupowej.

Problemy w sieciach wielodostępowych:
- tworzenie wielu przyległości - czasem nie jest to ani potrzebne, ani konieczne
- nasilone zalewowe wysyłanie pakietów LSA - zbyt duże zamieszanie w momencie zmiany w topologii

Liczba wymaganych przyległości n(n-1)/2

Router desygnowany, DR - sposób na rozwiązanie problemów pojawiających się w sieciach wielodostępowych. DR jest punktem zbierania i dystrybucji wysyłanych i odbieranych LSA.

Zapasowy router DR = BDR

>Router BDR nasłuchuje biernie wymianę informacji i utrzymuje relacje przyległości ze wszystkimi pozostałymi routerami.

DROTHER - wszystkie pozostałe routery

224.0.0.6 - grupowy adres na którym rozsyłane są komunikaty OSPF do DR i BDR

224.0.0.5 - grupowy adres na którym rozsyłane są komunikaty do wszytskich routerów OSPF

>Wybory routerów DR/BDR występują tylko w sieci wielodostępowej, a nie występują w sieci typu punkt-punkt.

Stan sąsiadów w ospf:
- FULL/DROTHER - stan na routerze DR lub BDR
- FULL/DR - przyległość z routerem DR
- FULL/BDR
- 2-WAY/DROTHER - między dwoma routerami DROTHER, routery wymieniają pakiety Hello

Wybór routera DR i BDR:
1. Najwyższy priorytet interfejsu 0-255 
   - domyślny 1, 
   - 0 oznacza, że router nie może być DR i BDR
2. Najwyższy identyfikator routera
   - ręczne ustawienie router-id
   - najwyższy adres IP interfejsu loopback
   - najwyższy adres IP z aktywnych interfejsów

>W sieci IPv6, jeśli na routerze nie ma skonfigurowanych adresów IPv4, identyfikator routera musi być skonfigurowany ręcznie za pomocą polecenia router-id rid inaczej `proces OSPFv3 nie wystartuje`.

>`Interfejsy szeregowe` mają domyślny priorytet ustawiony na 0, tak więc nie wybierają one routerów DR i BDR.

>DR staje się punktem centralnym zbierania i dystrybucji LSA, tak więc ważne jest, aby router ten dysponował wystarczająco dobrym procesorem i odpowiednią ilością pamięci, aby podołać obciążeniu.

Zmiana router-id podczas działania OSPF może nie odnieść efektu od razu, ponieważ wybrów routerów mógł zostać już zakończony. Trzeba wymusić ponowne wywołanie procesu wyboru routerów DR i BDR.

```
R1# clear ip ospf process
```

Router brzegowy - router podłączony do Internetu, router wejściowy, router brama, router ASBR

`ASBR` - Autonomous System Boundary Router, router brzegowy w terminologii OSPF.

## 5.2 Dostrajanie interfejsów OSPF

Wartości hello i dead są konfigurowane dla każdego interjesu osobno, sprawdzenie bieżącej wartości liczników:

```
R1# show ip ospf interface s0/0/0
```

Modyfikowanie liczników OSPFv2

```
R1(config-if)# ip ospf hello-interval 10
R1(config-if)# ip ospf hello-interval 30
```

Wartości liczników muszą być poprawne po obu stronach, aby OSPF działał poprawnie.

## 5.3 Zabezpieczanie OSPF

Ataki na protokoły routingu, sfałszowane informacje routingu:
- wzajemna dezinformacja,
- DoS,
- przesyłanie danych inną ścieżką, którą normalnie nie byłby przesyłany:
  - przekierowanie ruchu w celu stworzenia pętli routingu
  - przekierowanie ruchu, aby mógł on być monitorowany na niezabezpieczonym połączeniu
  - przekierowanie ruchu w celu jego odrzucenia

>Trasa z dłuższym dopasowaniem maski podsieci uważana jest za nadrzędną trasę nad trasą z krótszym dopasowaniem. W konsekwencji, jeśli router otrzymuje pakiet, wybiera dłuższą maskę podsieci, ponieważ to jest bardziej precyzyjna droga do miejsca przeznaczenia.

Rodzaje uwierzytelniania w OSPF:
- Null - brak
- Proste uwierzytelnianie za pomocą hasła - uwierzytelnianie za pomocą otwartego tekstu, metoda historyczna
- Uwierzytelnianie MD5 - najbardziej bezpieczna i zalecana metoda uwierzytelnienia
    
    podpis=md5(dane+klucz)

    Wiadomość nie jest szyfrowana, jedynie jest weryfikowana identyczność podpisów

>OSPFv3 (OSPF dla IPv6) nie zawiera żadnych własnych możliwości uwierzytelniania. Zamiast tego opiera się w całości na IPSec przy zabezpieczeniu komunikacji między sąsiadami za pomocą polecenia `ipv6 ospf authentication ipsec spi` wykonanego w trybie konfiguracji interfejsu.

Konfigurowanie uwierzytelniania MD5 w protokole OSPF:
- globalnie - wymusza uwierzytelnienia na wszystkich interfejsach z właczonym OSPF

    ```
    R1(config-router)# area 0 authentication message-digest


    R1(config-if)# ip ospf message-digest-key 1 md5 Cisco123
    ```

- na interfejsie - uwierzytelnienie na wybranych interfejsach
    
    ```
    R1(config-if)# ip ospf message-digest-key 1 md5 Cisco123
    R1(config-if)# ip ospf authentication message-digest
    ```

Weryfikacja uwierzytelnienia OSPF

```
R1# show ip ospf interface s0/0/0
```

## 5.4 Rozwiazywanie problemów z jednoobszarowym OSPF

Poprawne stany to FULL lub 2-WAY, wszystkie inne sa przejściowe i router nie powinien być w takim stanie zbyt długo.

Weryfikacja stanu OSPF:

```
R1# show ip ospf neighbour
R1# show ip protocols
R1# show ip ospf
R1# show ip ospf interface
R1# show ip ospf interface brief
R1# show ip route ospf
```
>Należy zachować ostrożność w przypadku, gdy interfejsy są szybsze niż 100 Mb / s, ponieważ domyślnie wszystkie interfejsy powyżej tego pasma mają ten sam koszt OSPF.

Najczęstsze problemy z OSPF:
- interfejsy w różnych podsieciach
- różne typy sieci OSPF
- hello i dead interval różnia się na połaczonych interfejsach
- interfejs do sasiada jest skonfigurowany jako pasywny
- nie prawidłowo dodano sieci, polecenie `network`
- błędnie skonfigurowane uwierzytelnienie
- interfejsy w złych obszarach ospf
- działanie innych protokołów routingu z niższymi wartościami administracyjnymi
- blokowanie na poziomie list ACL
- błędnie ustawione koszty ospf na interfejsach
- różne wartości mtu `ip mtu 1500`, domyślnie 1500 B


---

<div>
<a href="chapter-04.md">Prev: Chapter 4</a>
</div>
<div align="right">
<a href="chapter-06.md">Next: Chapter 6</a>
</div>