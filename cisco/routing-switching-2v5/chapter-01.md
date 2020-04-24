### Spis treści
- [Chapter 1: Wprowadzenie do sieci przełącznych](#chapter-1-wprowadzenie-do-sieci-prze%c5%82%c4%85cznych)
  - [1.1 Projekt sieci LAN](#11-projekt-sieci-lan)
  - [1.2 Budowa i zasada działania przełącznika](#12-budowa-i-zasada-dzia%c5%82ania-prze%c5%82%c4%85cznika)
  - [1.3 Domeny przełączania](#13-domeny-prze%c5%82%c4%85czania)

# Chapter 1: Wprowadzenie do sieci przełącznych

## 1.1 Projekt sieci LAN

Sieć konwergentna - wiele rodzajów usług(obraz, dźwięk i głos) jedna sieć. Budujemy i zarządamy jedną siecią, przez co znacznie tniemy koszty budowy osobnych sieci.

Sieć taka może wspierać następujące funkcje:
- `kontrola połączeń` - obsługa połączenia telefonicznego: ID rozmówcy, przekazywanie połączen, wstrzymywanie i konferncje
- `wiadomości głosowe` - poczta głosowa
- `mobilność` - otrzymywanie połączen w dowolnym miejscu
- `automatyczny operator` - przyśpiesza obsługę połączeń, przekierowując je bezpośrednio do odpowiedniego wydziału lub osoby

>Nieograniczona sieć Cisco to architektura sieciowa, która łączy różne innowacje i rozważania projektowe, aby umożliwić organizacjom bezpieczne i niezawodne połączenie każdego, z dowolnego miejsca i w dowolnym momencie. 

>Są budowane w oparci o skalowalną infrastrukturę programową i sprzętową. Umożliwia ona współpracę różnych elementów, od przełączników do bezprzewodowych punktów dostępowych oraz pozwala użytkownikom na dostęp do zasobów w dowolnym momencie, zapewniając optymalizację, skalowalność i bezpieczeństwo we współpracy i wirtualizacji.

Zasady budowania nieograniczonych sieci:
- `hierarchiczność` -  każde urządzenia w każdej warstwie pełni odpowiednią rolę, takie działanie upraszcza instalacjęm działanie i zarządzanie a także zmieniejsza domenę awarii w każdej warstwie
- `modułowość` - skalowalność, umożliwia bezproblemową rozbudowę sieci i zintegrowanych usług.
- `odporność` - sieć jest zawsze dostępna
- `elastyczność` - podział ruchu, przy użyciu wszystkich zasobów sieciowych

Hierarchiczny podział sieci, wyróżniamy następujące warstwy:
- `rdzeniowa` - stanowi szkielet sieci, zapewnia bardzo szybką łączność i izolację błędów
- `dystrybucji` - łączy warstwę dostępu z warstwą rdzeniową, agregują domeny rozgłoszeniowe(warstwa 2) i granice routingu(warstwa 3), wysoka dostępność ze względu na zastosowanie nadmiarowych przełączników. Zapewnia także różnorodne usługi dla różnych klas aplikacji na granicy sieci
- `dostępu` - zapewnienie użytkownikowi dostępu do sieci

Dla mniejszych sieci z niewielką liczbą użytkowników, obejmujący pojedynczy budynek stosuje się 2 warstwowy model, zwany projektem ze zwiniętym rdzeniem składa się on z warstw:
- `zmarginalizowana warstwa szkieletowa/dystrybucji`
- `warstwa dostępowa`

Przełączane sieci LAN pozawala na:
- większą elastyczność
- zarządznie ruchem, QoS
- dodatkowe bezpieczeństwo
- wsparcie dla urządzeń bezprzewodowych
- wsparcie dla nowych technologii tj. VoIP, usługi mobilne

## 1.2 Budowa i zasada działania przełącznika

Typy przełączników:
- `przełączniki o stałej konfiguracji` - nie można rozbudowywać pierwotnej konfiguracji(dodanie kolejnego slotu portów)
- `przełącznik w konfiguracji modułowej` - znacznie większa elastyczność, w zależności od obudowy możemy zainstalować różne karty modułowe rozszerzające, które zwiększają funkcjonalności oferowane przez przełącznik
- `przełącznik w konfiguracji stosu` - przełączniki są łączone specjalnym przewodem, który zapewnia dużą przepustowość połączenia między nimi, Cisco `StackWise` pozwala na łączenie do dziewięciu przełączników. Połączone w ten sposób przełączniki działają jak jeden większy przełącznik. Rozwiązanie stosowane w miejscach gdzie duże znaczenie ma odporność na awarie i dostępność szerokiego pasma. 

`StackPower` - rozwiązanie Cisco pozwala na współdzielenie lini zasilającej

Główne czynniki, które mają wpływ na wybór typu przełącznika:
- koszt
- gęstość portów
- moc
- niezawodność
- szybkość portu
- bufory ramek
- skalowalność

Przełączanie polega na wykorzystywaniu swojej tablicy adresów MAC, zwanej tablicą przełączania, w celu przesłania wiadomości na odpowiedni port `bazując na porcie wejściowym i adresie docelowym zawartym w wiadomości`. Tablica przełączania zawiera informacje o połączeniach port - adres fizyczny hosta podłączonego do tego interfejsu.

Tablica adresów MAC, nazywana jest czasami pamięcią skojarzoną, `CAM`(Content Addressable Memory). Jest umieszczona na specjalnym typie pamięci, który doskonale nadaje się do szybkiego wyszukiwania. 

>Przełącznik zapełnia tablicę adresów MAC bazując również na adresach źródłowych MAC. Kiedy przełącznik otrzymuje ramkę przychodzącą zawierającą adres MAC, który `nie istnieje w tablicy adresów MAC`, przesyła on tę ramkę na wszystkie porty (tzw. `zalewanie portów`) z wyjątkiem portu wejściowego. Kiedy `urządzenie docelowe odpowie, przełącznik dodaje do tablicy adres źródłowy z ramki i numer portu`, do którego nadeszła ta ramka. W sieciach z wieloma połączonymi ze sobą przełącznikami, `tablica adresów MAC może zawierać wiele adresów MAC przypisanych do pojedynczego portu`.

>Wpis z danym adresem MAC jest typowo przetrzymywany w tablicy przez `pięć minut`.

Most ethernetowy - wczesna wersja przełączników

> Przełączniki te były zdolne do przeniesienia decyzji o przesyłaniu ruchu z oprogramowania do wyspecjalizowanych układów scalonych (Application-Specific Integrated Circuit, ASIC). Układy `ASIC` redukują czas obsługi pakietów i umożliwiają urządzeniom obsługę większej liczby portów bez obniżenia wydajności w sieci. Ta metoda przesyłania ramek warstwy drugiej nazywana jest przełączaniem typu `store-and-forward`.

Przełączanie store-and-forward zapewnia następujące właściwości:
- sprawdzanie błędów - obliczanie sumy kontrolnej FCS
- automatyczne buforowanie - pozwala na obsługę ramek przychodzących na porty obsługujące równe prędkości

Metoda store-and-forward jest podstawową metodą przełączania w urządzeniach Cisco.

Przełączanie cut-through w przypadku sieci o małej przepustowości i wysokiej stopie błędu(niepoprawnych ramek) może mieć negatywny wpływ na wydajność takiej sieci.

Więcej o metodach przełączania znajduje się w 5 rozdziale 1 semestru kursu.

>Przełączanie `typu cut-through`, wprowadzające małe opóźnienia, jest odpowiednie dla bardzo wymagających aplikacji obliczeniowych wysokich wydajności (high-performance computing, `HPC`), które wymagają opóźnień na poziomie 10 mikrosekund i mniejszych.

## 1.3 Domeny przełączania

Segmenty sieci, w których pasmo jest współdzielone przez urządzenia, nazywane są domenami kolizyjnymi, ponieważ jeżeli w takim segmencie dwa lub więcej urządzeń chce nadawać w tym samym czasie, `mogą wystąpić kolizje`.

Wykorzystywanie przełączników i routerów w sieci umożliwia podział jej na segmenty, które ograniczają, a zarazem zwiększają ilość domen kolizyjnych. Tym samym zmniejszają liczbę urządzeń rywalizujących o pasmo, nowy segemnt = nowa domena kolizyjna. Taki proces nazywany jest mikrosegmentacją. Kolizja w jednej domenie nie zakłóca pracy w pozostałych segmentach.

Domena kolizyjna - zbiór wzajemnie połączonych przełączników. Routery stoją na granicy domen rozgłoszeniowych.

Przełączniki nie fitrują ramek rozgłoszeniowych rozsyłając je na wszystkie porty z wyjątkiem portu wejściowego.

>Ramki rozgłoszeniowe są czasami używane do wstępnej lokalizacji innych urządzeń i usług sieciowych, ale zmniejszają one również wydajność sieci. Szerokość pasma w sieci jest zużywana do przesyłania ruchu rozgłoszeniowego. Zbyt dużo rozgłoszeń oraz znaczny ruch w sieci może doprowadzić do przeciążenia - spadku wydajności sieci.

>Przełączniki zapewniają komunikację pomiędzy urządzeniami w trybie `pełnego dupleksu`. Połączenie w trybie pełnego dupleksu może jednocześnie odbierać i wysyłać sygnał. Połączenia takie drastycznie zwiększyły wydajność sieci LAN i są wymagane dla szybkości Ethernetu 1Gb/s i wyższych.

>Przełączniki łączą segmenty sieci LAN (domeny kolizyjne), używają tablicy adresów MAC w celu określenia segmentu, do którego jest kierowana dana ramka i mogą zmniejszyć lub nawet wyeliminować kolizje w sieci.

<div>
<a href="../routing-switching-1v5/chapter-11.md">Prev: Chapter 1.11</a>
</div>
<div align="right">
<a href="chapter-02.md">Next: Chapter 2</a>
</div>