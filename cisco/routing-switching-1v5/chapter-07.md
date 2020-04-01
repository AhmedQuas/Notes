### Spis treści
- [Chapter 7: Warstwa transportowa](#chapter-7-warstwa-transportowa)
  - [7.1 Protokoły wartswy transportowej](#71-protoko%c5%82y-wartswy-transportowej)
  - [7.2 Numery portów](#72-numery-port%c3%b3w)
  - [7.3 Uzgadnianie trójetapowe i podwójne uzgadnianie](#73-uzgadnianie-tr%c3%b3jetapowe-i-podw%c3%b3jne-uzgadnianie)
  - [7.4 Niezawodność i kontrola przepływu](#74-niezawodno%c5%9b%c4%87-i-kontrola-przep%c5%82ywu)
  - [7.5 Komunikacja z użyciem UDP](#75-komunikacja-z-u%c5%bcyciem-udp)

# Chapter 7: Warstwa transportowa

W warstwie transportowej dane dzielone są na segmenty i sposób dostarczenia (czy musimy upewnić się że wszystkie segmenty muszą być dostarczone).
Funkcje warstwy transportowej:
- ustanawianie sesji, śledzenie komunikacji, konwersacji między aplikacjami na hoście źródłowym i docelowym
- segmentowanie danych, w celu łatwiejszego zarządzania i powtórne składanie segmentów na urządzeniu końcowym. Segmentowanie jest spowodowane ograniczoną ilością danych, w pakiecie
- identyfikowanie właściwej aplikacji dla poszczególnych strumieni danych (np poprzez numer portu)
- zarządzanie wymaganiami co do niezawodności przepływu informacji

Podział danych na segmenty, pozwala multiplikację (przeplatanie) danych różnych aplikacji w ramach jednego medium. Często również nazywa się to `multipleksowaniem transmisji`.

## 7.1 Protokoły wartswy transportowej

- `TCP` - Transmission Control Protocol, `RFC 793`, zawiera mechanizmy niezawodnego dostarczania danych:
  - śledzenie transmitowanych segmentów danych
  - potwierdzenie odbioru danych w określonej jednostce czasu
  - retransmisję niepotwierdzonych danych
  - kontrola przepływu - możliwość zmniejszenia hasła gdy odbiorca nie wyrabia z odbiorem wiadomości
  
  TCP dzieli wiadomość na segmenty. Potwierdzenie odbioru segmentu wprowadza dodatkowy narzut na zasoby w sieci, może powodować opóźnienia w sieci.

  Komunikacja zorientowana połączeniowo (`stateful`), zestawiane jest połączenie, sesji.

- `UDP` - User Datagram Protocol, `RFC 768`, zapewnia podstawowe funkcje dostarczania danych pomiędzy aplikacjami, oferując przy tym minimalny narzut danych kontrolnych. Stosowany dla aplikacji, które nie wymagają gwaracji dostarczenie, a opóźnienia w transmisji wywołane narzutem spowodowanym przez TCP działało by na niekorzyść, lub nawet mogło być szkodliwe dla jakości wiadomości, np. transmisja głosu (VoIP), streaming wideo, radio internetowe.

    UDP jest określany jako protokół `best-effort`, czyli przy użyciu wszystkich dostępnych środków, innymi słowy dostarczenie z dołożeniem wszelkich starań.

Cechy charakterystyczne protokołu TCP:
- niezawodność
- potwierdzanie odbioru danych
- retransmisjia utraconych pakietów
- dostarczenie danych i złożenie w odpowiedniej kolejności
- 20 bajtów narzutu na segment

Cechy charakterystyczne protokołu UDP:
- szybkość
- mały narzut
- brak gwarancji dostarczenia
- brak retransmisji utraconych danych
- dane dostarcze w takiej kolejności w jakiej przyjdą do hosta docelowego
- 8 bajtów narzutu na datagram


Budowa segmentu TCP:
- `port źródłowy` 16b
- `port docelowy` 16b
- `numer sekwencyjny` 32b - używany podczas składania danych u hosta docelowego
- `numer potwierdzenia` 32b - potwierdzenie że dany dany segment dotarł do celu
- `długość nagłówka` 4b - informuje o długości nagłówka segmentu, nazywane także `data offset`
- `pole zarezerwowane` 6b - dodatkowe rozwiązania
- `bity kontrolne` 6b - wskazuje funkcje segmentu, flagi:
  - URG - ważność pola
  - `ACK` - acknowledgement, potwierdzenie otrzymania danych
  - PSH - funkcja PUSH
  - RST - resetowanie połączenia
  - `SYN` -  Synchronize Sequence Number
  - FIN - żądanie zakończenia transmisji

- `rozmiar okna` 16b - informuje o ilości segmentów, które mogą być zaakceptowane w określonym czasie, informuje o tym co wcześniej uzgodniliśmy
- `suma kontrolna` 16b - sprawdzenie poprawności przesłania danych
- `wskaźnik pilności` 16b

Budowa datagramu UDP:
- `port źródłowy` 16b
- `port docelowy` 16b
- `długość` 16b
- `suma kontrolna` 16b

`Różnice w segmentacji TCP i UDP` polegają na tym, że UDP nie nawiązuje wcześniej połączenia i nie rozróżnia kolejności napływania datagramów do hosta docelowego.

## 7.2 Numery portów

Numery portów pozwalają na rozróżnianie segmentów i datagramów pomiędzy aplikacjami.

>Numer portu źródłowego jest generowany losowo, przez urządzenie wysyłające dane w celu identyfikacji konwersacji pomiędzy dwoma urządzeniami. Umożliwia to prowadzenie wielu konwersacji równolegle. Innymi słowy, urządzenie może wysyłać wiele zapytań HTTP (np. wiele zakładek jednocześnie) do serwera jednocześnie. Osobne konwersacje są śledzone w oparciu o porty źródłowe.

>Kombinacja numeru portu warstwy transportu i adresu IP warstwy sieci, jednoznacznie identyfikuje poszczególny proces aplikacji, zwana jest `gniazdem`.

Para gniazd jednoznacznie identyfikuje konkretną konwersację między hostami.

>Numery portów przydziela organizacja IANA (ang. Internet Assigned Numbers Authority).

Typy numerów portów:
- `dobrze znane porty` (0 - 1023), usługi takie jak HTTP, IMAP, SMTP
- `porty zarejestrowane` (1024 - 49151), porty używane przez aplikacje i usługi zainstalowane przez użytkownika np. MS SQL, Elasticsearch, RADIUS. Porty nie wykorzystywane mogą być dynamicznie wybierane przez klienta jako port źródłowy
- `porty dynamiczne / prywantne` (49152 - 65535),  `ephemeral ports`, numery portów dynamicznie wybierane przez aplikacje klienckie podczas nawiązywania połączenia

Do uzyskania informacji o aktywnych połączeniach TCP wystartczy użyć polecenia `netstat`.

Netstat zwraca informacje takie jak:
- `użyty protokół w warstwy sieciowej`
- `port źródłowy`
- `adres lub nazwa zdalnego hosta`
- `port docelowy`
- `stan połączenia`

Protokoły warstwy sieciowej i skojarzone usługi warstw wyższych. 

`TCP`:
 - HTTP
 - Telnet
 - FTP
 - SMTP

`UDP`:
 - TFTP
 - DHCP
 - VoIP
 - Telewizja IP, IPTV
 - RIP
 - gry online

`TCP i UDP`:
 - DNS
 - SNMP

## 7.3 Uzgadnianie trójetapowe i podwójne uzgadnianie

`3 way handshake` - uzgadnianie trójetapowe, proces nawiązanie i uzgodnienia parametrów połączenia. Bity kontrolne w nagłówku TCP wskazują stan procesu. W tym procesie wykorzystujemy flagi `ACK` i `SYN`.

Cele uzgadniania trójetapowego:
- ustalenie czy urządzenie odbiorcze jest obecne w sieci
- sprawdzenie czy host docelowy ma aktywną usługę i akceptuje połączenia na danym porcie
- poinformowanie urządzenia docelowego, że ktoś zamierza ustanowić z nim sesję

Przebieg procesu uzgadniania trójetapowego:
- klient wysyła segment z flagą `SYN`, informuje że w segment zawiera w nagłówku początkową, wygenerowaną losowo wartość numeru sekwencyjnego, `ISN` -  Initial Sequence Number

    >Wartość ISN w nagłówku każdego kolejnego segmentu jest zwiększana o 1 dla każdego wysłanego bajtu, podczas trwania konwersacji między klientem i serwerem.

- serwer potwierdza otrzymanie segmentu ustawiając flagę `ACK` i ISN + 1 i ACK number. Jednak wymiana danych to tak naprawę dwie jednokierunkowe sesje, więc serwer również musi nawiązać sesje z klientem, w tym celu ustawia również flagę `SYN`.

- klient odpowiada na segment synchronizujący pochodzący od serwera flagą `ACK`

Czyli w skrócie uzgadnianie trójetapowe wygląda tak:
- `SYN`
- `SYN ACK`
- `ACK`


>Od momentu ustanowienia obu sesji pomiędzy klientem i serwerem, wszystkie pozostałe segmenty wymieniane w tej komunikacji będą miały ustawioną flagę ACK.

Bezpieczeństwo komonikacji w sieci możemy zapewnić poprzez:
- odmowę ustanawiania sesji TCP
- zezwolenie na ustanawianie sesji dla wybranych usług
- zezwolenie na ruch w ramach ustanowionych już sesji

Kończenie jednokierunkowej sesji TCP podwójnego uzgadniania, wysłania 2 komunikatów (`FIN` i `ACK`), dlatego, w celu zakończenia pojedynczej konwersacji musimy wymienić 4 komunikaty:
- `FIN` - klient do serwera
- `ACK` - serwer do klienta
- `FIN` - serwer do klienta
- `ACK` - klient do serwera

Proces ten może być również zainicjowany przez serwer.

>Możliwe jest również zakończenie sesji za pomocą uzgodniania trójetapowego. Kiedy klient nie ma więcej danych do przesłania, wysyła do serwera segment z ustawioną flagą FIN. Jeśli serwer również nie ma więcej danych do przesłania, odpowiada segmentem z ustawionymi flagami FIN oraz ACK, łącząc dwa etapy w jeden. W takim wypadku klient odpowiada ustawioną flagą ACK.

## 7.4 Niezawodność i kontrola przepływu

- Przestawianie segmentów, uporządkowane dostarczenie

  >Odbierający proces TCP, umieszcza dane z segmentu w buforze odbioru. Segmenty są składane we właściwej kolejności, wynikającej z numeru sekwencyjnego, a następnie przekazane aplikacji. Jeśli odebrane zostaną segmenty, których numery sekwencyjne nie zachowują ciągłości, to zostaną one zatrzymywane (w buforze) w celu późniejszego przetworzenia. Po otrzymaniu brakujących segmentów, dane zostają niezwłocznie ułożone we właściwej kolejności.

  >Usługa TCP na hoście docelowym zwykle potwierdza tylko otrzymanie ciągłej partii danych.

- Potwiedzanie otrzymania segmentów

  >Numer SEQ wskazuje na względną liczbę bajtów (łącznie z bieżącym segmentem), przesłanych podczas trwającej sesji.

  > TCP używa numeru potwierdzenia ACK, celem wskazania następnego bajtu w tej sesji, który spodziewa się otrzymać adresat. Nazywane jest to przewidywanym potwierdzeniem (`expectational acknowledgment`).

  >Na przykład, zaczynając z numerem sekwencyjnym równym 2000, jeśli host docelowy otrzyma 10 segmentów, każdy zawierający po 1000 bajtów danych, w potwierdzeniu wysłanym do nadawcy tej partii danych numer potwierdzenia wyniesie 12001.

- Zarządzanie utraconymi pakietami

  >Usługa TCP na hoście docelowym zwykle potwierdza tylko otrzymanie ciągłej partii danych.

  >Kiedy TCP na hoście źródłowym nie otrzyma potwierdzenia odebrania przez host docelowy, we wcześniej określonym czasie, wraca do ostatniego otrzymanego numeru ACK i rozpoczyna retransmisję od tego momentu.

  `Selective Acknowledgements` -  polega ona na tym, że jeśli oba hosty komunikujące się ze sobą wspierają selektywne potwierdzenia, możliwym się staje dla odbiorcy potwierdzanie bajtów w nieciągłych segmentach i po otrzymaniu takiego potwierdzenia, nadawca będzie potrzebował retransmitować tylko brakujące dane.

- `Flow control`, kontrola przepływu, dostosowanie tempa przepływu między 2 hostami w sesji. Jest to też ustalanie limitu ilości segmentów wysyłanych w ciągu, po którym oczekujemy potwierdzenia.

  >Nagłówek protokołu TCP zawiera 16-bitowe pole o nazwie „rozmiar okna” (ang. `window size`). Zawiera ono liczbę określającą ilość danych, które podczas sesji TCP będą akceptowane przez odbiorcę w pojedynczym ciągu (po którym wymagane będzie potwierdzenie dostarczenia). Początkowy rozmiar okna jest ustalany podczas nawiązywania sesji w uzgadnianiu trójetapowym, pomiędzy źródłem, a celem. Po uzgodnieniu, host źródłowy musi ograniczyć ilość segmentów, wysyłanych do odbiorcy, bazując na ustanowionym rozmiarze okna. I tylko wtedy, kiedy po wysłaniu uzgodnionej rozmiarem okna ilości segmentów, źródło otrzyma potwierdzenie dostarczenia, może wysłać kolejną porcję danych.

  >Protokół TCP używa `rozmiaru okna, aby zarządzać przepustowością`, tak aby osiągnęła ona możliwą wielkość maksymalną, którą urządzenie docelowe będzie mogło przetworzyć, bez strat i konieczności retransmisji.

- zmniejszanie rozmiaru okna w momencie, gdy sieć jest przeciążona
    >W komunikacji, opartej o TCP, to host docelowy ustala rozmiar okna wskazujący ilość bajtów, które jest gotów odebrać w jednej partii danej sesji. Jeśli host odbierający potrzebuje zmniejszyć ilość przesyłanych danych z powodu ograniczenia buforu pamięci, może wysłać zmniejszony rozmiar okna do nadawcy w segmencie potwierdzającym (z ustawionym bitem ACK).

    >Jeżeli w czasie dalszej transmisji nie dojdzie do strat w przesyłanych segmentach, odbiorca rozpocznie powiększanie wielkości okna, a co za tym idzie zmniejszy się narzut sieci, ponieważ potwierdzenia będą przesyłane rzadziej. Rozmiar okna będzie rósł, aż do wartości, przy której nastąpi utrata danych, co spowoduje ponowne zmniejszenie wartości rozmiaru okna.


## 7.5 Komunikacja z użyciem UDP

Protokół UDP nie jest wyposażony w skomplikowane mechanizmy retransmisji, dlatego jeśli jest to wymagane to trzeba to zapewnić w warstwach wyższych. Przykładowo DNS po prostu ponawia żądania. W UDP dane są scalane w kolejności ich otrzymania (pakiety mogą dotrzeć różnymi ścieżkami).
TFTP zawiera własny mechanizm kontroli przepływu, detekcji błędów, potwierdzania i usuwania błędów.


>Wiele aplikacji używających protokół UDP wysyła małe ilości danych, które mogą pomieścić się w jednym segmencie.

>Jednostka danych protokołu UDP jest określana jako datagram, należy jednak zauważyć, że oba terminy - "segment" i "datagram" `są często używane zamiennie`, jako ogólne określenie jednostki danych protokołu warstwy transportowej.

>Ponieważ nie ma sesji, które byłyby inicjowane przez UDP, to od razu kiedy aplikacja przygotuje dane i zostanie im przyporządkowany numer portu źródłowego, tworzone są datagramy, przekazywane do warstwy sieciowej w celu adresacji i wysłania.

Typy aplikacji, dla których lepiej wykorzystać UDP:
- brak tolerancji na opóźnienia, tolerancja na małe straty danych
- prosta wymiania typ żądanie - odpowiedź
- wykorzystujące jednokierunkowy typ transmisji bez konieczności potwierdzania dostarczenia

---

<div>
<a href="chapter-06.md">Prev: Chapter 6</a>
</div>
<div align="right">
<a href="chapter-08.md">Next: Chapter 8</a>
</div>
