### Spis treści
- [Chapter 11: To jest sieć](#chapter-11-to-jest-sie%c4%87)
  - [11.1  Tworzenie małej sieci](#111-tworzenie-ma%c5%82ej-sieci)
  - [11.2 Utrzymanie bezpieczeństwa w sieci](#112-utrzymanie-bezpiecze%c5%84stwa-w-sieci)
    - [11.2.1 Ograniaczanie ataków sieciowych](#1121-ograniaczanie-atak%c3%b3w-sieciowych)
    - [11.2.2 Uwierzytelnienie, autoryzacja i rozliczanie](#1122-uwierzytelnienie-autoryzacja-i-rozliczanie)
    - [11.2.3 Firewall](#1123-firewall)
    - [11.2.4 Zabezpieczenia urządzeń](#1124-zabezpieczenia-urz%c4%85dze%c5%84)
  - [11.3 Testowanie wydajności sieci](#113-testowanie-wydajno%c5%9bci-sieci)
  - [11.4 Zarządzanie plikami konfiguracyjnymi IOS](#114-zarz%c4%85dzanie-plikami-konfiguracyjnymi-ios)
  - [11.5 Routery z zintegrowanymi usługami](#115-routery-z-zintegrowanymi-us%c5%82ugami)

# Chapter 11: To jest sieć

## 11.1  Tworzenie małej sieci

Czynniki, które mają wpływ na dobór urządzeń w sieci:
- koszt
- zapewnienie nadmiarowości, redundancji
- szybkość, możliwa przepustowość
- możliwości rozbudowy, stała czy modularna budowa?
- dodatkowe funkcje, które muszą być wspierane, np. QoS, NAT, DHCP, bezpieczeństwo, przełączniki warstwy drugiej, moduł światłowodowy

Projekt adresacji IP powinien obejmować urządzenia takie jak:
- urządzenia końcowe - adresy nadawane przez DHCP
- serwery i urządzenia peryferyjne - nadanie statycznych adresów IP
- hosty, które powinny być osiągane z poziomu Internetu - przekierowania portów, port forwarding
- urządzenia pośredniczące np. switch\`e

W celu zachowania dużego stopnia niezawodności sieci wymagane jest zwielokrotnienie w sieci, eliminując tym samym pojedyncze punkty awarii (`SPOF` - Single Point of Failure). Zwielokrotnienie może zostać osiągnięte poprzez zainstalowanie dodatkowych zapasowych urządzeń i zapewnienie dodatkowych łączy w obszarach krytycznych.

>Im mniejsze sieci, tym mniejsza szansa, że ​​nadmiarowość sprzętu będzie osiągalna. Dlatego częstym sposobem na wprowadzenie zwielokrotnienia jest zastosowanie nadmiarowych połączeń pomiędzy wieloma przełącznikami w sieci oraz między przełącznikami a routerami.Im mniejsze sieci, tym mniejsza szansa, że ​​nadmiarowość sprzętu będzie osiągalna. Dlatego częstym sposobem na wprowadzenie zwielokrotnienia jest zastosowanie nadmiarowych połączeń pomiędzy wieloma przełącznikami w sieci oraz między przełącznikami a routerami.

>Celem dobrego projektowania sieci, nawet małej, jest zwiększenie produktywności pracowników i zminimalizowanie przestojów sieci.

`Aplikacje sieciowe` - oprogramowanie używane do komunikacji poprzez sieć np. poczta elektroniczna i przeglądarki internetowe

`Usługi sieciowe` - np. przesyłanie plików przez sieć lub drukowanie w udostępnionej drukarce sieciowej

Elementy wymagane w utrzymaniu skalowalnej sieci:
- `dokumentacja sieci`
- `inwentaryzacja sieci`
- `budżet`
- `analiza ruchu`

Określanie wzorców przepływu ruchu:
- rejestrowanie ruchu w godzinach szczytu, aby pobrana próbka była reprezentacyjne
- przechwytywanie ruchu w poszczególnych segmentach sieci, aby było wiadomo, jak bardzo obciążone są poszczególne segmenty

Dodatkowo pomocne może okazać monitorowanie informacji na hostach takich jak:
- system operacyjny i jego wersja
- aplikacje wykorzystujące i nie wykorzystujące sieci
- wykorzystanie procesora, RAM i zsobów dyskowych

## 11.2 Utrzymanie bezpieczeństwa w sieci

Zagrożenia powodowane przez hakera:
- `kradzież informacji` - wyniki badań, plany rozwojowe firmy
- `kradzież tożsamości` - informacje osobowe, zakupy w sieci, wyłudzenia kredytu
- `utrata i zmiana danych` - zmiana lub nieautoryzowane usunięcie zapisów na dysku
- `zakłócenia usług` - uniemożliwienie dostępu do usług do, których podmiot jest uprawniony, np. atak DoS

>Nawet w małych sieciach, konieczne jest, wzięcie pod uwagę zagrożeń bezpieczeństwa sieci i podatności z tym związanych podczas planowaniu wdrożenia sieci.

Zagrożenia fizyczne:
- `sprzętowe` - fizyczne uszkodzenia urządzeń
- `środowiskowe` - skrajna wilgotność i temperatury
- `elektryczne` - niespodziewany spadek zasilania, skoki napięcia, szumy
- `związane z utrzymaniem` - niewłaściwe obsługa urządzeń, wyładowania elektrostatyczne, złe oznakowanie okablowania, brak zapasów najbardziej wadliwych komponentów

Czynniki bezpieczeństwa sieciowego:
- `podatność` - technologiczne, konfiguracyjne, związane z polityką bezpieczeństwa
- `zagrożenie`
- `atak`

Typy złośliwego oprogramowania:
- `wirusy`
- `konie trojańskie`
- `robaki`

Typy ataków sieciowych:
- `rozpoznanie` - przeszukiwanie internetu, wykrywanie aktywnych hostów (ping sweeps), skanowanie portów, analizowanie pakietów, whois, nslookup
- `uzyskanie dostępu` - najczęściej wykorzystanie luki w zabezpieczeniach usługi. Do tej grupy możemy zaliczyć takie rodzaje ataków jak: atak słownikowy, brute-force, wykorzystanie zaufania, przekierowanie portów, Man-in-the-Middle
- `odmowa usługi` - osiągnięte zawyczaj poprzez wyczerpanie różnych zasobów

### 11.2.1 Ograniaczanie ataków sieciowych

Kroki ograniczania rozprzestrzeniania się zagrożenia:
- `powstrzymywanie` - odseparowanie od sieci
- `szczepienie` - uaktualnianie podatnego oprogramowania
- `kwarantanna` - wyłapanie każdej zainfekowanej maszyny wewnątrz sieci
- `leczenie` - czyszczenie i aktualizowanie każdego zainfekowanego hosta

### 11.2.2 Uwierzytelnienie, autoryzacja i rozliczanie

`AAA` - Authentication, Authorization, Accounting. 

`Uwierzytelnianie` - potwierdzenie swojej tożsamości np. poprzez podanie loginu i hasła. Uwierzytelnianie może być lokalne, w ramach pojedynczego urządzenia. Możemy również wykorzystywać zewnętrzne standardy uwierzytelniania takie jak `RADIUS` i `TACACS+`.

`Autoryzacja` - określenie do jakich zasobów użytkownik powinien mieć dostęp

`Rozliczanie` - dokumentowanie czynności wykonanych przez użytkownika

>Pojęcie AAA jest podobne do użycia karty kredytowej. Karta kredytowa określa, kto może jej używać, jak wiele użytkownik może wydać i rozlicza na jakie rzeczy użytkownik wydał pieniądze, tak jak pokazano na rysunku.

### 11.2.3 Firewall

Jest to urządzenie znajdujące się na granicy sieci, głównym jego zadaniem jest kontrolowanie, filtrowanie ruchu, co ma utrudnić nieautoryzowany dostęp. Główne możliwości tej klasy urządzeń:
- `filtrowanie pakietów`
- `filtrowanie na poziomie aplikacji`
- `filtrowanie URL`
- `SPI` - Stateful Packet Inspection, stanowa inspekcja pakietów, analizuje czy przychodzące pakiety są częścią nawiązanej sesji

Zapory ogniowe są często wykorzystywane do translacji adresów `NAT`.

>NAT tłumaczy wewnętrzny adres lub grupę adresów na zewnętrzny publiczny adres rozpoznawany w Internecie. Pozwala to na ukrycie wewnętrznych adresów IP przed zewnętrznymi użytkownikami.

### 11.2.4 Zabezpieczenia urządzeń

Domyślne ustawienia zabezpieczeń nigdy nie są wystarczające. Jedne z podstawowych czynności, które należy podjąć, aby podnieść poziom bezpieczeństwa:
- zmiana domyślnych haseł i nazw użytkownika
- dostęp do zasobów systemowych tylko dla uprawnionych osób
- wyłączenie wszystkich niepotrzebnych usług i aplikacji
- aktualizacja oprogramowania do najnowszej wersji

Wytyczne dotyczące tworzenia haseł 11.2.4.2.

Podstawowe praktyki bezpieczeństwa na IOS:
- szyfrowanie wszystkich haseł, które są nieszyfrowane
- wprowadzenie minimalnej długości hasła
- blokowanie logowania po kilku nieudanych próbach
- bannery
- wylogowanie po przekroczeniu czasu bezczynności
- zdalny dostęp do urządzenia realizowany przez `ssh`, wyłączenie możliwości połączenia przez `telnet`

## 11.3 Testowanie wydajności sieci

Wskaźniki polecenia ping w IOS:
- `!` - otrzymano odpowiedź na ICMP ECHO
- `.` - upłynął czas oczekiwania na komunikat ICMP REPLY
- `U` - otrzymano komunikat o nieosiągalności miejsca docelowego

>Polecenie `ping` używane jest także do sprawdzenia wewnętrznej konfiguracji IP lokalnego hosta.

Rozszerzona wersja komendy ping

```
R1# ping
```

>Jedną z najbardziej efektywnych metod monitorowania i rozwiązywania problemów z wydajnością sieci jest wyznaczenie jej `charakterystyki bazowej`. Wyznaczenie charakterystyki bazowej polega na studiowaniu sieci w regularnych odstępach czasu, bowiem tylko wtedy możemy mieć pewność, że sieć pracuje zgodnie z tym, jak ją zaprojektowano.

`Tracert` Linux, traceroute na Windows, zwraca adresy kolejnych przeskoków na trasie pakietu.

`Show` w Cisco IOS pozwala na wyświetlenie istostnych informacji o konfiguracji i sposobie działania urządzenia, popularne polecenia show:
- `show running-config`
- `show interfaces`
- `show arp`
- `show ip route`
- `show protocols`
- `show verstion`
- `show ip interface brief`

Polecenie ipcofig

```
C:\ ipconfig /all
```

Wyświtlenie przechowywanych odwzorowanych nazw, tych które są w buforze DNS, cache danego hosta.

```
C:\ ipconfig /displaydns
```

Czyszczenie buforu DNS.

```
C:\ ipconfig /flushdns
```

Polecenie arp

Wyświetlenie tablicy odwzorowań adresów fizycznych do znanych adresów IPv4, wyświetlenie tablicy ARP.

```
C:\ arp -a
```
Wyczyszczenie tablicy ARP

```
C:\ arp -d
```

>Uwaga: Tablica ARP zawiera jedynie informacje o urządzeniach, do których ostatnio był kierowany ruch. Aby mieć pewność, że tablica ARP będzie zawierała wpis dotyczący określonego hosta, należy uprzednio wysłać do niego ping.

Protokół CDP

`CDP` - Cisco Discovery Protocol, służy do automatycznego wykrywania sąsiednich urządzeń Cisco z uruchominym protokołem CDP. CDP działa w warstwie łącza danych, dwa lub więcej urządzenia Cisco, na przykład routery obsługujące różne protokoły warstwy sieci, mogą się dowiedzieć nawzajem o swoim istnieniu, nawet jak nie są bezpośrednio połączone.

```
R1# show cdp neighbors detail
```

>Aby globalnie wyłączyć CDP, należy użyć w konfiguracji globalnej polecenia no cdp run. Aby wyłączyć rozgłaszanie CDP na interfejsie trzeba w trybie konfiguracji tego interfejsu wydać polecenie no cdp enable.

## 11.4 Zarządzanie plikami konfiguracyjnymi IOS

Cisco IFS

```
R1# show file systems
```

`*` - oznacza domyślny system plików
`#` - wskazuje, że jest to dysk startowy

cd, pwd, dir, more

>Plik konfiguracji może zostać skopiowany z pliku do urządzenia. Podczas kopiowania do terminala IOS traktuje każdą linię tekstu pliku konfiguracji jako komendę. Oznacza to, że tekst w zapisanym na dysku pliku będzie często wymagał edycji, w celu zamiany haseł zaszyfrowanych na zapisane jawnym tekstem oraz usunięcia linii nie będących komendami, np. tekstu "--More--" oraz komunikatów IOS.

Do wykonania bieżącej i/lub startowej wersji konfiguracji należy wykorzystać polecenie `copy`. 

Flash USB powinno być sformatowane w formacie `FAT16`, aby była kompatybilna z urządzeniem CISCO.

Procedura odzyskiwanie hasła i interfejs `ROMMON`.

## 11.5 Routery z zintegrowanymi usługami

`NAS` - Network Attached Storage

`ISR router` - router z zintegorwanymi usługami może pełnić funkcje:
- routera`
- `przełącznika`
- `punktu bezprzewodowego, AP`
- `DHCP`
- `firewall`
- `serwer NAS`

>Dodając drukarkę bezprzewodową do sieci, działającą w standardzie 802.11b powodujemy, że oba urządzania, zarówno router jak i laptop `powróci do korzystania z wolniejszego standardu 802.11b dla całej komunikacji`. Dlatego utrzymanie starszych urządzeń bezprzewodowych w sieci będzie spowalniać całą sieć. Należy pamiętać o tym, przy podejmowaniu decyzji, czy utrzymywać starsze urządzenia bezprzewodowe.

`SSID` - Service Set Identifier, nazwa sieci bezprzewodowej

>Podstawowe zadania konfiguracyjne takie jak zmiana domyślnego użytkownika i hasła, zmienienie domyślnych adresów IP urządzenia, a nawet standardowe zakresy adresów IP serwera DHCP, należy przeprowadzić `przed połączeniem AP z siecią operatora`.

> Większość punktów dostępowych stosowanych obecnie umożliwia automatyczne wykrycie najmniej obciążonego kanału.

---

<div>
<a href="chapter-10.md">Prev: Chapter 10</a>
</div>