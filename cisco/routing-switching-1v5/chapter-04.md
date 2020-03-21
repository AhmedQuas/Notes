### Spis treści
- [Chapter 4: Warstwa dostępu do sieci](#chapter-4-warstwa-dost%c4%99pu-do-sieci)
  - [4.1 Protokoły warstwy fizycznej](#41-protoko%c5%82y-warstwy-fizycznej)
    - [4.1.1 Zadania warstwy fizycznej](#411-zadania-warstwy-fizycznej)
    - [4.1.2 Pojęcia opisujące transmisję](#412-poj%c4%99cia-opisuj%c4%85ce-transmisj%c4%99)
  - [4.2 Media sieciowe](#42-media-sieciowe)
    - [4.2.1 Okablowanie miedziane](#421-okablowanie-miedziane)
    - [4.2.2 Światłowód](#422-%c5%9awiat%c5%82ow%c3%b3d)
    - [4.2.3 Media bezprzewodowe](#423-media-bezprzewodowe)
  - [4.3 Protokoły warstwy łącza danych](#43-protoko%c5%82y-warstwy-%c5%82%c4%85cza-danych)
  - [4.4 Topologie](#44-topologie)
    - [4.4.1 Topologie WAN](#441-topologie-wan)
    - [4.4.2 Topologie LAN](#442-topologie-lan)
    - [4.4.3 Ramka w warstkie łącza danych](#443-ramka-w-warstkie-%c5%82%c4%85cza-danych)
      - [4.4.3.1 Ramka Ethernetowa](#4431-ramka-ethernetowa)
      - [4.4.3.2 Ramka protokołu PPP](#4432-ramka-protoko%c5%82u-ppp)
      - [4.4.3.2 Ramka 802.11](#4432-ramka-80211)

# Chapter 4: Warstwa dostępu do sieci

## 4.1 Protokoły warstwy fizycznej

`ISR` - Integrated Service Router - urządzenie oferujące łączność przewodową i bezprzewodową. Łączy w sobie takie urządzenia jak:
- router
- przełącznik
- `WAP` - Wireless Access Point

>Sygnał radiowy jest tym mniejszy im dalej znajduje się urządzenie odbiorcze od punktu dostępowego. To może przyczyniać się do mniejszej przepustowości albo całkowitego braku połączenia bezprzewodowego.

W celu ograniczenia tego zjawiska można używać regeneratora sygnału (`wireless repeater, range extender`).  

>Wszystkie urządzenia bezprzewodowe muszą dzielić dostęp do fal radiowych łączących te urządzenia z bezprzewodowym punktem dostępowym. To oznacza, że może wystąpić zmniejszenie wydajności sieci, gdy więcej urządzeń bezprzewodowych będzie miało równoczesny dostęp do sieci. 

### 4.1.1 Zadania warstwy fizycznej

Warstwa fizyczna obejmuje transport bitów przez medium. Ramki otrzymuje od warstyw wyższej - warstwy łącza danych.

Rodzaje mediów:
- `kabel miedziany` - sygnał to ciąg impulsów elektrycznych
- `kabel światłowodowy` - sygnał to ciąg impulsów świetlnych
- `łączność bezprzewodowa` - sygnał mikrofalowy

Za standardy warstwy fizycznej i łącza danych odpowiadają następujące organizacje: ISO, TIA/EIA, ITU, ANSI, IEEE i państwowe organizacje telekomunikacyjne (np `FCC` i `ESTI`). Organizacje odpowiadają za obszary takie jak:
- `komponety fizyczne` - obwody elektroniczne, media transmisyjne i złącza
- `kodowanie` - Non-Return to Zero (NRZ), Manchester (stary Ethernet, RFID, NFC), 4B/5B (1 bit nadmiarowy), 8B/10B. Dodatkową grupę stanowią kody sterowania transmisją (określają początek i koniec ramki).
- `sygnalizacja` - zamienie bitów na sygnał w danym medium. Wyróżniamy transmisję asynchroniczną i synchroniczną (sygnał taktujący)

`Modulacja` - proces, w którym charakterystyka jednej fali (sygnał) modyfikuje inną falę (zwaną falą nośną). Wyróżniamy następujące rodzaje modulacji:
- `FM` - Frequency Modulation - zmiana częstotliwości
- `AM` - Amplitude Modulation - zmiana amplitudy
- `PCM` - Pulse-coded Modulation - modulacja impulsowa

Za standardy warstw wyższych odpowiada głównie `IETF` - Internet Engineering Task Force.

### 4.1.2 Pojęcia opisujące transmisję

`Bandwidth` - szerokość pasma

>Szerokość pasma to zdolność medium do przenoszenia danych. Liczbowo szerokość pasma określa ilość danych, które mogą przepłynąć z jednego miejsca na drugie w określonym przedziale czasu. Szerokość pasma zwykle mierzona jest w kilobitach na sekundę (kb/s) albo w megabitach na sekundę (Mb/s).  
>W praktyce szerokość pasma w sieci zależy od kombinacji czynników:  
> - Właściwości fizycznych medium  
> - Technologii wybranych do sygnalizacji i wykrywania sygnałów w sieci

Tutaj mnożnikiem nie jest 2^10 (1024), a 10^3 (100)

Przykładowo:

1 kb/s = 1000 b/s

`Throughput` - przepustowość, miara transferu bitów przez medium w określonym czasie, zazwyczaj nie osiągamy tutaj wartości maksymalnej czyli całkowitej szerokości pasma ze względu na:
- natężenie ruchu sieciowego
- rodzaj ruchu (np QoS)
- opóźnienia generowane przez urządzenia pośredniczące w transmisji
- czas nawiązania połączenia

>Istnieje trzeci pomiar do mierzenia transferu użytecznych danych, który jest znany jako przepustowość efektywna (ang. goodput). Przepustowość efektywna jest miarą dla przetransferowanych użytecznych danych w podanym okresie czasu. Przepustowość efektywna to przepustowość pomniejszona o narzut związany z ustanowieniem połączenia, potwierdzeniami i enkapsulacją.

## 4.2 Media sieciowe

### 4.2.1 Okablowanie miedziane

>W sieciach wykorzystywane jest medium miedziane, ze względu na jego niską cenę, łatwość instalacji oraz małą rezystancję dla prądu elektrycznego.

`Tłumienie` - niekorzystne zjawisko pogarszania jakości sygnału wraz ze wzrostem odległości przesyłanego sygnału.

Rodzaje występujących zakłóceń:
- `Zakłócenia elektromagnetyczne` (EMI Electromagnetic interference) lub zakłócenia radiowe (RFI radio frequency interference) czyli nasz sygnał może być zakłócany przez: fale radiowe, urządzenia elektromagnetyczne (lampy fluorescencyjne oraz silniki elektryczne)
- `Przesłuch` - wspólne odziaływanie pola EM powstające w sąsiednich przewodach podczas transmisji sygnału

>Aby przeciwdziałać negatywnym skutkom EMI i RFI, niektóre rodzaje kabli miedzianych są umieszczone w osłonie metalowej i wymagają odpowiedniego podłączenia do uziemienia.
>Aby przeciwdziałać negatywnym skutkom przesłuchu niektóre rodzaje kabli miedzianych są zbudowane ze skręconych par przewodów, co skutecznie eliminuje przesłuchy.

Dlatego projektując infrastrukturę sieciową należy brać pod uwagę czynniki tj:
- środowisko pracy (wybór medium, odpowiedniej kategorii kabla)
- prawidłowe układanie i zakańczanie kabli
- unikanie różnych źródeł zakłóceń


>Kable miedziane są wykorzystywane do łączenia węzłów w sieci LAN z urządzeniami infrastruktury takimi jak: przełączniki, routery i bezprzewodowe punkty dostępowe

Media miedziane:
- `UTP` - Unshielded Twisted-Pair, skrętka nieekranowana, używa złączki RJ-45
  
  1. Miedź 
  2. Izolacja plastikowa
  3. Skręcona par (chroni przed interferencją)
  4. Zewnętrzny płaszcz (plastikowa osłona)

- `STP` - Shielded Twisted-Pair, skrętka ekranowana, zapewnia lepszą ochronę przed zakłóceniami niż UTP, jednak jest droższy i trudniejszy w montażu, również używa złączki RJ-45.
  >Najbardziej znane rodzaje kabla STP:  
  -Kabel STP chroni całą wiązkę przewodów wykorzystując folię eliminującą praktycznie wszystkie zakłócenia (bardziej powszechne)   
  >-Kabel STP chroni całą wiązkę przewodów, jak również poszczególne pary przewodów wykorzystując folię eliminującą wszelkie zakłócenia.

- `Coaxial cable` - kabel koncentryczny, wszystko w jednej osi. Zastosowanie głównie telewizja kablowa, wczesny Ethernet, przyłączenie anteny do sprzętu radiowego, instalacja internetu kablowego (`HFC` - Hybrid Fiber Coax). Tutaj występują złącza takie jak: BNC, typ N, typ F.
  1. Miedź
  2. Izolacja - plastik
  3. Ekran z plecionki miedzianej
  4. Płaszcz zewnętrzny

>Okablowanie miedziane może też przewodzić napięcia wyładowań atmosferycznych do urządzeń sieciowych.

Sens skręcania przewodów

>Przewody jednego obwodu elektrycznego są wzajemnie skręcone. Gdy dwa przewody w obwodzie elektrycznym są umieszczone blisko siebie, to ich pola magnetyczne są dokładnym przeciwieństwem siebie. Dlatego te dwa pola magnetyczne wzajemnie się znoszą. To samo dotyczy zewnętrznych sygnałów EMI i RFI.

>Zmienianie liczby skręceń poszczególnych par przewodów: Aby zwiększyć efekt znoszenia się pól każda para przewodów w kablu jest skręcona inną liczbę razy

`TIA/EIA-568A` i `TIA/EIA-568B` - te normy to m.i.n.: przypisanie kolejności przewodów (kolorów) do odpowiednich pinów w złączy RJ-45 (`ISO 8877`)

Nazwyane jest to też `efektem samo-ekranowania par przewodów`.

Podziału przewodów na kategorie dokonało IEEE, najpopularniejsze:
- kat 5 - do 100 Mb/s
- kat 5e - 100Mb/s - 1000Mb/s, minimum dla GigabitEthernet
- kat 6 - 1000Mb/s - 10Gb/s
- kat 6a

`Patch panel` - panel krosowniczy

Niedokładne wykonanie zakończeń może wprowadzać dodatkowe zakłócenia, a nawet utratę sygnału.

Typy kabli ze względu na ułożenie przewodów
- kabel `prosty` (bez przeplotu) - oba końce zgodne z jednym standardem. Wykorzystywane do łączenia hosta z urządzeniem sieciowym
- kabel z przeplotem, `crossover` - jeden koniec zgodny z T568A, a drugi z T568B. Wykorzystywany do łączenia urządzeń tego samego typu: host-host, łączy urządzenia sieciowe (router-router, router-przełącznik)
- kabel konsolowy (`rollover`) - podłączenie stacji roboczej do portu konsoli urządzenia Cisco

Do testowanie jakości wykonanego połączenia należy użyć `testera okablowania sieciowego`

### 4.2.2 Światłowód

>Światłowód jest elastyczny a cienkie przezroczyste włókno wytworzone z bardzo czystego szkła (krzemionki) jest niewiele grubsze od ludzkiego włosa. W przypadku transmisji światłowodowej przesyłane bity kodowane są jako impulsy świetlne. Kabel światłowodowy działa jako falowód lub "rura świetlna" służąca do przesyłania światła pomiędzy dwoma końcami przy minimalnych stratach sygnału.

>W przeciwieństwie do przewodów miedzianych, światłowód może przesyłać sygnały z mniejszym tłumieniem i jest całkowicie odporny na zakłócenia EMI oraz RFI.

Główne zastosowania:
- szkielety sieci korporacyjnych
- dostęp do sieci `FTTH` - Fiber to the home
- sieci podwodne i długodystansowe

Budowa:

1. `Core` - rdzeń, wykonany z czystego szkła, krzemionki, to tutaj przewodzone jest światło
2. `Płaszcz wewnętrzny` - działa jak lutro, aby utrzymać światło w rdzeniu, zjawisko całkowitego wewnętrznego odbicia
3. `Bufor` - chroni rdzeń i płaszcz przed uszkodzeniem
4. `Materiał wzmacniający` - kewlar (kamizelki kuloodporne), chroni materiał przed rozciąganiem
5. `Płaszcz zewnętrzny` - ochrona włókna przed ścieraniem, rozpuszczalnikami itp.

`Cladding` -płaszcz

>Włókna są wrażliwe na uszkodzenia przy dużych zgięciach, ale właściwości rdzenia i płaszcza zostały zmienione na poziomie molekularnym tak, aby były bardzo wytrzymałe.

Źródła impulsu:
- `Lasery` - droższe, częścieś wykorzystywane w światłowodach jednomodowych
- `Diody LED` (Light Emitting Diode)

>Światło laserowe transmitowane przez okablowanie światłowodowe może uszkodzić wzrok człowieka. Należy zachować ostrożność i nie patrzeć w zakończenie aktywnego światłowodu.

Rodzaje kabli światłowodowych:
- `SMF` - Single-Mode Fiber, włókna jednomodowe, bardzo mały rdzeń (9 mikrometrów), pojedynczy promień laseru => długi dystans
- `MMF` - Multi-Mode Fiber, wielomodowe, grubszy rdzeń (50/62,5 mikrometra), diody LED (światło z diody LED nie jest skupione, wchodzi do medium pod różnymi kątami). Rozwiązanie dobre na krótkie dystanse, popularne w sieciach LAN. Dystans transmisji do 2 km.

>`Dyspersja` - określa rozmycie impulsu świetlnego podczas jego transmisji w światłowodzie. Większa dyspersja oznacza większą utratę siły sygnału.

Najpowszechniejsze złącza światłowodowe:
- `ST` - Straight-Tip, starszy typ złącza przypominający bagnet, stosowany do włókien wielomodowych
- `SC` - Subscriber Connector, inaczej nazywane standardowym lub kwadratowym, wykorzystuje mechanizm push-pull. Stosowane dla światłowodów jedno i wielomodowych
- `LC` - Lucent Connector, nazywane małym, lokalnym. Używane głównie dla jednomodowych, jednak jest też stosowane dla światłowodów wielomodowych

>Biorąc pod uwagę wszystkie generacje złączy aktualnie w użyciu jest około 70 typów złączy.

>Ponieważ światło może przepływać tylko w jednym kierunku włókna światłowodowego, to aby zapewnić komunikację w trybie full-duplex potrzebne są dwa włókna.

>Kable światłowodowe powinny być chronione przy pomocy plastikowej zaślepki, gdy nie są używane.

>Norma `TIA-598`, zaleca stosowanie żółtej osłony dla jednomodowych kabli światłowodowych i pomarańczowej dla wielomodowych kabli światłowodowych .

Do testowanie wykonanego połączenia wykorzystuje się `OTDR` (Optical Time Domain Reflectometr), reflektometr optyczny.

>Szybki i prosty test światłowodu może być wykonany przez świecenie latarką w kierunku jednego końca włókna i obserwowanie drugiego końca światłowodu.

Podstawowe typy błędów dotyczące zakończenia złącz i spawania światłowodów: 
- `przesunięcie osiowe włókien` - włókna nie są precyzyjnie ułożone względem siebie na łączeniu.
- `szczelina na zakończeniu` -  Media niecałkowicie przylegają do siebie na spawie lub na połączeniu.
- `nieprawidłowe zakończenie włókna` - zakończenie medium nie jest dobrze wypolerowane albo zabrudzone.

### 4.2.3 Media bezprzewodowe

Medium przenoszone falami radiowymi, nie jest one ograniczone do jednej ścieżki jak miało miejsce we wcześniej opisywanych mediach. Pozwala na znacznie większą mobilność użytkowników, coraz więcej urządzeń (laptopy, smarfony) to medium.

Medium to jest bardzo podatne na zakłócenia, dodatkowo ze względu na specyfikę budynku, materiały, z których wykonano jego elementy medium to oferuje różne obszary pokrycia. Warto tutaj również wspomnieć o aspektach bezpieczeństwa. Ze względu na sposób przenoszenia sygnału istnieje możliwość dostępu nieupoważnionych użytkowników, którzy mogą uzyskać dostęp do transmisji.

Najbardziej powszechne standardy łączności bezprzewodowej:
- 802.11 (WLAN), WiFi `CSMA/CA` (Carrier Sense Multiple Access/Collision Avoidance)
- 802.15 (WPAN) Wireless Personal Area Network, powszechnie znany jako Bluetooth
- 802.16 `WiMAX` Worldwide Interoperability for Microwave Access, topologia punkt-wielopunkt

>Wi-Fi jest znakiem handlowym Wi-Fi Alliance. Wi-Fi jest używane razem z certyfikowanymi produktami, które należą do urządzeń WLAN i bazują na standardach IEEE 802.11.

Urządzenia w architekturze 802.11
- `AP` - Wireless Access Point, Bezprzewodowy punkt dostępowy
  >Dokonuje koncentracji sygnałów bezprzewodowych od użytkowników i przesyła te sygnały zwykle za pośrednictwem kabla miedzianego do istniejącej infrastruktury sieciowej zbudowanej z kabli miedzianych (sieci Ethernet). Domowe routery bezprzewodowe integrują funkcje routera, przełącznika i punktu dostępowego.
- `Bezprzewodowe karty sieciowe`

Standardy 802.11:
- `802.11a` - 5 GHz do 54 Mb/s
- `802.11b` - 2,4 GHz do 11 Mb/s, większy zasięg niż w a, lepsze zdolności przenikania
- `802.11g` - 2,4 GHz do 54 Mb/s, częstotliwość z b a prędkość z a
- `802.11n` - 2,4 GHz / 5 GHz, 100 Mb/s do 600 Mb/s, do 70 metrów
- `802.11ac` - 2,4 GHz / 5 GHz, 450 Mb/s do 1,3 Gb/s
- `802.11ad` - 2,4 GHz / 5 GHz / 60 GHz, teoretycznie do 7 Gb/s, znany również jako `WiGig`

## 4.3 Protokoły warstwy łącza danych

>W terminologii warstwy 2 urządzenia sieciowe podłączone do medium nazywane są węzłami.

Cele warstwy łącza danych:
- przyjumuje pakiety z warstwy 3, dodając swój narzut, tworząc w ten sposób ramkę
- steruje dostępem do medium i dokonuje detekcji błędów

Podwarstwy warstwy łącza danych:
- `LLC` - Logical Link Control, określa procesy wykonywane programowo
- `MAC` - Media Access Control, określa proces dostępu do medium wykonywany sprzętowo. Ta podwarstwa jest mocno związana ze sposobem przesyłu danych przez medium (np 802.3, 802.11). Końcowy produkt może być umieszczony, a później odebrany z medium. Dzęki tej warstwie dane mogą być przesyłane różnymi mediami.

>Rolą warstwy łącza danych modelu OSI jest przygotowanie pakietów warstwy sieci do transmisji i sterowania dostępem do fizycznych mediów.

Struktura ramki:
- `nagłówek` - adresacja, oznaczenie, które węzły komunikują się między sobą
- `dane` - odebrane z wyższej warstwy
- `stopka` - dane kontrolne do wykrywania błędów

Ramka zawiera następujące pole:
- `Preambuła` - właściwości synchronizacyjne
- `Początku ramki`
- `Adres MAC odbiorcy`
- `Adres MAC nadawcy`
- `Długość / typ`
- `Kontrola przepływu`
- `Dane` - 46-1500
- `Wykrywanie błędów`
- `Koniec ramki`

## 4.4 Topologie

### 4.4.1 Topologie WAN

- `Point-to-Point` - PPP, stałe połączenia między dwoma punktami końcowymi
- `hub and spoke` - czasem nazywana gwiazdą, występuje punkt centralny, który integruje inne urządzenia połączeniami punkt - punkt
- `mesh` - siatka, połączenia każdy z każdym. Wysoka dostępność, daje to również duże koszty zbudowania i utrzymania takich łącz.

>PPP - `Obwód wirtualny` jest logicznym połączeniem tworzonym poprzez sieć pomiędzy dwoma urządzeniami sieciowymi. Pomiędzy końcowymi węzłami tworzącymi obwód wirtualny występuję wymiana ramek. Występuje ona nawet wtedy, gdy ramki muszą przepływać przez urządzenia pośredniczące. Wirtualne obwody są bardzo ważnym pojęciem logicznym wykorzystywanym przez niektóre technologie warstwy 2.

Tryby komunikacji:
- `half duplex` - komunikacja jednokierunkowa,urządzenia nie mogą transmitować i nadawać dane jednocześnie
- `full duplex` - komunikacja dwukierunkowa, urządzenia mogą jednocześnie nadawać i odbierać w tym samym czasie

### 4.4.2 Topologie LAN

- `star` - gwiazda, urządzenia są połączone do centralnego urządzenia pośredniczącego (obecnie switch, wcześniej hub)
- `extended star` - rozgałęziona, rozszerzona gwiazda, połączenie dwóch lub więcej gwiazd (rolę połączenia może pełnić magrstrala magistrala)
- `bus` - magistrala, wszystkie urządzenia końcowe są połączone do wspólnego przewodu, końce przewodu powinny być odpowiednio zakończone (starszy Ethernet)
- `ring` - pierścień, urządzenie końcowe połączone z sąsiednim urządzeniem końcowym tworząc pierścień. `FDDI` (Fiber Distributed Data Interface) wykorzystywało także drugi pierścień do zwiększenia niezawodności. Jeśli ramka nie jest adresowana do urządzenia to przekazuje on ją do swojego sąsiada

Metody kontroli dostępu do współdzielonego medium:
- `rywalizacja` - każdy konkuruje o dostęp do medium. W momencie wystąpienia kolizji urządzenie podejmuje odpowiednie procedury 
- `dostęp kontrolowany` - każdy węzeł ma przydzielony kwant czasu np. przekazywanie żetonu w `802.5` Token Ring i `802.4` FDDI. Dostęp do medium ma każde urządzenie w sposób sekwencyjny

W celu uniknięcia nieporozumień w sieci do kontroli dostępu do medium strosuje się technologię `CSMA` (Carrier Sense Multiple Access). Systemy oparte na rywalizacji nie spisują się dobrze gdy sieć jest mocno obciążona.

Technologie bazujące na rywalizacji
- `CSMA/CD` - CSMA with Collision Detection, wielodostęp do łącza danych (wykrywanie nośnej) z badaniem stanu kanału, dla `802.3`. 
  
  Sprawdzamy czy nikt nie nadaje, jak nie to my sami możemy nadawać, jednak cały czas nasłuchujemy czy przypadkiem nie wystąpiła kolizja

- `CSMA/CA` - CSMA with Collision Avoidance, wielodostęp z wykrywaniem nośnej i unikaniem kolizji dla `802.11`
  
  Nasłuchujemy czy nikt nie nadaje, jeśli nie to wysyłamy do medium informację, że chcemy wysłać informacje. Po otrzymaniu potwierdzenia zaczynamy nadawanie.

`Wielodostęp` - możliwość komunikowania się z wieloma urządzeniami za pośrednictwem jednego medium. Każdy węzeł widzi wszystkie ramki przepływające przez sieć, jednak odbiera tylko te adresowane do niego samego.

### 4.4.3 Ramka w warstkie łącza danych

W zależności od protokołu format ramki łącza danych może wyglądać nieco inaczej, jednak każdy tryp ramki składa się z następujących części:
- Nagłówek
- Dane
- Pole końcowe

Narzut protokołu może naprzykład zależeć od środowiska, w którym będzie odbywała się transmisja. Jeżeli warunki transmisji są niesprzyjające to musimy zapewnić odpowiedni narzut pozwalający na wykrywanie i korekcję błędów. Większy narzut wpływa negatywnie na szybkość (efektywnej) transmisji w medium.

#### 4.4.3.1 Ramka Ethernetowa

Nagłówek:

- `preambuła` - początek ramki, informuje urządzenia sieciowe, że właśnie nadchodzi ramka
- `adres MAC docelowy i źródłowy` - 2 x 6 bajtów, 2 x 48 bitów 
- `typ / długość` - czasem wskazuje usługę wyższej warstwy

Stopka:

- `FCS` - Frame Check Sequence, wyliczenie sumy kontrolnej CRC (Cyclic Redundancy Check), mające na celu stwierdzić, czy odebrana ramka może zostać uznana za nie zniekształconą
- `koniec ramki` - wskazuje koniec transmisji. Wykorzystywane gdy przypadku, gdy długość ramki nie została określona w polu `typ / długość`.

Inne ramki mogą zawierać w części nagłówkowej:
- `priorytet lub jakości usługi`
- `sterowanie połączeniem fizycznym/logicznym`
- `sterowanie przepływem/obciążeniami w sieci`

> Adresacja warstwy łącza danych jest zawarta we wnętrzu nagłówka ramki i określa dla ramki docelowy węzeł w lokalnej sieci.

>Gdy router ustali, gdzie przekazać pakiet, tworzy on nową ramkę dla pakietu i ta nowa ramka jest wysyłana do następnego segmentu w kierunku miejsca przeznaczenia.

#### 4.4.3.2 Ramka protokołu PPP

- `Flaga` - wskazuje początek lub koniec ramki
- `Adres` - zawiera standardowy adres rozgłoszeniowy, w PPP stacje nie mają przypisanego do siebie adresu
- `Kontrola` - sekwencja wzywająca do transmisji danych w niesekwencjonowanej ramce 
- `Dane`
- `FCS` - 2/4 bajty

#### 4.4.3.2 Ramka 802.11

Struktura podobna do tej z 802.3 jednak pewne części są bardziej rozbudowane.

- `kontrola ramki`
  - `protocol version`
  - `type and subtype` - strerowanie, transmisja, zarządzanie
  - `to DS` - 1 kiedy ramka jest kierowana do systemu dystrubucyjnego
  - `from DS`
  - `more fragments` - 1 gdy ramka posiada kontunuację w postaci kolejnych fragmentów
  - `retry` - 1 gdy ramka, jest retransmisją wcześniejszej ramki
  - `power management` - 1 wskazuje, że węzeł pracuje w trybie power-safe, oszczędzania energii
  - `more data` - 1 wskazuje węzłowi w trybie power-safe, że jest więcej ramek buforowanych dla tego węzła
  - `WEP` (Wired Equivalent Privacy) - wskazuje, że dane są zaszyfrowane przy użyciu algorytmu WEP
  - `order` - 1 dla ramek używających usługi `Strictly Ordered`
- `Duration/ID` - identyfikator lub czas do przełania ramki
- `DA` - Destination MAC
- `SA` - Source MAC
- `RA` - Receiver Address - identyfikuje urządzenie bezprzewodowe, które jest pośredniczącym odbiorcą ramki
- `Fragment Number`
- `Sequence Number`
- `TA` - Transmitter Address, identyfikuje urządzenie bezprzewodowe, które nadało ramkę
- `Frame Body` - zawiera dane, które są transmitowane
- `FCS`

>Technika kodowania przetwarza strumień bitów danych w predefiniowany kod, który może być rozpoznany zarówno przez nadajnik, jak i odbiornik. Korzystanie z predefiniowanych wzorców pomaga odróżnić bity danych od bitów kontrolnych i zapewnia lepsze wykrywanie błędów w mediów.

>Przepustowość efektywna (ang. goodput) jest miarą dla przetransferowanych w podanym okresie czasu użytecznych danych (ang. throughput) pomniejszoną o narzut związany z ustanowieniem połączenia, potwierdzeniami i enkapsulacją. Jeżeli przepustowość wynosi 80 Mb/s, a narzut wynosi 15 Mb/s, to przepustowość efektywna (ang. goodput) wynosi 80 Mb/s - 15 Mb/s = 65 Mb/s.

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>