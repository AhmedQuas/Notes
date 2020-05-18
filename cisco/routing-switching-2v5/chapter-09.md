### Spis treści
- [Chapter 9: Listy kontroli dostępu](#chapter-9-listy-kontroli-dost%c4%99pu)
  - [9.1 Działanie IP ACL](#91-dzia%c5%82anie-ip-acl)
  - [9.2 Konfigurowanie list ACL IPv4](#92-konfigurowanie-list-acl-ipv4)
    - [9.2.1 Modyfikowanie list ACL](#921-modyfikowanie-list-acl)
    - [9.2.2 Weryfikowanie list ACL](#922-weryfikowanie-list-acl)
    - [9.2.3 Zabezpieczenie portów VTY przy pomocy standardowej ACL](#923-zabezpieczenie-port%c3%b3w-vty-przy-pomocy-standardowej-acl)
  - [9.3 Rozszerzone listy ACL IPv4](#93-rozszerzone-listy-acl-ipv4)
  - [9.4 Rozwiązywanie problemów z listami ACL](#94-rozwi%c4%85zywanie-problem%c3%b3w-z-listami-acl)
  - [9.5 Listy ACL dla IPv6](#95-listy-acl-dla-ipv6)

# Chapter 9: Listy kontroli dostępu

## 9.1 Działanie IP ACL

>`Firewalle` to wyspecjalizowane urządzenia, które posiadają szereg funkcji związanych z bezpieczeństwem, pewne `podstawowe funkcje filtrowania można uzyskać na routerze przy pomocy list kontroli dostępu (ACL)`.

>`ACL` to sekwencyjna lista instrukcji zezwoleń i zakazów, które stosowane są w odniesieniu do adresów lub protokołów wyższych warstw. Jest narzędziem posiadającym duże możliwości w zakresie kontrolowania ruchu przychodzącego do sieci jak i wychodzącego z sieci. Listy ACL można tworzyć dla wszystkich sieciowych protokołów routowanych.

Zadania ACL:
- `limitowanie ruchu, co przekłada się na zwiększenie wydajności sieci` - blokowanie ruchu video
- `zapewnienie kontroli ruchu w sieci` - ograniczyć dostarczanie aktualizacji tras
- `filtrowanie ruchu w oparciu o typ ruchu` - blokowanie ruchu telnet, ftp, zezwalanie na ruch ssh
- `zapewnienie podstawowych zabezpieczeń podczas dostępu do sieci` - ograniczanie ruchu dla wybranych hostów lub sieci

>Bez listy ACL, ruch przychodzący do routera jest kierowany wyłącznie w oparciu o informacje w tablicy routingu. W momencie, gdy umieścimy na interfejsie listę ACL, router dodatkowo przeanalizuje wszystkie pakiety przechodzące przez ten interfejs i ustali, czy dany pakiet ma być przekazany.

`ACE` - Access Control Entries, wpis kontroli dostępu

Podczas analizy ACL może wyodrębnić następujace informacje:
- warstwa 3:
  - źródłowy adres IP
  - docelowy adres IP
  - typ komunikatu ICMP
- warstaw 4:
  - port źródłowy tcp/udp
  - port docelowy tcp/udp

Zatem filtrowanie pakietów opiera się na 3 i 4 warstwie modelu OSI.

Ze względu na rodzaj obsługiwanego ruchu możemy wyróżnić następujące rodzaje list:
- `Inbound ACL` - wejściowe listy ACL, filtrowane przed rozpoczęciem procesu routingu na pakiecie. Bardzo efektywne podejście pozwalajace zaoszczędzić zasoby,
- `Outbound ACL` - wyjściowe listy ACL, wykonywane są gdy pakiet podczas procesu routingu trafi na interfejs wyjściowy. Takie podejście jest najbardziej użyteczne w sytuacji, gdy te same reguły filtrowania mają być zastosowane do pakietów przychodzących z wielu interfejsów wejściowych, zanim opuszczą interfejs wyjściowy.

>Na końcu każdej listy ACL umieszczona jest niejawna odmowa. `Niejawna odmowa` powoduje odrzucenie całego ruchu.

Urządzenia Cisco obsługują:
- `standardowe listy ACL` - mogą filtrować tylko na podstwie adresów źródłowych IPv4. Ze względu, że te listy nie uwzględniają adresów docelowych zaleca się, aby umieszczać je `blisko sieci docelowej`,
- `rozszerzone listy ACL` - filtrują na podstawie informacji zawartych w warstwie 3 i 4. Zaleca się umieszczać listy tego rodzaju tak blisko źródła filtrowanego ruchu jak tylko jest to możliwe

Listy ACL tworzymy z poziomu konfiguracji globalnej.

Listy ACL mogą być:
- `numerowane`:
  - 1-99 i 1300-1999 - standardowa lista ACL
  - 100-199 i 2000-2699 - rozszerzona lista ACL
- `nazywane`

`Maska blankietowa` to 32-bitowy ciąg cyfr binarnych, jest używana przez router, w celu określenia, na których bitach adresu występuje dopasowanie. Maska blankietowa często jest określana jako maska odwrotna, inverse mask.

`0 w masce blankietowej` - ten bit adresu IP musi być dopasowany

`1 w masce blankietowej` - pełna dowolność, ignorowanie odpowiadającego bitu adresu IP

`Maski blankietowe nie muszą być ciągiem jedynek, jak ma to miejsce dla masek podsieci.`

>Najszybszą metodą jest po prostu wykonanie odejmowania maski podsieci od pełnej maski 255.255.255.255.

Słowa kluczowe związane z maskami blankietowymi:
- `host` - wszystkie bity muszą się zgadzać, odpowiednik 0.0.0.0 
- `any` - każdy adres spełnia doposowanie 255.255.255.255

Wytyczne do tworzenia ACL:
- ACL powienien być na routerach pełniących funkcję zapory sieciowej między siecią wewnętrzną a zewnętrzną
- ACL na routerach między wybranymi segmentami naszej sieci
- konfigurowanie ACL na routerach brzegowych, routerach znajdujących się na granicach twoich sieci

`Zasada 3n:`
- `jedna ACL na protokół` - ipv4/ipv6
- `jedna ACL na kierunek` - inbound/outbound
- `jedna ACL na interfejs` - f0/0

Zasady praktyczne dotyczące ACL:
- ACL powinien uwzględniać politykę bezpieczeństwa
- warto wcześniej opisać co chcesz uzyskać tworząc listę ACL
- dobrze jest tworzyć listy ACL w edytorze tekstu
- __listy ACL powinny być sprawdzane w sieci testowej, a nie na produkcji__

Wybór i umiejscowienie listy ACL, zależy od:
- `zakresu kontroli administratora sieci` - czy mamy możliwość kontroli sieci źródłowej i docelowej
- `przepustowości sieci objętych ruchem` - futrowanie niechcianego ruchu na wejściu może może zwiększyć ilość dostępnego pasma
- `prostota konfiguracji`

Dzięki listom ACL można zredukować niepotrzebny ruch w sieci.

>`Generalną zasadą jest`, że rozszerzone listy ACL umieszczamy jak najbliżej źródła, a standardowe ACL - jak najbliżej celu.

>Gdy na interfejsie routera umieszczona jest lista ACL, cały ruch sieciowy przechodzący przez niego porównywany jest z poszczególnymi wpisami ACE w kolejności, w jakiej tam występują. Dla każdego pakietu router przetwarza kolejne reguły do momentu, aż znajdzie dopasowanie. `Gdy takie dopasowanie zostanie znalezione, pakiet zostanie przetworzony zgodnie z regułą, na której to dopasowanie wystąpiło, a pozostałe wpisy ACE nie będą już sprawdzane.`

>Jeśli dopasowanie nie zostanie znalezione do końca listy, ruch zostanie odrzucony. Jest tak, ponieważ, dla ruchu nie pasującego do żadnego wpisu, na końcu każdej listy ACL znajduje się tzw. niejawne odrzucenie.

## 9.2 Konfigurowanie list ACL IPv4

```
Router(config)# access-list access-list-number { deny | permit | remark } source [ source-wildcard ][ log ]
```

`remark` = służy do dodawania komentarzy do wybranych access-list

```
R1 #show access-lists
R1 #show running-config | include access-list 10
```

Tworząc ACL, zaczynamy od szczegółu, a później przechodzimy do szczegółu.

>Wewnętrzna logika systemu IOS dla standardowych ACL, odrzuca drugi wpis i zwraca komunikat błędu, ponieważ `instrukcja ta jest podzbiorem poprzedniej instrukcji.`

Podpięcie listy ACL do interfejsu

```
Router(config-if)# ip access-group { access-list-number | access-list-name } { in | out }
```

Tworzenie nazywanych standardowych list ACL

```
R1 (config)# ip access-list standard NO_ACCESS
R1 (config-std-nacl)# deny host 192.168.11.10
R1 (config-std-nacl)# remark Allow access from all other networks
R1 (config-std-nacl)# permit any
R1 (config-std-nacl)# exit
R1 (config)# interface g0/0
R1 (config-if)# ip access-group NO_ACCESS out  
```

>Nie jest to wymagane, ale zaleca się pisanie nazw list ACL wielkimi literami, ze względu na to, że ​​wyróżniają się podczas przeglądania wyniku komendy show running-config. `Zapobiega to także przypadkowemu utworzeniu dwóch różnych list ACL o tej samej nazwie ale pisanych różnymi wielkościami liter.`

### 9.2.1 Modyfikowanie list ACL

Sposoby modyfikowania list ACL:
1. Użycie edytora tekstu
   - wyświetlenie nieporawnego ACL `show running-config | include access-list 1`
   - skopiowanie i edycja w edytorze tekstkowym
   - usuń ACL `no access-list 1`
    
    >W przeciwnym wypadku, nowe wpisy zostaną dodane do istniejącej listy ACL
   
   - dodaj poprawnego ACL
   - zweryfikuj porawność konfiguracji

2. Użycie numerów sekwencyjnych
   - wyświetlenie niepoprawnego ACL i znalezienie odpowiedniego numeru sekwencyjnego
    `show access-lists 1`
    - modyfikacja ACL
    ```
    R1 (config)# ip access-list standard 1
    R1 (config-std-nacl)# no 10
    R1 (config-std-nacl)# 10 deny host 192.168.10.10
    ```
    >Uwaga: Nie można nadpisać istniejącej reguły, używając tego samego numeru sekwencyjnego co istniejąca reguła. `Bieżąca reguła musi być najpierw usunięta, dopiero potem można dodać nową.`

    - weryfikacja poprawności wprowadzonych zmian


>Należy zaznaczyć, że rożne wersje systemów IOS reagują w różny sposób na użycie komendy no access-list . Jeżeli usunięta właśnie lista ACL jest nadal powiązana do interfejsu, niektóre wersje IOS mogą zachować się tak, jakby interfejs nie był chroniony żadną ACL, podczas gdy inne wersje IOS mogą zablokować cały ruch na tym interfejsie. `Dlatego dobrą zasadą jest, aby najpierw usunąć powiązanie prowadzące z interfejsu do tej listy przed modyfikacją samej ACL.` Należy też pamiętać, że w czasie modyfikacji ACL, sieć nie jest chroniona.

### 9.2.2 Weryfikowanie list ACL

```
R1 # show access-lists
R1 # show ip interface s0/0/0
```

Wyświetlanie statystyk, liczba match\`y

```
R1 # show access-lists
```

>Aby przeglądać statystyki dla reguły niejawnego odrzucenia, należy ręcznie dodać ją do listy ACL, wtedy statystyki dla tego wpisu będą widoczne.

> Należy zachować `szczególną ostrożność przy ręcznym konfigurowaniu reguły deny any`, gdyż pasuje do niej cały ruch. Należy ją zawsze umieszczać na końcu listy ACL, gdyż w przeciwnym wypadku może powodować problemy.

Wyzerowanie liczników dopasowań

```
R1 # clear access-list counters 1
```

>Numer sekwencyjny określa kolejność, w jakiej dany wpis został wprowadzony do systemu a nie kolejność, w jakiej jest przetwarzany.

>Reguły dotyczące hostów są wyświetlane na początku listy, ale niekoniecznie w kolejności ich wprowadzania. Kolejność wpisów dla hostów ustalana jest przez system IOS, przy użyciu specjalnej funkcji hashującej. Celem jest zoptymalizowanie procesu wyszukiwania wpisu hosta na liście wpisów ACE.

>Wpisy dotyczące zakresów wyświetlane są po wpisach hostów. Kolejność wyświetlania tych wpisów jest dokładnie taka sama, jaka była kolejność ich wprowadzania.

Numery sekwencyjne mogą się zmienić po restarcie routera.

### 9.2.3 Zabezpieczenie portów VTY przy pomocy standardowej ACL

Sugestie dotyczące ALC na VTY:
- do sesji VTY można stosować tylko numerowane listy ACL
- użytkownik może się zalogować do dowolnego vty, dlatego te same ACL powinny być na każdym vty

```
R1 (config)# line vty 0 4
R1 (config-line)# login local
R1 (config-line)# transport input ssh
R1 (config-line)# access-class 21 in
R1 (config-line)# exit
R1 (config)# access-list 21 permit 192.168.10.0 0.0.0.255
R1 (config)# access-list 21 deny any
```

## 9.3 Rozszerzone listy ACL IPv4

>Rozszerzone listy ACL są stosowane częściej niż standardowe ze względu na ich większe możliwości i zakres kontroli.

>Przy pomocy rozszerzonych list ACL, podobnie jak przy standardowych, `można filtrować pakiety na podstawie adresów źródłowych, oprócz tego można też kontrolować adresy docelowe, protokoły i numery portów (lub usług).`

Wyrażenia logiczne stosowane w rozszerzonych listach ACL:
- eq - równy
- neq - różny
- gt - większy od
- lt - mniejszy od

```
R1 (config)# access-list 101 permit tcp any any eq 23
R1 (config)# access-list 101 permit tcp any any eq telnet
```

>`Uwaga:` Logika wewnętrzna, stosowana przy ustalaniu kolejności wpisów w standardowych listach ACL, w przypadku list rozszerzonych nie ma zastosowania. `Poszczególne reguły są wyświetlane i przetwarzane w takiej samej kolejności, w jakiej zostały wprowadzone.`

>Parametr `established` powoduje, że tylko ruch, który jest odpowiedzią na ruch rozpoczęty w sieci 192.168.10.0/24, będzie miał zezwolenie na wejście do tej sieci. `Zgodność występuje tylko wtedy, gdy powracający segment TCP posiada ustawiony bit potwierdzenia (ACK) lub reset (RST)`, które oznaczają, że pakiet należy do istniejącego połączenia.

```
R1 (config)# access-list 101 permit tcp 192.168.10.0 0.0.0.255 any eq 80
R1 (config)# interface g0/0
R1 (config-if)# ip access-group 101 in
```

Najpierw oczywiście jest adres źródłowy a później docelowy.

>Z punktu widzenia listy ACL, określenia ruch wejściowy i wyjściowy `zależą do interfejsu` routera, na którym tą listę stosujemy.

>Aby zapobiec niejawnemu odrzuceniu wszystkich pakietów, które występuje na końcu każdej ACL, należy dodać wpis `permit ip any any`. Bez umieszczenia wpisu permit na końcu listy, cały ruch na interfejsie, na którym dowiązano listę ACL, byłby odrzucony.

```
R1 (config)# ip access-list extended NAME
R1 (config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80
```

## 9.4 Rozwiązywanie problemów z listami ACL

>Jeśli adres ramki został zaakceptowany, informacja o ramce jest usuwana, a router sprawdza, czy istnieje lista ACL interfejsu wejściowego. Jeśli lista ACL istnieje, sprawdzane są zawarte w niej reguły i porównywane z informacjami w pakiecie.

>Najczęstsze błędy dotyczą wprowadzania wpisów ACE w `nieprawidłowej kolejności oraz braku stosowania odpowiednich kryteriów` dla reguł ACL.

## 9.5 Listy ACL dla IPv6

Listy ACL IPv6 posiadają następujące cechy:
- występują tylko w wersji nazywanej
- funkcjonalnie odpowiadają rozszerzonym listom ACL IPv4

>Listy ACL IPv4 oraz IPv6 nie mogą współdzielić tej samej nazwy.

Różnice ACL IPv4 i IPv6:
- przypisywanie do interfejsu `ipv6 traffic-filter`
- brak masek blankietowych, zamiast tego mamy prefix
- dodatkowe reguły domyślne
  - permit icmp any any nd-na
  - permit icmp any any nd-ns
  Służą głownie do wykrywanie sąsiadów.

```
R1 (config)# ipv6 access-list NO-R3-LAN-ACCESS
R1 (config-ipv6-acl)# deny ipv6 2001:db8:cafe:20::/64 any 
R1 (config-ipv6-acl)# end
R1 (config)# interface s0/0/0
R1 (config-if)# ipv6 traffic NO-R3-LAN-ACCESS in
```

Weryfikowanie IPv6 ACL

```
R1 # show ipv6 interface
R1 # show access-lists
R1 # show running-config
```

---

<div>
<a href="chapter-08.md">Prev: Chapter 8</a>
</div>
<div align="right">
<a href="chapter-10.md">Next: Chapter 10</a>
</div>