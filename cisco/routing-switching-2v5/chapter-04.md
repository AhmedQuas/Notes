### Spis treści
- [Chapter 4: Koncepcje routingu](#chapter-4-koncepcje-routingu)
  - [4.1 Wstępna konfiguracja routingu](#41-wst%c4%99pna-konfiguracja-routingu)
  - [4.2 Decyzje routingu](#42-decyzje-routingu)
  - [4.3 Działanie routera](#43-dzia%c5%82anie-routera)

# Chapter 4: Koncepcje routingu

## 4.1 Wstępna konfiguracja routingu

Charakterystyki opisujące wydajność i strukturę sieci:
- topologia fizyczna i logiczna
- szybkość - miara przepustowości w b/s
- koszt
- bezpieczeństwo
- dostępność
- skalowalność
- niezawodność, wskaźnik MTBF

Router jest urządzeniem służącym do łączenia wielu sieci. Router wykorzystuje swoją tablicę routingu do określenia sposobu dotarcia pakietu do sieci docelowej. Urządzenie posiada wiele interfejsów, z których każdy należy do innej sieci IP.

Router składa się z:
- `CPU`
- systemu operacyjnego `IOS` -  Internetwork Operating System
- pamięci wewnętrzynch i pamięci masowej:
  - `RAM` - tabela routingu, tabela ethernetowa ARP, bufor pakietów, running config, uruchomiony OS
  - `ROM` - POST, uproszczony firmware
  - `NVRAM` - startup config
  - `Flash` - IOS i inne pliki systemowe
- wyspecjalizowane porty i karty interfejsów sieciowych

>WAN, sieci rozległe łączą sieci na przestrzeni dużych obszarów geograficznych. Na przykład, łącze WAN jest często wykorzystywane do łączenia sieci LAN z siecią dostawcy usług internetowych (ISP).

Funkcje routera:
- określanie najlepszej ścieżki dla przesyłania pakietów
- przekierowanie pakietów w stronę miejsca docelowego

Ze względu na rodzaj przyłączonego medium i konfiguracji interfejsów router wykorzystywać różne technologie łącza danych tj: Ethernet, PPP, Frame Relay, DSL lub łącze bezprzewodowe 802.11

>Uwaga: Routery wykorzystują do budowania swoich tablic routingu oraz pozyskiwania wiedzy o zdalnych sieciach ścieżki statyczne oraz protokoły routingu dynamicznego.

Mechanizmy przekazywania pakietów:
- `przełączanie procesowe` - proces bardzo powolny, rzadko stoswany w sieciach. Polega na dopasowaniu przez CPU każdego pakietu na podstawie tablicy routingu do adresu docelowego. Router wykonuje takią operację na każdym pakiecie nawet jeśli jest to strumień pakietów.
- `przełączanie szybkie` - znacznie powszechniejszy mechanizm, wykorzystuje szybką pamięć podręczną zawierającą ostatnie adresy skoku, jeśli nie znajdzie dopasowania to pakiet jest przełączany procesowo.
- `CEF` - Cisco Express Forwarding, najnowszy, najszybszy i zalecany mechanizm przekazywania pakietów. Budowana jest tzw Baza Informacji Przekierowań, `FIB`(Forwarding Information Base). W tej bazie wpisy są dodawane w momencie osiągania zbieżności sieci i w momentach zmian w topologii, a nie jak we wcześniejszych rozwiązaniach, gdzie uaktualnienie następuje w momencie nadejścia pakietu. Taki sposób organizacji przekazywania pakietów pozwala na minimalne wykorzystanie procesora do tych zadań.

`WAP` - Wireless Access Point

Dokumentacja sieci powinna zawierać:
- nazwy urządzeń
- interfejsy
- adresy IP i maski podsieci
- adresy bram domyślnych

Dokumentacja może mieć formę:
- tabeli adresacji
- diagramu topologii - fizyczne połączenia i adresacja warstwy 3 np w MS Viso

Diody na urządzeniach Cisco:
- `zielony` - wszysto działa jak powinno
- `bursztynowy` - tryb awaryjny

Diody na interfejsie routera:
- `S` - Speed, szybkość 
  - 1 mrugnięcie, pauza = 10 Mb/s 
  - 2 mrugnięcie, pauza = 100 Mb/s
  - 3 mrugnięcie, pauza = 1000 Mb/s
- `L` - Link, łącze
  - zielony - łącze aktywne  

Podstawowa konfiguracja routera:
- nazwanie urządzenia
- zabezpieczenie dostępu (console i vty)
- konfigurowanie banneru
- konfiguracja adresacji ipv4 i ipv6
- `włączenie skonfigurowanych interfejsów`
- opcjonalne dodanie opisu interfejsów

>Kiedy router zostanie skonfigurowany za pomocą komendy `ipv6 unicast-routing` w trybie globalnym, zaczyna rozsyłać z interfejsu wiadomości ICMPv6 Router Advertisement. Pozwala to komputerowi podłączonemu do tego interfejsu na automatyczne skonfigurowanie swojego adresu IPv6 i ustawienie bramy domyślnej bez potrzeby odwoływania się do serwera DHCPv6.

>`Interfejs pętli zwrotnej` jest logicznym interfejsem zdefiniowanym lokalnie dla routera. Nie jest on przypisany żadnemu portowi fizycznemu, stąd nie można do niego podłączyć żadnego urządzenia. Jest interfejsem programistycznym, automatycznie włączanym (UP) na cały czas pracy routera.
Interfejs pętli zwrotnej jest użyteczny do testowania i zarządzania urządzeniem Cisco IOS, ponieważ gwarantuje, że co najmniej jeden interfejs będzie zawsze dostępny. 

Sprawdzanie poprawności konfiguracji

Wyświetlenie podsumowania dla wszystkich interfejsów
```
R1 # show interface brief
```

Wyświetlenie zawartości tablicy routingu
```
R1 # show ip route
```

Wyświetlenie komend skonfigurowanych na danym interfejsie
```
R1 # show running-config interface g0/0
```

Filtrowanie komendy show:
- `section` - wyświetla całą sekcję zaczynającą się od wyrażanie filtrującego
- `include` - tylko te linijki, które zawierają wyrażenie filtrujące
- `exclude` - pomijaj te linijki, które spełniają wyrażenie filtrujące
- `begin` - wyświetl wszystko od linijki, która spełniła wyrażenie filtrujące

```
R1 # show running-config | section line vty
```

Polecenia przydatne do zarządzania historią poleceń

```
R1 # show history size 120
R1 # show history
```

## 4.2 Decyzje routingu

>Główną funkcją routera jest przekazywanie pakietów w stronę węzła docelowego. Jest to możliwe dzięki funkcji przełączania, która jest procesem wykorzystywanym przez router do `przyjmowania pakietu na jednym interfejsie i przekazywania go na inny interfejs.` Najważniejsza w funkcji przełączania jest enkapsulacja pakietów w typ ramki warstwy łącza danych odpowiedni dla łącza wyjściowego.

>Ramka enkapsulowana w standardzie Ethernet może zostać odebrana przez router na interfejsie ethernetowym, następnie przetworzona i przekazana na wyjściowy interfejs szeregowy już jako pakiet enkapsulowany w ramkę protokołu Point-to-Point Protocol (PPP).

Określenie czy adres docelowy znajduje się w sieci, w której jest host.

>Określamy swoją własną podsieć wykonując `operację logicznego AND` na własnym adresie IPv4 i z użyciem własnej maski podsieci. W wyniku tej operacji uzyskujemy adres sieci, do której należymy. Następnie, wykonujemy taką samą operację logicznego AND z użyciem docelowego adresu IPv4 zapisanego w pakiecie oraz maski podsieci.

Jeśli odbiorca znajduje się w naszej sieci zostaje przeszukać własną tablicę adresów MAC, tablicę ARP i w razie braku adresu wysłać zapytanie ARP.

Główne rodzaje wpisów znajdujących się w tablicy routingu:
- sieć bezpośrednio przyłączona
- sieć zdalna
- brak określonej trasy - trasa domyślna, `Gateway of Last Resort`

>Odrzuceniu pakietu przez router powoduje, że na źródłowy adres IP pakietu wysyłany jest komunikat `ICMP` (Internet Control Message Protocol) o nieosiągalności.

>Najlepsza trasa jest wybierana przez protokół routingu na podstawie wartości, czyli metryki używanej do ustalenia odległości do sieci. Metryka to ilościowa wartość wskazująca odległość do danej sieci. Najlepszą trasą do danej sieci jest trasa z najniższą metryką.

Jeśli do tej samej sieci docelowej prowadzi kilka dróg to router będzie równomiernie rozkładał obciążenie.

`AD` - dystans administracyjny, jest miarą wiarygodności trasy im niższa tym bardziej wiarygodna

| Źródło trasy  | Dystans administracyjny  |
|---            |---        
|Bezpośrednio dołączona     |0      
|Trasa statyczna            |1      
|Zsumaryzowana trasa EIGRP  |5      
|Zewnętrzna trasa BGP       |20     
|Wewnętrzna trasa BGP       |90    
|IGRP                       |100     
|OSPF                       |110     
|IS-IS                      |115     
|RIP                        |120     
|Zewnętrzna EIGRP           |170     
|Wewnętrzna BGP             |200 

## 4.3 Działanie routera

Interfejs usyskuje stan [up/up] gdy:
- posiada przypisany adres IPv4 lub IPv6
- został włączony - `no shutdown`
- otrzymał sygnał elektryczny od innego urządzenia np. innego routera, przełącznika lub komputera

Źródła tras routingu:
- `interfejsy tras lokalnych`
- `interfejsy tras bezpośrednio podłączonych`
- `trasy statyczne`
- `dynamiczne protokoły routingu`

>Aktywny, poprawnie skonfigurowany i bezpośrednio podłączony interfejs w rzeczywistości tworzy dwa wpisy w tablicy routingu, `L i C`.

L vs C w show ip route

W `L` wskazuje na konkretny interfejs routera, pojdeynczy adres IP, przypisany do sieci bezpośrednio połączonej.

Typy tras statycznych:
- trasa statyczna do określonej sieci
- domyślna trasa statyczna

>Aby określić, które protokoły routingu są obsługiwane przez IOS, należy użyć komendy router ? w trybie konfiguracji globalnej, jak pokazano na rysunku.

>Aby uruchomić routery IPv6 w trybie przekazywania ruchu, należy wydać komendę `ipv6 unicast-routing` wywołaną w trybie konfiguracji globalnej.

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>