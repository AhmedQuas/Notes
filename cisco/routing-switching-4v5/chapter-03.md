### Spis treści
- [Chapter 3: Połączenia punkt - punkt](#chapter-3-połączenia-punkt---punkt)
  - [3.1 Łącza szeregowe typu punkt - punkt](#31-łącza-szeregowe-typu-punkt---punkt)
  - [3.2 Enkapsulacja HDLC](#32-enkapsulacja-hdlc)
  - [3.3 Działanie PPP](#33-działanie-ppp)
  - [3.4 Konfigurowanie PPP](#34-konfigurowanie-ppp)
  - [3.5 Rozwiązywanie problemów z PPP](#35-rozwiązywanie-problemów-z-ppp)

# Chapter 3: Połączenia punkt - punkt

## 3.1 Łącza szeregowe typu punkt - punkt

Najpopularniejszym rodzajem połączenia w komunikacji na duże odległości jest połączenie punkt-punkt, zwane także połączeniem szeregowym lub dzierżawionym. Tego typu połączenia są zazwyczaj obsługiwane przez dostawców np operatora telefonicznego.

Standardy komunikacji szeregowej dotyczące połączeń LAN-WAN:
- `RS-232` - interfejs ogólnego przeznaczenia występujący w 9 i 25 stykowym standardzie
- `V.35` - zazwyczaj używane do komunikacji modem - multiplekser, wykorzystywany również do wymiany danych w łączach(duże prędkości) synchronicznych na kilku obwodach telefonicznych(T1)
- `HSSI` - High-Speed Serial Interface, interfejs szeregowy o przepustowości do 52 Mb/s, wykorzystywany do łączenia routerów w sieciach LAN z routerami w sieciach WAN poprzez linie dużych prędkości(T3). HSSI i Token Ring

>Połączenia typu punkt-punkt nie mają ograniczeń związanych z odległością. Mogą to być setki tysięcy mil światłowodów podmorskich, które łączą kraje i kontynenty na całym świecie(undersea Internet cable map).

Zwielokrotnienie - możliwość przesyłania wielu sygnałów logicznych w jednym kanale fizycznym

`TDM` - Time Division Multiplexing, technologia polegająca na dzielenie pasma łącza na oddzielne szczeliny czasowe dla transmisji na kilku kanałach. TDM działa w warstwie fizycznej.

Następuje podział czasu transmisji na mniejsze, równe odstępy (szczeliny), zapewniające przesyłanie bitów z wielu źródeł.

`MUX` - Multiplekser

byte interleaving - przeplatanie bajtów

`STDM` - pozwala zmieniać długość szczeliny czasowej, dzięki czemu nie ma pustych przebiegów

>Dla każdej transmisji, STDM musi zapewnić przesyłanie informacji identyfikacyjnych lub identyfikatora kanału.

Przykładu TDM -> SONET/SDH

`DTE` - zakończenie łącza WAN po stronie klienta jest nim najczęściej router, jednak nic nie stoi na przeszkodzie, żeby nim było urządzenie końcowe: drukarka, komputer itp.

`DCE` - zakończenie łącza WAN po stronie operatora. Odpowiada za dostaczenie sygnału taktowania

Kable szeregowe:
- `DTE -> DTE` - null modem, 2xRS 232(DB9) i cross Tx z Rx (2(Rx)->3(Tx),3->2, 5 masa)
- `DTE -> DCE` - ekranowany szeregowy kabel transmisyjny zakończony DB-60 lub Smart Serial

## 3.2 Enkapsulacja HDLC

Protokoły WAN:
- `HDLC` - domyślny typ enkapsulacji w połączeniach punkt - punkt. Podstawa dla synchronicznego PPP,
- `PPP` - zapewnia połączenia router - router, działa z IPv4 i IPv6. Korzysta z enkapsulacji HDLC, posiada także wbudowane mechanizmy bezpieczeństwa(PAP i CHAP),
- `SLIP` - Serial Line Internet Protocol, szeregowe połączenia punkt - punkt wykorzystujący TCP/IP. Obecnie został zastąpiony przez PPP,
- `X.25/LAPB` - Link Access Procedure Balanced, protokół warstwy łącza danych. X.25 to poprzednik Frame Relay. Stosowany w publicznych sieciach danych(PDN),
- `Frame Relay` - protokół warstwy łącza danych, obsługuje wiele obwodów wirtualnych. FR eliminuje niektóre z czasochłonnych procesów z X.25 tj. korekcja błędów i kontrola przepływu,
- `ATM` - przykazywanie danych w komórkach o długości 53B, stosowana w E3, T3 i SONET

`HDLC` - High-Level Data Link Control, protokół warstwy łącza danych opracowny przez ISO(obecnie ISO 13239), powstał na bazie SDLC. Zapewnia usługi połączeniowe jak i te bezpołączeniowe. Oparty jest na synchronicznej transmisji szeregowej i zapewnia bezbłędną komunikcję między dwoma punktami. Struktura ramki(2.OSI) umożliwia kontrolę przepływu i kontrolę błędów przy użyciu potwierdzeń

Rodzaje ramek HDLC:
- standardowa
- Cisco - cHDLC, posiada dodatkowe pole protokół(umożliwia obsługę wielu protokołów z warstwy sieciowej)

Ramka HDLC:
- `flaga` - rozpoczyna i kończy ramkę, ma postać 011111110, aby ten sam wzorzec nie pojawił się w polu danych to system nadaje '0' po każdych 5 '1' w polu danych(`bit stuffing`). W momencie odbioru '0' są usuwane. W przypadku wysyłki seryjnej flaga kończąca jest jednocześnie flagą początkową dla kolejnej ramki,
- `adres` - adres stacji odbiorczej lub podrzędnej może mieć formę pojednynczego adresu, grupowego lub rozgłoszeniowego,
- `pole sterujące` - skład w zależności od typu ramki:
  - `I` - informacyjna, przenosi informacje z warstwy nadrzędnej i informacje sterujące
  - `S` - zarządzająca, dostarcza informacji sterujących, może zażądać i zawiesić transmisję, raportować o statusie, potwierdzić otrzymanie ramek `I`. Ramki `S` nie posiadają pola informacyjnego
  - `U` - nienumerowana, wspierają sterowanie i nie podlegają sekwencjonowaniu, niektóre z nich mają pola informacyjne
- `protokół` - używane wyłączenie w Cisco HDLC(cHDLC), określa protokół enkapsulowany w ramce(0x0800 to IP)
- `dane` - zawiera Path Information Unit(PIU), Exchange Identification(XID)
- `FCS` - sekwencja kontrolna ramki, wynik CRC bądź jego część

Typy ramek w HDLC różnią się tylko zawartością pola sterującego.

Domyślnie włączoną metodą enkapsulacji w urządzeniach Cisco na łączach szeregowych jest cHDLC.

Konfigurowanie enkapsulacji HDLC

```
R1(config)# int s0/0/0
R1(config-if)# encapsulation hdlc
```

Weryfikacja konfiguracji

```
R1# show interfaces s0/0/0
R1# show controllers s0/0/0
```

## 3.3 Działanie PPP

Enkapsulacja PPP jest stosowana, gdy trzeba połączyć routery różnych producentów. Dziłanie polega na ustawieniu bezpośredniego połączenia za pomocą kabli szeregowych, lini telefonicznych, specjalizowanych łącz radiowych, czy łącz światłowodowych.

PPP składa się z:
- ramek podobnych od HDLC do transportu pakietów wieloprotokołowych przez łącza punkt - punkt
- `NCP` - Network Control Protocol, ustawianie i konfigurowanie różnych protokołów w warstwie sieci. Pozwala to na obsługę: IPv4(IPCP - IP Control Protocol NCP dla IPv4), IPv6, AppleTalk, Novel IPX, SNA itp. NCP definiuje pola funkcjonalne, które zawierają kody wskazujące typ protokołu warstwy sieci enkapsulowanego przez PPP
- `LCP` - Extensible Link Control Protocol, stosowany do ustanawiania(negocjacja), konfigurowania, zamykanie i testowania(poprawność działania i występowania błędów w konfiguracji) połączeń

Korzyści wynikające z wykorzystania PPP:
- protokół niezastrzeżony,
- funkcje zarządzania jakością łącza, dużo błędów => zamknięcie połączenia
- obsługa uwierzytelniania PAP i CHAP

Architektura PPP

```
+----------------+
|                |
|      NCP       | <- warstwa sieci
|                | <- warstwa łącza danych
+----------------+
|      LCP       |
+----------------+ <-warstwa fizyczna
| Media fizyczne |
+----------------+
```

W warstwie fizycznej możemy konfigurować PPP dla interfejsów:
- (a)synchroniczny szeregowy - RS-232, V.35
- HSSI
- ISDN

>Protokół PPP nie narzuca żadnych ograniczeń dotyczących szybkości transmisji, która jest zależna jedynie od konkretnego interfejsu DTE/DCE.

Ramka PPP:
- `flaga` - jak w HDLC, od flagi się zaczyna i kończy
- `adres` - zawiera adres rozgłoszeniowy(same '1'), PPP nie przypisuje stacjom indywidualnych adresów,
- `sterowanie` - może żądać transmisji danych użytkownika w ramce niesekwencjonowanej,
- `protokół` - identyfikuje protokół danych w PPP,
- `dane` - pakiety protokołu określonego w polu wyżej 
- `fcs`

Ustanawianie sesji PPP, fazy:
1. Zestawienie łącza i negocjacja konfiguracji - zakończona w momencie gdy router otrzyma ramkę potwierdzającą konfigurację od routera inicjującego połączenia
2. Negocjacja jakości łącza(opcjonalne)
3. Negocjacja konfiguracji protokołu warstwy sieci - NCP

>Protokół LCP może zamknąć połączenie w dowolnym momencie. Zazwyczaj występuje to, gdy jeden z routerów żąda zakończenia, ale może się to także zdarzyć w przypadku fizycznego zdarzenia, takiego jak utrata dostępu do nośnika lub po upływie czasu bezczynności.

Klasy ramek LCP, w zależności od fazy LCP:
- ustanawianie połączenia - wysyłana jest ramka `Configure-Request`(zawiera opcje konfiguracyjne wymagane do utworzenia połączenia). Odpowiedzi:
  - `Configure-Reject`, `Configure-Nak` - opcje są niedopuszczalne lub nierozpoznawalne
  - `Configure-Ack` - opcje są zaakceptowane i proces przechodzi do uwierzytelnienia i wymiany pakietów NCP
- zarządzanie połączeniem - LCP może używać komunikatu zwrotnego i testować łącze. Komunikaty:
  - `Echo-Request`, `Echo-Reply`, `Discard-Request` - testowanie połączenia
  - `Code-Reject`, `Protocol-Reject` - informacja zwrotna w przypadku otrzymania nieprawidłowej ramki lub nierozpoznanym kodem typu ramki
- zamykanie połączenia - w każdym momencie, jednak standardowo w momencie gdy wykonania przekazywania danych w sieci. Mamy tutaj takie komunikaty jak: `Terminate-Request` i `Terminate-Ack`

Pakiet LCP - jego zawartość dotyczy pola dane w ramce PPP:
- `kod` - określa typ pakietu LCP
- `indentyfikator` - służy do dopasowania pakietów odpowiedzi żądania do pakietów odpowiedzi
- `długość` - całkowita długość(wszystkie pola) pakietu LCP
- `dane` - pole zmniennej długości(0,x), format pola ustalone jest przez pole kod

>Każdy pakiet LCP jest pojedynczym komunikatem LCP

Opcje konfiguracji PPP:
- uwierzytelnianie za pomocą PAP lub CHAP,
- kompresja za pomocą Stacker lub Predictor,
- połąaczenie multilink - może łączyć dwa lub więcej kanałów, aby zwiększyć przepustowość WAN

>Aby wykonać negocjowanie tych opcji PPP, ramki LCP ustanawiające połączenie muszą mieć dodatkowe informacje (opcje) w polu danych ramki LCP. Brak opcji w ramce oznacza przyjęcie wartości domyślnych

Zasada działania NCP

>Po zainicjowaniu połączenia, LCP przekazuje kontrolę do odpowiedniego NCP. Modułowa budowa modelu PPP pozwala protokołowi LCP skonfigurować łącze, a następnie przesłać szczegóły dotyczące protokołu warstwy sieci do konkretnego NCP.

>Gdy NCP skonfiguruje pomyślnie protokół warstwy sieci, protokół sieci jest w stanie otwartym na ustanowionym połączeniu LCP. W tym momencie PPP może przenosić pakiety odpowiedniego protokołu warstwy sieci.

Pakiety LCP mogą też negocjować pewne parametry, dla przykładu IPCP może negocjować:
- `kompresję` - nagłówków TCP/IP, w celu zaoszczędzenia pasma. Kompresja metodą Van Jacobsona zmniejsza rozmiar nagłówków TCP/IP do 3B. Takie działania mogą poprawić przepustowość w wolnym łączu szeregowym
- `adres IPv4` - pozwala urządzeniu inicjującemu na określenia adresu IPv4 używanego w routingu

>Po zakończeniu procesu NCP, połączenie przechodzi do stanu "otwarty" a LCP ponownie przejmuje łącze w fazie zarządzania. Ruch w połączeniu może składać się z każdej możliwej kombinacji pakietów LCP, NCP i warstwy sieci. Po zakończeniu transferu danych NCP zamyka swoje połączenie a LCP zamyka połączenie PPP.

## 3.4 Konfigurowanie PPP

PPP obsługuje następujące opcje LCP:
- `uwierzytelnianie`:
  - `PAP` - Password Authentication Protocol, bardzo prosty nie szyfrowany proces dwukierunkowy, dwuetapowy
  - `CHAP` - Challenge Handshake Authenticatio Protocol, bezpieczniejszy niż PAP, trójetapowa wymiana wspólnych zaszyfrowanych haseł. Procedura składa się z wysłania challenge\`u, odpowiedzi na niego(hash M5) i zaakceptowania bądź odrzucenia uwierzytelnienia(porównanie własnych obliczeń z tymi odebranymi od urządzenia)
- `kompresja`:
  - `Stacker`
  - `Predictor`
- `wykrywanie błędów` - definiuje warunki błędów, opcja quantity(niezawodność) i magic-number(łącze wolne od zapętleń, liczba generowana losowo na każdym końcu połączenia)
- `wywołanie zwrotne PPP` - klient nawiązuje połączenie wstępne, następnie żąda użycia przez serwer wywołania zwrotnego i kończy połączenie wstępne. Konfiguracja tej opcji na routerze `ppp callback {accept|request}`
- `multilink` - multilink PPP, MP, MPPP, MLP, opcja pozwalająca na równoważenie obciążenia na używanych przez PPP interfejsach routera. Umożliwia jednocześnie fragmentację i defragmentację pakietów, prawidłową kolejność, obsługę wielu dostawców

Włączanie protokołu PPP(enkapsulacji w warstwie 2)na interfejsie

```
R1(config)# interface s0/0/0
R1(config-if)# encapsulation ppp
```

Kompresja(może wpłynąć negatywnie na wydajność systemu)

```
R1(config-if)# encapsulation ppp
R1(config-if)# compress {predictor|stac}
```

LQM - Link Quality Monitoring

```
R1(config-if)# encapsulation ppp
R1(config-if)# ppp quality 80[%]
```

Multilink

```
R1(config)# interface multilink 1
R1(config)# interface s0/0/0
R1(config-if)# encapsulation ppp
R1(config-if)# ppp multilink
R1(config-if)# ppp multilink group 1
R1(config-if)# interface s0/0/1
R1(config-if)# encapsulation ppp
R1(config-if)# ppp multilink
R1(config-if)# ppp multilink group 1
```

Wyłączenie ML -> no ppp multilink

Proces uwierzytelniania CHAP:
1. R1 inicjuje negocjowanie połączenia za pomocą LCP i uzyskanie zgody na korzystanie z uwierzytelnianie CHAP
2. R2 generuje identyfikator oraz liczbę losową i wysyła swoją nazwę użytkownika w pakiecie CHAP jako wezwanie do R1.
3. R1 szuka użytkownika i odpowiedniego hasła, generuje odpowiedź na podstawie funkcji skrótu np MD5(nazwa usera R2,ID, numer losowy, tajne wspólne hasło)
4. R1 wysyła pakiet do R2
5. R2 generuje własną wartość funkcji skrótu
6. Porównuje z tym co otrzymał od R1, wysyła odpowiedź do R2(np. potwierdzenie zestawienia połączenia)

>Uwierzytelnianie, autoryzacja oraz rozliczanie (AAA)/TACACS to dedykowany serwer używany do uwierzytelniania użytkowników. Klienci TACACS wysyłają żądania do serwera uwierzytelniania TACACS. Serwer może uwierzytelniać użytkownika, kontrolować, co użytkownik może zrobić i śledzić, co użytkownik zrobił.

Konfigurowanie uwierzytelniania PAP

>Nazwa hosta na jednym routerze musi odpowiadać nazwie użytkownika skonfigurowanej na drugim routerze. Hasła również muszą się zgadzać.

```
Router(config)# hostname R1
R1(config)# username R2 password someone
R1(config)# interface s0/0/0
R1(config-if)# ppp authentication pap
R1(config-if)# ppp pap sent-username R1 password sameone

Router(config)# hostname R2
R2(config)# username R1 password someone
R2(config)# interface s0/0/0
R2(config-if)# ppp authentication pap
R2(config-if)# ppp pap sent-username R2 password sameone
```

Konfigurowanie uwierzytelniania CHAP

>Nazwa hosta na jednym routerze musi odpowiadać nazwie użytkownika skonfigurowanej na drugim routerze. Hasła również muszą się zgadzać.

```
Router(config)# hostname R1
R1(config)# username R2 password someone
R1(config)# interface s0/0/0
R1(config-if)# ppp authentication chap

Router(config)# hostname R2
R2(config)# username R1 password someone
R2(config)# interface s0/0/0
R2(config-if)# ppp authentication chap
```

## 3.5 Rozwiązywanie problemów z PPP

>Polecenie debug nie powinno być używane jako narzędzie monitorowania; przeciwnie, jest ono przeznaczone do użycia w krótkim okresie czasu, do rozwiązania problemów.

```
R1# debug ppp {packet|negotiation|error|authentication|compression|cbcp}
R1# show interfaces serial
```

---

<div>
<a href="chapter-01.md">Prev: Chapter 2</a>
</div>
<div align="right">
<a href="chapter-03.md">Next: Chapter 4</a>
</div>