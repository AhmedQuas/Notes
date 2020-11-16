### Spis treści
- [Chapter 7: EIGRP](#chapter-7-eigrp)
  - [7.1 Charakterystyka EIGPR](#71-charakterystyka-eigpr)
  - [7.2 Konfiguracja EIGRP dla IPv4](#72-konfiguracja-eigrp-dla-ipv4)
  - [7.3 Weryfikowanie EIGRP dla IPv4](#73-weryfikowanie-eigrp-dla-ipv4)
  - [7.4 Działanie EIGRP](#74-działanie-eigrp)
  - [7.5 DUAL](#75-dual)
  - [7.6 EIGRP dla IPv6](#76-eigrp-dla-ipv6)

# Chapter 7: EIGRP

## 7.1 Charakterystyka EIGPR

`EIGRP` - Enhanced Interior Gateway Routing Protocol, bezklasowy, zaawansowany protokół routingu wektora odległości opracowany przez Cisco. Posiada też cechy typowe dla protokołów routingu stanu łącza.

>W 2013, Cisco `udostępnił podstawową funkcjonalność` EIGRP jako otwarty standard dla IETF w formie informacyjnego RFC.

`DMVPN` - Dynamic Multipoint Virtual Private Network, dynamicznie wielopunktowe wirtualne sieci prywatne

Cechy EIGRP:
- `DUAL` - Diffusing Update Algorithm, algorytm rozsyłania aktualizacji, główny silnik obliczeniowy. Zapewnia trasy wole od pętli oraz zapasowe w całej domenie routingu

  >EIGRP przechowuje wszystkie dostępne trasy zapasowe do sieci docelowych, aby szybko przełączyć się na trasę zastępczą w razie potrzeby.

  Przyległości sąsiedzkie są ustanawiane z routerami bezpośrednio połączonymi i wykorzystywane są do śledzenia statusu sąsiadów.

- `RTP` - Reliable Transport Protocol, protokół warstwy transportowej wykorzystywany do dostarczania pakietów EIGRP do sąsiadów. RTP jest potrzebny z tego względu, że EIGRP z założenia jest niezależny od warstwy sieciowej, czyli możemy też korzystać z protokołów nie należących do stosu TCP/IP. RTP może wysyłać komunikaty na unicast i multicast:
  - 224.0.0.10, 01-00-5E-00-00-0A
  - FF02::A

  >RTP obsługuje zarówno dostarczanie niezawodne jak i bez gwarancji niezawodności pakietów EIGRP. Pakiety Hello EIGRP nie wymagają potwierdzenia.

- `cząstkowe i ograniczone aktualizacje` - nie rozsyłane są okresowe aktualizacje, wpisy o trasach nie ulegają przedawnieniu. 
  
  `Częściowa` - aktualizacja zawiera jedynie informacje o zamianach trasy(ustanowienie nowego połączenia, zerwanie dotychczasowego).

  `Ograniczona` - aktualizacje wysyłane jedynie do routerów, których zmiany dotyczą.

- `load balancing` - rozkładanie obciążenia z równym i nierównym kosztem, co pozwala na lepsze rozłożenie przepływu ruchu w ich sieciach

- `bezpieczeństwo` - możliwość skonfigurowania uwierzytelnienia

>Uwaga: W odniesieniu do EIGRP, w starszej dokumentacji czasami używane jest określenie hybrydowy protokół routingu (ang. `hybrid routing protocol`). Wprowadza to jednak w błąd, ponieważ EIGRP nie jest hybrydą pomiędzy protokołami routingu wektora odległości i stanu łącza. EIGRP jest wyłącznie protokołem wektora odległości; dlatego też, Cisco nie stosuje obecnie już tego określenia.

`PDM` - Protocol-dependent modules, moduły odpowiedzialne za wykonywanie zadań warstwy sieciowej typowej dla danego protokołu:
- zarządzanie tabelami sąsiadów
- budowanie i translacja pakietów charakterystycznych dla danego protokołu na potrzeby DUAL
- tworzenie punktu styku pomiędzy algorytmem DUAL a tabelą routingu
- obliczanie metryki i przekazywanie tej informacji do DUAL
- redystrybucja tras
- stsowanie filtracji i list dostępowych

IPv4, IPv6, IPX, Novell, Apple Talk

Moduły PDM dla IPv4/IPv6 utrzymują następujące tablice:
- sąsiadów
- topologii
- routingu

Typy/formaty pakietów EIGRP:
- `Hello` - wykorzystywane do wykrywania sąsiedztwa(łącza bezpośrednio przyłączone) i obsługi przyległości sąsiedzkich. Pakiety rozsyłane jako multicast, dodatkowo RTP bez zawodności. Standardowo są wysyłane co 5 s, natomiast w sieciach NBMA co 60 s.
    
    >EIGRP stosuje pakiety Hello to obsługi utworzonych przyległości. Router EIGRP zakłada, że dopóki otrzymuje pakiety Hello od sąsiada, zarówno sąsiad jak i jego trasy są osiągalne.

    Licznik `Hold` - stosowany do określenia maksymalnego czasu, jaki router powinien odczekać od ostatniego pakietu Hello, aby mógł uznać, że sąsiad jest nieosiągalny. Z reguły jest to 3 * Hello_interval. Po przekroczeniu tego czasu EIGRP uznaje, że trasa jest nieczynna, a DUAL szuka nowej trasy rozsyłając zapytania.

- `Acknowledgement` - stosowane do potwierdzania odbioru wiadomości EIGRP. Ruch unicast, wykorzystywane do potwierdzenia wiadomości, które są przesyłane niezawodnym RTP. Ten pakiet to nic innego jak zwykły pakiet Hello EIRGP bez żadnych danych, wysyłane są one w trybie bez niezawodności jako pakiet jednostkowy(`unreliable unicast`)
- `Update` - pakiety aktualizacyjne,dystrybucja informacji o trasach do sąsiadów. Pakiety rozsyłane unicast i multicast, z opcją niezawodnego dostarczenia za pomocą RTP Wysyłane są tylko wtedy, gdy jest to konieczne.
  
    >Pakiety aktualizacyjne są wysyłane jako komunikaty grupowe, kiedy wymagane są przez wiele routerów, albo jako komunikaty jednostkowe, kiedy potrzebne są tylko jednemu routerowi.

    Potwierdzenie informuje nadawcę zapytania, że wiadomość zapytania została otrzymana.

- `Query` - odpytanie sąsiadów o trasy. Pakiety rozsyłane unicast i multicast, niezawodny RTP.
- `Reply` - odpowiedzi na zapytania EIGRP. Pakiety rozsyłane unicast, niezawodny RTP.


>Można zadać pytanie, dlaczego R2 wysyła `zapytanie o sieć, o której wie, że jest wyłączona`. Tak naprawdę, jedynie interfejs przyłączony do tej sieci jest wyłączony. Do sieci tej może być tymczasem przyłączony inny router i tym samym posiadać do niej alternatywną ścieżkę.

>W polu `Protokół IPv4` stosuje się liczbę `88` dla oznaczenia, że dane w pakiecie stanowią wiadomość EIGRP dla IPv4. W IPv6 pole next header będzie ustawione na 88.

Wiadomości EIGRP:
- nagłówek paiektu EIGRP - dołączany do każdego pakietu EIGRP:
  - opcode - kod operacyjny:
    - 1 - Update
    - 3 - Query
    - 4 - Reply
    - 5 - Hello
  - numer systemu autonomicznego - identyfikator procesu EIGRP
- `TLV` - Time, Length, Value - komunikat parametrów EIGPR, zawiera wagi uzywane  przez EIGRP w złożonej metryce:
  - `K1` - szerokość pasma
  - `K3` - opóźnienia
  - `Hold time`

TLV tras wewnętrznych IP zawiera metryki(szerokość pasma i opóźnienia), maskę podsieci oraz miejsce przeznaczenia(adres sieci docelowej). Stosowane do rozgłaszania tras EIGRP w obrębie systemu autonomicznego.

TLV tras zewnętrznych - używany, gdy do procesu routingu EIGRP są importowane trasy zewnętrzne

>`Opóźnienie` oblicza się jako sumę opóźnień od źródła do celu w jednostkach 10 mikrosekundowych. `Szerokość` pasma to najniższa szerokość pasma skonfigurowana na którymkolwiek z interfejsów znajdujących się na trasie.

>Uwaga: jednostka MTU jest uwzględniana w aktualizacjach routingu, ale nie służy do ustalania metryki.

## 7.2 Konfiguracja EIGRP dla IPv4

```
R1(config)# router eigrp as_number{1-65535}
R1(config-router)# eigrp router-id {IPv4_address_uniq}
R1(config-router)# network 172.16.0.0 #classful notation
R1(config-router)# network 172.16.0.0 0.0.0.255 #option with wildcard mask
R1(config-router)# passive-interface
```

Wybór identyfikatora routera EIGRP:
1. eigrp router-id {IPv4_address_uniq}
2. najwyższy adres IP na interfejsie loopback
3. najwyższy adres IPv4 spośród swoich aktywnych interfejsów fizycznych

>`Numer systemu autonomicznego` używany podczas konfiguracji EIGRP nie jest powiązany z numerami systemu autonomicznego globalnie przypisanymi przez Internet Assigned Numbers Authority (IANA), stosowanymi przez protokoły routingu zewnętrznego. Miejscowy regionalny rejestr Internetowy (RIR) jest odpowiedzialny za przydzielenie podmiotowi numeru z bloku otrzymanych numerów systemów autonomicznych. `Przed` rokiem `2007` numery systemów autonomicznych były `16-bitowe` i mieściły się w przedziale od 0 do 65 535. `Obecnie` przypisywane są `32-bitowe` numery systemów autonomicznych, przez co liczba dostępnych numerów systemów autonomicznych wzrosła do ponad czterech miliardów. Z reguły są to dostawcy Internetu (ISP), operatorzy szkieletu Internetu oraz duże instytucje łączące się z innymi podmiotami, które również mają numer systemu autonomicznego. BGP jest jedynym protokołem routingu, do którego konfiguracji potrzebny jest numer systemu autonomicznego. ISP jest odpowiedzialny za przesyłanie pakietów w obrębie swojego systemu autonomicznego oraz pomiędzy innymi systemami autonomicznymi.

Dlaczego zatem takie same nazwy?
>System autonomiczny (ang. autonomous system) to zbiór sieci pod administracyjną kontrolą jednego podmiotu, z perspektywy Internetu postrzeganej jako wspólna polityka routingu.

Wszystkie routery w domenie EIGRP muszą używać tego samego numery systemu autonomicznego.

>Numer systemu autonomicznego zastosowany w konfiguracji EIGRP ma jedynie znaczenie w obrębie domeny routingu EIGRP. Działa on jako ID procesu, pomocny routerom podczas śledzenia wielu uruchomionych instancji EIGRP.

Usunięcie procesu EIGRP

```
R1(config)# no router eigrp as_number
```

Ustawienie wszystkich interfejsów w trybie passive-interface

```
R1(config-router)# passive-interface default
```

Wyłączenie automatycznej sumaryzacji

```
R1(config-router)# no auto-summary
```


## 7.3 Weryfikowanie EIGRP dla IPv4

```
R1# show ip route
R1# show ip protocols
R1# show ip interface brief
R1# show ip eigrp neighbors
R1# show ip eigrp topology
R1# show ip eigrp topology all-links
```

Show ip eigrp neighbor:
- H - lista sąsiadów w kolejności w jakiej zostali wykryci
- Hold - odliczanie do zera, podnoszona do max w momencie otrzymania pakietu Hello
- SRTT i RTO - wykorzystywane przez RTP do zarządzania niezawodnymi pakietami EIGRP
- Queue count - oznacza liczbę pakietów EIGRP oczekujących na wysyłkę
- Sequence number - służy do śledzenia pakietów aktualizacji, zapytań i odpowiedzi

D w tablicy routingu oznacza, że trasa została otrzymana z wykorzystaniem algorytmu DUAL w EIGRP.

Domyślne odległościa administracyjne, AD w EIGRP:
- zsumaryzowana trasa - 5
- trasa wewnętrzna - 90
- trasa zewnątrzna - 170

## 7.4 Działanie EIGRP

>Sąsiedzi EIGRP to inne routery w bezpośrednio dołączonych sieciach, używające protokołu EIGRP .

Utworzenie przyległości między routerami jest możliwe gdy zgodne są:
- parametry w metrykach
- być skonfigurowane w tym samym systemie autonomicznym

`Tablica sąsiadów` - zawiera listę sąsiadów zawierającą listę routerów na łączach współdzielonych, które tworzą przyległość EIGRP z tym routermem

`Reguła rodziału horyzontu` - pakiety aktualizacyjne zawierają wszystkie trasy zawarte w tablicy routingu, z wyjątkiem tych, które pozyskał z tego interfejsu

`Tablica topologii` - zawiera wpisy dla każdej sieci docelowej, do której router pozyskał od swoich bezpośrednio przyłączonych sąsiadów EIGRP(aktualizację EIGRP). Najlepsza trasy do danej sieci docelowej(metryka) jest dodawana do tablicy routingu. Tabela topologii EIGRP zawiera wszystkie trasy znane każdemu sąsiadowi EIGRP.

Metryki w EIGRP:
- `przepustowowść`, szerokość pasma - najniższe pasmo ze wszystkich interfejsów wychodzących na ścieżce do sieci docelowej. 10 mln / bw
- `opóźnienie` - suma opóźnień wszystkich interfejsów wzdłuż ścieżki(w 10 ms)

Domyślna metryka = [K1 * przepustowość + K3 * opóźnienie] * 256

*256 - zmiana z posatci 24 bitowej na 32 bitową

Inne wartości, które mogą być uwzględnianie w metryce, jednak są one nie zalecane, może to prowadzić do częstych przeliczeń w tablicy topologii:
- `niezawodność` - najmniejsza niezawodność na trasie w oparciu o wymianę komunikatów podtrzymujących(keepalive)
- `obciążenie` - reprezentuje największe obciążenie na łączu na trasie. Obliczane w oparciu o liczbę pakietów i ustawione pasmo interfejsu 

Pełny wzór = [K1 * przepustowość + (K2 * przepustowość) / (256-obciążenie) + K3 * opóźnienie] * [ K5 / (niezawodność+K4)]

Domyślnie wagi metryki K1 i K3 są ustawione na 1, reszta współczynników jest równa 0.

Zmiana wartości współczynników Kx

```
R1(config-router)# metric weights tos k1 k2 k3 k4 k5
```

Sprawdzenie wartości paramertów wykorzystywanych do obliczenia metryk

```
R1(config)# show interfaces serial 0/1/0
```

Badanie wartości metryki:
- `BW` - bandwidth, szerokość pasma [kb/s]
- `DLY` - delay, opóźnienie, czas potrzebny pakietowi do przejścia trasy, wartość statystyczna wynikająca z typu łącza  [ms]
- `Reliability` - niezawodność, 255/255 - 100% niezawodności, średnia wykładnicza z 5 min
- `Txload, Rxload` - obciązenie nadawcze i odbiorcze, 255/255 całkowite przepełnienie, średnia wykładnicza z 5 min

Konfiguracja szerokości pasma

```
R1(config-if)# bandwidth {kb-bandwidth-value}
R1(config-if)# no bandwidth
R1(config)# show interfaces
```

Domyślnie 1544 -> 1,544 Mb/s, jak szerokośc T1

## 7.5 DUAL

`DUAL` - Diffusing Update Algorithm, algorytm stosowany przez EIGRP do zapewniania ścieżek i ścieżek zapasowych(mogą być użyte natychmiast), wszystko wolne od pętli. Dodatkowo zapewnia osiągnięcie szybkiej zbieżności i użycie minimalnej szerokości pasma z ograniczonymi aktualizacjami.

Terminologia stosowana przez DUAL:
- `Successor` - sąsiedni router, który jest używany do przesyłania pakietów i biegnie przez niego najkorzystniejsza trasa do sieci docelowej. Adres IP successora jest widoczna we wpisie tablicy routingu, po słowie via.
- `FD` - Feasible Distance - najmniejsza przeliczona metryka do sieci docelowej, mertyka trasy w show ip route
- `FS` - Feasible Successor - sąsiad posiadający wolną od pętli ścieżkę zapasową do tej samej sieci co następca i spełnia warunki dopuszczalności FC
- `RD` i `RA` - Reported Distance i Adversary Distance
- `FC` - Feasible(Feasibility) Condition - warunki dopuszczalności. Są one spełnione kiedy ogłaszana odległość RD do sieci jest mniejsza niż FD danego routera do tej samej sieci docelowej
  
    >Jeżeli RD jest mniejsza, reprezentuje ścieżkę wolną od pętli. Odległość RD jest po prostu odległością FD sąsiada do tej samej sieci docelowej. RD to metryka, którą router ogłasza swojemu sąsiadowi, aby poinformować go o koszcie do danej sieci.


>Algorytm DUAL oblicza trasy w taki sposób, aby w każdej chwili sieć była wolna od pętli. Pozwala to wszystkim routerom, na które zmiana w topologii miała wpływ, na wykonanie synchronizacji.

Wygląd tabeli topologii:
- `P` - trasa w trybie pasywnym(stablinym), DUAL nie wykonuje swoich obliczeń zmierzających do określenia ścieżki do sieci
- `A` - tryb aktywny, trasa przeliczana przez DUAL
- `192.168.1.0` - sieć docelowa
- `1 successors` - liczba succesorów dla tej sieci. Jeśli istnieje wiele równorzędnych tras do danej sieci, będzie wiele successorów do tej sieci
- `FD is 3012096` - FD, metryka EIGRP do sieci docelowej. Jest to metryka wyświetlona w tablicy routingu IP
- `via` - adres następnego skoku successora
- `3012096` - FD
- `2816` - RD - koszt urządzenia następnego przeskoku do osiągnięcia tej sieci
- `Serial0/0/1` - interfejs wyjściowy wykorzystywany do osiągnięcia tej sieci, wartość jest też pokazana w tabeli routingu

```
P 192.168.1.0/24 1 successors, FD is 3012096
        via 192.168.10.20 (3012096/2816), Serial0/0/1
        via 172.164.2.1 (4102456/2170112), Serial0/0/0
```


```
R1(config)# show ip eigrp topology
```

DUAL jest automatem skończonym.

`FSM` - Finite State Machine

> Automaty skończone definiują zbiór możliwych stanów, zdarzenia wywołujące te stany i zdarzenia będące rezultatami tych stanów

## 7.6 EIGRP dla IPv6

Komunikaty EIGRP dla IPv6 są wysyłane z użyciem:
- `adresu źródłowego IPv6` - adres link-local interfejsu wyjściowego, wykorzystywane w komunikacji unicast
- `adres docelowy IPv6` - jak multicast to FF02::A

```
R1(config)# ipv6 unicast routing
R1(config)# ipv6 router eigrp 1
R1(config-router)# eigrp router-id 1.1.1.1
R1(config-router)# no shutdown
R1(config-router)# passive-interface g0/0/0

R1(config)# interface s0/1/0
R1(config-if)# ipv6 eigrp 1
```

```
R1# show ipv6 protocols
R1# show ipv6 eigrp neighbors
R1# show ipv6 interface brief
R1# show ipv6 route
```

---

<div>
<a href="chapter-06.md">Prev: Chapter 6</a>
</div>
<div align="right">
<a href="chapter-08.md">Next: Chapter 8</a>
</div>