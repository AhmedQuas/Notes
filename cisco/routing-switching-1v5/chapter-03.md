### Spis treści
- [Chapter 3: Protokoły sieciowe i komunikacja](#chapter-3-protoko%c5%82y-sieciowe-i-komunikacja)
  - [3.1 Reguły komunikacji](#31-regu%c5%82y-komunikacji)
  - [3.2 Protokoły](#32-protoko%c5%82y)
    - [3.2.1 Organizacje standaryzujące](#321-organizacje-standaryzuj%c4%85ce)
    - [3.2.2 Modele odniesienia](#322-modele-odniesienia)
      - [3.2.2.1 Model OSI](#3221-model-osi)
      - [3.2.2.1 Model TCP/IP](#3221-model-tcpip)
      - [3.2.2.1 ISO OSI vs TCP/IP](#3221-iso-osi-vs-tcpip)
    - [3.3 Przenoszenie danych w sieci](#33-przenoszenie-danych-w-sieci)
    - [3.3.1 Dostęp do zasobów lokalnych](#331-dost%c4%99p-do-zasob%c3%b3w-lokalnych)
    - [3.3.2 Dostęp do zasobów zdalnych](#332-dost%c4%99p-do-zasob%c3%b3w-zdalnych)

# Chapter 3: Protokoły sieciowe i komunikacja

## 3.1 Reguły komunikacji

`Protokół` - zbiór jasno określonych reguł postępowania wykonywanych w celu nawiązania łączności i wymiany danych w sieci.

Protokoły opisują elementy takie jak:
- `kodowanie wiadomości`
    
    Nadanie informacji nadmiarowej, w celu zminimalizowania wpływu zakłóceń podczas przesyłu danych przez medium (kanał komunikacyjny). Informacja dodatkowa pozwala nam określić, czy w odebranej informacji wystąpił błąd. Jeśli informacja uległa niewielkiemu zniekształceniu to jestśmy w stanie ten błąd skorygować.

- `modulacja sygnału`

    Zmiana parametrów fali nośnej tak, aby można było nią przesyłać informację.

- `formatowanie wiadomości`
    
    Wiadomości powinny posiadać odpowiedni format, dzięki temu mamy pewność, że wiadmość trafi do poprawnego odbiorcy i zostanie poprawnie przetworzona.

- `message size`
    
    Dopuszczalny rozmiar wiadomości, dane przesyłane przez sieć powinny dodatkowo spełniać wymogi dotyczące swojej długości. Mniejsze wiadomości powinny być dopełnione, a dłuższe wiadomości podzielone na kilka mniejszych. Ramki nie spełniające tych wymogów nie są dostarczane do hosta docelowego.

- `message timimg`

    Zarządzanie przesyłaniem wiadomości w czasie, nadawanie i odbiór wiadomości w określonych ramach czasowych.

    `Access method` - ustalenie momentu, w którym host może zacząć nadawać, jak zareagować w momencie, gdy medium jest zajęte, wykrywanie kolizji

    `Flow control` - uzgodnienie szybkości transmisji danych, tak aby oba hosty zdążyły przetworzyć informację

    `Reponse timeout` - ile hosty mają oczekiwać na odpowiedź i jakie kroki podjąć gdy jej nie otrzymamy w założonym czasie (retransmisja lub kontynuowanie transmisji)


- `message delivery options`

    W jaki sposób mają być dostarczone wiadomości: 
    
    - `unicast` - do konkretnego hosta, 
    - `multicast` - grupy 
    - `broadcast` - wszystkich w sieci
  
  Czy potrzebujemy potwierdzenia dostarczenia wiadomości (TCP czy UDP)

---

`Enkapsulacja` - kapsułkowanie, opakowywanie, proces umieszczania wiadomości posiadającej określony format wewnątrz innej wiadomości o innym formacie. Proces odwrotny to `dekapsulacja`.

`Segmentacja` - dostosowanie wiadomości, tak aby spełniała założenia co do jej poprawnej długości (np. podział na kilka mniejszych). 

>Każdy segment enkapsulowany jest w oddzielną ramkę zawierającą informację adresową, a następnie wysyłany poprzez sieć. Host docelowy odbiera poszczególne ramki, dokonuje ich deenkapsulacji, a następnie składa segmenty w jedną całość odtwarzając tym samym przesyłaną wiadomość, która następnie może być przetworzona i zinterpretowana.

[Dokładniejsze definicje segmentacji i enkapsulacji znajdują sie nieco niżej.](#33-przenoszenie-danych-w-sieci)

## 3.2 Protokoły

>Grupę powiązanych ze sobą protokołów wymaganych do zapewnienia komunikacji nazywamy zestawem lub stosem protokołów. Zestawy protokołów są implementowane na hostach i urządzeniach sieciowych, w oprogramowaniu, sprzęcie lub równocześnie w sprzęcie i oprogramowaniu.

>Niższe warstwy stosu zajmują się przesyłaniem danych w sieci oraz zapewnianiem odpowiednich usług warstwom wyższym, które z kolei skupiają się na zawartości wysyłanej wiadomości.

Przykład stosu protokołów:
- HTTP
- TCP
- IP
- Ethernet

>Pierwszą siecią wykorzystującą przełączanie pakietów i jednocześnie poprzednikiem dzisiejszego Internetu była sieć ARPANET (Advanced Research Projects Agency Network)

ALOHAnet - pierwsza pakietowa sieć radiowa, została ona opracowana na Uniwerytecie Hawajskim.

Popularne protokoły TCP/IP:

- Wartstwa aplikacji:
  - `DNS` - Domain Name System
  - `HTTP` - Hypertext Transfer Protocol
  - Konfiguracja sieciowa hosta:
    - `BOOTP` - poprzednik DHCP
    - `DHCP` - Dynamic Host Configuration Protocol
  - Poczta:
    - `SMTP` - Simple Mail Transfer Protocol, wysyłanie wiadomości email
    - `POP3` - Post Office Protocol, umożliwia pobieranie i kasowanie poczty
    - `IMAP` - Internet Message Access Protocol, to samo co POP3, dodatakowo pozwala na zarządzanie folderami pocztowymi
  - Transfer plików:
    - `FTP` - File Transfer Protocol
    - `TFTP` - Trivial File Transfer Protocol (UDP)
- Warstwa transportowa:
  - `TCP` - Transmission Control Protocol
  - `UDP` - User Datagram Protocol
- Warstwa internetu:
  - `IP` - Internet Protocol
  - `NAT` - Network Address Translation
  - `ICMP` - Internet Control Message Protocol, wspacie dla IP np. ping, traceroute
  - Protokoły routingu:
    - `OSPF` - Open Shortest Path First
    - `EIGRP` - Enhanced Interior Gateway Routing Protocol
- Warstwa dostępu do sieci:
  - `ARP` - Address Resolution Protocol - na podstawie MAC uzyskujemy adres IP hosta
  - `PPP` - Point-to-Point Protocol
  - `Ethernet`
  - `Drivers` - sterowniki NIC

Oczywiście zestaw protokołów TCP/IP musi być poprawnie zaimplementowany na oby hostach, żeby komunikacja przebiegała poprawnie.

### 3.2.1 Organizacje standaryzujące

- `ISOC` - The Internet Society - promowanie rozwoju, ewolucji oraz zwiększenia wykorzystania Internetu na świecie
- `IAB` - The Internet Architecture Board - Rada ds. Architektury Internetu, nadzór nad architekturą protokołów i procedur w Internecie
- `IETF` -  The Internet Engineering Task Force - opracowanie, utrzymanie Internetu i technologii TCP/IP. Tworzą dokumenty `RFC` (Request for Comments) opisujące protokoły i technologie w Internecie. Organizacja składa się z `WG` (Working Group) tworzonych na krótki czas w celu wykonania określonego zadania.
- `IRTF` - Internet Research Task Force - badania długoterminowe, obecnie powołone są grupy takie jak:
  - ASRG - Anti-Spam Research Group
  - CFRG - Crypto Forum Research Group
  - P2PRG - Peer-to-Peer Research Group
  - RRG - Router Research Group

- `IEEE` - The Institute of Electrical and Electronics Engineers - zrzesza osoby z dziedziny elektroniki i elektroniki. Standardy mają wpływ na różnie branże przemysłu
  - `IEEE 802.3` - definiuje działanie podwarstwy MAC dla przewodowej sieci Ethernet (LAN)
  - `IEEE 802.5` - Token Ring
  - `IEEE 802.11` - definiuje standardy realizujące komunikację bezprzewodową WLAN
  - `IEEE 802.15` - Bluetooth
  - `IEEE 802.16` - WiMAX
- `ISO` - The International Organization for Standardization - największy na świecie twórca standardów dla branż m.i.n.: opracowała 7 warstwowy model odniesienia `ISO OSI` (Open Systems Interconnection) i  Standard zapisu płyt CD/DVD `ISO 9660`
- `EIA` - Electronic Industries Alliance - stanadardy, handel elektroniczny, instalacje elektryczne, złącza (`EIA-TIA 568A/B`), szafy 19 calowe do montażu urządzeń sieciowych (szafy typu RACK)
- `TIA` - Telecommunications Industry Association - głównie stanadardy dotyczące: urządzeń radiowych, wieże komórkowych, urządzeń `VoIP` i łączności satelitarnej. Często współpracuje z `EIA`.
- `ITU-T` - nternational Telecommunication Union-Telecommunication Standardization Sector - najstarsza komunikacyjna organizacja normalizująca. Opracowane standardy: kompresja wideo, telewizja internetowa, łącza szerokopasmowe, DSL, numery kierunkowe do innych krajów ->`kody krajów ITU`
- `ICANN` - The Internet Corporation for Assigned Names and Numbers - odpowiedzialność za zarządzanie zasobami internetu tj. koordynacja przydzielenie adresów IP, zarządzanie nazwami domen, identyfikowanie protokołów i numerów portów. 
- `IANA` - Internet Assigned Numbers Authority - departamentem ICANN odpowiadający za nadzorowanie i zarządzanie przydziałem adresów IP, zarządzanie nazwami domenowymi i identyfikatorami protokołów dla ICANN.

### 3.2.2 Modele odniesienia

Przedstawienie sieci w formie modelu pozwala na łatwe przedstawienie sposobu działania i zależności między protokołami.

Zalety stosowania modelu warstwowego:
- ułatwia projektowanie protokołów
- umożliwia uczciwą konkurencję
- zmiany w obrębie jednej warstwy nie mają wpływu na inne
- zapewnia wspólny spoób przedstawienia funkcji i możliwości sieci

`Model protokołów` - przedstawia schemat zbliżony do struktury konkretnego zestawu protokołów np. model `TCP/IP` - 4 warstwy, najpopularniejszy w sieciach komputerowych ze względu na powszechność implementacji stosu `TCP/IP`

`Model odniesienia` - zapewnia spójność między wszystkimi typami protokołów, określa co powinno być zrobione w określonej warstwie a nie w jaki sposób powinno być to zrobione. Podstawowy cel tego modelu to łatwe przedstawienie funkcji i porcesów zachodzących podczas komunikacji. Bardziej wykorzystywany w tradycyjnej komunikacji, model starszy niż TCP/IP. `IOS OSI` - Open Systems Interconnection 7 warstw.

#### 3.2.2.1 Model OSI

- 7.` Warstwa aplikacji` - warstwa najbliżej człowieka, interfejs 
- 6.` Warstwa prezentacji` - zapewnia odpowiedni format reprezentacji danych, przedstawianych w warstwie 7
- 5.` Warstwa sesji` - zarządza wymianą danych, utrzymaniem sesji między hostami
- 4.` Warstwa transportowa` -  tu następuje podział danych na segmety i składanie ich w całość u hosta docelowego
- 3.` Warstwa sieciowa` - nadawanie adresu IP, routing, opakowanie segmentów w pakiet
- 2.` Warstwa łącza danych` - opisuje metody wymiany ramek między urządzeniami
  - `MAC` - Media Access Control
  - `LLC` - Logical Link Control
- 1.` Warstwa fizyczna` - transmisja bitów przez kanał komunikacyjny w postaci odpowiedniej dla danego medium (sygnał elektryczny, fala elektromagnetyczna, światło), sterowniki karty sieciowej

#### 3.2.2.1 Model TCP/IP

Zbiór protokołów, stos, określający jakie warunki i protokoły muszą być użyte, aby komunikacja przebiegała bez problemów.

- `Warstwa aplikacji` - reprezentacja danych, kodowanie
- `Warstwa transportowa` - zapewnienie ciągłości sesji dla różnych urządzeń w różnych sieciach
- `Warstwa Internetu` - adresy IP, routing
- `Warstwa dostępu do sieci` - media i inne aspekty fizyczne

#### 3.2.2.1 ISO OSI vs TCP/IP

| ISO OSI               | TCP/IP                  |
|---                    |---:                     |
|Warstwa aplikacji      |Warstwa aplikacji        |
|Warstwa prezentacji    |                         |
|Warstwa sesji          |                         |
|Warstwa transportowa   |Warstwa transportowa     |
|Warstwa sieciowa       |Warstwa Internetu        |
|Warstwa łącza danych   |Warstwa dostępu do sieci |
|Warstwa fizyczna       |                         |

### 3.3 Przenoszenie danych w sieci

Tak jak wcześniej pisałem `segmentacja` to nic innego jak podział wiadomości na mniejsze części.

Zalety segmentacji:
- przesyłając mniejsze porcje danych możemy wykorzystać z przeplataniem jej z innymi wiadomościami. To przeplatanie jest inaczej nazywane `multipleksowaniem`.
- wpływa pozytywnie na niezawodność komunikacji, ramki mogą być kierowane różnymi ścieżkami, unikająć w ten sposób zatłoczonych ścieżek
- znacznie szybciej dokonać retransmisji jednego pakietu niż całej wiadomości

`PDU` - Protocol Data Unit - nazwa formy jaką przyjmują dane w danej warstwie:

- `Dane` - jednostka PDU w warstwie aplikacji
- `Segment` - warstwa transportowa
- `Pakiet` - warstwa Internetu
- `Ramka` - warstwa dostępu do sieci
- `Bity` - nazwa jednostki PDU podczas fizycznej transmisji przez medium

`Enkapsulacja` - proces podczas, którego do transmitowanych danych dodawane są dodatkowe informacje, które wynikają z natury funkcjonowania danego protkołu np. nagłówek zawierający numer portu nadawcy i odbiorcy nadawany w warstwie transportowej

`Dekapsulacja` - rozpakowywanie, proces odwrotny realizowany u hosta docelowego (odbiorcy), polega na usuwaniu wcześniej dołożonych nagłówków protokołów. Przesyłanie danych w górę stosu TCP/IP

### 3.3.1 Dostęp do zasobów lokalnych

`Adres IP` - adres znajdujący się w warstwie 3 składa się z dwóch części:
- `prefix sieciowy` - pozwala routerom określić do której sieci należy host
- `część identyfikująca hosta` - indetyfikuje konkretnego hosta w sieci

Pakiet IP zawiera 2 adresy IP:
- `source` - źródłowy host, który wysłał dane
- `destination` - docelowy, adres IP urządzenia odbierającego dane, adresat wiadomości. Ten adres jest wykorzystywany przez routery podczas określenia trasy

`Adres MAC` - zwany adresem warstwy 2, adres łącza danych. Dotyczny lokalnych połączeń. Tutaj dodajemy adres MAC źródłowy i docelowy.

>Umożliwia on poprawne dostarczenie ramki łącza danych z jednego interfejsu sieciowego do innego interfejsu sieciowego znajdującego się w tej samej sieci.

> W sieci Ethernet, adresy łącza danych znane są jako Ethernetowe adresy MAC. Adresy MAC składają się z 48-bitów i są fizycznie zaimplementowane w karcie sieciowej Ethernet. Adres MAC nazywany jest również adresem fizycznym lub adresem BIA (burned−in address – adres wypalony).

Jeśli chcemy wysłać dane do hosta to musimy znać jego adres fizyczny (MAC) i logiczny (IP).

Zazwyczaj znamy adres IP hosta docelowego, jednak nie znamy adresu MAC. W tym celu możemy użyć protokołu `ARP` (Address Resolution Protocol), który mapuje IP na MAC.

>W pierwszym kroku, nadawca wysyła żądanie ARP (ARP Request) do całej sieci LAN. Żądanie to jest komunikatem rozgłoszeniowym zawierającym adres IP urządzenia docelowego. Każde urządzenie w sieci LAN analizuje otrzymane żądanie ARP, aby sprawdzić, czy nie zawiera ono jego adresu IP. W następnym kroku, urządzenie, którego adres IP jest zgodny z adresem zawartym w zapytaniu wysyła odpowiedź (ARP Reply), która skierowana jest tylko do nadawcy. W odpowiedzi ARP zawarty jest adres MAC związany z adresem IP występującym w żądaniu ARP.

### 3.3.2 Dostęp do zasobów zdalnych

Kiedy chcemy wysłać dane do hosta zdalnego, czyli takiego, który znajduje się poza naszą siecią to musimy skorzystać z urządzenia, które łączy nas z innymi sieciami, czyli `routera`. 

`Default gateway` - brama domyślna, adres IP routera znajdujący się w tej samej sieci co host chcący wysłać dane.

`Gdy chcemy wysłać dane do hosta zdalnego to jako docelowy adres fizyczny wskazujemy adres MAC bramy sieciowej. Adres logiczny pozostaje bez zmian.`

>Jeżeli brama domyślna nie zostanie skonfigurowana w ustawieniach TCP/IP hosta, lub zostanie określona nieprawidłowo, wiadomości przeznaczone dla odległych sieci nie będą mogły być dostarczone.

>Gdy nadawca i odbiorca pakietu IP znajdują się w różnych sieciach, ramki łącza danych Ethernet nie mogą być wysyłane bezpośrednio do hosta docelowego, ponieważ host nie jest bezpośrednio osiągalny w sieci nadawcy. W takim przypadku ramkę Ethernet należy przesłać do innego urządzenia znanego jako brama domyślna lub router. 

Skąd mamy MAC bramy domyślnej ?

>Każde urządzenie zna adres IP routera jako adres bramy domyślnej, skonfigurowanej w ustawieniach TCP/IP. Wszystkie urządzenia w sieci lokalnej w celu wysłania wiadomości do routera używają adresu bramy domyślnej. Host znając adres IP bramy domyślnej, może użyć protokołu ARP, w celu poznania adresu MAC bramy. 

---

<div>
<a href="chapter-02.md">Prev: Chapter 2</a>
</div>
<div align="right">
<a href="chapter-04.md">Next: Chapter 4</a>
</div>