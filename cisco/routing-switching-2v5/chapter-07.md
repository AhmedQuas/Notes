### Spis treści
- [Chapter 7: Routing dynamiczny](#chapter-7-routing-dynamiczny)
  - [7.1 Protokoły routingu dynamicznego](#71-protoko%c5%82y-routingu-dynamicznego)
  - [7.2 Routing typu wektor - odległość](#72-routing-typu-wektor---odleg%c5%82o%c5%9b%c4%87)
  - [7.3 Routing RIP i RIPng](#73-routing-rip-i-ripng)
  - [7.4 Protokoły stanu łącza](#74-protoko%c5%82y-stanu-%c5%82%c4%85cza)
  - [7.5 Tablica routingu](#75-tablica-routingu)

# Chapter 7: Routing dynamiczny

## 7.1 Protokoły routingu dynamicznego

Protokoły routingu dynamicznego dla IPv4:
- `RIP` - Routing Information Protocol
- `OSPF` - Open Shortest Path First
- `IS-IS` - Intermediate System-to-Intermediate-System
- `IGRP` - Interior Gateway Routing Protocol
- `EIGRP` - Enhanced IGRP
- `BGP` - Border Gateway Protocol

Protokoły routingu dynamicznego dla IPv6:
- `RIPng`
- `EIGRP dla IPv6`
- `OSPFv3`
- `IS-IS dla IPv6`
- `BGP-MP`

>`Protokół routingu` to zbiór procesów, algorytmów i komunikatów służących do wymiany informacji o trasach i wypełniania tablicy routingu najlepszymi trasami, wybranymi przez protokół.

Główne zdania:
- wykrywanie zdalnych sieci
- aktualizacja informacji o trasach
- wybór najlepszej trasy do sieci docelowych
- znalezienie trasy do sieci zdalnej, jeśli obecna jest nie dostępna

Składniki dynamicznych protokołów routingu na podstawie `EIGRP`:
- `struktura danych` - tablica sąsiadów i topologii, najlepszą trasę/y w tablicy routingu
- `komunikaty protokołu routingu` - hello, update, query, reply, acknowledge
- `algorytm` - dual

>Protokoły routingu dynamicznego pozwalają administratorowi sieci uniknąć czasochłonnego i ścisłego obowiązku konfigurowania i obsługiwania tras statycznych.

`Zimny start` - moment zaraz po włączeniu zasilania, gdy router nie ma jeszcze żadnej informacji o topologii sieci.

Schemat działania protokołów routingu:
- zimny start
- wykrywanie sieci bezpośrednio podłączonych
- wymiana informacji o routingu
- osiągnięcie zbieżności

>Protokoły routingu wektora odległości z reguły implementują technikę zwaną podzielonym horyzontem (ang. `split horizon`), zapobiegającą tworzeniu się pętli routingu. Podzielony horyzont zapobiega wysyłaniu informacji z tego samego interfejsu, z którego została już odebrana.

>Czas zbieżności to czas potrzebny routerom na podzielenie się informacjami, obliczenie najlepszych tras i aktualizację ich tablic routingu.

Protokoły routingu mogą być podzielone ze względu na:
- `przeznaczenie` - protokoły bramy wewnętrznej (IGP) i zewnętrznej (EGP)
- `sposób działania` - protokół wektora odległości lub stanu łącza
- zachowania względem klas adresu - klasowy(przestarzały np RIPv1, IGRP) lub bezklasowy

Porównanie protokołów IGP oraz EGP

`AS` - Autonomous System, system autonomiczny to zbiór sieci znajdujących się pod współną administracją mających podobna strategię routingu.

`IGP` - protokoły bramy wewnętrznej - używany wewnątrz systemu autonomicznego, w granicach tego systemu

`EGP` - protokoły bramy zewnętrznej - wykorzystywany w routingu między różnymi strefami autonomicznymmi. Do tej grupy zaliczamy protokół `BGP`.

Protokoły routingu typu wektor - odległość polegają na ogłaszaniu tras uwzględniając następujące parametry:
- odległość - metrykę, koszt trasy
- wektor - router następnego skoku lub interfejs wyjściowy

RIPv1, RIPv2, IGRP, EIGRP

>Router z protokołem routingu `stanu łącza`, na podstawie informacji zebranych od wszystkich pozostałych routerów, może utworzyć pełną mapę sieci, czyli topologię sieci. 

>Po osiągnięciu stanu zbieżności sieci aktualizacja stanu łącza jest wysyłana tylko wtedy, gdy w topologii nastąpi jakaś zmiana.

Zastosowanie protokołów stanu łącza:
- wielkie sieci hierarchiczne
- osiągnięcie szybkiej zbieżności jest sprawą kluczową
- administratorzy mają bardzo dużą wiedzę na temat zaimplementowanego protokołu routingu stanu łącza

`OSPF` - najpopularniejszy i postawowy protokół routingu
`IS-IS` - popularny w sieciach ISP

>Gdy w tablicy routingu znajdują się `dwa identyczne wpisy`, czyli informacje o drodze do tej samej sieci z taką samą metryką, wtedy router równomiernie rozkłada obciążenie na obie trasy. Zjawisko to znane jest jako rozkładanie obciążenia (ang. `load balancing`).

>Trasy nadrzędne nigdy nie zawierają informacji o interfejsie wyjściowym lub adresie następnego przeskoku.

Cechy protokołów routingu:
- szybkość osiągania zbieżności
- skalowalność
- klasowość lub bezklasowość
- wykorzystanie zasobów
- czas wdrożenia i koszt utrzymania

Najlepsza trasa to najniższy koszt, najniższa metryka. Metryki są różnie wyliczane przez równe protokoły routingu, dlatego metryki możemy porównywać w ramach jednego protokołu.

Przykładowe metryki:
- RIP - liczba przeskoków
- OSPF - zsumowana szerokość pasma łącza
- EIGRP - bazuje na opóźnieniu i szerokości pasma, dodatkowo możne uwzględniać obciążeniu i niezawodności łącza

## 7.2 Routing typu wektor - odległość

>Router zna tylko adresy sieciowe własnych interfejsów oraz zdalne adresy sieciowe, przez które ma dostęp do swoich sąsiadów. Routery używające routingu wektora odległości nie mają informacji o topologii sieci.

Sąsiedzi to routery bezpośrednio połączone z włączonym odpowiednim protokołem routingu.

Do wysyłania aktualizacji można wykorzystać:
- broadcast - bardzo nieefektywny, np. RIPv1
- multicast - RIPv2 i EIGRP
- unicast - wiadomość do pojednynczego sąsiada, np EIGRP

Algorytm protokołu routingu ma za zadanie:
- wysyłać i odbierać informacje o trasach
- obliczać najlepsze drogi i dodawaniu ich do tablicy routingu
- wykrywać i reagować na zmiany w topologii sieci

Wyzanczanie najlepszej trasy:
- RIP - algorytm Bellmana-Forda
- IGRP - DUAL

Protokoły wektor - odległość:
- RIPv1
  - aktualizacje wysyłane na adres rozgłoszeniowy `255.255.255.255` co 30 sekund
  - metryka opiera się na liczbie przeskoków
  - liczba przeskoków większa niż 15 to tras nieosiągalna
  - protokół klasowy
- RIPv2
  - aktualizacje przesyłane na adres multicastowy `224.0.0.9`
  - wspiera ręczną sumaryzację tras
  - stosuje mechanizm uwierzytelniania pomiędzy sąsiadami
  - max 15 hops
  - 520/UDP
  - AD 120
  - router wysyłający dane `nie uwzględna` siebie hop=>0
- RIPng - RIP new generation
  - tutaj też obowiązuje ograniczenie do 15 przeskoków
  - router wysyłający dane też siebe `uwzględna` hop=>1
- IGRP
  - broadcast, co 90 sekund
  - klasowy
  - nie wspiera uwierzytelniania
- EIGRP
  - `224.0.0.10` multicast, ograniczone i wyzwalane aktualizacje
  - potrzymanie i nawiązanie połączenia krótkim Hello, tworzy tablicę przyległości sąsiadów
  - tablica topologii wszystkich poznanych tras
  - szybkie osiąganie zbieżności
  - wsparcie dla różych protokołów warstwy sieciowej np IPv4, IPv6, IPX i Apple Talk
  - do 255 hopów
  - wykorzystuje algorytm DUAL

## 7.3 Routing RIP i RIPng

1. Aktywacja protokołu RIPv1
    
    Wyłączenie protokoły RIPv2 `no ip router rip`
2. Włączenie RIP na interfejsach należących do wskazanej sieci. Od tego momentu odbieramy i wysyłamy aktualizacje RIP co 30 sekund.

```
1. R1 (config)# router rip
2. R1 (config-router)# network 192.168.1.0
```

`show ip protocols` - wyświetla ustawienia protokołu routingu, który jest skonfigurowany na routerze.

Włączenie RIPv2

```
R1 (config)# router rip
R1 (config-router)# version 2
```

Wyłączenie autosumaryzacji

```
R1 (config)# router rip
R1 (config-router)# version 2
R1 (config-router)# no auto-summary
```

Domyślnie RIP przesyła aktualizację na wszystkie aktywne interfejsy. Zaleca się wyłączenie rozgłaszania aktualizacji na zbędnych interfejsach np LAN. Domyślne rozgłaszanie negatywnie pływa na:
- niepotrzebne wykorzystanie pasma i zasobów
- potencjalne zagrożenie bezpieczeństwa, możliwość nielegalnego wpłynięcia na zawartość tablicy routingu na routerze

Konfigurowanie pasywnego interfejsu

```
R1 (config-router)# passive-interface g0/0
```

`passive-interface default` - ustawienie wszystkich interfejsów jako pasywne 

`no passsive-interface` - ustawienie interfejsu jako aktywny, rozgłaszający na tym interfejsie

>`Uwaga:` Wszystkie protokoły routingu obsługują polecenie passive-interface.

Propagowanie trasy statycznej trasy domyślnej

```
R1 (config)# defult-information originate
```

---

Konfiguracja RIPng, RIP next generation
Drobna różnica polega na tym, że rozgłaszanie RIPng włącza się na konkretym interfejsie

```
R1 (config)# ipv6 unicast-routing
R1 (config)# interface g0/0
R1 (config-if)# ipv6 rip RIP-AS enable 
*R1 (config-if)# ipv6 rip RIP-AS default-information originate
```
\* rozgłaszanie statycznej trasy domyślnej

`show ipv6 protocols` - sprawdzenie konfiguracji RIPng

`show ipv6 route`

## 7.4 Protokoły stanu łącza

Protokoły stanu łącza, protokoły wyboru najkrótszej ścieżki opierają na algorytmie Dijkstry `SPF`(Shortes Path First). Ta grupa protokołów jest uznawana za bardziej złożone. 

Protokoły stanu łącza: `OSPF` i `IS-IS`.

Stan łącza - stan interfejsu routera

Schemat działania protokołów stanu łącza:
- wykrywanie sieci bezpośrednio podłączonych
- poznanie swoich sąsiadów, wysyłanie pakietów Hello
- tworzenie pakietu `LSP`(link-state packet) zawierającego inforomacje o directly connected i informacje o sąsiadach
- pakiety LSP są rozsyłane do sąsiadów, którzy zapamiętują to w swojej bazie i przesyłają do swoich sąsiadów
- routery wykorzystują bazę danych do budowy mapy topologii i wyznaczenia najlepszej trasy do sieci docelowych

Informacja o stanie łącza składa się z:
- adresu IP i maski podsieci interfejsu
- typu sieci np ethernet, szeregowe, punkt - punkt
- wszystkich sąsiednich routerów na tym łączu

Przyległość - jest tworzona w momencie gdy dwa routery dowiadują się, że są sąsiadami. Co jakiś czas są wysyłane pakiety hello, w celu monitorowania stanu sąsiada.

Pakiety LSP są przekazywane niemal natychmiastowo, algorytm SPF jest wykonywany zaraz po skompletowaniu danych w bazie stanu łącza.

Pakiety LSP nie muszą być wysyłane regularnie, mogą być wysyłane w momencie:
- cold start,
- po wyłączeniu routingu na tym routerze
- gdy nastąpi zmiana topologii sieci, nawiązanie lub zerwanie relacji przyległości

>Oprócz informacji o stanie łącza, w pakiecie LSP znajdują się również inne informacje – na przykład numery sekwencyjne i daty ważności – ułatwiające zarządzanie procesem zalewania. Informacje te są używane przez każdy router w celu ustalenia, czy już odebrał dany pakiet LSP od innego routera lub czy w pakiecie LSP znajdują się nowsze informacje niż te, które są w bazie danych stanu łącza (ang. `link-state database`). Dzięki temu procesowi router przechowuje w swojej bazie danych stanu łącza tylko najbardziej aktualne informacje.

Zalety protokołów routing\`u stanu łącza względem protokołów typu wektor - odległość:
- tworzenie mapy topologii sieci
- szybkie uzyskiwanie zbieżności sieci
- aktualizacje po zainstnieniu zdarzenia
- projektowanie hierarchiczne, wyskorzystują koncepcję obszarów

Wady:
- więkeszę wymagania dotyczące zasobów - pamięć i czas procesora
- większe wymagania dotyczące szerokości pasma podczas uruchamianie routerów i w niestabilnych sieciach

OSPFv2 - IPv4
OSPFv3 - IPv6 i IPv4

IS-IS

## 7.5 Tablica routingu

Sieci bezpośrednio podłączone są automatycznie dodawane do tablicy routingu zaraz po poprawnym skonfigurowaniu interfejsu (adres IP i maska podsieci).

Opisy trasy zawierają następujące informacje
- `źródło trasy` - sposób w jaki trasa została pozyskana
    - `L` - local network
    - `S` - trasa dodane ręcznie, statycznie
    - `D` - EIGRP
    - `O` - OSPF
    - `R` - RIP

- `sieć docelowa` - adres sieci docelowej i sposób połączenia
- `interfejs wyjściowy` - identyfikuje interfejs, przez który zostanie wysłany pakiet, aby dotarł dos sieci docelowej

Format wpisów sieci zdalnych:
- `źródło trasy`
- `sieć docelowa`
- `dystans administracyjny`
- `metryka`
- `następny przeskok`
- `znacznik czasowy trasy`
- `interfejs wyjściowy`

Typy tras:
- `trasy ostateczne` - to trasa, która zawiera jeden lub oba elementy: adres następnego przeskoku lub interfejs wyjściowy. Trasy bezpośrednie, dynamiczne lub lokalne są trasami ostatecznymi.
- `trasy poziomu 1` - to trasa z maską podsieci równą lub mniejszą niż klasowa maska adresu sieci np trasa sieci, trasa supersieci i trasa domyślna
- `trasy nadrzędne poziomu 1` - trasa nadrzędna poziomu 1 jest trasą 1 poziomu, posiadającą podsieci. Trasa nadrzędna nie może być trasą ostateczną.
- `trasy podrzędne poziomu 2` - to taka trasa, która jest podsiecią klasowego adresu sieciowego.

>Podobnie jak w przypadku trasy poziomu 1, źródłem trasy poziomu 2 może być sieć połączona bezpośrednio, trasa statyczna albo protokół routingu dynamicznego. Trasy podrzędne poziomu 2 są również trasami ostatecznymi.

---

>`Uwaga`: Trasa odwołująca się jedynie do adresu IP następnego skoku, a nie do interfejsu wyjściowego, musi zostać przekształcona na trasę z interfejsem wyjściowym. Na podstawie adresu IP następnego skoku wykonywane jest wyszukiwanie rekurencyjne, którego wynikiem jest trasa trasę do interfejsu wyjściowego.

`Najlepsza trasa to najdłuższ dopasowanie.`

>`Protokół IPv6` został zaprojektowany jako bezklasowy, w związku z tym wszystkie trasy poziomu 1 są trasami ostatecznymi. Nie ma tras nadrzędnych poziomu 1 ani tras podrzędnych poziomu 2.

---

<div>
<a href="chapter-06.md">Prev: Chapter 6</a>
</div>
<div align="right">
<a href="chapter-08.md">Next: Chapter 8</a>
</div>