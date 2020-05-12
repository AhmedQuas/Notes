### Spis treści
- [Chapter 3: Sieci wirtualne (VLANs)](#chapter-3-sieci-wirtualne-vlans)
  - [3.1 Segmentacja sieci za pomocą VLAN](#31-segmentacja-sieci-za-pomoc%c4%85-vlan)
  - [3.2 Implementacje sieci VLAN](#32-implementacje-sieci-vlan)
    - [3.2.1 Podstawowa konfiguracja sieci VLAN](#321-podstawowa-konfiguracja-sieci-vlan)
    - [3.2.2 Łącza trunk](#322-%c5%81%c4%85cza-trunk)
    - [3.2.3 Protokół DTP](#323-protok%c3%b3%c5%82-dtp)
    - [3.2.4 Rozwiązywanie problemów z sieciami VLAN](#324-rozwi%c4%85zywanie-problem%c3%b3w-z-sieciami-vlan)
  - [3.3 Bezpieczeństwo i projektowanie sieci VLAN](#33-bezpiecze%c5%84stwo-i-projektowanie-sieci-vlan)
    - [3.3.1 Ataki na sieć VLAN](#331-ataki-na-sie%c4%87-vlan)
    - [3.3.2 Wytyczne, praktyki projektowania VLAN](#332-wytyczne-praktyki-projektowania-vlan)

# Chapter 3: Sieci wirtualne (VLANs)

## 3.1 Segmentacja sieci za pomocą VLAN

Przełączniki LAN mogą być podzielone na logiczne grupy portów zwanych VLAN-ami w celu zwiększenia wydajności sieci oraz efektywniejszego zarządzania przełącznikami i ich bezpieczeństwem. Sieci wirtualne bazują na połączeniach logicznych, a nie fizycznych.

>Każda sieć wirtualna traktowana jest jak odseparowana sieć logiczna i pakiety zaadresowane do stacji, które nie należą do tej samej sieci wirtualnej co nadawca, muszą być obsłużone przez urządzenie routujące.

>Sieci wirtualne umożliwiają implementację polityk dostępowych i bezpieczeństwa w oparciu o określone grupy użytkowników. Każdy port przełącznika może być przypisany do tylko jednej sieci wirtualnej (z jednym wyjątkiem portu, do którego podłączony jest telefon IP lub inny przełącznik).

Zalety sieci wirtualnych:
- `bezpieczeństwo` - odseparowanie wrażliwych danych od reszty sieci
- `redukcja kosztów` - wymaga mniej sprzętu, efetywniejsze wykorzystanie dostępnej przepustowości
- `lepsza wydajność` - mniej ruchu rozgłoszeniowego
- `zmniejszenie rozmiaru domen rozgłoszeniowych` 
- `zwiększenie wydajności personelu IT` - łatwiejsze zarządzanie, dodanie nowego użytkownika polega na przypisaniu go do odpowiedniej, wcześniej ustawionej grupy. Łatwiej można zidentyfikować do jakiej grupy należy dany użytkownik, jakie ma uprawnienia
- `prostszy projekt i łatwiejsze zarządzanie aplikacjami`

Poszczególne sieci VLAN należą do innych sieci IP.

Rodzaje sieci VLAN:
- `VLAN danych` - zwany także VLAN użytkowników, przenosi ruch generowany przez użytkowników
- `VLAN domyślny` - w nim znajdują się wszystkie porty przełącznika podczas pierwszego uruchomienia. Dla Cisco domyślnie jest to VLAN 1. Nie można jesj usunąć. Porty przełącznika należące do domyślnej sieci VLAN znajdują się w tej samej domenie rozgłoszeniowej. Ten ruch nazywa się też `ruchem nieoznakowanym`.
- `VLAN natywny` - VLAN przypisany do portu trunk 802.1Q. Porty te służą do łączenia przełączników, transportuje ruch oznakowany(`tagged`) i nieoznakowany.
- `VLAN zarządzający` - zdefiniowany jako sieć zapewniająca dostęp do funkcji zarządzania przełącznikiem

>Typową praktyką jest oddzielenie ruchu głosowego i zarządzającego od ruchu danych.

>Dobrą praktyką jest skonfigurowanie jako VLAN domyślny nieużywanej sieci VLAN, innej niż VLAN 1. Nie jest rzadką praktyką konfigurowanie jednej określonej sieci VLAN jako VLAN natywny dla wszystkich portów trunk w sieci.(?)

>Pomimo, że teoretycznie przełącznik może mieć więcej niż jeden VLAN zarządzający, to posiadanie kilku sieci VLAN zarządzających `zwiększa możliwości ataków` na przełącznik.

Dla obsługi VoIP jest wymagany oddzielny VLAN, dzięki temu możemy łatwiej zagwarantować:
- odpowiednią szerokość pasma
- wyższy priorytet, a czasem nawet pierszeństwo
- opóźnienie w mniejsze niż 150ms
- możliwość przekierowania z pominięciem zatorów w sieci

>`Połączenie magistralowe, VLAN trunk` lub po prostu trunk jest to połączenie typu punkt-punkt pomiędzy dwoma urządzeniami, które przenosi ruch z więcej niż jednej sieci VLAN. Połączenie trunk rozszerza zasięg sieci VLAN na całą sieć. 

>Połączenie trunk nie należy do konkretnej sieci VLAN, lecz służy jako kanał transmisyjny dla wielu sieci VLAN między przełącznikami i routerami. Trunk może być również używany pomiędzy urządzeniami sieciowymi i serwerami lub innymi urządzeniami, które wyposażone są w kartę sieciową z obsługą 802.1Q.

Jeśli ramka rozgłoszeniowa pochodzi z VLAN 10 to zostanie przekazana tylko do tych portów, które obsługują sieć VLAN 10 + oczywiście rozgłoszenie idzie na port trunk.

>Kiedy na przełączniku skonfigurowano sieci VLAN, transmisja ruchu jednostkowego, grupowego i rozgłoszeniowego z urządzenia znajdującego się w określonej sieci VLAN, `ograniczona jest tylko do urządzeń znajdujących się w tej samej sieci VLAN`.

>Nagłówek standardowej ramki Ethernet nie zawiera informacji o sieci VLAN, do której ramka należy. `Zatem kiedy ramka Ethernet jest umieszczana na łączu trunk, informacja o sieci VLAN, do której ramka należy, musi być do niej dodana.` Ten proces, zwany znakowaniem, jest wykonywany w oparciu o nagłówek IEEE 802.1Q.

>Kiedy przełącznik otrzymuje ramkę na porcie skonfigurowanym w trybie dostępu i z przypisaną siecią VLAN, `dodaje znacznik do nagłówka ramki, przelicza FCS i wysyła ramkę z dodatkowym znacznikiem przez port trunk.`

4-bajtowy znacznik VLAN:
- `typ` - 2-bajtowy identyfikator `TPID`(Tag Protocol ID), dla Ethernet 0x8100
- `priorytet` - 3-bitowa wartość, wykorzystywana do zapewnienia QoS
- `Identyfikator formatu CFI`(Canonical Form Indicator) - jeden bit, którego ustawienie umożliwia przesyłanie ramek technologii Token Ring przez łącza obsługiwane technologią Ethernet.
- VID - VLAN ID - 12 bitowa liczba pozwalająca na zapisanie do 4096 różnych ID

>Niektóre urządzenia używające łączy trunk znakują na nich ruch odbywający się w natywnej sieci VLAN. `Ruch kontrolny wysyłany w natywnej sieci VLAN nie powinien być znakowany.` Jeśli port magistrali 802.1Q odbierze oznakowaną ramkę z identyfikatorem natywnej sieci VLAN, odrzuci ją.


## 3.2 Implementacje sieci VLAN

Sieci VLAN z zakresu normalnego są numerowane od 1 do 1005, a z zakresu rozszerzonego - od 1006 do 4094.

Sieci LAN z zakresu normalnego:
- VLAN ID 1 - 1005
- stosowane głównie przez małe i średnie firmy
- VLAN ID 1002-5 zarezerwowane dla VLAN Token Ring i FDDI
- VLAN 1, 1002 i 1005 są tworzone automaycznie i nie można ich usunąć
- konfiguracja jest przechowywana w pliku vlan.dat w pamięci flash
- `VTP`(VLAN Trunking Protocol) wspomaga zarządzanie konfiguracją sieci VLAN między przełącznikami

Sieci LAN z zakresu rozszerzonego:
- umożliwia dostawcom usług(ISP?) rozwijanie infrastruktury dla obsługi większej ilości klientów
- VLAN ID 1006 - 4094
- obsługuje mniej funkcji VLAN niż te z zakresu podstawowego
- są zapisywane w konfiguracji bieżącej, a nie w vlan.dat
- VTP nie obsługuje sieci VLAN z tego zakresu

### 3.2.1 Podstawowa konfiguracja sieci VLAN

Ustawienia sieci VLAN automatycznie zapisują się w pliku vlan.dat, jednak również ze względu częstą konfigurację innych elementów, dobrą praktyką jest zapisywanie konfiguracji bieżącej do pliku startup-config. 

Poniżej przedstawiam utworzenie sieci vlan.

```
S1# configure terminal
S1 (config)# vlan 10
S1 (config-vlan)# name student
S1 (config-vlan)# end
```

Następnym krokiem jest przypisanie portów do utworzonych sieci VLAN.

>Port dostępowy `może należeć w danym momencie tylko do jednej sieci VLAN`; jedynym wyjątkiem jest tutaj port podłączony do telefonu IP. W tym przypadku do portu mamy przypisane dwie sieci VLAN: jedna dla ruchu głosowego i druga dla ruchu danych.

```
S1# configure terminal
S1 (config)# interface fa0/0
S1 (config-if)# switchport mode access
S1 (config-if)# switchport access vlan 10
```

>Polecenie `switchport access vlan` wymusza utworzenie sieci VLAN jeśli podana w poleceniu sieć VLAN nie istnieje na przełączniku.

Zmiana przynależności portu

```
S1 (config-if)# no switchport access vlan
```

>Port może mieć bardzo łatwo zmienioną przynależność do sieci VLAN. `Nie jest koniecznym uprzednie usunięcie przypisania portu do sieci VLAN zanim przypisze się go do innej sieci VLAN.`


Usuwanie sieci vlan

```
S1 (config)# no vlan 10
```

>Uwaga: Przed usunięciem sieci VLAN upewnij się, że zmieniłeś przypisanie do sieci VLAN wszystkich portów należących uprzednio do usuwanej sieci VLAN. `Wszystkie porty, które nie zostaną przypisane do innej aktywnej sieci VLAN, po usunięciu sieci VLAN, do której są przypisane, stracą połączenie z pozostałymi portami przełącznika` do momentu ponownego przypisania ich do aktywnej sieci VLAN.

`Usuwanie całej bazy vlan.dat`

```
S1 # delete flash:vlan.dat
```

`Lub jeśli vlan.dat jest w domyślnej lokalizacji.`

```
S1 # delete vlan.dat
```

Weryfikowanie poprawności konfiguracji VLAN.

```
S1# show vlan brief
S1# show interfaces f0/0 switchport
```

Zatem jeśli chcemu przywrócić ustawienia domyślne, fabryczne

```
S1 # delete vlan.dat
S2 # erase startup-config
```

### 3.2.2 Łącza trunk


>Magistrala to łącze w warstwie 2 pomiędzy dwoma przełącznikami przenoszące ruch z wszystkich sieci VLAN (chyba że ręcznie lub dynamicznie stworzona jest lista sieci VLAN dopuszczalnych na tym łączu). Aby zestawić połączenie trunk należy skonfigurować porty na obu jego końcach tym samym zestawem poleceń.

```
S1 (config)# interface f0/0
S1 (config-if)# switchport mode trunk
S1 (config-if)# switchport trunk native vlan 99
S1 (config-if)# switchport trunk allowed vlan 10,20,99
```

Powrót do ustawień fabrycznych polega na wykorzytsaniu zaprzeczenia `no`  z podanymi wcześniej interfejsami bez precyzowania vlan-id.

`switchport mode trunk` - port zmienia tryb pracy na trunk, dodatkowo włączany jest proces negocjacji protokołu DTP. `DTP próbuje wymusić proces zmiany połączenia na trunk nawet gdy port w drugim przełączniku nie zgadza się na zmianę.`

Do weryfikacji poprawności konfiguracji łącza trunk przydatne jest polecenie

```
S1 # show interfaces f0/0 switchport
```

### 3.2.3 Protokół DTP

`DTP` - Dynamic Trunking Protocol, to protokół odpowiadający za zarządzanie negocjowaniem połączenia trunk między połączonymi urządzeniami sieciowymi. Protokół ten jest własnością Cisco.

>`Uwaga :` Niektóre urządzenia sieciowe mogą niewłaściwie przekazywać ramki DTP, co prowadzić może do nieprawidłowych konfiguracji. W celu uniknięcia tego należy wyłączyć protokół DTP na portach przełącznika Cisco podłączonego do urządzenia nieobsługującego protokołu DTP.

Możemy wyłączyć DTP na wybranym interfejsie za pomocą polecenia.

```
S1 (config-if)# switchport nonegotiate
```

Wyróżniamy następujące tryby pracy interfejsu:
- switchport mode access
- switchport mode dynamic auto
- switchport mode dynamic desirable
- switchport mode trunk
- switchport mode nonegotiate

Dokładyn opis tych trybów znajduje się w podrozdziale [3.2.3.2](https://static-course-assets.s3.amazonaws.com/RSE50PL/course/module3/index.html?display=html#3.2.3.2)

### 3.2.4 Rozwiązywanie problemów z sieciami VLAN

1. Niepoprawna adresacja IP
2. Złe przypisanie vlan-ów do odpowiednich portów, przydatne polecenia 
    ```
    S1 # show vlan
    S1 # show interfaces switchport
    S1 # show mac-address-table
    S1 # show interface f0/0 switchport
    ```
3. Wykrywanie niepoprawnej konfiguracji łączy trunk
    ```
    S1 # show interface f0/0 trunk
    ```
    1.  Wykrywanie przecieków VLAN, czyli interfejsów które działają jako trunk, a nie powinny
    2.  Niezgodność natywnych sieci VLAN
    3.  Niezgodność trybów trunk
    4.  Dozwolone sieci VLAN na połączeniu trunk


>Gdy ustawienia natywnej sieci VLAN są niezgodne, pojawiają się problemy z łącznością w sieci. Ruch z sieci VLAN innych niż te ustawione jako sieci VLAN natywne przesyłany jest poprawnie przez łącze trunk, ale ten związany z sieciami VLAN ustawionymi jako natywne nie jest przesyłany poprawnie przez to połączenie.


## 3.3 Bezpieczeństwo i projektowanie sieci VLAN

### 3.3.1 Ataki na sieć VLAN

>Przeskakiwanie między sieciami VLAN pozwala na pojawienie się ruchu z jednej sieci VLAN w innej. `Podszywanie się pod przełącznik` jest jednym z rodzajów ataków typu przeskakiwania między sieciami VLAN poprzez wykorzystanie niepoprawnie skonfigurowanego portu trunk. Domyślnie połączenie trunk ma dostęp do wszystkich sieci VLAN i przepuszcza ruch dla wielu sieci VLAN, zasadniczo pomiędzy przełącznikami.

W tym ataku wykorzystujemy fakt, że atakowany przełącznik może pracować w trybie `dynamic auto`.

>Najlepszym sposobem na `uniemożliwienie` tego podstawowego rodzaju `ataku podszywania się` jest wyłączenie trybu trunk na wszystkich portach przełącznika, za wyjątkiem tych, które rzeczywiście mają zestawić połączenia trunk. Na portach, które mają zestawić połączenia trunk należy wyłączyć protokół DTP i ręcznie skonfigurować tryb trunk.

Kolejnym rodzajem ataku jest przeskakiwanie między sieciami VLAN poprzez `podwójne znakowanie`, czyli dodanie dwóch nagłówków 802.1Q. 

>Najlepszym sposobem na uniknięcie podwójnego znakowania jest upewnienie się, że VLAN natywny na portach trunk jest inny od sieci VLAN wszystkich użytkowników. A zatem, dobrą praktyką w zakresie bezpieczeństwa jest używanie jako natywnej sieci VLAN ustalonej sieci VLAN, do której nie jest przypisany żaden port w całej sieci.

`PVLAN` - Private VLAN, wprowadza podział na porty zabezpieczeone i niezabezpieczone. Ruch między portem zabezpieczeonym i niezabezpieczonym jest dozwolony, ruch między portami zabezpieczonymi jest zablokowany. Technologia stosowana w celu ukrycia dla urządzenia ruchu generowanego przez inne hosty.

```
S1 (config-if)# switchport protected
```

### 3.3.2 Wytyczne, praktyki projektowania VLAN

1. Skonfigurowanie wszystkich portów wszystkich przełączników, aby były przypisane do innych sieci VLAN niż VLAN 1. Przypisanie do sieci VLAN nazywanej czarną dziurą, która nie jest używana przez żadne urządzenie w sieci.
2. Można również wyłączyć nieużywane porty przełącznika
3. Oddzielenie ruchu zarządzającego od ruchu danych użytkownika
4. Ruch zarządzający powinien obsługiwać tylko szyfrowane sesje SSH
5. Rekomendowaną praktyką bezpieczeństwa jest zmiana natywnej sieci VLAN z domyślnego VLAN 1
    >Cały ruch kontrolny wysyłany jest do sieci VLAN 1. Dlatego, jeśli VLAN natywny zostanie zmieniony na inny niż VLAN 1, cały ruch kontrolny będzie znakowany na połączeniach trunk 802.1Q.

    `Należy upewnić się, że VLAN natywny jest taki sam na obu końcach połączenia trunk 802.1Q.`
6. Wyłączenie autonegocjacji DTP
7. Rozdzielenie ruchu VoIP i ruchu danych, głównie ze względu na QoS

---

<div>
<a href="chapter-02.md">Prev: Chapter 2</a>
</div>
<div align="right">
<a href="chapter-04.md">Next: Chapter 4</a>
</div>