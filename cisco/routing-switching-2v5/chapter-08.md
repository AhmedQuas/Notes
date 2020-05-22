### Spis treści
- [Chapter 8: Jednoobszarowy OSPF](#chapter-8-jednoobszarowy-ospf)
  - [8.1 Charakterystyka OSPF](#81-charakterystyka-ospf)
    - [8.1.1 Komunikaty OSPF](#811-komunikaty-ospf)
  - [8.2 Konfiguracja jednoobszarowego OSPFv2](#82-konfiguracja-jednoobszarowego-ospfv2)
    - [8.2.1 Koszt OSPF](#821-koszt-ospf)
    - [8.2.2 Weryfikacja OSPF](#822-weryfikacja-ospf)
  - [8.3 OSPFv3](#83-ospfv3)

# Chapter 8: Jednoobszarowy OSPF

## 8.1 Charakterystyka OSPF

`OSPF` (Open Shortest Path First) to protokół routingu `stanu łącza`, który powstał, aby zastąpić protokół routingu wektora odległości RIP. RIP źle skaluje się dla sieci, w których występują łącza z różnymi szybkościami. W takich sieciach OSPF szybciej osiąga zbieżność i ogólnie jest lepiej skalowalne.

OSPFv2 dla IPv4

OSPFv3 dla IPv6

Cechy OSPF:
- `protokół bezklasowy`
- `wydajność` - brak aktualizacji okresowych, aktualizacje występują wtedy gdy zajdą zmiany w topologii sieci
- `szybka zbieżność`
- `skalowalność` - dobrze działa zarówno w małych jak i w dużych sieciach. Routery można organizować w obszary w celu zminieszenia zużycia zasobów i wprowadzenia hierarchii
- `bezpieczeństwo` - obsługa mechanizmu uwierzytelniania MD5, za pomocą współdzielonego hasła (?)

Komponenty protokołu routingu:
- `struktury danych`:
  - baza danych przyległości, tabela sąsiadów 
    ```
    R1# show ip ospf neighbour
    ```
  - LSDB - Link-state database, tabela topologii
    ```
    R1# show ip ospf database
    ```
  - forwarding db, baza danych przekazywania - tworzy tablicę routingu
    ```
    R1# show ip route
    ```
- `komunikaty`:
  - `pakiet Hello` - ustanowienie i utrzymanie sąsiedztwa z innymi routerami OSPF
  - `DBD` - Database Description, pakiet opisu bazy danych, służy do porównaniu stanu bazy, która na wszystkich routerach powinna być taka sama
  - `LSR` - Link-state Request, pakiet żądania stanu łącza, żądanie uzyskania dodatkowych informacji o dowolnym wpisie z opisu DBD
  - `LSU` - LS Update, pakiet aktualizacji stanu łącza, odpowiedź na LSR
  - `LSAck` - LS acknowledgment, potwierdzenie stanu łącza, potwierdzenie odbioru LSU

- `algorytm` - algorytm Dijkstry, `SPF`(Shortest Path First)

Sposoby implementacji OSPF
- `jednoobszarowy` - wszystkie routery należą do jednego obszaru, zwanego rdzeniem(obszar 0)
- `wieloobszarowy` - hierarchiczna struktura, której routery łączące obszary nazywane są ABR(Area Border Routers)

>Wieloobszarowość protokołu OSPF polega na tym, że jeden duży system autonomiczny (AS) z dużą liczbą routerów można podzielić na mniejsze obszary, w celu zapewnienia hierarchiczności routingu. `Zastosowanie hierarchiczności` sprawia, że operacje obciążające procesor routera, takie jak np. przeliczanie bazy danych topologii, wykonywane są w obrębie jednego obszaru, nie wpływając na inne obszary, przy czym nadal występuje routing między tymi obszarami.

Korzyści ze stosowania hierarchicznej topologii:
- zmniejszenie tablic routingu
- zredukowana liczba aktualizacji stanu łącza
- zredukowana częstotliwość obliczeń algorytmu OSPF


### 8.1.1 Komunikaty OSPF

- `multicastowy mac`
  
  01-00-5E-00-00-05 lub 01-00-5E-00-00-06

- `adres IP`

  224.0.0.5 lub 224.0.0.6

- `nagłówek OSPF`
  
  router ID i area ID

- `dane specyficzne dla danego typu pakietu`

>Interwał czasu nieaktywności - to wyrażony w sekundach czas, w którym router nasłuchuje obecności sąsiada, zanim zostanie on uznany za wyłączony. Domyślnie interwał czasu nieaktywności to czterokrotność interwału Hello.

`DR` - Router desygnowany, jest to punkt zbierania oraz dystrybucji wysyłanych i odbieranych LSA i wybierany jest w wielodostępowych sieciach OSPF

`BRD` - Zapasowy router desygnowany

`DROTHER` to router, który nie jest ani DR, ani BDR.

Stany protokou OSPF:
- `Down` - brak odebranych pakietów, wysyłanie pakietów Hello
- `Init` - odebranie Hello przez sąsiada, zawierają one ID routera wysyłającego
- `Two-way` - wybranie DR i BDR
- `ExStart` - Negocjacja relacji master/slave
- `Exchange` - wymiana pakietów DBD
- `Loading` - w celu uzyskania dodatkowych informacji o trasach wykorzystuje się LSR i LSU
- `Full` - uzyskanie zbieżności

## 8.2 Konfiguracja jednoobszarowego OSPFv2

Włączenie protokołu OSPF

```
R1 (config)# router ospf 1
```
1 - lokalny id procesu ospf, na innych routerch nie musi być identyczny, aby ustanowić relację sąsiedztwa. Id procesu może być liczba z zakresu 1 - 65535

Identyfikator routera jest potrzebny, aby rouetr prawidłowo funkcjonował w domenie ospf. Może być nadany wprost przez administratora lub przypisany automatycznie przez router. Numer ten służy do jednoznacznej identyfikacji routera i jest również wykorzystywany w procesie wyboru routera desygnowanego (wyższy id = router DR, jeśli priorytety są takie same)

Jawne ustawienie id rouera

```
R1 (config-router)# router-id 1.1.1.1
```

>Parametr id-routera jest 32-bitową wartością, zapisaną w postaci adresu IPv4. Jest to najbardziej zalecana metoda przypisania identyfikatora routera.

Wyczyszczenie procesu OSPF, zerwanie istniejących przyległości

```
R1# clear ip ospf process
```

Ustawienie interfejsu loopback jako identyfikatora routera

```
R1 (config)# interface loopback0
R1 (config-if)# ip address 1.1.1.1 255.255.255.255
```

>Uwaga: Niektóre, starsze wersje IOS nie rozpoznają polecenia router-id ; wtedy najlepszym sposobem ustawienia identyfikatora na tych routerach jest użycie interfejsu loopback.

Wskazywanie interfejsów, które biorą udział w procesie routingu w danym obszarze OSPF.

>Można użyć dowolnej wartości ID obszaru, jednak `dobrą praktyką` jest, aby dla jednoobszarowego OSPF używać wartości `0 jako numeru obszaru.` Łatwiej jest taką sieć dostosować później do obsługi wieloobszarowego OSPF.

>Każdy interfejs na routerze, na którym skonfigurowano adres pasujący do adresu podanego za komendą network, zostaje dołączony do protokołu i od tej pory może wysyłać i odbierać pakiety OSPF.

Można również bezpośrednio wskazać interfejs podając adres ip interfejsu i 0.0.0.0 jako maskę blankietową.

```
R1 (config-router)# network 192.168.1.0 0.0.0.255 area 0
R1 (config-router)# network 192.168.2.1 0.0.0.0 area 0 
```

>Domyślnie komunikaty OSPF wysyłane są przez wszystkie interfejsy dołączone do protokołu OSPF. `Jednak w praktyce` powinny one być wysyłane tylko na tych interfejsach, na których jest połączenie z innym routerem OSPF.

Niepotrzebne wysyłanie komunikatów to:
- nieefektywne wykorzystanie przepustowości
- nieefektywne wykorzystanie zasobów
- większe zagrożenie bezpieczeństwa

Konfiguracja pasywnych interfejsów

```
R1 (config-router)# passive-interface g0/0
```

>Przez pasywne interfejsy nie ma możliwości ustanowienia przyległości z sąsiadami. Dzieje się tak, ponieważ pakiety stanu łącza nie mogą być wysyłane ani potwierdzane na tym interfejsie.

Ustawienie g0/0 jako jedynego interfejsu który przsyła komunikaty OSPF. Wszystkie inne są ustawione jako pasywne.

```
R1 (config-router)# passive interface default
R1 (config-router)# no passive interface g0/0
```

### 8.2.1 Koszt OSPF

>Metryka jest miarą nakładu wymaganego do przesłania pakietów przez dany interfejs. Im mniejszy koszt tym lepsza trasa do celu. 

Koszt interfejsu jest odwrotnie proporcjonalny do jego przepustowości.

Koszt = przepustowość referencyjna / przepustowość interfejsu

`Zaokrąglanie zawsze w dół`.

Domyślnie przepustowość referencyjna wynosi 10^8 => 100 Mbps.

Koszt musi być liczbą całkowitą dlatego dla łącz o przpustowości większej niż 100 Mbps koszt zawsze będzie wynosił jeden, oczywiście zakładając, że wykorzystujemy domyślną przepustowość referencyjną.

Koszt łącza to suma kosztów poszczególnych odcinków.

Zmiana domyślnej przepustowości referencyjnej na 1Gbps.

```
R1 (config)# auto-const reference bandwidth 1000
```

Ustawienie wartości szerokości pasma - ta wartość nie wpływa na rzeczywistą prędkość, natomiast jest wykorzystywana w procesie obliczania kosztu trasy.

```
R1 (config)# interface s0/0/1
R1 (config-if)# bandwidth 64
```

Ręczne ustawianie kosztu trasy

```
R1 (config)# interface s0/0/1
R1 (config-if)# ip ospf cost 1
```

### 8.2.2 Weryfikacja OSPF

Sprawdzenie czy router utworzył relację sąsiedztwa z innymi routerami

```
R1 # show ip ospf neighbour
```

Routery nie mogą utworzyć przyległości z ospf jeżeli:
- routery znajdują się w innych podsieciach, ich maski się nie zgadzają
- interwały hello i dead time nie zgadzają się
- różne typy sieci ospf
- brak, lub rozgłaszana jest nieprawidłowa sieć

Weryfikacja najważniejszych parametrów konfiguracyjnych

```
R1 # show ip protocols
```

Sprawdzenie szczegółowych informacji dotyczących procesu OSPF takich jaki process ID i router ID.

```
R1 # show ip ospf
```

Wylistowanie interfejsów należących do OSPF.

```
R1 # show ip ospf interface
R1 # show ip ospf interface brief 
```

## 8.3 OSPFv3

Funkcjonalność OSPFv3 zapewnia obsługę obydwu protokołów, IPv4 oraz IPv6.

Podobieństwa OSPFv2 i OSPFv3:
- bezklasowy protokół stanu łącza
- algorytm routingu
- metryka
- podział na obszary
- typy pakietów
- mechanizm wykrywania sąsiadów
- proces wyboru DR i BDR
- ID routera

Różnice między OSPFv2 a OSPFv3:
- w OSPFv3 komunikaty są wysyłane na adres link-local interfejsu wyjściowego
- adres grupowy dla wszystkich routerów FF02::5
- adres grupowy dla DR/BDR FF02::6
- ogłaszanie sieci `network` vs `ipv6 ospf process-id area area-id`
- trzeba włączyć routing IPv6 `ipv6 unicast-routing`
- v3 używa uwierzytelniania IPv6, IPSec

Przejście do trybu konfiguracji protokołu OSPF

```
R1 (config)# ipv6 router ospf 10
R1 (config-rtr)# router-id 1.1.1.1
```

Wyczyszczenie procesu OSPF

```
R1 # clear ipv6 ospf process
```

Aktywowanie OSPFv3 na interfejsie odbywa się bezpośrednio na poziomie konfiguracji interfejsu, a nie z poziomu trybu konfiguracji OSPF.

```
R1 (config)# interface s0/0/1
R1 (config-if)# ipv6 ospf 10 area 0
```

>Pakiety Hello domyślnie wysyłane są co 10 sekund w sieciach wielodostępowych oraz punkt-punkt, natomiast co 30 sekund w segmentach sieci NBMA (Frame Relay, X.25, ATM). Czas uznania za nieczynny jest domyślnie cztery razy dłuższy od interwału hello.

>W sieciach wielodostępowych OSPF wybiera tzw. `router desygnowany`, który działa jak punkt gromadzenia i dystrybucji wysyłanych i odbieranych pakietów LSA. Wybierany jest także router BDR, który przejmuje rolę routera DR, gdy ten ulegnie awarii. Pozostałe routery nazywane są DROTHERs.

---

<div>
<a href="chapter-07.md">Prev: Chapter 7</a>
</div>
<div align="right">
<a href="chapter-09.md">Next: Chapter 9</a>
</div>