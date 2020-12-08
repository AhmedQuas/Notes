### Spis treści
- [Chapter 2: Podłączenie do sieci WAN](#chapter-2-podłączenie-do-sieci-wan)
  - [2.1 Przegląd technologii używanych w sieciach WAN](#21-przegląd-technologii-używanych-w-sieciach-wan)
  - [2.2 Prywatna infrastrukura WAN](#22-prywatna-infrastrukura-wan)
  - [2.3 Publiczna infrastrukura WAN](#23-publiczna-infrastrukura-wan)

# Chapter 2: Podłączenie do sieci WAN

## 2.1 Przegląd technologii używanych w sieciach WAN

Sieci WAN są wykorzystywane do łączenia sieci LAN. WAN działa na znacznie większym obszarze geograficznym niż sieć LAN. Właścicielami WAN są zazwyczaj ISP, dostawcy internetowi, operatorzy. Można z nich skorzystać po uiszczeniu opłaty do właściciela łącza. Transmitują one dane, wideo i głos.

>Sieci LAN odnoszą się do sieci skupiających lokalne zasoby sprzętowe, takie jak: komputery, urządzenia peryferyjne, w obrębie danego budynku lub małego obszaru geograficznego

>Coraz częściej, Internet jest wykorzystywany jako tańsza alternatywa łącz typu WAN. Wraz z rozwojem nowych technologii, komunikacja i transakcje wykonywane za pośrednictwem sieci Internet, zachowują odpowiedni poziom bezpieczeństwa i prywatności.

Wiele sieci LAN -> kampus

Rodzaje technologii WAN:
- sieć małego biura,
- sieć kampusowa,
- sieć oddziału,
- sieć rozproszona

>Standardy sieci WAN zazwyczaj opisują metody dostarczania danych w warstwie fizycznej(1 warstwa w modelu ISO OSI) oraz wymagania dotyczące łącza danych(2 warstwa), takie jak reguły tworzenia adresów fizycznych, kontrola przepływu i enkapsulacja.

Instytuje, które definiuja standardy WAN:
- `TIA` - Telecommunications Industry Association
- `EIA` - Electronic Industries Alliance
- `ISO` - International Organization for Standardization
- `IEEE` - Institute of Electrical and Electronics Engineers

>Większość łącz WAN, to łącza typu punkt-punkt, z tego też powodu pole adresu ramki warstwy 2 nie jest wykorzystywane.

Terminologia stosowana w odniesieniu do łącz WAN:
- `CPE` - Customer Premise Equipment, własne urządzenia zamiast tych dostarczonych przez ISP, urządzenie końcowe użytkownika podłączone są bezpośrednio do łącza operatorskiego.
  >Abonent jest właścicielem wyposażenia CPE lub dzierżawi je od dostawcy usług.
- `DCE` - Data Communication Equipment, urządzenia zakończenia obwodu, odpowiadają za przesyła danych przez pętlę lokalną
  >Urządzenie DCE zapewniają możliwość podłączenia abonenta do chmury WAN.
- `DTE` - Data Terminal Equipment, urządzenia znajdujące się po stronie klienta, przekazują one dane do urządzeń DCE
- `punkt demarkacyjny` - punkt zmiany odpowiedzialności za sieć między abonentem a operatorem. Miejsce, które wyznacza linię między urządzeniami komsumenta, a ISP.
- `pętla lokalna` - ostatnia mila, media łączące urządzenia CPE do centrali operatora
- `centrala` - miejsce łączenia CPE lokalnego ISP z infrastrukturą innego dostawcy
- `toll network` - sieć wewnętrzna operatora WAN, obejmuje łącza cyfrowe dalekiego zasięgu, lini optycznych, urządzeń sieciowych znajdujących się wewnątrz sieci WAN operatora

Urządzenia w sieci WAN:
- `modem dialup` - zamienia (moduluje) sygnały cyfrowe wytwarzane przez komputer do częstotliwości transmisji głosu, te zaś zostają wysłane do publicznej sieci telekomunikacyjnej, i na odwrót u hosta docelowego(raczej technologia przeszłości)
- `serwer dostępowy` - spełnia funkcje telefonicznej komunikacji przychodzącej i wychodzącej użytkowników
- `modem szerokopasmowy` - modem cyfrowy wykorzystywany w łączach kablowych i DSL. W porównaniu do modemów głosowych(dialup) pracują na wyższych częstotliwościach i transmitują z większą prędkością
- `CSU/DSU` - urządzenia mogące być modemem jak i specjalnym urządzeniem potrzebne do utworzenia cyfrowego łącza dzierżawionego
  > Urządzenie `CSU` jest punktem zakończenia połączenia cyfrowego, zapewniając integralność połączenia poprzez korekcję błędów i monitoring łącza. 
  >`DSU` zaś, konwertuje sygnały pochodzące z sieci operatorskiej na ramki zrozumiałe dla urządzeń sieci LAN i odwrotnie.
- `przełącznik WAN` - urządzenie posiadające wiele portów, które zapewniają połączenie z róznymi sieciami
- `router` - urządzenie realizujące wiele usług, posiada kilka portów, które są podłączone do różnych sieci(nawet WAN operatora): szeregowe, Ethernet itp.
- `przełącznik wielowarstwowy/router warstwy rdzenia` - urządzenie znajdujące się w szkielecie sieci operatora WAN(różne interfejsy i wysokie przepustowości)

`Komutacja połączeń` - dedykowane fizyczne połączenie lub kanał, pomiędzy dwoma węzłami/terminalami w sieci, dynamiczne wirtualne połączenie musi być zestawione przed rozpoczęciem komunikacji.

`PSTN` - public switched telephone network, publiczna komutowalna sie telefoniczna

>Jeśli w obwodzie przesyłane są dane komputerowe, przepustowość ta może okazać się niewystarczająca. Abonent ma wyłączność na przydzieloną sobie przepustowość, komutacja obwodów jest w ogólnym przypadku kosztownym sposobem przesyłania danych.

`Komutacja pakietów` - dane dzielone są na pakiety, które następnie przesyłane są przez sieć. Nie trzeba wcześniej zestawiać obwodu, a kilka urządzeń może korzystać z tego samego kanału.

`PSN` - packet-switched network

Sposoby realizacji sieci z komutacją pakietów:
- `system bezpołączeniowy` - każdy pakiet zawiera informację adresową
- `system zorientowany połączeniowo` - trasa pakietu jest z góry określona, a każdy pakiet ma swój identyfikator. Przykładem takiej sieci jest Frame Relay, a idetyfikator `DLCI` - Data Link Control Identifier.

`VC` - Virtual Circuit, obwód wirtualny

>System przełączający określa dalszą trasę, wyszukując identyfikator w tabelach przechowywanych w pamięci.

>W sieciach z komutacją pakietów czasy opóźnienia i jego zmienność są większe niż w sieciach z komutacją obwodów. W końcu mamy łącze współdzielone.

Przedsiębiorstwo może uzyskać dostęp do WAN za pomocą infrastruktury WAN:
- `prywatnej`,
- `publicznej`

>Łącza dalekiego zasięgu, są to zazwyczaj połączenia pomiędzy dostawcami usług internetowych lub też połączenia pomiędzy oddziałami wielkich korporacji.

Sieci ISP składają się z mediów o wysokiej przepustowości - głównie światłowodów.

`SONET` - Synchronus Optical Networkin, standard ANSI, USA

`SDH` - Synchronus Digital Hierarchy, standard ETSI(EU) i ITU(świat)

>Zasadniczo nie różnią się one od siebie tak bardzo, dlatego też często oznaczane się je SONET/SDH.

`DWDM` - Dense Wavelength Division Multiplexing, umożliwia zwiększenie dostępnego pasma w pojedynczym włóknie światłowodowym(do 80 kanałów, długości fali, a każdy z nich po 10 Gb/s), technika wykorzystywana do przesyłania danych na duże odległości np. kablach morskich i łączach dalekiego zasięgu.

## 2.2 Prywatna infrastrukura WAN

Rodzaje prywatnej infrastrukutry WAN:
- `linie dzierżawione` - połączenie dedykowane, łącze punkt-punkt, szeregowe, linie T1(USA, 1,544 Mb/s)/E1(EU), T3/E3 - firma podnajmująca płaci stały abonament za jego używanie. Cena zależy od szerokości pasma i odległości. Połączenie te są wysokiej jakości, zawsze dostępne i proste w konfiguracji. Wadami tego typu rozwiązań jest koszt i mała elastyczność
- `połączenie dialup` - dostęp za pomocą modemów telefonicznych, analogowych, prędkość zazwyczaj mniej niż 56 kb/s. Taki dostęp sprawdza się w przypadku gdzie występują sporadyczne transfery danych o niewielkim rozmiarze. Tanie rozwiązanie, jednak długi czas łączenia i niska przepustowość nie pozwalająca na prawidłowe przesyłanie głosu nie wspominając o wideo
- `ISDN` - Integrated Services Digital Network, technologia, która umożliwia tradycyjnym siecą PSTN przenosić sygnały cyfrowe. Wykorzystuje TDM - Time Division Multiplexing, zwielokrotnienie z podziałem czasu, w ten sposób umożliwia przesył kilku syngnałów w jednym kanale. Podłączenie do sieci może wymagać adaptera terminala(`TA`). Tryby interfejsów ISDN:
  - `BRI` - Basic Rate Interface, interfejs podstawowy BRI, przeznaczony do stosowania w mieszkaniach oraz małych firmach. Skaład się z 2 x 64 kb/s(B) i 1 x 16(D),
  - `PRI` - Primary Rate Interface, interfejs rozszerzony PRI:
    - USA 23 x 64 kb/s(B) + 1x 16(D) = 1,544 Mb/s
    - EU, Australia i inne części świata
      
      30 x 64 kb/s(B) + 1x 16(D) = 2,048 Mb/s

    Obie wartości uwzględniają narzut związany z synchronizacją.

  >Połączenie jest realizowane w kanałach nośnych (`B`) o przepustowości 64 Kb/s, używanych do przesyłania głosu i danych, oraz w kanale sygnalizacyjnym delta (`D`) służącym do zestawiania połączeń oraz do innych zastosowań.

  Czasem ISDN jest stosowane jako dodatkowa przepustowość, która w razie potrzeby(okresy szczytu) może uzupełnić przepustowość łacza dzierżawionego
- `Frame Relay` - technologia warstwy 2, `NBMA`(sieci wielodostępowej bez obsługi rozgłaszania). Pojedynczy interfejs routera może służyć do zapewniania połączeń z wieloma sieciami za pomocą `PVC`(Pernament Virtual Circuit), stałych obwodów wirtualnych. Takie rozwiązanie oferuje prędkość do 4 Mb/s. FR zapewnia ekonomiczne połączenie między rozproszonymi geograficznie sieciami LAN
  
  >Frame Relay tworzy stałe połączenia wirtualne, a każde z nich jest identyfikowane w sposób unikalny za pomocą identyfikatora DLCI (data link connection identifier). Połączenia PVC i identyfikator DLCI zapewniają dwukierunkową komunikację pomiędzy jednym urządzeniem DTE i drugim.
- `ATM` - Asynchronous Transfer Mode, przesył danych przez sieci prywatne i publiczne. Technika oparta jest na architekturze komórek(53B = 48B(dane, treść zasadnicza) + 5B(nagłówek ATM)), a nie ramek. Ze względu na małą długość komórki ATM jest mniej wydajna niż większe ramki i pakiety w sieciach Frame Relay i X.25. ATM umożliwia ustanawianie wielu obwodów wirtualnych na jednym połączeniu z brzegiem sieci realizowanym za pośrednictwem łącza dzierżawionego.

  >Gdy komórki zawierają posegmentowane pakiety warstwy sieciowej, narzut będzie jeszcze większy, ponieważ system przełączający ATM musi być w stanie ponownie poskładać pakiety u adresata. Typowe łącze ATM wymaga niemal o 20% większej przepustowości niż Frame Relay, aby możliwe było przesyłanie przez nie takiej samej ilości danych warstwy sieciowej.
- `Ethernet WAN` - MetroE(Metropolitan Ethernet), EoMPLS(Ethernet over MPLS), VPSL(Virtual Private LAN Services), Ethernet na długich włóknach światłowodowych. Zalety tej technologii to redukcja wydatków, administracji i łatwa integracja z istniejącymi sieciami. Rozwiązanie coraz bardziej popularne kosztem Frame Relay i ATM.
- `MPLS` - Multiprotocol Label Switching, technologia pozwalająca na trasowanie pakietów na podstawie przełaczania etykiet, a nie adresów IP. MPLS jest wieloprotokołowy(IPv4, IPv6, Ethernet, ATM, DSL, czy Frame Relay). Etykiety wskazują na drogę pomiędzy routerami, a nie węzłami docelowymi. IPv{4|6} jest trasowane a reszta jest przełączana
- `VSAT` - Very small aperture terminal, umożliwia stworzenie prywatnej sieci WAN z wykorzystaniem łaćz satelitarnych. Router jest połączony do talerza satelitarnego, który wycelowany jest w satelitę geostacjonarnego(orbita około ziemska, 35 786 km)

>W tradycyjnej telefonii stosuje się kable miedziane, nazywane pętlami lokalnymi.

## 2.3 Publiczna infrastrukura WAN
Rodzaje publicznej infrastrukutry WAN:
- `DSL` - Digital Subscriber Line, technologia szerokopasmowa, duża przepustowość na liniach telefonicznych
  >Wiele linii abonenckich DSL jest multipleksowane w jedno łącze o dużej przepustowości za pomocą multipleksera DSL (DSLAM, DSL Access Multiplexer, korzysta z TDM) znajdującego się po stronie dostawcy.
- `łącze kablowe` - modem kablowy, przepustowość większa niż w tradycyjnych pętlach telefonicznych. Lokalny punkt dystrybucji, nazywany stacją czołową(jednym z jej komponentów jest `CMTS` - Cabke modem terminatio system, urządzenia umożliwiające wysyłanie i odbieranie sygnałów cyforwych z modemów kablowych), składa się z komputera i systemu bazodanowego. Punkt ten umożliwia połączenie się z sieci Internet
  >Abonenci usług kablowych muszą korzystać z dostawcy usług internetowych wskazanego przez operatora telewizji. Wszyscy abonenci lokalni korzystają z tego samego pasma kablowego. Jeśli z usługi korzysta wielu użytkowników przepustowość może być niższa od oczekiwanej
- `łączność bezprzewodowa` - działa w paśmei nielicencjonowanych częstotliwości radiowych. Wymagany jest jedynie router z obslugą technologii bezprzewodowej, kiedyś niewielki zasięg ~30 m. Technologie bezprzewodowe umożliwiające połączenie na większe zasięgi:
  - `miejska sieć Wi-Fi` - wymaga posiadania specjalnego modemu bezprzewodowego, dostęp darmowy lub znacznie tańszy niż u tradycyjnych dostawców. W niektórych przypadkach taka sieć jest wykorzystywana tylko przez niektóre służby jak policja czy straż pożarna 
  - `WiMAX` - Worldwide Interoperability for Microwave Access, IEEE 802.16
  >Zapewnia szybki, szerokopasmowy dostęp bezprzewodowy i `działa na podobnej zasadzie jak stacje bazowe telefonii komórkowej`.  Aby uzyskać dostęp do sieci WiMAX, abonent musi znajdować się w odległości do ok 50 km od stacji nadawczej operatora. i musi korzystać ze `specjalnego odbiornika` oraz `kodu autoryzującego` go w stacji bazowej.
  - `internet satelitarny` - wykorzystywany na wsiach, gdzie nie ma dostępu do sieci za pomocą DSL. Połączenie wolniejsze od DSL i sieci kablowej, jednak szybsze od modemu analogowego(~10x). Do obsługi takiej sieci wymagny jest talerz satelitarny, 2 modemy(Tx,Rx) i kabel koncentryczny łączący modem z talerzem
- `sieć komórkowa` - połączenie do sieci za pomocą transmisji danych w sieci komórkowej. Urządzenia komunikują się ze stacją bazową za pomocą fal radiowych. Standardy telefonii:
  - `GSM 900/1800`
  - `UMTS`
  - `3G/4G Wireless`
  - `LTE` - Long Term Evolution
  - `5G`
  >Sprzęty abonentów wyposażone są w małe anteny, a urządzenia operatorskie w znacznie większe i są one umieszczone na szczycie wież, oddalonych kilka kilometrów od telefonów użytkownika
- `VPN` - Virtual Private Network, technologia pozwalająca na przekazywanie ruchu w sposób zaszyfrowany pomiędzy komputerami w sieci prywatnej i publicznej. Już nie musimy wykorzystywać dedykowanych połączeń warstwy 2. Zalety VPN:
  - oszczędność
  - bezpieczeństwo
  - skalowalność
  - kompatybilność z technologiami szerokopasmowymi

  Rodzaje VPN:
  - `site-to-site` - pozwalają na łączenie wszystkich, całych sieci ze sobą, każda ze stron musi mieć tzw bramę VPN(router, firewall, koncentrator VPN itp). Przykładowym rozwiązaniem jest połączenie biur do centrali firmy
  - `zdalnego dostępu` - łączenie hostów do sieci firmowej przez Internet(pracownik zdalny). Każdy użytkownik musi posiadać programu klienta VPN lub korzystać z aplikacji webowej. Logując się trzeba podać nazwę uzytkownika i hasło

---

<div>
<a href="chapter-01.md">Prev: Chapter 1</a>
</div>
<div align="right">
<a href="chapter-03.md">Next: Chapter 3</a>
</div>