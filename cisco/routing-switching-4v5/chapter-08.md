### Spis treści
- [Chapter 8: Monitorowanie sieci](#chapter-8-monitorowanie-sieci)
  - [8.1 Syslog](#81-syslog)
  - [8.2 SNMP](#82-snmp)
  - [8.3 NetFlow](#83-netflow)

# Chapter 8: Monitorowanie sieci

>Czasami w sieci dochodzi do różnych zdarzeń. W takich przypadkach urządzenia sieciowe mogą przy pomocy odpowiednich zaufanych mechanizmów poinformować o tym fakcie administratora, wykorzystując do tego celu specjalne szczegółowe komunikaty systemowe.

## 8.1 Syslog

`Syslog` - protokół opisany w RFC 3164 przez IETF w 2001, jest wykorzystywany do przesyłania przez sieć informacji o zdarzeniach systemowych do tzw. serwera logów. Wykorzystuje port 514/udp.

Syslog spełnia następujące zadania:
- zbieranie rejestrowaych danych na potrzeby monitoringu i rozwiązywania problemów
- wybór rodzaju informaji, które mają zostać przechwycone
- definiowania odbiorców przechwytywanych informacji syslog

>W przypadku urządzeń sieciowych Cisco, protokół syslog rozpoczyna działanie wysyłając komunikaty systemowe oraz wynik polecenia debug do lokalnego procesu rejestrowania na danym urządzeniu.

W zależności od konfiguracji zdarzenia mogą być:
- wysyłane przez sieć do zewnętrznego serwera
- przesyłane do wewnętrznego bufora(pamięć RAM) i są widoczne jedynie przez wiersz poleceń CLI na urządzeniu
- konsola
- terminal

Przesyłane do różnych miejsc w zależności od konfiguracji i typu komunikatu

Poziom ważności - severity

<div align='center'>

| Nazwa poziomu | Poziom | Opis   |
|:---:        |:---:      |:---:       |
| Emergency | 0 | System nie nadaje się do użytku |
| Alert | 1 | Potrzebne natychmiastowe działanie |
| Critical | 2 | Stan krytyczny|
| Error | 3 | Stan błędu |
| Warning | 4 | Stan ostrzegawczy |
| Notification | 5 | Stan normalny, ale istotny |
| Informational | 6 | Komunikat informacyjny |
| Debugging | 7 | Komunikat debug |

</div>

Oprócz poziomu ważności, komunikaty syslog zawierają także informacje o obiekcie, którego dotyczą. Typowe obiekty syslog:
- IP,
- OSPF,
- system operacyjny (`SYS`)
- zabezpieczenia na poziome IP (IPsec)
- interfejs IP (IF)

Format komunikatów syslog:
- `seq no` - numer sekwencyjny, gdy włączono `sequence-numbers` w trybie konfiguracji globalnej
- `timestamp` - data i czas komunikatu/zdarzenia, wyświetlene gdy skonfigurowanp `service timestamp` w trybie konfiguracji globalnej
- `facility` - obiekt do którego odnosi się komunikat
- `severity` - ważność, poziom komunikatu,
- `MNEMONIC` - text jednoznacznie określający wiadomość,
- `description` - text zawierający szczegółowe informacje na temat zdarzenia


```
seq no: timestamp: %facility-severity-MNEMONIC: description
```

```
00:00:46: %LINK-3-UPDOWN: Interface Port-channel1, changed state to up
```

W zarejestrowanych zdarzeniach zostanie zapisany czas jaki upłynął od ostatniego uruchomienia urządzenia(`uptime`) lub data i czas zdarzenia(`datetime`)

```
R1(config)# service timestamp log {uptime|datetime}
``` 

`NTP` - Network Time Protocol, protokół wykorzystywany do synchronizacji czasu między urządzeniami. Zsynchronizowany czas pozwala na prównywanie plików logów pochodzących z różnych urządzeń.

Konfiguracja NTP

```
R1(config)# ntp master

R1(config)# ntp server r1_ip_address
```

>Domyślnie routery i przełączniki Cisco wysyłają komunikaty logów dla wszystkich poziomów ważności na konsolę.

`level debugging` - rejestrowane są wszystkie zdarzenia od debugging w górę(czyli wszystko)

Konfiguracja syslog:
1. Instalacja servera syslog na stacji roboczej np. Kiwi Syslog Serwer
2. Konfiguracja urządzenia sieciowego
   - adres IP serwera syslog,
   - określenie ważności komunikatów, które mają zostać przesyłane
   - wskazanie interfejsu źródłowego

```
R1(config)# logging 192.168.1.10
R1(config)# logging trap {4|warning}
R1(config)# logging source-interface g0/1
```

Weryfikacja syslog
- show logging
- show logging | include changed state to up
- show logging | begin April 2 21:37

## 8.2 SNMP

`SNMP` - Simple Network Management Protocol, protokół warstwy aplikacji, pozwala na zarządzanie wydajnością sieci, odnajdywanie i rozwiązywanie problemów sieciowych oraz planowanie rozwoju sieci. Domyślnie działa na porcie 162/udp.

Architektura SNMP:
- `Manager SNMP` - część systemu zarządzania, uruchamia się na nim oprogramowanie pozwalające na: 
  - zbieranie informacji(`get`), 
  - zmiany konfiguracji(`set`),
  - przyjmowanie informacji od agentów za pomocą pułapek(`trap`), natychmiast generowane komunikaty alarmowe generowane bez żądania
- `Agent SNMP` - węzeł zarządzalny, odpowiada za zapewnienie dostępu do lokalnej bazy danych MIB, w której odzwierciedlone są zasoby oraz wykonywane działania
- `MIB` - Management Information Base, baza danych zarządzania, przechowują dane o czynnościach wykonywanych przez urządzenie i udostępniają je uwierzytelnionym zdalnym użytkownikom

>Zadaniem agentów SNMP, rezydujących na zarządzanych urządzeniach, jest zbieranie i przechowywanie informacji o urządzeniach oraz podejmowanych przez nie działaniach. Informacje te przechowywane są w lokalnej bazie MIB. Jeżeli menedżer SNMP zechce uzyskać dostęp do tych informacji, korzysta z pomocy agenta SNMP.

Operacje SNMP:
- `get-request` - pobiera wartość z określonej zmiennej MIB
- `get-next-request` - pobranie kolejnej wartości, sekwencyjnej wyszukiwanie(?)
- `get-bulk-request` - pobiera duży blok danych, wiele zmiennych, dostępne od SNMPv2
- `get-response` - odpowiedź na powyższe 3 operacje wysłane przez `NMS`(Network Management System)
- `set-request` - ustaw wartość konkretnej zmiennej
- `trap` - wysyła informację o niepożądanym stanie alarmowym np. w sytuacji, gdy dany parametr osiągnie określony próg.

>Powiadomienia oparte na komunikatach `trap` zmniejszają wykorzystanie zasobów sieci oraz agenta, poprzez wyeliminowanie konieczności wysyłania niektórych żądań odpytywania SNMP.

Wersje SNMP:
- `SNMPv1` - historyczna wersja, obecnie nie używana, RFC 1157,
- `SNMPv2c` - wykorzystuje formy zabezpieczeń opart na społeczności, zawiera rozszerzone kody błędów z typami, RFC 1901 - 1908
- `SNMPv3` - wspiera bezpieczny dostęp do urządzeń poprzez obsługę uwierzytelniania i szyfrowania pakietów w sieci. W celu wykrycia zmiany podczas transmiji weryfikuje integralność pakietów, RFC 3410 - 3415. Obsługuje `modele`(ustalona strategia uwierzytelniania przypisana do użytkownika i grupy) i `poziomy` bezpieczeństwa(maksymalny dozwolony poziom zabezpieczeń wewnątrz modelu bezpieczeństwa)

>Żeby użyć wybranej wersji SNMP, obsługiwanej przez stację zarządzającą, administrator sieci musi odpowiednio skonfigurować agenta SNMP. Ponieważ agent może komunikować się z wieloma menadżerami SNMP, można tak skonfigurować oprogramowanie, żeby używało SNMPv1, SNMPv2 lub SNMPv3.

Community, łańcuchy community, odpowiadają za kontrolę dostępu(uwierzytelniania) do bazy MIB, są to hasła zapisane jawnym tekstem. Wyróżniamy następujące typy:
- `ro` - tylko do odczytu
- `rw` - odczyt i zapis

>Hasła zapisane tekstem jawnym nie są uznawane za mechanizm bezpieczeństwa. Jest tak ponieważ hasła tego typu są szczególnie wrażliwe na ataki typu `Man-In-The-Middle`, podczas których pakiety mogą zostać przechwycone.

Zmienne MIB w hierarchiczny sposób definiuje każdą zmienną prz pomocy identyfikatora objektu(`OID`), obiekty są organizowane w postaci drzewa, wykorzystując standardy RFC

Identyfikatory OID należące do Cisco:
- .iso(1).org(3).dod(6).internet(1).private(4).enterprises(1).cisco(9)
- co jest równoważne 1.3.6.1.4.1.9

Rozszywanie OID -> SNMP Object Navigator

Konfiguracja SNMP

```
R1(config)# snmp-server community ccommunity_sting {ro|rw} ACL_number_or_name
R1(config)# snmp-server location NOC_custom_name #dokumentacja lokalizacja urządzenia
R1(config)# snmp-server concat AhmedQuas #kontakt
R1(config)# snmp-server host 192.168.1.11 [version { 1 | 2c | 3 [auth|noauth|priv] } community_string
R1(config)# snmp-server enable traps [notification_type]*
```

*Jeśli nie określono za pomocą tej komendy żadnych typów powiadomień o komunikatach trap, wtedy wysyłane są wszystkie typy komunikatów trap. Jeśli chcemy wykorzystać określony podzbiór typów tego komunikatu, wymagane jest powtórne użycie tej komendy.

>Domyślnie, SNMP nie wysyła żadnych komunikatów trap. Bez wydawania tej komendy, oprogramowanie menadżera SNMP musi pobierać wszystkie istotne informacje.

Weryfikacja:
- show snmp
- show snmp community

Praktyki bezpieczeństwa:
- jeśli korzystamy z SNMPv1 i SNMPv2c - czyli wykorzystujemy community string to hasła powinny być trudne do złamania, zmieniane w regularnych odstępach czasu, zgodnie z polityką bezpieczeństwa sieci(gdy administrator opuszcza firmę)
- upewnij się, że komunikaty SNMP nie są wysyłane poza konsolę zarządzania, w tym celu skonfiguruj odpowiednio listy ACL
- gdzie tylko można to stosuj SNMPv3, korzystając z poleceń `snmp-server group` i `snmp-server user`

Programy do obsługi SNMP(na stacji roboczej):
- snmpget

## 8.3 NetFlow

`NetFlow` to rozwiązanie Cisco pozwalające na tworzenie statysty dotyczących pakietów przepływających przez urządzenie sieciowe, śledzenia liczby bajtów/pakietów w odniesieniu do strumienia danych. Jest on także standardem służącym do zbierania danych operacyjnych w sieciach IP. W dużej mierze koncentruje się na dostarczaniu danych statystycznych, które mogą być wykorzystane do monitorowania sieci, jej zabezpieczeń, analizę ruchu(wykrywanie wąskich gardeł, zliczanie pakietó IP do celów rozliczeniowych). Następnie na podstawie danych generowane są odpowiednie statystyki i przesyłane są do zewnętrznego serwera zwanego kolektorem NetFlow.

Obecnie najnowszym rozwiązaniem jest `Flexible NetFlow`, który pozwala na dostosowanie parametrów analizy do wymagań administratora, co pozwala na tworzenie bardziej kompleksowych konfiguracji. Wykorzystują format 9, który bazuje na szablonach, które z kolei umożliwiają rozszerzenie formatu rekordu.

Możliwości wykorzystania NetFlow:
- zmierzenie kto używa jakich zasobów sieciowych i w jakim celu,
- jakie strony są regularnie odwiedzane i co jest pobierane,
- rozliczanie i ponowne doładowanie w zależności od poziomu wykorzystania zasobów, czy obecna przepustowość łącza jest wystarczająca,
- wykorzystanie statystyk do bardziej efektywnego rozplanowania sieci i ulepszenie infrastruktury

>Porównując funkcjonalność SNMP i NetFlow, można powiedzieć że SNMP odpowiada oprogramowaniu do zdalnego sterowania bezzałogowego pojazdu, podczas gdy NetFlow to szczegółowy rachunek telefoniczny.

>W przeciwieństwie do SNMP, NetFlow wykorzystuje model typu `push-based`(urządzenie klienckie inicjuje wysyłanie danych do skonfigurowanego serwera). Urządzenia sieciowe, na podstawie zmieniających się wartości pamięci podręcznej, wysyłają dane NetFlow do kolektora, który nasłuchuje tego ruchu na swoim interfejsie. Kolejną różnicą między. 

NetFlow gromadzi tylko statystyki ruchu, które są bardziej szczegółowe podczas, gdy SNMP obsługuje wiele różnych wskaźników wydajności(błędy interfejsu, wykorzystanie procesora i pamięci)

>Kluczowe kryteria, obowiązujące jako wytyczne przy jej tworzeniu:
>- NetFlow powinien być całkowicie przeźroczysty dla aplikacji i urządzeń w sieci
>- NetFlow nie musi być uruchamiany i obsługiwany na wszystkich urządzeniach w sieci, aby funkcjonować prawidłowo.

>Pomimo, że wdrożenie NetFlow jest stosunkowo proste i jest on przeźroczysty w sieci, zużywa dodatkową pamięć urządzenia Cisco, ponieważ zapisuje poszczególne rekordy w pamięci podręcznej urządzenia. Domyślny rozmiar tej pamięci cache różni się, w zależności od platformy, ale administrator może zmienić tą wartość.

>Strumień (przepływ) określany jest jako jednokierunkowy przepływ pakietów pomiędzy określonym systemem źródłowym a docelowym.

"Oryginalny NetFlow" obejmuje definicję przepływu jako połączenie `siedmiu pól`. Jeżeli jedno z tych pól różni się co do wartości od analogicznego pola w innym pakiecie, można bezpiecznie założyć, że pochodzą one z różnych strumieni:
- źródłowy adres IP,
- docelowy adres IP,
- numer portu źródłowego,
- numer portu docelowego,
- typ protokołu warstwy trzeciej - identyfikuje typ nagłówka występujący po nagłówku IP(zazwyczaj chodzi o TCP, UDP lub ICMP),
- znacznik typu usługi(ToS) - zawiera informacje(z nagłówka IPv4) o tym w jaki sposób urządzenia powinny stosować zasady QoS
- logiczny interfejs wejściowy

Konfiguracja NetFlow

NetFlow przechwytuje dane z pakietów przychodzących (`ingress`) i wychodzących (`egress`)

```
R1(config)# interface s0/0/1
R1(config-if)# ip flow ingress
R1(config-if)# ip flow egress
R1(config-if)# exit
R1(config)# ip flow-export destination 192.168.1.3 2055 #udp_port
R1(config)# ip flow-export version 5
R1(config)# ip flow-export source f0/1
```

Aplikacje do analizy NetFlow:
- SolarWinds NetFlow Traffic Analyzer :D
- (NCF) Cisco NetFlow Collector
- Plixer Scrutinizer
- polecenie show wydane na urządzeniu sieciowym

>Strumień NetFlow jest jednokierunkowy. Oznacza to, że każde połączenie między użytkownikiem a aplikacją występuje w postaci dwóch strumieni NetFlow.

Popularne porty dla NetFlow:
- 99,
- 2055,
- 9996

Wersje formatów NetFlow:
- 1,
- 5,
- 7,
- 8,
- 9 - Flexible NetFlow

Weryfikacja:
- weryfikacja danych, statystyk otrzymanych na kolektorze NetFlow,
- show ip cache flow,
- show ip flow interface,
- show ip flow export

`MRTG` - Multi Router Traffic Grapher

---

<div>
<a href="chapter-07.md">Prev: Chapter 7</a>
</div>
<div align="right">
<a href="chapter-09.md">Next: Chapter 9</a>
</div>