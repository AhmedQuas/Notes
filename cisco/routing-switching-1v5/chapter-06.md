### Spis treści
- [Chapter 6: Warstwa sieci](#chapter-6-warstwa-sieci)
  - [6.1 Protokoły warstwy sieciowej](#61-protoko%c5%82y-warstwy-sieciowej)
    - [6.1.1 IPv4](#611-ipv4)
    - [6.1.2 IPv6](#612-ipv6)
  - [6.2 Routing](#62-routing)
    - [6.2.1 Tablica routingu hosta IPv4](#621-tablica-routingu-hosta-ipv4)
    - [6.2.2 Tablica routingu hosta IPv6](#622-tablica-routingu-hosta-ipv6)
    - [6.2.3 Tablica routingu routera](#623-tablica-routingu-routera)
  - [6.3 Router](#63-router)
    - [6.3.1 Budowa routera](#631-budowa-routera)
  - [6.4 Postawowa konfiguracja routera](#64-postawowa-konfiguracja-routera)

# Chapter 6: Warstwa sieci

Warstwa sieciowa realizuje wymianę danych między urządzeniami końcowymi poprzez sieć.

Funkcje warstwy sieciowej:
- `Adresacja urządzeń końcowych` - nadawanie unikalnych adresów IP dla urządzeń, które w sieci zwane są hostami
- `Routing` - umożliwia kierowanie pakietów do hosta docelowego znajdującego się w innej sieci
- `Enkapsulacja i deenkapsulacja`

## 6.1 Protokoły warstwy sieciowej

Protokoły wykorzystywane obecnie:
- `IPv4`
- `IPv6`

Protokoły wykorzystywane w przeszłości: Novell IPX, AppleTalk, CLNS/DECNet.

Charakterystyka protokołów IP:
- `Connectionless`, bezpołączeniowy - przed wysłaniem danych nie jest ustanawiane żadne połączenie
- `Best Effort Service` - nic nie gwarantuje dostarczenia pakietów
- `Medium independent` - działanie protokołu nie jest zależne od wykorzystywanego medium

`Zawodność protokołu IP` wynika z tego, że nie posiada funkcji odpowiedzialnej za zarządzanie i odzyskiwanie niedostarczonych pakietów. Wynika to pierwotnych założeń, aby protokół IP nie dodawał zbyt dużego nadmiaru informacji nagłówkowych, dlatego zapewnia on niezbędne funkcje umożliwiające dostarczenie pakietu.

`MTU` - Maximum Transmission Unit, maksymalny rozmiar transportowanego pakietu, który medium jest w stanie przesłać. Uzgadnianie odbywa się na lini warstwy sieci i warstwy łącza danych.

Protokół IP dodaje nagłówek IP do segmentu, który otrzymuje od warstwy transportowej.

### 6.1.1 IPv4

Pakiet IPv4 składa się z:
- `header` - nagłówek
- `payload` - ładunek

Nagłówek składa się z następujących pól:

- `wersja` - zawiera 4 bitową wartość określającą wersję pakietu ip 
    
    - 0100 -> IPv4
    - 0110 -> IPv6

- `Internet Header Length` - IHL, przedstawia liczbę słów 32 bitowych użytych w nagłówku (5,15).
Wartość zależy od zastosowanych wartości w polach `options` i `padding`.

- `Differentiated Services` - DS, wcześniej ToS, służy do określenia priorytetu w ramach QoS

- `Total length` - określa wielkość całego pakietu w bajtach, pole to 16 bitowa liczba. Minimalna wartość to 20B(nagłówek + 0 danych), a maksymalna wartość to 65535 bajtów

- `Indentification` - 16 bitowe pole zapewniające jednoznaczną identyfikację fragmetów oryginalnego pakietu, pole `wykorzystywane do fragmentacji pakietów`, taka sytuacja występuje w momencie, gdy medium wymaga mniejszego MTU.

- `Flags` - 3 bitowe pole odotyczące podziału przesyłanego pakietu. Przykładowo pole to może zabronić routerom dzielenia pakietów na fragmenty, może też informować, że przesyłany pakiet jest częściom większego pakietu.

- `Fragment Offset` - 13 bitowe pole określa gdzie należy wstawić obecny pakiet podzas rekonstrukcji większego pakietu.

- `TTL` - Time to Live - wartość zmniejszana o jeden za każdym razem, gdy następuje przejście przez router. Wartość 0 oznacza odrzucenie pakietu i wysłanie do nadawcy komunikatu o błędzie za pomocą protokołu `ICMP`. Polecenie `traceroute` korzysta z tego pola w celu indetyfikacji routerów (skoków).
- `protokół` - umożliwia warstwie sieciowej przekazanie pakietu odpowiedniemu nadrzędnemu protokołowi

    - ICMP 0x01
    - TCP 0x06
    - UDP 0x11

- `Header Checksum` - 16 bitowe pole

- `adres źródłowy IP`
- `adres docelowy IP`
- `opcje`
- `padding` - wypełnienie

Problemy protokołu IPv4:
- wyczerpanie się puli adresów IP (4,294 mld adresów w puli, 3,7 mld dostępnych)
- zwiększanie wielkości tablic routingu - większe wykorzystanie pamięci i CPU
- ze względu na powszechne NAT mogą wystąpić problemy przy wykorzystaniu technologii end-to-end

`NAT` - Network Address Translation, pozwala na podłączenie wielu urządzeń do sieci za pomocą jednego publicznego adresu IP.

### 6.1.2 IPv6

Zalety IPv6:
- `340 sekstylionów adresów` - 128 bitowy adres, znaczne zwiększenie przestrzeni adresowej
- `udoskonalenie obsługi pakietów` - nagłówek IPv4 jest uproszczony, posiada mniejszą liczbę pól niż w IPv4, wypłynęło to pozytywnie na obsługę pakietów przez routery pośredniczące
- `eliminuje potrzebę wykorzystania NAT` - nie ma już więcej problemów z aplikacjami wymagającymi połączenia typu end-to-end
- `zintegrowane bezpieczeństwo` - w sposób natywny obsługuje funkcje uwierzytelniania i prywatności

Nagłówek IPv6:
- `wersja` - identycznie jak w IPv4
- `Traffic class` - odpowiednik pola DS z IPv4
- `Flow label` - znacznik strumienia, przydatne dla usług, aplikacji czasu rzeczywistego, utrzymania odpowiedniej kolejności tak aby host nie musiał ich porządkować
- `Payload length` - odpowiednik Total length z IPv4
- `Next header` - równoważne polu protokół
- `Hop limit` - odpowiednik TTL
- `Source Address` - 128 bitowy adres IPv6
- `Destination Address` 

## 6.2 Routing

Host może wysłać pakiet do:
- `samego siebie` - adres sprzężenia zwrotnego, loopback. Wyszystkie adresy z zakresu 127.0.0.1 /8 odnoszą się do interfejsu lokalnego hosta. Adres wykorzystywany do testowania poprawności działania stosu TCP/IP
-  `hosta lokalnego` - urządzenie znajdujące się w tej samej sieci co host źródłowy
-  `host zdalny` - hosty znajdują się w różnych sieciach (inne pule adresowe). Do hosta zdalnego pakiety wychodzą z naszej sieci przez bramę domyślną

>To czy konkretny pakiet ma zostać przesłany do hosta lokalnego (znajdującego się w tej samej sieci co host docelowy), czy hosta zdalnego (poza siecią hosta docelowego) określane jest przez system operacyjny nadawcy. Analizie podlega tutaj adres IP nadawcy, jego maska oraz adres IP hosta docelowego.

>`Brama domyślna` jest urządzeniem przesyłającym dane z sieci lokalnej do urządzeń znajdujących się w innych sieciach tzw. zdalnych sieciach. W przypadku sieci domowej lub małej firmy, brama domyślna często wykorzystywana jest do połączenia sieci lokalnej z Internetem.

Brama sieciowa to najczęściej router posiadający informacje o trasach do poszczególnych sieci. Informacje o trasach przechowyje w `tablicy routingu` (routing table), znajdującym się w pamięci RAM routera.

W celu określenia gdzie kierować pakiet, urządzenie nadawcze też posiada prostą tablicę routingu, zawierającą wpisy:
- `połączenie bezpośrednie` - trasa do interfejsu pętli zwrotnej
- `trasa do sieci lokalnej` - do której host jest bezpośrednio podłączony
- `lokalna trasa domyślna` - pojawia się automatycznie po ustawieniu adresu bramy domyślnej

### 6.2.1 Tablica routingu hosta IPv4

Wyświetlenie lokalnej tablicy routingu

```
C:\> netstat -r
C:\> route print
```

Powyższe polecenia wyświetlają informacje takie jak:
- `Lista interfejsów` - indywidualny numer przydzielony przez system operacyjny, MAC i nazwę interfejsu
- `Tabela tras IPv4`
- `Tabela tras IPv6`

Tabela routingu IPv4 zawiera:
- `Sieć docelową`
- `Maskę podsieci`
- `Gateway`
- `Interfejs`
- `Metrykę` - koszt trasy, lepsza droga niższa metryka

Charakterystyczne wpisy w tablicy routingu IPv4:
- `0.0.0.0` - lokalna trasa domyślna, jak żaden wpis nie pasuje to pakiet jest wysłany na ten adres, w gateway podany jest adres naszej bramy sieciowej
- `127.0.0.0 - 127.255.255.255` - adresy pętli zwrotnej, umożliwiają funcjonowanie usług na lokalnym hości dla niego samego
- `192.168.10.0 - 192.168.10.255` - adresy związane z siecią lokalną do której należy host
    
    `192.168.10.0` adres sieci lokalnej, reprezentujący wszystkie komputery należące do sieci 192.168.10.x
- `224.0.0.0` - adresy multicastowe należące do klasy D, zastrzeżone dla użytku przez loopback i adres IP hosta
- `255.255.255.255` - ograniczone adresy rozgłoszeniowe do użytku użytku przez loopback i adres IP hosta. Może być wykorzystane do odnalezienia serwera DHCP

`On-link` - wskazuje że nadawca i odbiorca znajdują się w tej samej sieci

### 6.2.2 Tablica routingu hosta IPv6

Tabela routingu IPv6 zawiera:
- `If` - indywidualny numer przydzielony przez system operacyjny
- `Metric`
- `Network Destination` - niższa wartość = preferowana trasa
- `Gateway`

Charakterystyczne wpisy w tablicy routingu IPv6:
- `::/0` - lokalna trasa domyślna
- `::1/128` - loopback
- `2001::/32` - globalny prefix dla transmisji jednostkowej / unicast
- `2001:0:9d38:953c:2c30:3071:e718:a926/128` - unkatowy globalny adres IPv6
- `fe80::/64` - określa trase `link-local`, reprezentuje wszystkie komputery w sieci LAN, odpowiednik adresu sieci w IPv4
- `fe80::2c30:3071:e718:a926/128` - adres `local-link` komputera
- `ff00::/8` - specjalnie zarezerwowane adresy grupowe

>Uwaga: Interfejsy skonfigurowane w protokole IPv6 `najczęściej posiadają dwa adresy IPv6`: adres lokalnego łącza (ang. `link local` address) oraz unikalny globalny adres sieciowy hosta (ang. `global unicast` address). Należy również zauważyć, że w protokole `IPv6 nie ma zdefiniowanych adresów rozgłoszeniowych` (ang. broadcast addresses).

### 6.2.3 Tablica routingu routera

Tablica routingu routera przechowuje:
- `directly-connected routes` - dodawane automatycznie po skonfigurowaniu interfejsu routera
- `remote routes` - trasy zdalne mogą być dodane statycznie, lub dodane dynamicznie za pomocą protkołów routingu

Tablica routingu routera może również zawierać adres trasy domyślnej -> 0.0.0.0

```
Router# show ip route
```

Wpis w tablicy routingu routera zawiera następujące informacje:
- `źródło trasy` - wskazuje w jaki sposób router dowiedział się o konkretnej trasie
  - `C` - określa bezpośrednio podłączoną sieć
  - `L` - określa, że jest to trasa do sieci lokalnej
  - `S` - wpis dodany ręcznie, statycznie przez administratora
  - `D` - trasa dodona dynamicznie za pomocą protokołu `EIGRP` (Enhanced Interior Gateway Routing Protocol)
  - `O` - trasa dodona dynamicznie za pomocą protokołu `OSPF` (Open Shortest Path First)
- `sieć docelowa` - adres sieci zdalnej
- `AD` - dystans administracyjny, określa poziom zaufania
- `metryka` - 
- `next hop` - via, określa adres IP następnego routera na drodze do sieci docelowej. Innymi słowy określa, które następne urządzenie zajmie się przetwarzaniem wysłanego pakietu
- `znacznik czasowy trasy` - wskazuje ile czasu minęło od ostatniego ogłoszenia danej trasy
- `interfejs wyjściowy` - wskazuje interfejs wyjściowy przez który musi zostać wysłany do wysłania parkietu

Przykładowy wpis:

```
D 10.1.2.0/24 [90/271012] via 209.167.220.226, )0:00:05 Serial0/0/0
```
 
 >Oznacza to, że z punktu widzenia routera podejmującego decyzję o kierunku przesłania pakietu, router następnego skoku jest bramą prowadzącą do zdalnych sieci.

 >Pakiety, nie mogą zostać przekazane przez router do kolejnego urządzenia bez znalezienia informacji o trasie do sieci docelowej w jego tablicy routingu. Jeżeli trasa reprezentująca sieć docelową, nie występuje w tablicy routingu, pakiet będzie odrzucony (czyli, nie zostanie przekazany dalej).

 `Gateway of Last Resort` - bramy ostatniej szansy, jeśli ustawione, to tutaj są wysyłane pakiety dla których router nie znalazł trasy.

 ## 6.3 Router

 Routery Cisco możemy podzieli na następujące kategorie:
 - `Oddziały` - routery przeznaczona dla małych i średnich firm
 - `WAN` - routery przeznaczone dla dużych firm
 - `Dostawcy usług`

### 6.3.1 Budowa routera

W routerze można wyróżnić następujące elementy:
- `IOS` - system operacyjny
- `CPU`
- `pamięć RAM` - DRAM i SRAM
  - uruchomiony IOS
  - running-config - konfiguracja bieżąca
  - tablice ARP i routingu
  - bufor pakietów 
- `pamięć ROM`
  - instrukcje bootowania systemu
  - ograniczona wersja IOS i proste oprogramowanie diagnostyczne (np POST)
- `Flash` - nieultona pamięć, to tu zazwyczaj zapisany jest IOS i inne pliki związane z systemem
- `NVRAM` - Nonvolatile Random-access memory, rodzaj nieulotnej pamięci, tutaj przechowywany jest plik konfiguracji startowej startup-config
- `moduł AMI` - Advanced Integration Module, odciąża procesor podczas wykonywania złożonych operacji takich jak szyfrowanie

Porty, złącza routera:
- `porty konsoli`
- `porty AUX`
- `porty interfejsu LAN`
- `gniazda interfejsów WAN EHWIC` - Enhanced high-speed WAN interface card
- `porty USB` - może też służyć do nawiązania połączenia konsolowego

Zatem te złącza można podzielić na dwie kategorie:
- porty zarządzania
- interfejsy przesyłania danych, inband interfaces

>W systemie Cisco IOS dwa interfejsy tego samego routera nie mogą należeć do tej samej podsieci.

System IOS zapewnia następujące działanie funkcji i komponentów:
- `Adresowanie`
- `Interfejsy`
- `Routing`
- `Bezpieczeństwo`
- `QoS`
- `Zarządzanie zasobami`

Podczas uruchamiania systemu router kopiuje następujące pliki do pamięci RAM:
- plik zawierający IOS, przechowywany w Flash, lub na serwerze TFTP
- plik konfiguracji startowej, przechowywany w NVRAM

Etapy rozruchu routera:
- test POST (Powe-on self test), oprogramowanie z ROM
- wyszukanie i załadowanie IOS
- wyszukanie i załadowanie pliku konfiguracji startowej

>Podczas rozpakowywania pliku obrazu systemu IOS, zostaje wyświetlony ciąg znaków (#).

Informacje o wersji IOS

```
Router# show version
```

> W pierwszej części wiersza wyświetlana jest informacja o typie procesora zainstalowanego w routerze. W dalszej części przedstawiona jest informacja o ilości zainstalowanej pamięci DRAM. Niektóre serie routerów, takie jak na przykład Cisco 1941 ISR, wydzielają z części pamięci DRAM tzw. pamięć dla pakietów (ang. packet memory). Jest ona przeznaczona do buforowania pakietów podczas realizowania procesu routingu. Zatem ostatecznie całkowita ilość zainstalowanej na routerze pamięci DRAM jest sumą obu wyświetlanych liczb.

>Ostatnia linia wyniku wywołania komendy show version wyświetla aktualną, skonfigurowaną wartość rejestru konfiguracji oprogramowania podaną w systemie szesnastkowym.

Fabryczna wartość to `0x2102`. Wartość `0x2142` wskazuje, że podczas uruchomienia został pominięty plik konfiguracyjny, może to być przydatne podczas odzyskiwania hasła, `password recovery`.


## 6.4 Postawowa konfiguracja routera

Ustawianie nazwy urządzenia, haseł, banneru, interfejsów ustawiamy tak samo jak na switchu  -> [rodział 2](chapter-02.md#22-podstawowa-konfiguracja).

>Pomimo tego, że opisywanie interfejsu nie jest formalnie wymagane, to do dobrej praktyki zalicza się opisywanie każdego interfejsu sieciowego.

Weryfikowanie konfiguracji:

```
Router# show ip interface brief
```

```
Router# show  ip interface
```
```
Router# show ip interfaces
```
```
Router# show ip route
```

---

<div>
<a href="chapter-05.md">Prev: Chapter 5</a>
</div>
<div align="right">
<a href="chapter-07.md">Next: Chapter 7</a>
</div>