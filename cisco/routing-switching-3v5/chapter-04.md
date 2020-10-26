### Spis treści
- [Chapter 4: Bezprzewodowe sieci LAN](#chapter-4-bezprzewodowe-sieci-lan)
  - [4.1 Wprowadzenie do sieci bezprzewodowych](#41-wprowadzenie-do-sieci-bezprzewodowych)
  - [4.2 WLAN vs LAN](#42-wlan-vs-lan)
  - [4.3 Komponenty sieci WLAN](#43-komponenty-sieci-wlan)
  - [4.4 Topologie 802.11](#44-topologie-80211)
  - [4.5 Struktura ramki 802.11](#45-struktura-ramki-80211)
  - [4.6 Działanie sieci bezprzewodowej](#46-działanie-sieci-bezprzewodowej)
  - [4.7 Zarządzanie kanałem transmisji](#47-zarządzanie-kanałem-transmisji)
  - [4.8 Zagrożenia w sieci WLAN](#48-zagrożenia-w-sieci-wlan)
  - [4.9 Konfiguracja bezprzewodowej sieci LAN](#49-konfiguracja-bezprzewodowej-sieci-lan)

# Chapter 4: Bezprzewodowe sieci LAN

## 4.1 Wprowadzenie do sieci bezprzewodowych

`WLAN` - Wireless LAN, sieć bezprzewodowa

`Wi-Fi` - Wirelss Fidelity - standard IEEE 802.11

`WPS` - Wi-Fi Protected Setup, funkcja uproszczająca wstępną procedurę podłączenia nowego urządzenia do sieci

Zalete łączności bezprzewodowej:
- `większa elastyczność` - dostęp do internetu nie tylko przy biurku,
- `wzrost produktywności i wygody pracowników`,
- `redukcja kosztów` - nie trzeba modyfikować, reorganizować istniejącej infrastruktury,
- `możliwość rozwoju i przystosowania do zmieniających się wymagań` - bezinwazyjne dodawanie innych urządzeń

Technologie bezprzewodowe:
- `WPAN` - Wirelsss Personal Area Network, zasięg do kilku metrów: Bluetooth(IEEE 802.15), Wi-Fi Direct(uproszczone udostępnianie mediów między urządzeniami)
- `WLAN` - Wireless LAN, bezprzewodowa sieć LAN
- `WWAN` - Wireless Wide Area Network, obejmują obszar metropolii, szerokopasmowe łącze satelitarne lub innym przykładem są hierarchiczne sieci komórkowe czy WiMAX

`WiMAX` - Worldwide Interoperability for Microwave Access, standard IEEE 802.16 zapewniający szerokopasmowy dostęp do 50 km

Szerokopasmowa sieć komórkowa:
- `2G` - GSM, CDMA, TDMA
- `3G`- UMTS, CDMA2000, EDGE, HSPA+
- `4G` - WiMAX lub LTE

`Szerokopasmowe łącze satelitarne` - zapewnia dostęp do odległych lokalizacji przy pomocy kierunkowej anteny satelitarnej zwróconej w stronę satelity znajdującej się na orbicie geostacjonarnej Ziemi. Technologia wymaga bezpośredniej widoczności z satelitą, zwykle droższa od innych.

Przydział częstotliwości zależy od `ITU-R` - International Telecomunication Union - Radiocommunication Sector

`Pasmo` - zakres częstotliwości

Pasmo ISM - `Industrial, Scientific, and Medial` - pasmo przeznaczone do dowolnych zastosowań. W Polsce nie możemy przekroczyć dozwolonej mocy promieniowania - `100mW, czyli 20dbm` - decybeli względem 1 mW.

Komunikacja bezprzewodowa występuje w zakresie fal radiowych, czyli od 3 Hz do 300 GHz. Ten obaszar jest podzielony na częstotliwości radiowe i mikrofale.

Sieć WLAN operuje w paśmie 2,4 GHz(UHF), 5 GHz(SHF) i 60 GHz(EHF). Te pasma należą do zakresu mikrofal.

802.11n - do osiągnięcia większych prędkości zastosowano większą liczbę anten nadawczych i odbiorczych, technologia MIMO(Multiple-input and multiple-output)

Urządzeni dwupasmowe - urządzenie mogące pracować w standardzie 2,4 GHz i 5 GHz

802.11ad - WiGig, standard operujący na 3 pasmach(tri-band) częstotliwości: 2,4 GHz, 5 GHz i 60 GHz(do 7 Gb/s, sygnał nie może przechodzić przez ściany, technologia line-of-site, następuje przełączenie do niższego pasma)

Obsługa środowisk mieszanych powoduje ograniczenie oczekiwanej przepustowości.

## 4.2 WLAN vs LAN

| Cecha                     |     WLAN                |                   LAN    |
|---                        |:---                  |:---                          |
|Warstwa fizyczna           |Fale radiowe(RF)         |Łącze kablowe                 |
|Metoda dostępu do sieci    |Unikanie kolizji CSMA/CA         |Wykrywanie CSMA/CD kolizji            |
|Dostępność                 |Każde urządzenie z bezprzewodową kartą sieciową w zasięgu AP      |Wymagane połączenie kablowe |
|Zakłócenia sygnału     |Występują      |Są, jednak nie są aż tak istotne      |
|Regulacje |Mogą występować dodatkowe regulacje ustanowione przez władze lokalne|Określa IEEE|

Ramki w WLAN docierają do każdego, kto może odebrać sygnał. Sieci WLAN są czułe na zakłócenia pochodzące od innych pól. Oddalając się od źródła sygnału, wzrastają zakłócenia i siła odebranego sygnału, do momentu aż całkowicie zaniknie.

`AP` - Access Point, punkt dostępu, odpowiednik przełącznika w sieciach ethernetowych

> W sieciach WLAN występuje znacznie większy problem zapewnienia prywatności, ponieważ fale radiowe mogą wychodzić poza przewidywaną lokalizację.

## 4.3 Komponenty sieci WLAN

Do wdrożenia sieci bezprzewodowej potrzebujemy następujących komponentów:
- urządzeń końcowych z bezprzewodowymi kartami sieciowymi (nadajnik/odbiornik i sterownik)
- urządzeń infrastruktury - router bezprzewodowy lub AP

Routery bezprzewodowe w sieciach domowych często łączą funkcje:
- AP, 
- przełącznika ethernet,
- routera

Bezprzewodowe punkty dostępowe:
- `Autonomous AP` - samodzielne urządzenia, działające niezależnie od pozostałych, wymagają z osobna ręcznej konfiguracji i zarządzania,
- `WLC i Lightweight AP` - punkty dostępowe na bazie kontrolera, nie wymagające ręcznej konfiguracji, zarządzane za pomocą serwera
- `Klastrowanie` - zarządzanie odbywa się z jednego miejsca, jednak bez konieczności kupowania kontrolera
    >Administrator może zobaczyć ich rozmieszczenie jako jedną sieć bezprzewodową, zamiast zestawu osobnych urządzeń bezprzewodowych.

    Wymagane warunki, do utworzenia klastra:
    - włączenie na obu AP trybu klastrowania
    - AP mają ustawioną tą samą nazwę klastra
    - AP podłączone do tego samego segmentu sieci
    - AP używają tego samego trybu łączności radiowej np 802.11n

>Większość AP przeznaczonych dla przedsiębiorstw obsługuje PoE.

`SPS` - Single Point of Setup, pojedynczy punkt konfiguracji

`Cisco Meraki` - rozwiązanie oparte na chmurze, zaprojektowane w ceu uproszczenia wdrażania sieci bezprzewodowych, pozwana na centralne zarządzanie, podgląd i kontrolę bez konieczności zakupu kontrolera.

> Jedynie dane związane z zarządzaniem urządzeniami przechodzą przez chmurę Meraki.

Architektura Cisco Meraki:
- punkty dostępu Cisco MR Cloud Managed Wireless,
- `MMC` - Meraki Cloud Controller, usługa oparta na chmurze, stosowana w celu monitorowania, optymalizaji i raportowania zachowań w sieci,
- panel do zarządzania Meraki, zwykła przeglądarka i wejście pod dany adres WWW

Architektura Cisco Unified:
- `WLC` - Wireless LAN Controller, kontrola AP za pomocą kontrolera WLAN,
- `Lightweight APs` - Lekkie punkty dostępowe, AP zapewniające hostom dostęp do sieci bezprzewodowej

>Cisco OfficeExtend - Cisco OfficeExtend access point (`Cisco OEAP`) provides secure communications from a Cisco WLC to a Cisco AP at a remote location, seamlessly extending the corporate WLAN over the Internet to an employee’s residence.

Lepsze zasięgi sieci są możliwe do uzyskania po dołączeniu anten zewnętrznych. Najczęściej wykorzystywane anteny w AP:
- `dookólne anteny` - fabrycznie montowane w urządzeniach, zapewniają 360-stopniowy zasięg, całkiem dobrze spisują się na otwartych przestrzeniach 
- `anteny kierunkowe` - koncentrują sygnał radiowy w określonym kierunku, zapewniają większą moc sygnału w danym kierunku, jednocześnie zmniejszając w pozostałych
- `Anteny Yagi` - rodzaj anteny kierunkowej przeznaczonej do użycia w sieciach Wi-Fi na dużych dystansach: przesłanie sygnału do sąsiedniego budynku, czy zwiększenie zasięgu zewnętrznych hotspotów
  
## 4.4 Topologie 802.11

Topologie w WLAN, tryby:
- `ad hoc` - łączenie urządzeń bezprzewodowych bez pośrednictwa AP, peer-to-peer, przykłady: Wi-Fi Direct, na podobnej zasadzie działa Bluetooth.
    
    Pewną odmianą sieci ad hoc jest tworzenie hotspota na komórce(dostęp do internetu za pośrednictwem sieci komórkowej) określanej czasami jako Tethering, Portable Hotspot.

  `IBSS` - Independent BSS

- `infrastrutury` - klienci łączą się ze sobą za pośrednictwem AP, a AP jest połączone z resztą infrastrukury(część kablowa)
  - `BSS` - Basic Service Set, jeden punkt dostępowy łączący wszystkie urządzenia klienckie.
    
    `BSSID` - Basic Server Set Identifier, adres MAC służący do identyfikacji każdego BSS, oficjalna nazwa BSS, która jest powiązana tylko z jednym AP.
  - `ESS` - Extended Service Set, łączenie kilku BSS, w jeden obszar dystrybucyjny(`DS` - Distribution System) przy pomocy połączenia kablowego. 
    - klienci z bezprzewodowi z jednego BSS, BSA(BS Basic Service Area) mogą komunikować się z klientami w innym BSA w ramach jednego ESS. 
    - przemieszczanie się klientów w ramach BSS będących w jednym ESA(Extended Service Area), nie powoduje problemów z połączeniem
    - zazwyczaj ESA obejmuje kilka nakładających się BSS.
  
    `SSID` - Service Set Identifier, służy do identyfikacji danego ESS, w obrębie którego każdy BSS jest identyfikowany przy pomocy BSSID.

    >Ze względów bezpieczeństwa, ESS może rozsyłać także dodatkowe SSID, w celu zapewnienia różnych poziomów dostępu do sieci.

>Nakładanie BSA w systemie ESS, powinno wynosić w przybliżeniu 10% - 15%.

## 4.5 Struktura ramki 802.11

Struktura ramki 802.11:
- nagłówek
  - `pole kontrolne ramki` - identyfikuje rodzaj ramki bezprzewodowej: 
    - `wersja protokołu` - wersja stamdardu 802.11
    - `typ ramki` - określa funkcję ramki:
      - `zarządzania 0x0` - stosowana do utrzymania łączności np. wyszukiwanie,uwierzytelnianie, powiązanie z AP 
      - `kontrolna 0x1` - wspomaganie wymiany danych pomiędzy bezprzewodowymi urządzeniami klienckimi
      - `danych 0x2` - przesył właściwych danych 
    - `ToDS FromDS` - czy ramka jest skierowana do systemu dystrybucyjnego, czy może go opuszcza, dla klientów bezprzewodowych skojarzonych z AP. `Czyli jakby coś wychodziło poza AP?` 
    - `więcej fragmentów` - określa, czy istnieją następne fragmenty. Stosowane dla ramek danych i zarządzania 
    - `ponów` - czy ramka powinna być ponownie przesłana. Stosowane dla ramek danych i zarządzania
    - `zarządzanie energią` - określa czy urządzenie działa w trybie aktywnym, czy też w trybie oszczędzania energii
    - `więcej danych` - informuje urządzenia działające w trybie oszczędziania energii, że AP ma do wysłania więcej ramek
    - `bezpieczeństwo` - informuje, czy w przypadku ramki zastosowano szyforwanie i uwierzytelnianie
    - `zarezerwowane`
  - `czas trwania` - do określenia czasu pozostałego do odbioru następnej ramki
  - `adres 1` - zazwyczaj adres MAC urządzenia odbierającego lub AP
  - `adres 2` - zazwyczaj adres MAC urządzenia wysłającego lub AP
  - `adres 3` - docelowy adres MAC(np brama domyślna)
  - `sekwencja kontrolna` - numer sekwencyjny(kolejny numer każdej ramki) i numer fragmentu(numer fragmentu podzielonej ramki)
  - `adres 4` - stowsowany w trybie ad hoc
- dane - zawiera transmitowane dane
- FCS - Frame Check Sequence, pole sumy kontrolnej, służy do wykrywania błędów

Podtypy ramek typu zarządzania:
- `Association request 0x00` - klient wysyła ramkę, w celu poinformowania AP, że chce nawiązać z nim połączenie, ramka zawiera SSID sieci i obsługiwane szybkości
- `Association response 0x01` - odpowiedź AP do klienta o akceptacji/odrzuceniu 0x00. W przypadku akceptacji zawiera `association ID` i obsługiwane przez AP prędkości transmisji
- `Reassociation request 0x02` - urządzenie wyjdzie z poza zasięg bierzącego AP i znajdzie nowy z mocniejszym sygnałem
- `Reassociation response 0x03`
- `Probe request 0x04` - wysyłana przez klienta gdy żąda onon informacji od innego klienta bezprzewodowegi
- `Probe response 0x05`
- `Beacon frame 0x08` - ramka sygnału nawigacyjnego, wysyłana w określonych odstępach czasowych, `regularnie` w celu ogłoszenia swojej obecności, zawiera nazwę SSID, obsługiwane standardy i ustawienia zabezpieczeń. Ramki tego typu są stosowane w celu poinformowania klientów jakie sieci i jakie punkty dostępowe są dostępne na danym obszarze
- `Deassociation 0x0A` - wysłane z urządzenia, które chce zakończyć połączenie. W AP usuwane z tabeli asocjacji i skasowana zostaje przydzielona do niego pamięć
- `Ramka uwierzytelnienia 0x0B` - klient wysyła do AP ramkę zawierającą jego informacje uwierzytelniające
- `Ramka usuwająca uwierzytelnienie 0x0C` - wysłana przez klienta, który chce zakończyć połączenie z innym klientem bezprzewodowym 

Podtypy ramek typu sterowania:
- `Request to Send 0x1B`, RTS wraz z CTS zapewnia AP dodatkową metodę ograniczania kolizji tzw. two-way handshake. Klient zanim zacznie wysyłać ramki z danymi, wysła ramkę RTS
- `Clear to Send 0x1C`, CTS - odpowiedź AP na RTS, informuje, że kanał jest wolny i można wysyłać ramkę z danymi. 
     
     >Zawiera ona wartość opóźnienia czasowego, w ciągu którego urządzenia nadawcze nie powinny przesyłać żadnych ramek. To opóźnienie czasowe zmniejsza ryzyko, że jakiś inny klient bezprzewodowy rozpocznie transmisję w czasie, gdy klient który wysłał wcześniej żądanie dostępu do medium, przesyła swoje dane.

- `Acknowledgment`, ACK, potwierdzenie, że klient otrzymał bez błędu ramkę z danymi

## 4.6 Działanie sieci bezprzewodowej

Sieć WiFi - medium współdzielone, praca w trybie half-duplex, co daje nam możliwość nadawania i odbierania na tym samym kanale radiowym. Niestety nadając nie możemy jednocześnie nasłuchiwać, dlatego nie możemy wykryć kolizji

>Aby rozwiązać problem, IEEE opracowało dodatkowy mechanizm zapobiegania kolizjom, zwany Funkcją koordynacji dostępu (ang. `Distributed Coordination Function`, DCF). Zgodnie z tym mechanizmem, klient wysyła ramki `tylko wówczas, gdy kanał jest czysty`. Każda transmisja musi być potwierdzona; jeśli klient nie otrzyma takiego potwierdzenia, przyjmuje założenie, że wystąpiła kolizja i ponawia próbę wysłania po określonym, losowym czasie.

`CSMA/CA` = RTS + CTS

> `Pozostałe urządzenia`, podłączone do punktu dostępu, odbierają ramkę `CTS`, po czym wstrzymują przesyłanie swoich danych do węzła na czas tej transmisji.

Łączenie z AP - trójstopniowy proces kojarzenia:
- wykrycie nowego AP,
- uwierzytelnienie w AP,
- skojarzenie w AP - tu muszą się zgadzać pewne parametry

Parametry asocjacji:
- `SSID` - kilka routerów może współdzielić ten sam SSID, używane przez klientów do wyodrębnienia danej sieci spośród innych w tej samej lokalizacji 
- `Hasło` - wymagane do uwierzytelnienia się klienta, jeden z elementów, które tworzą klucz szyforwania
- `Tryb sieci` - standardy WLAN, AP mogą działać w trybie mieszanym - umożliwiając korzystanie z różnych standardów, daje to większą elastyczność, jednak spowalnia komunikację
- `Tryb zabezpieczeń` - WEP, WPA, `WPA2`
- `Kanał` - pasmo częstotliwości używane do transmisji danych
- `Tryby pracy ze względu na częstotliwość` - 2,4 GHz i/lub 5 GHz

Wykrywanie dostępu:
- `tryb pasywny` - urządzenie nasłuchuje ramek beacon(nawigacyjnych),
- `tryb aktywny` - urządzenie wysyła rozgłoszenie na wszystkich kanałach ramkę sondującą - Probe Request 0x04, podaje SSID i obsługiwane standardy. Stosowane w przypadku, gdy AP nie rozgłasza SSID, czyli nie wysyła ramek Beacon.
  
  >Klient bezprzewodowy może także wysłać rozgłoszeniowo sondującą ramkę żądania, w której nie ma ustawionego określonego SSID. Ma to na celu wykrycie wszystkich pobliskich sieci WLAN. Z kolei na taką ramkę nie odpowiadają AP z wyłączonym SSID broadcast.

Mechanizmy uwierzytelniania:
- `otwarte` - brak uwierzytelniania, poziom zerowy, bez podawania hasła, wysyłamy do AP informację "uwierzytelnij mnie", mało bezpieczne
- `uwierzytelnianie z kluczem współdzielonym` - bazuje na kluczu, który jest współdzielony między klientem, a AP

Proces uwierzytelniania klienta zależy od zabezpieczeń stosowanych w sieci(WEP, WPA, WPA-2).

Etap asocjacji finalizuje ustawienia i ustanawia połączenie na poziomie warstwy łącza danych między klientem a AP.

>Punkt dostępu mapuje logiczny port (identyfikator skojarzenia, AID) do klienta bezprzewodowego. `Wartość AID jest równoważna z numerem portu na przełączniku` i umożliwia w tej infrastrukturze przełącznika kierowanie ramek do odpowiedniego klienta.

## 4.7 Zarządzanie kanałem transmisji

>Jeśli wykorzystanie konkretnego kanału jest zbyt intensywne, może on zostać przesycony. Nasycenie medium bezprzewodowego pogarsza jakość komunikacji.

Techniki pozwalające zmniejszyć nasycenie kanałów, rozproszyć widmo:
- `DSSS` - Direct-sequence spread spectrum, rozpraszanie widma sekwencją bezpośrednią, przekształcenia na sygnał o większej szerokości pasma, co zwiększa odporność na zakłócenia. 
  
  Zastosowania: 802.11b, telefony bezprzewodowe działające w paśmie 900 MHz, 2,4 GHz, 5 GHz, GPS i sieci komórkowe CDMA

- `FHSS` - Frequency-hopping spread spectrum, rozpraszanie widma przeskokami częstotliwości, szybkie przełączanie sygnału fali nośnej między wieloma kanałami częstotliwości. Nadawca i odbiorca muszą być ze sobą zsynchronizowani. Bardziej efektywny niż DSSS, zmniejsza przeciążenia kanału. 
  
  Zastosowania: walkie-tolkie, Bluetooth, częśc telefonów komrókowych pracujących w paśmie 900 MHz i wykorzystywany w oryginalnym standardzie 802.11

- `OFDM` - Orthogonal ferquency - division multiplexing, podbne do FHSS, jednak w ramach jednego podkanału tworzy podkanały

  Zastosowanie: 802.11a/g/n/ac

Nieco o kanałach w paśmie 2,4 GHz
Pasmo podzielone jest na kanały o szerokości 22 MHz, a odległość między nimi wynosi 5 MHz. 

Ameryka północna - 11 kanałów

Europa - 13 kanałów

Taki dobór szerokości i odległości między kanałami, powoduje nakładanie się sąsiednich kanałów.

Kanały nie nakładające się np. 1, 6, 11

Nakładanie się sygnałów, interferencja =  zniekształcenie sygnału

Łączenie kanałów, zwiększa przepustowość efektywną zastosowanie dwóch kanałów jednocześnie w celu dostarczania danych

Planowanie wdrażania sieci WLAN:
- liczba użytkowników,
- oczekiwana przepustowość,
- odpowiednia moc transmisji, obszar pokrycia,
- gdzie AP mają być podpięte do infrastruktury kablowej,
- AP powinny być umieszczone nad przeszkodami,
- AP na środku obszaru pokrycia - anteny dookólne,
- AP tam, gdzie spodziewamy się użytkowników np sale konferencyjne

## 4.8 Zagrożenia w sieci WLAN

>Sieć WLAN jest dostępna `dla każdego, kto znajdzie się w zasięgu punktu dostępowego` i będzie mieć odpowiednie poświadczenia, umożliwiające pomyślne skojarzenie urządzeń. Atakujący nie musi fizycznie znajdować się w pomieszczeniach roboczych, aby uzyskać dostęp do sieci WLAN, wystarczy bezprzewodowa karta sieciowa oraz znajomość technik włamywania się do sieci.

Zagrożenia w sieciach LAN:
- `intruzi bezprzewodowi` - nieautoryzowani użytkownicy próbujący uzyskać dostęp do zasobów sieciowych, w celu ochrony stosujmy mocne uwierzytelnienie
- `podsłuch` - przechwytywanie danych, mocne szyfrowanie
- `ataki typu DoS`
  - błędna konfiguracja - pomyłka administratora
  - celowe wprowadzanie interferecji w komunikacji bezprzewodowej
  - przypadkowa interferencja
  - ataki DoS wykorzystujące ramki zarządzania:
    - fake deassociation - fałszywe rozłączenia, klienci próbują ponownie się połączyć, co powoduje duży ruch w sieci
    - zalewanie CTS - wysyłanie fałszywych ramkek CTS do nieistniejącego urządzenia, w tym czasie inne urządzenia wstrzymują na pewien czas transmisję 
- `obce punkty dostępu` - nieautoryzowane AP, które są wykorzystywane do złych celów:
  - AP podłączony do sieci firmowej bez wyraźnego zezwolenia, może to być AP podłączony do Windowsa
  - AP podłączony do sieci i mający za zadanie przechwytywać wybrane pakiety, uzyskania dostępu do zasobów w sieci, czy też do przeprowadzenia ataku man-in-the-middle
- `MITM` - Man-in-the-middle:
  - `złośliwy bliźniak` - dodatkowy AP z takim samym SSID jak prawdziwy AP, szczególnie jest to widoczne w sieciach otwarych: kawiarnie, lotniska, stacje kolejowe. Dla niektórych urządzeń ruch jest kierowany od bliźniaka do prawdziwego AP


>Cisco `Management Frame Protection` (MFP) zapewnia pełną zapobiegawczą ochronę przed podszywaniem się intruza pod urządzenie lub generowaniem fałszywych ramek zarządzania. Z kolei oprogramowanie `Cisco Adaptive Wireless IPS` wspomaga te rozwiązania poprzez wczesne wykrywanie sygnatur ataków na podstawie ich dopasowania.

>Bezprzewodowym IPS (ang. Intrusion Prevention System, System zapobiegania włamaniom). W skład takiego systemu wchodzą skanery identyfikujące obce punkty dostępowe i sieci ad hoc, a także narzędzia do zarządzania zasobami radiowymi (radio resource management, RRM), monitorujące pasmo RF pod kątem aktywności oraz obciążenia punktów dostępowych.

>Jeśli bowiem jakiś punkt dostępowy jest obciążony bardziej niż zazwyczaj, powinno to stanowić sygnał dla administratora o możliwości nieautoryzowanego ruchu.

Zabezpieczanie sieci WLAN:
- ukrywanie SSID - SSID cloaking, słabe rozwiązanie
- filtrowanie adresów MAC - też dosyć słabe
- uwierzytelnienie
  - otwarty system uwierzytelniania
  - z kluczem współdzielonym:
    - `WEP` - Wired Equivalent Privacy, szyfrowanie RC4 z kluczem stałym, podczas wymiany pakietów klucz nie ulega zmianie, co czyni ją podatną na włamanie
    - `WPA` - WiFi Protected Access, metoda przejściowa mająca zapewnić kompatybilność z WEP, tutaj dane są szyfrowane nieco silniejszym algorytmem szyfrowania `TKIP`(Temporal Key Integrity Protocol), każdy pakiet szyfrowany innym kluczem
    - `WPA-2` - inaczej 802.11i,  
- szyfrowanie danych:
  - `TKIP` - obsługa starszych urządzeń WLAN poprzez usunięcie niektórych wad metody szyfrowania 802.11 WEP. Metoda polega na użyciu WEP, przy czym ładunek ramki warstwy 2 szyfrowany jest metodą TKIP
  - `AES` - Advanced Encryption Standard, metoda szyfrowania stosowana w WPA2

Rodzaje uwierzytelniania:
- `Personal` - sieci domowe, klucz współdzielony PSK, współdzielone hasło
- `Enterprise` - sieci korporacyjne, wymaga serwera do uwierzytelnienia tzw. RADIUS, z pomocą standardu 802.11X,a dokładnie protokołu `EAP`(Extensible Authentication Protocol)

RADIUS zapewnia AAA - Authentication, Authorization and Accounting

RADIUS standardowo nasłuchuje na:
- 1812/udp - uwierzytelnienie
- 1813/udp - rejestrowanie

>Współdzielony klucz - Potrzebny do uwierzytelniania punktu dostępu na serwerze RADIUS.

>Klucz współdzielony nie jest parametrem, który należy skonfigurować na kliencie. Jest on wymagany na punkcie dostępu w celu uwierzytelnienia go do serwera RADIUS.

## 4.9 Konfiguracja bezprzewodowej sieci LAN

Wstępna konfiguracja domyślna

Wi-Fi Range Extender - wzmacniacz Wi-Fi

Z powodów bezpieczeństwa, sieć gości powinna być odizolowana, z osobnym SSID.

DMZ - demilitarized zone, strefa zdemilitaryzowana, miejsce, w którym umieszczamy serwery, które udostępniamy użytkownikom z poza naszej sieci

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>