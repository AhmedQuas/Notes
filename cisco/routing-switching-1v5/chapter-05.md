### Spis treści
- [Chapter 5: Ethernet](#chapter-5-ethernet)
  - [5.1 Protokół Ethernet](#51-protok%c3%b3%c5%82-ethernet)
    - [5.1.1 Adres MAC](#511-adres-mac)
    - [5.1.2 Ramka Ethernet](#512-ramka-ethernet)
    - [5.1.3 MAC vs IP](#513-mac-vs-ip)
  - [5.2 Protokół ARP](#52-protok%c3%b3%c5%82-arp)
    - [5.2.1 Wyświetlanie tablicy ARP](#521-wy%c5%9bwietlanie-tablicy-arp)
    - [5.2.2 Wady i ograniczenia związane z ARP](#522-wady-i-ograniczenia-zwi%c4%85zane-z-arp)
  - [5.3 Przełączniki LAN](#53-prze%c5%82%c4%85czniki-lan)
    - [5.3.1 Metody przekazywania ramek](#531-metody-przekazywania-ramek)
    - [5.3.2 Metody buforowania](#532-metody-buforowania)
    - [5.3.3 Typy przełączników](#533-typy-prze%c5%82%c4%85cznik%c3%b3w)
    - [5.3.4 Przełącznik warstwy 3](#534-prze%c5%82%c4%85cznik-warstwy-3)

# Chapter 5: Ethernet

## 5.1 Protokół Ethernet

Ethernet obejmuje warstwę łącza danych i fizyczną. Umożliwia transmiję danych z prędkością od 10 Mb/s do 100 Gb/s. Obejmuje technologie zdefiniowane w standardach 802.2 i 802.3. Podstawową topologią logiczną jest magstrala wielodostępowa.

Jeszcze raz przedstawienie różnic między LLC i MAC w warstwie łącza danych.

`LLC` - Logical Link Control, podwarstwa wyższa odpowiada za komunikację między warstwami wyższymi a niższymi. Podwarstwa ta jest implemetowana w oprogramowaniu, zatem implementacja ta jest niezależna od fizycznego urządzenia, np w postaci sterownika karty sieciowej.

`MAC` - Media Access Control, warstwa implementowana sprzętowo np w formie karty sieciowej. Podstawowe zadania tej podwarstwy to:
- `Ekapsulacja danych` - utworzenie ramki i jej analizę po odebraniu:
  - `ograniczenie ramki`
  - `adresowanie` - nadanie adresu MAC nadawcy i odbiorcy w ramach najbliższego węzła
  - `wykrywanie błędów` - obliczanie sum CRC
- `Kontrola dostępu do medium` - umieszczanie i usuwanie ramek z medium, podejmowanie odpowiednich działań przed rozpoczęciemi transmisji i podjęci odpowiednich procedur po wykryciu kolizji - mechanizm `CSMA/CD` dla Ethernet.

>Powszechne wykorzystanie technologii przełączanej (switch) we współczesnych sieciach, wymusiło potrzebę stosowania metody CSMA/CD w sieciach lokalnych. Dziś, prawie wszystkie połączenia przewodowe pomiędzy urządzeniami w sieci LAN, to połączenie typu full-duplex, oznacza to, że urządzenia te mają możliwość jednoczesnego wysyłania i odbierana danych. Z uwagi na ten fakt kolizje w dzisiejszych sieciach nie występują, dlatego też procesy wykorzystywane przez CSMA/CD stają się niepotrzebne.

>Jednakże, trzeba wziąć pod uwagę to, iż kolizje wciąż mogą mieć miejsce w przypadku połączeń bezprzewodowych.

### 5.1.1 Adres MAC

Historycznie ten adres nazywa się fizycznym, lub `BIA` (burned-in address) dlatego, że był trwale zapisany w pamięci ROM karty sieciowej i nie może być zmieniony przez oprogramowanie.

W nowoczesnych kartach sieciowych adres MAC jest zapisany w innym miejscu (np. pamięć Flash) i może być dowolnie zmieniony, w ten sposób można łatwo dostać się do sieci gdzie zastosowano filtrowanie adresów MAC. Dlatego też kontrola dostępu opierająca się na filtrowaniu adresów MAC nie może być uznana za bezpieczną.

>Aby zapobiec nadmiernemu obciążeniu, spowodowanemu koniecznością przetwarzania każdej ramki, do pomocy został stworzony adres MAC, który określa adres źródłowy i docelowy wewnątrz sieci Ethernet.

Adres MAC jest przedstawiany jako 48-bitowa wartość przedstawiana za pomocą 12 cyfr hexadecymalnych (4 bity na każdą z cyfr w hex). 

Adresy MAC muszą być globalnie unikalne, z tego względu producenci kart sieciowych muszą być zarejstrowani w IEEE, wtedy mają przydzielony identyfikator sprzedawcy i każdy ich produkt posiada unikalny adres MAC.

Zatem adres MAC składa się z dwóch części:
- `OUI` - Organizational Unique Identifier (24 bitowy, 3 bajtowy) identyfikator sprzedawcy
- ostatnie 6 cyfr hexadecymalnych to numer seryjny karty sieciowej. Unikalne wartości w ramach jednego OUI, producenta

Formaty adresu MAC:
- 00-05-9A-3C-78-00 - Windows
- 00:05:9A:3C:78:00 - Linux
- 0005.9A3C.7800 - Cisco IOS

To kiedy odebrać ramkę jak jesteśmy w wielodostępowej sieci?

>Każda karta sieciowa analizuje informacje, aby stwierdzić, czy docelowy adres MAC jest zgodny z jej adresem fizycznym, znajdującym się w pamięci RAM. Jeżeli nie ma zgodności adresów, to urządzenie odrzuca ramkę.

### 5.1.2 Ramka Ethernet

Wyróżniamy 2 rodzaje ramek Ethernet, różnice między nimi są niewielkie (pole długość jest polem typ, znika SFD):
- `Ethernet` - IEEE 802.3:
  - `Preambuła` - 7B
  - `SFD` -  1B, Start Frame Delimeter, znacznik początku ramki
  - `Adres docelowy MAC` - 6B
  - `Adres źródłowy MAC` - 6B
  - `Długość` - 2B, określa dokładną długość pola danych
  - `Dane` - 46-1500B
  - `Check sum` - 4B CRC

- `Ethernet II` - Ethernet DIX:
  - `Preambuła` - 8B -synchronizacja, powiadamia węzeł odbiorczy o nadchodzącej ramce
  - `Adres docelowy MAC` - 6B
  - `Adres źródłowy MAC` - 6B
  - `Typ` - 2B
  - `Dane` - 46-1500B
  - `Check sum` - 4B CRC

Ethernet II jest stosowany w sieciach TCP/IP.

W przypadku, gdy musimy przesłać mniej niż 64B danych to dokładamy wypełnienie (padding).

Na jakiej podstawie mamy wykryć czy przychodząca do nas ramka jest 802.3 czy Ethernet II (DIX) ?

>Jeżeli dwubajtowa wartość jest równa lub większa od szesnastkowej liczby 0x0600 albo dziesiętnej 1536, to zawartość pola danych jest dekodowana jako identyfikator Ethernet (II?) ~~Typ protokołu~~. Natomiast jeżeli wartość jest równa lub mniejsza niż 0x05DC szesnastkowo albo 1500 dziesiętnie, wówczas pole Długość jest używane w celu wskazania wykorzystanego formatu ramki IEEE 802.3. W taki sposób rozróżniane są ramki Ethernet II i 802.3.

Po krótkich rachunkach wychodzi że minimalny rozmiar ramki to 64 bajty a maksymalny to 1518 bajty. `Standardowo do długości ramki nie wlicza się pola preambuły i SFD`.

Ramka krótsza niż 64B jest uznawana za:  
- `collision fragment` - twór wynikły z kolizji
- ` runt frame` - zbyt krótka ramka

Taka ramka jest automatycznie odrzucana przez stację roboczą. Tak samo wygląda sytuacja, gdy urządzenie odbierze zbyt dużą ramkę. Obaw warianty są uważane za nieprawidłowe i automatycznie odrzucane.

Istnieje standard IEEE 802.3ac, który rozszerzył maksymalny rozmiar ramki do 1522B (dodatkowe 4B), w celu obsługi technologii VLAN

Pole `802.1Q VLAN TAG`:
- `Tag protocol identifier (TPID)` - identyfikator protokołu tagowania
- `Tag control information (TCI)`
  - `Priority code point (PCP)` - priorytet użytkownika, związany z QoS
  - `Drop eligible indicator (DEI)`
  - `VLAN identifier (VID)` - VLAN ID

Grupy adresów MAC:
- `unicast` - 
- `broadcast` - same 1 w części adresu docelowego MAC -> FF-FF-FF-FF-FF-FF
- `multicast` - nadawane w jako adres docelowy, do danej grupy hostów

    >Grupowy adres MAC ma specjalną wartość, która rozpoczyna się liczbą `01-00-5E` w reprezentacji heksadecymalnej. Pozostała część adresu MAC typu multicast jest tworzona przez `przekształcenie mniej znaczących 23 bitów grupowego adresu IP na 6 znaków szesnastkowych`.

### 5.1.3 MAC vs IP

>MAC, adres fizyczny pozostaje ten sam - niezależnie od tego, gdzie host zostanie umieszczony w sieci.

>Adres IP lub adres sieciowy, jest znany jako adres logiczny, ponieważ jest przypisany zgodnie z logiczną topologią sieci.

>Urządzenie źródłowe wyśle pakiet w oparciu o adres IP. Jednym z najczęstszych sposobów określania adresu IP urządzenia docelowego przez urządzenie źródłowego jest korzystanie z serwera DNS (ang. Domain Name Service), w którym adres IP jest powiązany z nazwą domeny. Na przykład www.cisco.com odpowiada adres 209.165.200.225.

>W sieciach Ethernet adresy MAC są wykorzystywane do identyfikacji hostów źródłowych i docelowych na niższym poziomie. Każdy host, który odbiera ramkę, odczytuje docelowy adres MAC. Jeśli docelowy adres MAC, zgadza się z adresem MAC skonfigurowanym w karcie sieciowej hosta, wiadomość zostanie przetworzona.

## 5.2 Protokół ARP

`ARP` - Adress Resolution Protocol, umożliwia  mapowanie logicznych adresów warstwy sieciowej na fizyczne adresy warstwy łącza danych. Innymi słowy odnajduje adres MAC dla podanego adresu IP. Funkcje protokołu ARP:
- odwzorowywanie adresów IPv4 na MAC
- utrzymanie tablicy odwzrowanie (mapowania)

Pierwszym krokiem jest przeszukanie tablicy ARP, zwanej także pamięcią podręczną ARP, lub ARP cache. Jeśli w niej nie znajdzemy wpisu to jesteśmy zmuszeni wysłać żadanie ARP na adres rozgłoszeniowy. Na to rozgłoszenie (`ARP Request`) odpowiada tylko ten węzeł, który ma zgodny adres z adresem IP podanym w żądaniu ARP. Odpowiedź (`ARP Reply`) będzie typu unicast.

Inną metodą utrzymania tablicy ARP jest monitorowanie ruchu sieci i na tej podstawie gromadzenie wpisów w tablicy MAC.

Wpisy w tablicy ARP są opatrzone znacznikiem czasu, po którym są usuwane. Jest też możliwość dodania statycznych wpisów, które nie tracą ważności i mogą być tylko ręcznie usunięte.

>Jeżeli żadne urządzenie nie odpowie na zapytanie ARP, to pakiet jest porzucany, ponieważ nie ma możliwości utworzenia ramki. Informacja o braku powodzenia enkapsulacji jest przekazywana do wyższych warstw urządzenia. Jeżeli urządzenie jest urządzeniem pośredniczącym, np. routerem, to wyższe warstwy mogą poinformować nadawcę o takiej sytuacji wysyłając mu pakiet ICMPv4.

>Adres bramy interfejsu routera jest przechowywany w konfiguracji hosta. Gdy host tworzy pakiet dla docelowego węzła, to porównuje docelowy adres IP ze swoim adresem, aby określić, czy te dwa adresy IP są ulokowane w tej samej sieci warstwy 3. Jeżeli docelowy host nie jest w tej samej sieci, nadawca wykorzystuje proces ARP do określenia adresu MAC interfejsu routera pracującego jako brama.

Potrzeba usunięcia wpisu z tablicy ARP może wyniknąć gdy pewien host został usunięty z sieci, a nasza tablica ARP ma nieaktualny wpis dotyczący tego usuniętego hosta.

### 5.2.1 Wyświetlanie tablicy ARP

Urządzenia Cisco
```
Router# show ip arp
```

Windows
```
C:\ arp -a
```

### 5.2.2 Wady i ograniczenia związane z ARP

- `narzut komunikacyjny` - skoro broadcast to musi je przetworzyć każdy host w sieci. Obniżenie wydajności w sieci gdy w jednoczesnym momencie włączymy dużą ilość hostów i zaraz po uruchomieniu będziemy chcieli skorzystać z internetu

- bezpieczeńswto - `ARP spoofing` - podszywanie i `ARP poisoning` zatruwanie ARP. Wstrzykiwanie niepoprawnych odwzorowań do tablicy ARP co umożliwia wysyłanie ramek do złego węzła (możliwość podszycia sie za innego hosta, np bramę sieciową). Statyczne dodawanie wpisów w tablicy ARP zapobiega podszywaniu
    >Autoryzowany adres MAC może być skonfigurowany na kilku urządzeniach sieciowych w celu ograniczenia dostępu tylko do znanych urządzeń.

>Przełączniki zapewniają segmentację sieci LAN, dzieląc sieć lokalną na niezależne domeny kolizyjne. Każdy port przełącznika reprezentuje oddzielną domenę kolizyjną i dostarcza pełną szerokość pasma medium węzłowi lub węzłom podłączonym do tego portu. Chociaż przełączniki domyślnie nie zapobiegają propagacji transmisji rozgłoszeniowej do podłączonych urządzeń, to izolują one Ethernetowe komunikacje typu unicast tak, że są one "słyszane" tylko przez urządzenie źródłowe i docelowe. Tak więc, jeśli występują liczne żądania ARP to każda odpowiedź ARP wystąpi tylko pomiędzy dwoma urządzeniami.

>W odniesieniu do łagodzenia różnych rodzajów ataków opartych na rozgłoszeniu, na które sieci Ethernet są podatne, inżynierowie sieciowi wdrażają technologie zabezpieczeń przełączników Cisco takie jak specjalizowane listy dostępu (ang. `access list`) i bezpieczeństwo portu (ang. `port security`).

## 5.3 Przełączniki LAN

Podstawową topologią logiczną jest magstrala wielodostępowa. Jednak dzisiaj fizyczną topologią w większości sieci Ethernet to topologia gwiazdy lub gwiazdy rozszerzonej. Czyli centralnym punktem jest przełącznik, a urządzenia końcowe są połączone punkt-punkt do wspomnianego wcześniej przełącznika.

>Przełącznik LAN działający na poziomie warstwy 2 wykonuje przełączanie i filtrowanie tylko na podstawie adresu MAC. Przełącznik jest całkowicie przezroczysty dla protokołów sieciowych i aplikacji użytkownika. Przełącznik warstwy 2 buduje tablicę adresów MAC, z której korzysta podczas podejmowania decyzji odnośnie przekazywania. Przełączniki warstwy 2 korzystają z routerów aby przekazywać dane pomiędzy niezależnymi podsieciami IP.

Uzupełnianie tablicy adresów MAC przełącznika

>Gdy w tablicy adresów zostanie zapisany adres MAC określonego węzła podłączonego do konkretnego portu, wówczas przełącznik wie, że następne transmisje adresowane do tego węzła powinien kierować przez ten właśnie port. Jeśli przełącznik odbierze ramkę z danymi i w swojej tablicy nie znajdzie docelowego adresu MAC, to przekaże ramkę przez wszystkie porty z wyjątkiem tego, przez który ją odebrał. Gdy węzeł docelowy udzieli odpowiedzi, przełącznik zarejestruje adres MAC tego węzła w swojej tablicy adresów.

Co jeśli do przełącznika jest podłączony inny przełącznik?
>W sieciach, w których występuje wiele wzajemnie połączonych przełączników, w tablicach adresów MAC są rejestrowane adresy MAC tych portów łączących przełączniki, które odpowiadają znajdującym się za nimi węzłom. Zazwyczaj dla portów przełączników wykorzystanych do połączenia dwóch przełączników jest w tablicy adresów MAC zarejestrowanych wiele adresów.

`Proces zalewania` - wysyłanie ramki na wszystkich interfejsach w przypadku gdy nie mamy wpisu dotyczącego tego adresu w tablicy adresów MAC.

`Tablica CAM` - Content Addressable Memory - inna popularniejsza nazwa tablicy adresów MAC.

>Większość oferowanych dzisiaj kart Ethernet, Fast Ethernet i Gigabit Ethernet może pracować w trybie `pełnego dupleksu`. W trybie tym jest `wyłączony obwód wykrywania kolizji`. Między ramkami, które są wysyłane przez dwa połączone węzły końcowe, nie może wystąpić kolizja, ponieważ `węzły te korzystają z dwóch osobnych obwodów` w kablu sieciowym.

`Auto-MDIX` - funkcja pozwalająca na automatyczne wykrycie wymaganego typu połączenia (full-dupex half-duplex), czy połączenie wymaga przeplotu, czy zwykłego przewodu.

```
Switch (config-if)# mdix auto
```

### 5.3.1 Metody przekazywania ramek

`Store and forward` - przechowaj i przekaż, przełącznik odbiera całą ramkę, oblicza sumę kontrolną CRC, jeśli CRC jest poprawny to przekazujemy dalej, jeśli CRC się nie zgadza, czyli wystąpił błąd to odrzucamy taką ramkę. Odrzucanie ramek na poziomie przełącznika powoduje mniejsze obciążenie sieci spowodowane ruchem uszkodzonych ramek. Ta metoda przekazywania ramek jest też wymagana jeżeli mamy zapewnić `QoS`.

`Cut-throught` - przekazuje ramkę na bieżąco, do rozpoczęcia przesyłania ramki potrzebny jest tylko jej adres docelowy, czyli musimy buforować do tego momentu. Jest to szybsza metoda przełączania, jednak może w ten sposób przełączać ramki uszkodzone, które i tak w urządzeniu końcowym zostaną odrzucone. W ramach przełącznia w locie wyróżniamy jeszcze 2 metody przełączania:
- `Fast-forward switching` - najmniejsze opóźnienia, przesyłanie pakietu natychmiast po odczytaniu adresu docelowego
- `Fragment-free switching` - kompromis między fast-forward i store and forward, przechowuje pierwsze 64B ramki zanim zacznie ją przekazywać. Statystycznie najczęściej błędy występują w pierwszych 64B, najczęstszy powód to kolizja i inne błędy sieciowe.

>Niektóre przełączniki są skonfigurowane do stosowania dla poszczególnych portów metody przełączania w locie dopóty, dopóki nie zostanie osiągnięty zdefiniowany przez użytkownika limit błędów. Gdy to nastąpi, przełącznik automatycznie zacznie stosować metodę „przechowaj i przekaż”.

### 5.3.2 Metody buforowania
`port-based` - ramki przechowywane w kolejkach, które są `powiązane z konkretnymi portami wejściowymi`. Jedna ramka oczekująca na dostępność swojego portu może blokować inne ramki, nawet mające inny port docelowy. W tej metodzie buforowania ramki trzeba przenosić między kolejkami (wejściową a wyjściową).

`shared memory` - użycie współdzielonej pamięci do przechowywania ramek i dynamicznym wiązaniem z portem docelowym, odbywa się to bez przenoszenia między kolejkami. Umożliwia przesyłanie większych ramek jednocześnie nie zwiększając liczby odzrzuconych ramek. Pozwala na przełączanie asymetryczne, umożliwia wzrost przepustowości.

### 5.3.3 Typy przełączników

`SFP` - Switch Form-Factor Pluggable, powala na połączenie płyty głównej urządzenia (switcha, routera) z kablem świtłowodowym. Podobnym rozwiązaniem jest Direct Cable.

`PoE` - Power over Ethernet, pozwala na dostarczenie zasialania poprzez istniejące okablowanie.

`Forwarding rate` - określa ile danych na sekundę przełącznik może przetwarzać

`stackable` - czy przełączniki można łączyć w wieżę

Innym parametrem jest grubość przełącznika, często wyrażana w tzw rack unit `U`. 1U to wysokość pojedynczego slotu w szafie montażowej. Inaczej ta jednostka określa ile slotów w takiej szafie zajmuje urządzenie.

`1U` = 1 i 3/4 cala = 44,45 mm

`Przełącznik o stałej konfiguracji` - fabrycznie wyposażony przełącznik, później nie można dodać żadnych innych funkcji ani opcji

`Przełącznik modularny` - ich budowa pozwala na instalację różnych kart rozszerzających (line card). Im większa obudowa tym więcej kart możemy zamontować.

### 5.3.4 Przełącznik warstwy 3

>Przełącznik warstwy 3, działa podobnie do przełącznika warstwy 2, ale przy podejmowaniu decyzji o przekazywaniu korzysta nie tylko z informacji o adresie MAC, `ale także z informacji o adresie IP`. Przełącznik warstwy 3 może nie tylko się uczyć, które adresy MAC są powiązane z jego portami, ale także które adresy IP są powiązane z jego interfejsami. Dzięki temu może również kierować ruch przez sieć na podstawie informacji o adresie IP. 
>Przełączniki warstwy 3 mogą także wykonywać funkcje routingu z warstwy 3, a tym samym zmniejsza się konieczność stosowania dedykowanych routerów w sieci LAN. Ponieważ przełączniki te są wyposażone w specjalizowany chipset przełączający, zazwyczaj mogą ustalać trasę dla danych równie szybko, jak przełączać.

W przełącznikach warstwy 3 domyślnie jest włączona technologia `CEF` - Cisco Express Forwarding.

CEF składa się z:
- `FIB` - Forwarding Information Base - z tej struktury korzysta urządzenie w momencie podejmowania decyzji o przekazaniu pakietu bez koniczność korzystania z pamięci podręcznej tablicy routingu
- `Adjacency tables` - Tablice sąsiedztwa, zawierają adresy MAC następnego przeskoku dla wszystkich wpisów FIB

>Zasadniczo CEF oddziela ścisłą współzależność w podejmowaniu decyzji pomiędzy warstwą 2 i warstwą 3. To co spowalnia przesyłanie pakietu IP to są ciągłe odwołania między warstwą 2 i 3 w urządzeniach sieciowych.

Rodzaje interfejsów warstwy 3:
- `Switch Virtual Interface` (SVI) - wirtualny interfejs przełącznika połączony z siecią VLAN.
- `Routed Port` - fizyczny interfejs przełącznika warstwy 3 skonfigurowany tak aby działać jak port routera.
- `Layer 3 EtherChannel` - logiczny interfejs na urządzeniach Cisco powiązany z grupą portów routujących, agregacja przepustowości

Istniją jeszcze interfejsy pętli zwrotnych i interfejsy w postaci tuneli.

Port przełącznika warstwy 3 aby działał jako port routera musi spełniać następujące warunki:
- nie należy do żadnej sieci VLAN
- może być na nim skonfigurowany port routingu
- jest interfejsem warstwy 3, więc nie obsługuje wtedy protokołów warstwy 2

Wszystko to ogranicza się do włączenia trybu warsty 3 czyli wydania polecenia `no switchport`

```
Switch (config)# interface f0/1
Switch (config-if)# no switchport
Switch (config-if)# ip address 192.168.1.10 255.255.255.0
Switch (config-if)# no shutdown
```

---

<div>
<a href="chapter-04.md">Prev: Chapter 4</a>
</div>
<div align="right">
<a href="chapter-06.md">Next: Chapter 6</a>
</div>