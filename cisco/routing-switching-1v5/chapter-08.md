### Spis treści
- [Chapter 8: Adresowanie IP](#chapter-8-adresowanie-ip)
  - [8.1 Adresy sieciowe IPv4](#81-adresy-sieciowe-ipv4)
    - [8.1.1 Metody przydziału adresu sieciowego](#811-metody-przydzia%c5%82u-adresu-sieciowego)
    - [8.1.2 Sposoby komunikacji w IPv4](#812-sposoby-komunikacji-w-ipv4)
    - [8.1.3 Rodzaje adresów IP](#813-rodzaje-adres%c3%b3w-ip)
  - [8.2 Adresy IPv6](#82-adresy-ipv6)
    - [8.2.1 Wpółistnienie IPv4 i IPv6](#821-wp%c3%b3%c5%82istnienie-ipv4-i-ipv6)
    - [8.2.2 Rodzaje adresów IPv6](#822-rodzaje-adres%c3%b3w-ipv6)
    - [8.2.3 Unicast IPv6](#823-unicast-ipv6)
    - [8.2.4 Metody konfiguracji adresu IPv6](#824-metody-konfiguracji-adresu-ipv6)
    - [8.2.5 Adresy multicast IPv6](#825-adresy-multicast-ipv6)
  - [8.3 ICMP](#83-icmp)
    - [8.3.1 Testowanie i sprawdzanie](#831-testowanie-i-sprawdzanie)

# Chapter 8: Adresowanie IP

## 8.1 Adresy sieciowe IPv4

Poprawna adresacja pozwala na umożliwienie komunikacji pomiędzy urządzeniami w sieciach (różnych lub w obrębie jednej sieci).

Adres IPv4, 32 bitowa liczba jest przedstawiana w najczęściej w notacji kropkowo dziesiętnej. 32 bitowa liczba jest dzielona na cztery 8 bitowe oktety.

Maska podsieci jest również liczbą 32 bitową, jej zadaniem jest wskazanie która część adresu IP należy do adresu sieci, a która identyfikuje hosta w danej sieci.
- `1` - reprezentują bity adresu `sieci`
- `0` - reprezentuje bity adresu `hosta`

`Maska podsieci` wskazuje gdzie szukać w adresie IP części odpowiedzialnej za adres sieci i adres hosta w tej sieci.

Inny sposób zapisu maski podsieci to tzw. `prefix`\`y sieciowe. Długość prefixu to liczba bitów, których wartość w masce podsieci wynosi `1`.

Maska sieciowa: `255.255.255.0`

Prefix: `/24`

W każdej sieci wyróżniamy następujące adresy:
- `adres sieci` - wystkie hosty w tej samej sieci maja identyczną część adresu IP. Adres sieci to taki, który w części identyfikującej hosta ma same 0
- `adres broadcast` - rozgłoszeniowy, umożliwia komunikowanie się ze wszystkimi hostami w danej sieci. Wysyłamy jeden pakiet na ten adres, a każdy host go przetwarza. Adres ten zawiera same jedynki w części hosta. Adres ten jest czasami nazywany `adresem rozgłoszeniowym skierowanym`
- `adres hosta` - wartość pomiędzy adresem sieci a adresem broadcastowym, unikalny adres umożliwiający komunikację w sieci

`Adres pierwszego hosta` - kolejny adres po adresie sieci, adres sieci+1

`Adres ostatniego hosta` - adres bezpośrednio poprzedzający adres rozgłoszeniowy, adres broadcast-1

Podczas wysyłania pakietu host na podstawie porównania adresu IP określa czy host docelowy należy do tej samej sieci, w ten sposób również określa czy pakiety powędrują do bramy sieciowej czy do lokalnego hosta docelowego. 

`AND` - iloczyn logiczny, może być wykorzystany do uzyskania adresu sieci (zestawiając adres IP i maskę podsieci).

>`Pamiętaj`, że jeśli jakieś dwa adresy należą do tej samej sieci lub podsieci, będą traktowane jako adresy dla siebie lokalnie i że komunikują się między sobą bezpośrednio (tj. bez pośrednictwa urządzenia warstwy 3Pamiętaj, że jeśli jakieś dwa adresy należą do tej samej sieci lub podsieci, będą traktowane jako adresy dla siebie lokalnie i że komunikują się między sobą bezpośrednio (tj. bez pośrednictwa urządzenia warstwy 3).

### 8.1.1 Metody przydziału adresu sieciowego

Metody przydziału adresów IPv4:
- `statyczna` - ręczna konfiguracja TCP/IP na każdym hoście. Zazwyczaj ten typ adresu mają nadawane urządzenia, które często powinny być dostępne pod tym samym adresem np. serwery i drukarki
- `dynamiczna` - przypisanie autmatyczne za pomocą DHCP (Dynamic Host Configuration Protocol)

Przy konfiguracji IPv4 lub TCP/IPv4 należy podać:
- adres IP urządzenia
- maskę podsieci
- bramę domyślną

### 8.1.2 Sposoby komunikacji w IPv4

Sposoby komunikacji w IPv4:

- `unicast` - wysyłanie pakietu do pojedynczego hosta, transmisja pojedyncza. Wykorzystywana zarówno w architekturze klient/serwer jak i w węzłach równoważnych (peer-to-peer). Zawsze minimum adres źródłowy jest tego typu. Obejmują zakres od 0.0.0.0 do 223.255.255.255, część z nich jest zarezerwowana do innych celów

- `broadcast` - pakiet wysyłany przez jednego hosta trafia do wszystkich hostów w sieci, transmisja rozgłoszeniowa. Najczęściej ich zasięg jest ograniczony do sieci lokalnej, jednak może to zależeć od konfiguracji routera. Z tego typu transmisji korzysta np ARP i DHCP. Wyróżniamy dwa typy rozgłoszeń:
  - `rozgłoszenie ograniczone` - używane do komunikacji tylko i wyłącznie w obrębie sieci loklanej, w ramach domeny rozgłoszeniowej. Zawsze będzie użyty adres `255.255.255.255`

  - `rozgłoszenie skierowane` - rozgłoszenie wysyłane do wszystkich hostów w danej sieci, adres rozgłoszeniowy. To rozgłoszenie może trafić do hostów poza siecią lokalną, routery standardowo nie przesyłają tego typu pakietów, jednak taką konfigurację można czasem zmienić

- `multicast` - pakiet wysyłany przez hosta trafia do wybranej grupy hostów. Wykorzystywane w celu zaoszczędzenia pasma np. transmisja dźwięku i wideo, gry sieciowe, wymiana informacji między protokołami routingu. Obejmują adresy z zakresu 224.0.0.0 do 239.255.255.255. W śród nich wyróżniamy następujące grupy:
  - link local - adresy lokalnej grupy multicastowej, transmisja grupowa w sieciach lokalnych, adresy 224.0.0.0-255. Router nie przesyła tych pakietów dalej. Z tego względu typowym zastosowaniem tej grupy mogą być protokoły routingu

  - adresy o zasięgu globalnym - umożliwiają wysyłanie danych grupowych w sieci Internet, obejmują adresy 224.0.1.0 - 239.255.255.255. Przykładowo adres 224.0.1.1 jest zarezerwowany do synchronizacji czasu urządzeń sieciowych, czyli `NTP` Network Time Protocol

>Host otrzymujący określony ruch grupowy nazywany jest klientem grupowym (multicastowym). Klienci grupowi mają uruchomione programy (usługi), które zapisują ich do określonej grupy transmisji grupowej (ang. multicast group).

### 8.1.3 Rodzaje adresów IP

`Adresy publiczne` - są wykorzystywane przez hosty, które są dostępne w internecie

`Adresy prywatne` - używane w sieciach prywatnych, które do komunikacji z internetem i tak potrzebują min. 1 adresu publicznego. Hosty znajdujące się w różnych sieciach mogą korzystać z tej samej przestrzeni adresów prywatnych (np LAN). Zakresy adresów prywatnych:
- `10.0.0.0` - `10.255.255.255` (10.0.0.0/8)
- `172.16.0.0` - `172.31.255.255` (172.16.0.0/12)
- `192.168.0.0` - `192.168.255.255` (192.168.0.0/16)

Wyróznia się jeszcze tzw `adresy współdzielone` są one używane w sieciach dostawców internetowych `100.64.0.0/10`.

Specjalne adresy IPv4:
- `adresy sieciowe i rozgłoszeniowe`
- `loopback` - pętla zwrotna 127.0.0.0/8
- `link local adresses` - adresy lokalnego łącza, grupa adresów, które mogą być przydzielone automatycznie przez system operacyjny w sieci gdzie nie ma DHCP. Adresy te powalają na komunikację tylko i wyłącznie w obrębie sieci lokalnej. Do tej grupy należą adresy z grupy `169.254.0.0` - `169.254.255.255`
- `adresy TEST-NET` - adresy przeznaczone do celów nauczania, można je znaleźć w dokumentach RFC, adresy te należą do `192.0.2.0` - `192.0.2.255`.
- `adresy badawcze / eksperymentalne` - `240.0.0.0` - `255.255.255.255`

Historyczna adresacja klasowa:
- `A` - obsługa dużych firm, czyli inaczej sieci z prefixem /8 `0.0.0.0/8 - 127.0.0.0/8`
- `B` - obsługa średnich i dużych sieci, prefix /16, `128.0.0.0 - 191.255.0.0/16`
- `C` - małe sieci do 254 hostów, prefix /24, adresy od `192.0.0.0/24` do `223.255.255.255`

`Adresacja klasowa` nie jest dobrym rozwiązaniem ponieważ często to prowadziło do nie wykorzystania części adresów z danej puli.

Adresacja bezklasowa, CIDR (Classless Inter-Domain Routing) pozwalała na stosowanie prefixów o dowolnej długości.

`RIR` - Regional Internet Registries - nazwy organizacji zarządzających dysponowaniem adresów IP na danym obszarze

Poziomy dostawców interetowych (ISP):
- `Tier 1` - duże krajowe lub międzynarodowe organizacje, które tworzą sieć szkieletową internetu (duża szybkość i niezawodność, ale wysokie koszty)
- `Tier 2` - uzyskuje dostęp do internetu za pośrednictwem ISP klasy 1 (wolniejszy dostęp, mniejsza niezawodność, jednak oferują wiecej usług, są bardziej nastawieni na klientów niż 1 tier, obsługa dużych firm)
- `Tier 3` - w swoich działaniach są głównie nastawieni na odbiorcę indywidualnego związanego z lokalnym rynkiem, tutaj potrzeby odbiorcy końcowego ograniczają sie uzyskania dostępu do internetu


## 8.2 Adresy IPv6

Głównym powodem opracowania IPv6 była zbyt mała przstrzeń adresowa w IPv4. W pewnym opóźnieniu wyczerpania adresów pomógł `NAT` (Network Address Translation), jednak on również ma swoje ograniczenia.

IoT = duża liczba urządzeń = duża liczba zajętych adresów

### 8.2.1 Wpółistnienie IPv4 i IPv6

- `dual stack` - podwójny stos, współistnienie protokołów IPv4 i IPv6 w tej samej sieci
- `tunelowanie` - opakowywanie pakietów IPv6 przez pakiety IPv4
- `translacja` - NAT64, przekształcanie pakietów IPv6 na IPv4 i odwrotnie. Trochę podobne do tradycyjnego NAT.

Adres IPv6 to 128 bitowy adres zapisywany najczęściej w postaci łańcucha liczba szesnastkowych. IPv4 dzielimy na oktety, IPv6 na  hextety (16 bitów, 4 cyfry w hex). Najczęściej IPv6 jest zapisywany w postaci 32 cyfr szesnastkowych.

Zasady skracania adresu IPv6:
- `pomijanie wiodących zer`
  - 01AB -> 1AB
  - 00AB -> AB

- `pomijanie hextetów złożonych z samych zer` - ale tylko jednego takiego ciągu, przjęło się aby pomijać ten hextet, który jest najbliżej prawej strony (czyli ten najbardziej w części hosta)

Przykład kompresji zapisu adresu IPv6:

2001:0000:0BD8:1111:0000:0000:0000:0200 -> 2001:0:DB8:1111::200

### 8.2.2 Rodzaje adresów IPv6
Rodzaje adresów IPv6:
- `unicast` - transmisja pojedynczas
- `multicast` - transmisja/komunikacja grupowa, wysyłaje pojedynczego pakietu do wielu urządzeń jednocześnie
- `anycast` - anycast to adres unicast, który może być przypisany do wielu urządzeń. Pakiet wysyłany na ten adres jest przekierowany do najbliższego urządzenia posiadającego ten adres.

W IPv6 nie ma adresu rozgłoszeniowego, jednak `all-nodes-multicast` ma podobny charakter.
`FF02::1` - all-nodes-multicast
`FF02::2` - all-routers-multicast

`Prefix` w IPv6 pełni podobne funkcje jak w IPv4, wskazuje część sieciową adresu, tutaj rozdziela prefix (część sieciowa) od id interfejsu. Typowa długość prefixu dla sieci LAN to /64.

IPv6 unicast:
- `global unicast` - podobnie jak publiczny adres IPv4, jest globalnie unikalny. Może być ustawiony statycznie lub pozyskany dynamicznie

- `link-local` - adres lokalnego łącza, używany do komunikacji w tej samej sieci lokalnej (w IPv6 odnosi się to również do podsieci). Adresy te nie są routowalne poza obręb podsieci lokalnej. `Każde urządzenie z zainstalowanym protokołem IPv6 musi posiadać adres link-local`. Jeśli nie otrzyma go od DHCP do sam sobie go wygeneruje. Adres ten należy do zakresu `FE80::/10`. Adresy te są również wykorzystywane przez routery do wymiany informacji między sobą, oraz jako next hop w tablicy routingu. Ponad to host używa adresu link-local routera jako adres swojej bramy sieciowej

- `::1/128` - adres loopback, nie może być skonfigurowany dla interfejsu fizycznego

- `::/128` - unspecified, adres nieokreślony, może być użyty jako jako adres źródłowy w momencie gdy urządzenie nie ma jeszcze skonfigurowanego adresu IPv6 lub w przypadku gdy źródło pakietu nie jest istotne dla przeznaczenia

- `FC00::/7 - FDFF::/7` - unique local, adresy unikalne lokalnie, używane do adresacji w ramach pojedynczej lub ograniczonej liczby sieci

- `IPv4 embedded` - wbudowane adresy IPv4, adresy te są używane w celu ułatwienia przejścia z IPv4 do IPv6

>Uwaga: Typowo to adres link-local routera stosowany jest jako adres bramy domyślnej przez urządzenia znajdujące się w (pod)sieci.

### 8.2.3 Unicast IPv6

Adres unicast musi być unikalny w skali całego internetu, jest to grupa adresów routowalnych.

Globalny adres unicast skałda się z 3 części:
- `globalny prefix` - część adresu przydzielana przez ISP z prefixem /48, czyli 3 hextety to prefix, czyli część sieciowa adresu
- `identyfikator podsieci` - używany przez klienta do identyfikacji podsieci w swojej strukturze
- `identyfikator interfejsu` - odpowiednik części identyfikującej hosta w IPv4. W IPv6 używa się terminu identyfikator interfejsu, ponieważ pojedynczy host może mieć wiele interfejsów, a na każdym interfejsie może mieć więcej niż jeden adres IPv6

/64 = /48 + /16

>Uwaga: W odróżnieniu od IPv4 w adresie IPv6 można identyfikatorowi interfejsu przypisać same 0. Jednak adres posiadający identyfikator interfejsu składający się z samych zer jest zarezerwowany dla adresu anycast routera i powinien być konfigurowany tylko na routerach.

### 8.2.4 Metody konfiguracji adresu IPv6
- statycznia
- dynamiczna:
  - `SLAAC` (Stateless Address Autoconfiguration) - bezstanowa autokonfiguracja
  - `DHCPv6`

Konfiguracja adresu IPv6 w Cisco IOS

```
Router (config-if)# ipv6 address 2001:db8:acad:2::2/64
Router (config-if)# no shutdown
```

`SLAAC` - jest to metoda uzyskania prefixu, długości prefixu, bramy domyślnej od routera z uruchomionym IPv6, bez konieczności korzystania z serwera DHCPv6. 

>Routery ze skonfigurowanym IPv6 wysyłają cyklicznie komunikaty ogłoszeniowe ICMPv6 (RA) do wszystkich urządzeń w sieci z uruchomionym protokołem IPv6. Domyślnie, routery Cisco wysyłają komunikaty `RA` (ang. Router Advertisement) co 200 sekund na adres IPv6 komunikacji grupowej "wszystkie węzły". Urządzenie z uruchomionym IPv6 w sieci nie musi oczekiwać na okresowy komunikat RA. Może ono wysłać wiadomość wywołania routera (`RS` - ang. Router Solicitation) używając adresu IPv6 komunikacji grupowej "wszystkie routery". Kiedy router IPv6 otrzymuje wiadomość RS natychmiast odpowie ogłoszeniem routera (RA).

Aby router był uznawany za router IPv6 musi:
- przekazywać pakiety między sieciami
- ma skonfigurowane trasy IPv6 statycznie lub dynamicznie przez protokoły routingu dynamicznego IPv6
- wysyła pakiety RA ICMPv6

Uruchomienie routingu w routerach Cisco
```
Router (config)# ipv6 unicast-routing
```

Komunikat `RA ICMPv6` zawiera prefix, długość prefixu i inne informacje przeznaczone dla urządzenia IPv6, zawiera również informacje jak mamy pozyskać dane konfiguracyjne i tak mamy do wyboru następujące opcje:
- `tylko SLAAC` - czyli mam wszystko czego potrzebujesz
- `SLAAC i DHCPv6` - otrzymuję to samo co powyżej, a bezstanowy serwer DHCP (bezstanowy bo nie śledzi rezerwacji adresów) może dostarczyć dodatkowych informacji tj. adres serwera DNS
- `tylko DHCPv6` - urządzenie nie korzysta z danych uzyskanych z komunikatu RA ICMPv6, tylko tradycyjnie odpytuje serwer DHCP (stanowy, przydziela i rejestruje użycie adresów IPv6)

>Routery wysyłają komunikaty ICMPv6 RA używając adresu link-local jako adresu źródłowego. Urządzenia stosujące SLAAC jako adresu bramy domyślnej używają adresu link-local routera.

`DHCPv6 Solicit` - do wszystkich serwerów DHCPv6

`Proces EUI-64` - Extended Unique Identifier, metoda uzyskiwania 64 bitowego identyfikatora interfejsu na podstawie 48 bitowego adresu MAC dodatkowo dokładająć 16 bitów do środka `FFFE`. Dzięki temu łatwo można przyporządkować adres IPv6 do MAC urządzenia. Jednak takie odwzorowanie budziło nie pokój użytkowników ze względu na bezpieczeństwo i możliwość łatwiejszego śledzenia pakietów

`Losowo generowane identyfikatory interfejsów` - domyślnie od Windows Vista

>Prostym sposobem (dającym prawie pewność) określenia czy adres był stworzony w procesie EUI-64 jest znalezienie wartości FFFE po środku identyfikatora interfejsu.

Po stworzeniu identyfikatora interfejsu może być on połączony z prefixem IPv6 w celu stworzenia globalnego adresu unicast lub adresu link-local. 

Dynamicznie generowany adres link-local jest tworzony na podstawie prefixu FE80::/10 i indetyfikatora interfejsu.

Statyczne konfigurowanie adresu link-local na routerze

```
Router (config-if)# ipv6 address fe80::1 link local
```

W ten sposób generujemy łatwy do zapamiętania adres link local.

Sprawdzanie konfiguracji IPv6.
```
Router# show ipv6 interface brief
Router# show ipv6 route 
```

>Zauważ również, że adres link-local interfejsu serial 0/0/0 jest taki sam jak dla interfejsu GigabitEthernet 0/0. Interfejs szeregowy nie posiada ethernetowego adresu MAC dlatego Cisco IOS używa adresu MAC pierwszego dostępnego interfejsu ethernetowego. Jest to możliwe ponieważ adresy link-local muszą być unikalne w obrębie jednego połączenia.

### 8.2.5 Adresy multicast IPv6

Multicast pełni tutaj podobne funkcje do tych z IPv4. Adresy te mają prefix `FF00::/8`. 

>Uwaga: Adres komunikacji grupowej (ang. multicast) może być tylko adresem docelowym, nigdy adresem źródłowym.

Rodzaje adresów komunikacji grupowej IPv6:
- `przypisany adres komunikacji grupowej`
    Są to zarezerwowane adresy dla predefiniowanych grup urządzeń. Pojedyncze adresy umożliwiające wysłanie pakietu do grupy urządzeń z pewnym uruchomionym protokołem lub usługą np
    - `FE02::1` - grupa multicastowa do, której należą wszystkie urządzenia z IPv6 tzw.`all-nodes-multicast`, np ICMPv6 RA
    - `FE02::2` - grupa multicastowa do której należą wszystkie routery IPv6 (polecenie `ipv6 unicast-routing`), np ICMPv6 RS wysyłany przez hosta, które chce otrzymać ICMPv6 RA
- `adres solicited node multicast`
  >Adres multicastowy solicited node to adres, w którym ostatnie dwadzieścia cztery bity są zgodne z tymi samymi bitami w globalnym adresie unicast interfejsu. Jedynymi urządzeniami, które muszą przetworzyć te pakiety to są urządzenia posiadające taką samą wartość 24 najmniej znaczących bitów ze skrajnej prawej części identyfikatora interfejsu.

## 8.3 ICMP

Protokół ICMP jest najczęściej wykorzystywany do informowania nadawcy o wystąpieniu błędu w transmisji (np gdy TTL=0). W niektórych sieciach ze względów bezpieczeństwa te komunikaty są często blokowane.   

Przykłady komunikatów IPv6:
- `potwierdzenie dostępności hosta` - ICMP Echo Request, jak host docelowy jest dostępny to odpowiada komunikatem ICMP Echo Reply
- `przeznaczenie lub usługa niedostępna` - Destination Unreachable, możliwe komunikaty:
  - sieć niedostępna
  - host niedostępny
  - protokół niedostępny
  - port niedostępny

- przekroczony czas - TTL (IPv4), lub Hop Limit(IPv6) = 0
- przekierowanie trasy - ICMP Redirect Message, ma na celu powiadomienie hostów o lepszej trasie do konkretnej sieci docelowej. Taka sytuacja może nastąpić w momencie, gdy mamy kilka bram sieciowych w jednej sieci.

ICMPv6 zawiera dodatkowo protokoły wchodzące w skład protokołów `ND` - Neighbor Discovery Protocol:
- `Router Solicitation message` - Komunikat wywołania routera, host wysyła ten komunikat do routerów
- `Router Advertisement message` - Komunikat rozgłoszenia routera, komunikaty wysyłane przez routera do hostów w celu dostarczenia informacji o adresacji 
- `Neighbor Solicitation message` - Komunikat wywołania sąsiada 
- `Neighbor Advertisement message` - Komunikat rozgłoszenia sąsiad

Komunikaty NS i NA są wykorzystywane do:
- odwzorowywania adresów - tak jak ARP w IPv4, tu znamy IPv6 a nie znamy adresu MAC, oczywiście gdy oba hosty są w tej samej sieci LAN
- `DAD` - Duplicate Address Detection, wykrywanie konfliktów adresów, zasada działania podobna jak w odwzorowywaniu adresów, jednak gdy tutaj nie dostaniemy informacji zwrotnej to zakładamy, że tylko my mamy ten adres IPv6

### 8.3.1 Testowanie i sprawdzanie

>Ping to narzędzie, które używa żądania ICMP echo (ang. echo request) i odpowiedzi na to żądanie (ang. echo reply) do testowania połączenia pomiędzy hostami. Komenda ping działa na hostach z IPv4 oraz IPv6. 

Ping może być też używany jako miara wydajności sieci.

Testowanie adresu pętli zwrotnej wskazuje, że protokół IP jest zainstalowany poprawnie na hoście.

Testowanie połączeń w sieci lokalnej

>W przypadku takiego testu, najczęściej używany jest adres bramy, ponieważ podczas normalnej pracy sieci, router cały czas jest włączony. Jeśli adres bramy domyślnej nie odpowie, można użyć do testu adresu dowolnego hosta znajdującego się w sieci, o którym wiemy, że działa poprawnie.

>`Uwaga`: Wielu administratorów sieci ogranicza lub wręcz zabrania przesyłania komunikatów ICMP w sieciach korporacyjnych, tak więc brak odpowiedzi może być spowodowany nałożonymi restrykcjami bezpieczeństwa.

Traceroute pozwala nam na uzyskanie szczegółowych informacji o urządzeniach na trasie między hostami.

>Czas RTT (Round Trip Time) mierzony jest od wysłania żądania echa aż do momentu uzyskania odpowiedzi od docelowego hosta. Gwiazdka (*) w zapisie oznacza, że pakiet pozostał bez odpowiedzi.

>Traceroute używa z nagłówka pakietu warstwy 3 pola TTL w IPv4 lub pola limitu liczby przeskoków w IPv6 w powiązaniu z komunikatem ICMP czas przekroczony.

>Jak tylko docelowy host zostanie osiągnięty odpowie on komunikatem ICMP port nieosiągalny lub komunikatem ICMP odpowiedzi na żądanie echa zamiast komunikatu ICMP czas przekroczony.

---

<div>
<a href="chapter-07.md">Prev: Chapter 7</a>
</div>
<div align="right">
<a href="chapter-09.md">Next: Chapter 9</a>
</div>