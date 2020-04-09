### Spis treści
- [Chapter 10: Warstwa aplikacji](#chapter-10-warstwa-aplikacji)
  - [10.1  Protokoły warstwy aplikacji](#101-protoko%c5%82y-warstwy-aplikacji)
    - [10.1.1  Peer-to-peer](#1011-peer-to-peer)
    - [10.1.2  HTTP](#1012-http)
    - [10.1.3  Poczta elektroniczna](#1013-poczta-elektroniczna)
    - [10.1.4  DNS](#1014-dns)
    - [10.1.4 DHCP](#1014-dhcp)
    - [10.1.5  Usługi udostępniania plików](#1015-us%c5%82ugi-udost%c4%99pniania-plik%c3%b3w)

# Chapter 10: Warstwa aplikacji

## 10.1  Protokoły warstwy aplikacji

Warstwa aplikacji w modelu TCP/IP zawiera warstwy aplikacji, prezentacji i sesji znane z ISO/OSI.

`Warstwa aplikacji` - znajduje się najbliżej użytkownika końcowego, zapewnia interfejs między aplikacjiami, których używamy a siecią, przez którą transmitujemy dane. Protokoły zaliczane do warstwy aplikacji:
- `HTTP` - Hypertext Transfer Protocol
- `FTP` - File Transfer Protocol
- `TFTP` - Trivial File Transfer Protocol
- `IMAP` - Internet Message Access Protocol
- `DNS` -  Domain Name System

`Warstaw prezentacji` - odpowiada za formatowanie, prezentację danych, kompresję i szyfrowanie. Standardy zaliczane do tej warstwy:
- `MPEG` - Motion Picture Experts Group
- `GIF` - Graphics Interchange Format
- `JPEG` - Joint Photographic Experts Group
- `PNG` - Portable Network Graphics
- `Quick Time` - standard video i audio od Apple

`Warstwa sesji` - odpowiedzialna za tworzenie i utrzymanie sesji komunikacyjnych między aplikacją źródłową a docelową.

### 10.1.1  Peer-to-peer

`Sieć P2P` - peer-to-peer, decentralizacja zasobów w sieci, dane mogą być ulokowane na dowolnym urządzeniu w sieci. Przykładem może być współdzielenie plików i drukarek.

`Aplikacja P2P` - możliwość pracy jako klient i serwer w ramach jednej komunikacji. Przykłady aplikacji P2P: eMule, BitTorrent, Bitcoin, eDonkey i LionShare. Część aplikacji jest oparta o protokół `Gnutella`.

`Model klient - serwer` - klient żąda informacji a serwer odpowiada. Protokoły warstwy aplikacji opisują format żądań i odpowiedzi.

>Przesyłanie danych z klienta do serwera nazywane jest wysyłaniem (ang. `upload`), natomiast z serwera do klienta pobieraniem (ang. `download`).

### 10.1.2  HTTP

`URL` - Uniform Resource Locator, adres strony
`URI` - Uniform Resource Identifier

Najpopularniejsze typu komunikatów HTTP:
- `GET` - żądanie określonych zasobów, np. plik HTML
- `POST` - służy najczęściej do przesyłania zbiorów danych, np. dane wprowadzone w formularzu
- `PUT` - przesyłanie danych w postaci pliku np. obraz

`HTTPS` - HTTP Secure, ochrona przesyłanych danych polega na uwierzytelnieniu, i szyfrowaniu połączenia. Komunikacja jest szyfrowana za pomocą `SSL` (Secure Socket Layer).

### 10.1.3  Poczta elektroniczna

Poczta elektroniczna działa w oparciu o następujące protokoły:
- `SMTP` - Simple Mail Transfer Protocol, `port 25`,  niezawodne i skuteczne wysyłanie poczty. Jeśli docelowy serwer pocztowy nie jest tymczasowo dostępny do wiadomość jest umieszczona w buforze i okresowo wysyłana. Po upływie określonego czasu "ważności" nadawca otrzymuje wiadomość o braku możliwości dostarczenia poczty.
- `POP` - Post Office Protocol, `port 110`, umożliwia odbieranie/pobieranie poczty z serwera pocztowego. Po pobraniu poczta jest automatycznie usuwana z serwera pocztowego. Ze względu na usuwanie poczty zaraz po pobraniu nie mamy centralnego archiwum, w którym znajdują się nasza poczta
- `IMAP` - Internet Message Access Protocol, tutaj w odróżnieniu od POP pobierane są kopie wiadomości. Klient musi ręcznie kasować wiadomości. Dodatkowo użytkownik może tworzyć hierarchię plików na serwerze w celu organizacji poczty (np. foldery)  

`MDA` - Mail Delivery Agent, agent odpowiedzialny za zarządzanie i dostarczanie wiadomości email między serwerami i klientami

`MUA` - Mail User Agent - określenia programu służącego do odbioru i wysyłania poczty elektronicznej.

### 10.1.4  DNS

`DNS` - Domain Name System, służy do dopasowania nazwy domenowej do odpowiadającej jej adresowi IP. DNS do komunikacji używa pojedynczej struktury zwanej `komunikatem`, dla:
- wszystkich typów zapytań klienta i odpowiedzi serwera
- komunikatów o błędach
- przesyłania rekordów z informacjami o zasobach (`RR` - Resource Record) pomiędzy serwerami

Format komunkatu:
- `nagłówek`
- `pytanie` - zapytanie do serwera nazw
- `odpowiedź` - serwer zwraca rekordy zasobów odpowiadające na pytanie
- `autoryzacja`
- `dodatkowe informacje`

>Serwer DNS zapewnia rozpoznawanie nazw przy użyciu `BIND` (ang. Berkeley Internet Name Domain) lub inaczej demona nazw, który często określany jest mianem `named` (wym. name-dee

Typy rekordów serwera DNS:
- `A` - adres urządzenia końcowego
- `NS` - autorytatywny serwer nazw
- `CNAME` - pełna nazwa domenowa, `FQDN` (Fully Qualified Domain Name, zwany także `canonical name`)
- `MX` - rekord dotyczący poczty, przyporządkowuje nazwę domeny do serwera odbierającego pocztę

Początkow serwer DNS przegląda własne rekordy jeśli nie znajdzie odpowiedzi to wysyła zapytanie do innych serwerów. Odpowiedź trafia do pamięci podręcznej serwera i komputera pytającego o ten adres.

Wyświetlenie przechowywanych wpisów DNS w Windows:

```
C:\ ipconfig /displaydns
```

Struktura nazw jest skalowalna, ze względu na podział ma małe strefy. Strukturą przypomina odwrócone drzewo. Domeny najwyższego poziomu, `root DNS` zazwyczaj reprezentują typ organizacji lub kraj pochodzenia:
- `.com` - działalność komercyjna lub przemysł
- `.org` - organizacje non-profit
- `.pl` - Polska

>DNS opiera się na tej hierarchii zdecentralizowanych serwerów przechowując i utrzymując rekordy zasobów. Rekordy zasobów rejestrują nazwy domen, które serwer może odwzorować oraz alternatywne serwery, które również mogą przetwarzać żądania. Jeśli dany serwer posiada rekordy zasobów odpowiadające jego poziomowi w hierarchii, to mówi się, że jest on `autorytatywny` dla tych rekordów. Na przykład serwer nazw w domenie cisco.netacad.net nie byłby autorytatywny dla rekordu mail.cisco.com, ponieważ ten rekord jest utrzymywany na serwerze domeny wyższego poziomu (serwer nazw w domenie cisco.com).

`DNS resolver` - klient DNS, odpowiedzialny za rozpoczęcie procesu rozpoznawania nazw u klienta dla usług, które tego potrzebują. 

Narzędzie `nslookup` służy do manualnego wysyłania zapytań DNS w celu odwzorowania danej nazwy hosta, może być również użyte do rozwiązywania problemów, weryfikacją stanu serwerów DNS.

```
C:\ nslookup
```

```
C:\ nslookup www.google.com
```

### 10.1.4 DHCP

`DHCP` - Dynamic Host Configuration Protocol, odpowiada za przydzielenia automatycznej konfiguracji TCP/IP, hostom, które tego żądają. Adresy nadane przez DHCP nie są na stałe nadane, lecz są dzierżawione przez hosta. Wyłączanie hosta powoduje zwrot jego adresu do puli dostępnych adresów.

>DHCP może powodować zagrożenie bezpieczeństwa, ponieważ dowolne urządzenie połączone z siecią może otrzymać adres IP.

>W wielu sieciach stosuje się zarówno DHCP jak i adresowanie statyczne. DHCP jest używany dla hostów ogólnego przeznaczenia, takich jak urządzenia użytkownika końcowego; adresowanie statyczne jest używane do urządzeń sieciowych takich jak bramy domyślne, przełączniki, serwery i drukarki.

Działanie protokołu DHCP:
1. Klient rozgłasza(broadcast) pakiet `DHCPDISCOVER`, który ma na celu wykrycie dostępnych serwerów DHCP.
2. Serwer DHCP odpowiada komunikatem `DHCPOFFER`, który jest ofertą dzierżawy adresu, komunikat ten zawiera adres IP, maskę podsieci, adres bramy domyślnej, serwera DNS i czas trwania dzierżawy.
3. Z względu na to, że w sieci może pracować kilka serwerów DHCP, klient może dostać kilka takich ofert. W celu potwierdzenia wyboru, zaakceptowania oferty, klient wysyła do serwera żądanie DHCP `DHCPREQUEST`. W tym momencie serwer może wysłać swój poprzedni adres nadany przez serwer DHCP (a to nie powinno być wcześniej?).
4. Jeśli oferta jest nadal aktualna, a serwer otrzymał żądania DHCP od klienta to serwer wysyła `DHCPACK`, w przeciwnym wypadku przerywa proces komunikatem `DHCPNAK` (Negative Acknowlegement), po otrzymaniu takiego kominikatu klient musi rozpocząć pocedurę od początku, czyli wysłania rozgłoszenia `DHCPDISCOVER`.

>Gdy klient posiada `dzierżawę` adresu IP, musi być ona odnawiana przed wygaśnięciem terminu dzierżawy przez następny komunikat DHCPREQUEST.

### 10.1.5  Usługi udostępniania plików

`FTP` - File Transfer Protokol, protokół wykorzystywany do przesyłania danych między klientem a serwerem. W celu poprawnego przesyłania danych wymagane jest nawiązanie dwóch połączeń:
- `port 21`, połączenie do przesyłania poleceń, sterowania ruchem, odpowiedzi serwera
- `port 20 lub >1024 (tryb pasywny)`, kolejne połączenie służy do faktycznego przesyłania danych. Ustawiane za każdym razem gdy dane są przesyłane

>Przesyłanie danych może odbywać się w którymkolwiek kierunku. Klient może pobierać (ang. `download, pull`) dane z serwera lub przesyłać (ang. `upload, push`) dane na serwer.

`SMB` - Server Message Block, służy do nawiązywania długoterminowych połączeń z serwerem. SMB pozwala na udostępnianie katalogów, plików, drukarek i portów szeregowych. Dostęp do udostępnionych zasobów odbywa się na podobnej zasadzie co dostęp do lokalnych zasobów. Dzieki temu protokołowi możliwe jest współdzielenie zasobów między systemami Linux, Windows i MacOS  za pomocą oprogramowania `SAMBA`.

---

`M2M` - połączenie Machine-to-machine, określenia urządzeń wymieniających między sobą dane, bez pośrednictwa człowieka.

---

<div>
<a href="chapter-09.md">Prev: Chapter 9</a>
</div>
<div align="right">
<a href="chapter-11.md">Next: Chapter 11</a>
</div>