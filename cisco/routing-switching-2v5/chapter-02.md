### Spis treści
- [Chapter 2: Podstawowe pojęcia i konfigurowanie przełacznika](#chapter-2-podstawowe-poj%c4%99cia-i-konfigurowanie-prze%c5%82acznika)
  - [2.1 Podstawowa konfiguracja przełacznika](#21-podstawowa-konfiguracja-prze%c5%82acznika)
  - [2.2 Bezpieczeństwo przełącznika: zarządzanie i implementacja](#22-bezpiecze%c5%84stwo-prze%c5%82%c4%85cznika-zarz%c4%85dzanie-i-implementacja)
    - [2.2.1 Konfiguracja SSH](#221-konfiguracja-ssh)
    - [2.2.2 Przykłady ataków w sieci LAN](#222-przyk%c5%82ady-atak%c3%b3w-w-sieci-lan)
    - [2.2.3 Najlepsze praktyki dotyczące bezpieczeństwa sieci](#223-najlepsze-praktyki-dotycz%c4%85ce-bezpiecze%c5%84stwa-sieci)
    - [2.2.4 Zabezpieczenie portów na przełączniku](#224-zabezpieczenie-port%c3%b3w-na-prze%c5%82%c4%85czniku)

# Chapter 2: Podstawowe pojęcia i konfigurowanie przełacznika

## 2.1 Podstawowa konfiguracja przełacznika

Proces uruchamiania przełącznika:
1. wykonanie procedury POST zapisanej w ROM, czyli sprawdzenie stanu CPU, DRAM i File System w Flash
2. ładowanie do pamięci `boot loadera`, uruchamiany po pomyślnie zakończonej procedurze POST
3. `Boot loader` ustawia domyślne wartości rejestrów adresujących obszar pamięci fizycznej, określenie jej wielkości i czasu dostępu
4. `Boot loadre` inicjalizuje system plików dla pamięci flash znajdującej się na płycie głównej
5. `Boot loader` wyszukuje lokalizację obrazu systemu IOS, następnie ładuje go do pamięci i przekazuje kontrolę nad przełącznikiem do IOS
    
    Lokalizacja obrazu systemu IOS w przełączniku opiera się na zmiennej środowiskowej BOOT, jeśli nie jest ona ustawiona to przełącznik przeszukuje rekurencyjnie cały system plików pamięci Flash.

6. IOS inicjalizuje interfejsy w oparciu o polecenia znajdujące się w pliku konfiguracji startowej, która znajduje się w NVRAM

Zmienną środowiskową BOOT ustawiamy za pomocą poniższego polecenia

```
Switch (config)# boot system flash:/c2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin
```

Zawartość zmiennej wyświetlamy za pomocą polecenia

```
Switch# show bootvar
```

W starszych wersjach

```
Switch# show boot
```

2.1.1.2

Uruchomienie lini poleceń boot loadera:

1. Połączenie za pomocą kabala konsolowego
2. Wyłączenie przełącznika
3. Włączenie z jednoczesnym wciśnięciem przycisku Mode do momentu, aż dioda System LED będzie świeciła się ciągle kolorem zielonym

Ten tryb umożliwia m.i.n.: formatowanie systemu plików znajdującego się na pamięci flash, ponowną instalację systemu operacyjnego oraz przywrócenie utraconego hasła.

Na przełącznikach Cisco możemy znaleźć następujące rodzaje diód LED:
- `System LED` - przełącznik jest podłączony do źródła zasilania i działa prawidłowo. Jeśli dioda nie świeci się, oznacza to, że system nie jest włączony. Jeśli dioda świeci na zielono, system działa normalnie. Jeśli dioda świeci na pomarańczowo, system jest zasilany, ale nie działa prawidłowo.
- `Redundant Power System LED` (RPS)
- `Port status LED` - zielony kolor oznacza, że został wybrany tryb stanu portu 
- `Port Duplex LED` - wybrany został tryb duplex\`u. Włączona zielona dioda przy interfejsie oznacza Full Duplex, wyłączona Half Duplex
- `Port Speed LED` - tryb prędkości portu
  - Dioda LED nie świeci się -> 10 Mb/s
  - Ciągły zielony -> 100 Mb/s
  - Migający zielony -> 1000 Mb/s

- `Power over Ethernet` PoE

Do zmiany trybów wykorzystywany jest przycisk `Mode`.

`SVI` - Switch Virtual Interface

>SVI to pojęcie związane z sieciami VLAN. Sieci VLAN są numerowanymi grupami logicznymi, do których można przypisać porty fizyczne. Konfiguracje i ustawienia użyte w stosunku do sieci VLAN są także stosowane do wszystkich portów przypisanych do tej sieci VLAN.

Utworzenie sieci VLAN i skojarzenie jej z interfejsem

```
Switch (config)# vlan 10
Switch (config-vlan)# name it-department

Switch (config)# interface fa0/1
Switch (config-if)# switchport access vlan 10
```
Sprawdzenie konfiguracji vlan i innych interfejsów

```
Switch# show ip interface brief
```

Konfigurowanie bramy domyślnej

```
Switch (config)# ip default-gateway 192.168.1.1
```

Praca w trybie full-duplex jest możliwa, gdy występuje mikro-segmentacja, czyli port przełącznika połączony jest bezpośrednio do urządzenia końcowego.

W trybie full-duplex obwód wykrywania kolizji na karcie sieciowej jest wyłączony. W full-duplex nie występują kolizje, ponieważ do komunikacji urządzenia używają dwóch oddzielnych obwodów w sieci kablowej. 

Hub -> 50-60% wydajność pasma
Przełącznik w full-duplex -> 100% skuteczności w obu kierunkach

Ustawianie trybu duplex\`u i prędkości interfejsu

```
Switch (config-if)# duplex full
Switch (config-if)# speed 100
```

Cisco zaleca ustawiać obie wartości na `auto`, w celu uniknięcia problemów z łącznością

`Auto-MDIX` - automatic medium-dependent interface crossover, funkcja pozwalająca na automatyczne wykrywanie wymaganego rodzaju połączenia kablowego, prosty czy z przeplotem

```
Switch (config-if)# mdix auto
```

```
Switch# show controllers ethernet-controller
```
 
Błędy `Input Errors`, które powodują problemy z wydajnością sieci:
- `ramka runt` - ramki krótsze od 64B minimalnej dozwolonej długości. Mogą powstać w momencie kolizji, mogą być też wygenerowane przez uszkodzoną kartę sieciową
- `ramka giant` - ramki dłuższe niż maksymalna długość ramki, wywoływane przez te same problemy co ramki runt
- `błędu CRC` - wynikają najcześciej z zakłóceń elektromagnetycznych, nieprawidołowo wykonanych połączeń, złącz, zbyt długich przewodów itp. 

```
Switch# show interfaces fa0/1
```

Output errors = collisions + late collisions

`Late collisions` - odnoszą się do kolizji, które występują po 512 bitach ramki, preambule. Najczęstszą przyczyną tego typu kolizji jest zbyt długie okablowanie.

## 2.2 Bezpieczeństwo przełącznika: zarządzanie i implementacja

### 2.2.1 Konfiguracja SSH

`K9` w nazwie wersji IOS oznacza, że wspiera on funkcje i możliwości kryptograficzne m.i.n.: obsługuje protokół SSH. Wersję IOS możemy sprawdzić za pomocą polecenia `show version`.

Konfiguracja SSH:
1. Sprawdzamy czy sprzęt wspiera SSH za pomocą polecenia `show ip ssh`
2. Ustawienie nazwy hosta i nazwy domeny IP
3. Generowanie pary kluczy RSA
4. Konfigurowanie autoryzacji użytkownika (`username admin password class`)
5. Konfiguracja lini vty (`transport input ssh`, `login local`)
6. Weryfikacja poprawności konfiguracji SSH

### 2.2.2 Przykłady ataków w sieci LAN

`Przepełnienie tablicy adresów MAC`, zalewanie sieci adresami MAC, polega na zalewaniu sieci ramkami z losowymi adresami MAC, przełącznik otrzymując ramkę z nieznanym adresem docelowym, wysyła je na wszystkie porty z wyjątkiem wejściowego. Każda tablica adresów MAC ma ograniczone rozmiary. Gdy tablica jest przepełniona przełącznik przechodzi, w tryb pracy zwany `fail-open`. W tym trybie przełącznik rozsiewa wszystkie ramki do wszystkich urządzeń, w ten sposób atakujący widzi wszystkie ramki.

Ataki na DHCP:
- wyczerpanie zasobów DHCP - zalewanie urządzenia fałszywymi żądaniami przydzielenia adresu IP, w pewnym momencie nowi klienci nie mogą uzyskać adres IP
- fałszywy serwer DHCP - nadaje poprawną konfigurację adresu IP, podając jako adres bramy domyślnej adres urządzenia kontrolowanego przez atakującego

Często te dwa ataki są ze sobą powiązane najpierw wyczerpuje się zasoby legalnego serwera DHCP, aby póżniej zmusić klientów do korzystania z fałszywego serwera DHCP.

Ataki wykorzystujące CDP.
CDP jest domyślnie włączony na urządzeniach Cisco, pakiety są wysyłane w okresowych, niekodowanych transmisjach. Jeśli nie musimy korzystać z CDP, to lepiej go wyłączyć, możemy się też ograniczych do poszczególnych portów.

```
Switch (config)# no cdp run
```

Ataki siłowe na Telnet i SSH

### 2.2.3 Najlepsze praktyki dotyczące bezpieczeństwa sieci

- wyłączenie zbędnych portów i usług
- używanie silnych haseł
- kontrola fizycznego dostępu do urządzenia
- wykorzystywanie HTTPS szczególnie do ekranów logowania
- wykonywanie na bieżąco kopii zapasowej
- szyfrowanie i zabezpieczanie danych poufnych
- częsta, regularna instalacja poprawek zabezpieczeń, aktualizacji oprogramowania
- wykorzystywanie sprzętu i oprogramowania zabezpieczającego tj zapory sieciowe
- wdrożenie polityk bezpieczeństwa
- edukacja pracowników o atakach socjotechnicznych

>Organizacje muszą zachować ostrożność przez cały czas, by bronić się przed nieustannie ewoluującymi zagrożeniami.

Podstawowe działania kontrolujące ogólny stan bezpieczeństwa sieci to audyt bezpieczeństwa i testy penetrcyjne.

### 2.2.4 Zabezpieczenie portów na przełączniku

1. Wyłączenie wszystkich nieużywanych portów przełącznika
    ```
    Switch (config)# interface range fa0/1-10
    Switch (config-if-range)# shutdown
    ```
2. Włączenie DHCP snooping. Funkcja ta dzieli porty na zaufane, czyli takie, które obsługują serwer DHCP, wysyłanają odpowiedzi DHCP. Kolejna grupa portów to niezaufane, które mogą tylko i wyłącznie wysyłać
    ```
    Switch (config)# ip dhcp snooping
    Switch (config)# ip dhcp snooping vlan 10,20
    Switch (config)# interface fa0/0
    Switch (config-if)# ip dhcp snooping trust
    ``` 

  `ip dhcp limit rate 5` -> ogranicza do 5 możliwość wysyłania fałszychych żądań przez niezaufane porty

3. Zabezpieczenie portów - określa autoryzowany/e adresy MAC, które mogą być podłączone do przełącznika, reszta jest odrzucana.
    `switchport port-security mac_address/es`
    `switchport port-security mac-address sticky`

4. Tryby naruszenia bezpieczeństwa
    `switchport port-securit violation {protected | restrict | shutdown}`
    Protected - nie jesteśmy podwiadamiani o naruszeniach bezpieczenstwa
    Restrict - jesteśmy powiadomieni o naruszeniach bezpieczeństwa
    Shutdown - domyślny, naruszenie zasad bezpieczeństwa powoduje przejści portu w stan `error-disabled`, natychmiastowe wyłączenie interfejsu

Weryfikowanie wprowadzonych zabezpieczeń
```
Switch# show port-sercurity interface fa0/1
Switch# show port-sercurity address
```

Zgodne i prawidłowe znaczniki czasu, znacznie ułatwiają analizę logów syslog i dla sprawdzania proprawności, nie przedawnienia certyfikatów cyfrowych. Protoków NTP(Network Time Protocol) jest wykorzystywany do synchronizacji zegarów w systemach komputerowych. NTP może uzyskać wzorcowy czas z:
- lokalnego zegaru wzorcowego
- internetowego zegaru wzorcowego
- GPS lub zegaru atomowego

Konfigurowanie NTP

```
R1 (config)# ntp master 1
```

```
R2 (config)# ntp server 192.168.1.1
```

Weryfikacja stanu NTP

```
R2 (config)# show ntp associations
R2 (config)# show ntp status
```

---

<div>
<a href="chapter-01.md">Prev: Chapter 1</a>
</div>
<div align="right">
<a href="chapter-03.md">Next: Chapter 3</a>
</div>