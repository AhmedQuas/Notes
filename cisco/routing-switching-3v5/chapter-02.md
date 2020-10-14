### Spis treści
- [Chapter 2: Redundancja w sieciach LAN](#chapter-2-redundancja-w-sieciach-lan)
  - [2.1 Koncepcja STP](#21-koncepcja-stp)
  - [2.2 Działanie STP](#22-działanie-stp)
  - [2.3 802.1D RSTP](#23-8021d-rstp)
  - [2.4 Odmiany protokołów STP](#24-odmiany-protokołów-stp)
  - [2.4 PVST+](#24-pvst)
  - [2.5 Konfiguracja PVST+](#25-konfiguracja-pvst)
  - [2.6 Rapid PVST+](#26-rapid-pvst)
  - [2.7 Konfiguracja Rapid PVST+](#27-konfiguracja-rapid-pvst)
  - [2.8 Problemy konfiguracji STP](#28-problemy-konfiguracji-stp)
  - [2.9 Protokoły zwielokrotniania rotera pierwszego przeskoku](#29-protokoły-zwielokrotniania-rotera-pierwszego-przeskoku)

# Chapter 2: Redundancja w sieciach LAN

## 2.1 Koncepcja STP

Burza ruchu, broadcast storm - bardzo dużo, często niepotrzebnego ruchu wynikającego z madmiernie dużych domen rozgłoszeniowych, objawami mogą być światełka statusu na przełącznikach bezustannie mrugające w bardzo szybkim tempie.

Redundacja - zapewnienie nadmiarowości, stosowane głównie po to aby wyeliminować pojedynczy punkt awarii(uszkodzenie kabla lub przełącznika), zapewniając jednocześnie dużą dostępność sieci. Realizowane jest to dzięki dodatkowym połączeniom fizycznym, ale i również nadmiarowości logicznej

>Nadmiarowe połączenia w przełączanych sieciach Ethernet, mogą powodować `powstanie pętli w warstwie 2`, zarówno tej fizycznej jak i logicznej.

`SPT` - Spanning Tree Protocol, protokół drzewa opinającego służący do zapobiegania powstawania pętli w warstwie 2.

`CST` - Common Spanning Tree, sieć z nadmiarowością połączeń

Problemy w implementacji nadmiarowości:
- `niestabilność bazy adresów MAC` - powodowana przez odebranie kopii tej samej ramki na różnych portach przełącznika
    >Ramki ethernetowe nie posiadają atrybutu time to live `(TTL)`, w skutek czego, jeśli nie ma wdrożonego mechanizmu blokującego ciągłą propagację tych ramek w sieciach przełączanych, to mogą one krążyć między przełącznikami bez końca, bądź do chwili, w której nastąpi przerwanie łącza powodujące rozłączenie pętli.
    
    Ramka rozgłoszeniowa jest przekazywana przez wszystkie porty przełącznika z wyjątkiem tego portu, na którym została odebrana. 

- `burze rozgłoszeniowe` - ciągłe zalewanie sieci rozgłoszeniami bez końca, liczba ramek rozgłoszeniowych w sieci jest tak duża, że wykorzystuje one całą dostępną szerokość pasma, przez co sieć uniemożliwia transmisję danych. Można powiedzieć, że jest to pewna forma ataku DoS. Powstają w sieciach, które posiadają fizyczne pętle i nie mają włączonego protokołu STP.
  > Urządzenia podłączone do sieci stale wysyłają ramki rozgłoszeniowe, takie jak np. żądania ARP, burza rozgłoszeniowa może powstać w ciągu kilku sekund. W rezultacie sieć z pętlą zostaje szybko zatkana, co uniemożliwia dalszą pracę.

- `wielokrotna transmisja ramki` - do urządzenia końcowego może dotrzeć wiele kopii tej samej ramki
  >Większość protokołów warstw wyższych nie jest zaprojektowana pod kątem wykrywania ani radzenia sobie z powielonymi transmisjami. Ogólnie rzecz biorąc, protokoły, które wykorzystują mechanizm numerowania sekwencji, przyjmą, że transmisja się nie powiodła, zaś dany numer sekwencji został użyty ponownie. Inne protokoły spróbują przekazać powieloną transmisję do odpowiedniego protokołu warstwy wyższej w celu przetworzenia przesyłanych danych i najprawdopodobniej ich odrzucenia.

## 2.2 Działanie STP

Zadaniem STP jest zapewnienie tylko jednej logicznej ścieżki pomiędzy sieciami docelowymi za pomocą blokowania logicznych połączeń. Blokada ta nie dotyczy komunikatów `BPDU` (Bridge Protocol Data Unit) wykorzystywanych przez STP do zapobiegania pętlom. W razie awarii STP ponownie obliczy ścieżki i odblokuje odpowiednie porty, interfejsy.

`Genralnie im mniejszy numer tym lepiej.`

STP = 802.1D

STA - Spanning Tree Algorithm

Działanie STP:
1. Wymiana ramek BPDU między przełącznikami, w celu ustalenia najmniejszej wartości identyfikatora mostu (`BID` - Bridge ID). Każdy przełącznik zakłada, że to on jest mostem głównym
  
2. Przełącznik z najmniejszym BID staje się mostem główny, `root bridge`, który jest punktem odniesienia w momencie obliczania tras.

3. Obliczanie najkrótszych ścieżek prowadzących do mostu głównego przez każdy przełącznik za pomocą algorytmu STA 
   >Każdy przełącznik używa algorytmu STA w celu ustalenia portów do zablokowania. W czasie, w którym dla każdego z węzłów w domenie rozgłoszeniowej jest obliczana optymalna ścieżka do mostu głównego, następuje wstrzymanie przekazywania całego ruchu danych przez sieć.

4. Obliczanie kosztów ścieżek, które rachuje się uzględniając koszty i prędkość portów na każdym przełączniku wzdłuż całej trasy od początku do końca, a to stanowi właśnie `koszt ścieżki` do mostu głównego

5. Jeśli do mostu głównego występuje więcej niż jedna ścieżka to STA wybiera tą o najmniejszym koszcie

6. Ustawienie odpowiedniej roli portów 

>`Role portów`, opisują ich relacje w odniesieniu do mostu głównego oraz określają, czy dany port może przekazywać ruch w sieci.

Role portów w RSTP:
- `root` - główne, porty przełącznika najbliższe mostowi głównemu

- `desygnowane` - wszystkie inne porty niż porty główne
  >Porty desygnowane wybierane są na każdym z łącz typu trunk. Jeśli jeden z końców łącza trunk jest portem głównym, to drugi koniec przyjmuję rolę porty desygnowanego. W przypadku mostu głównego, wszystkie porty przyjmują rolę desygnowanych.

- `alternatywne i zapasowe` - znajdują się w stanie blokowania
  >Porty alternatywne wybierane są na łączach trunk, wtedy gdy drugi koniec tego łącza ma przypisaną rolę inną niż port główny. `Zwróć uwagę`, iż tylko jeden koniec łącza trunk jest w stanie blokowania. Pozwala to na szybsze przejście do stanu przekazywania, gdy okaże się to konieczne.

- `wyłączone` - porty, które zostały wyłączone

`BID` - Bridge ID, identyfikator mostu, składa się z następujących pól:
- bridge priority, wartość identyfikatora mostu, można ją modyfikować, domyślnie jest to 32786, a można zmienić na dowolną wielokrotność 4096, inne wartości zostaną odrzucone, 4b

  Wynika to z tego,że w starszych wersjach nie było identyfikacji vlanów, zatem nie było następnego pola

- extended system ID - `opcjonalnie`, wartość rozszerzonego identyfikatora systemowego, wykorzystywana do wskazania instancji(wersji?) protokołu STP 12b, różne instacje dla różnych vlan\`ów
- adres MAC wysyłającego przełącznika 48b,

>W procesie wyboru mostu głównego uczestniczą wszystkie przełączniki z danej domeny rozgłoszeniowej.

>Najmniejsza wartość BID ustalana jest na podstawie tych trzech wartości.

>Jeśli wartość identyfikatora mostu głównego zawarta w otrzymanej ramce BPDU jest mniejsza niż na przełączniku, który tę ramkę odebrał, to przełącznik ten uaktualni przechowywany w swojej pamięci podręcznej identyfikator mostu głównego, przyjmując identyfikator używany przez sąsiadujący przełącznik.

Ustalanie kosztu portów

```
S1(config)# interface f0/1
S1(config-if)# spanning-tree cost 25
```

Resetowanie kosztu portów

```
S1(config)# interface f0/1
S1(config-if)# no spanning-tree cost
```

Weryfikacja kosztu portów

```
S1# show spanning-tree
```

## 2.3 802.1D RSTP

RSTP - Rapid STP

Format ramki BPDU w 802.1D RSTP
>`To do in future`

Role portów:
- root
- designated
- alternate

>Przełącznik z mniejszą wartością BID, przypisuje swoim portom rolę desygnowanych, a przełącznik z większą wartością BID swoje porty ma ustawione jako alternatywne. Należy jednak pamiętać, że `identyfikator BID przełącznika wysyłającego jest używany do ustalenia portu desygnowanego tylko wtedy, gdy koszty ścieżek do mostu głównego są jednakowe`.

**To co tu jest tak naprawdę prawdziwe**

>W przypadku gdy koszt ścieżek prowadzących z dwóch portów przełącznika do mostu głównego jest identyczny, to przełącznik musi ustalić, który z nich będzie portem głównym. Przełącznik korzysta z konfigurowalnej wartości priorytetu portu lub z numeru portu. Jeżeli wartości konfigurowalne są takie same, pod uwagę jest brany numer portu (wybierany jest mniejszy).

>Portowi, którego ścieżka do mostu głównego ma najmniejszy koszt, automatycznie przypada rola portu głównego, ponieważ jest on najbliżej mostu głównego. W topologii sieci wszystkie przełączniki z wyjątkiem mostu głównego, mają zdefiniowany jeden port główny o najmniejszym koszcie ścieżki prowadzącym do mostu głównego.

>W sytuacji gdy dwa przełączniki mają przypisane takie same priorytety oraz rozszerzoną wartość systemową, przełącznik o mniejszej heksadecymalnej wartości adresu MAC będzie miał mniejszą wartość identyfikatora BID.
`Do priorytetu czasem dodaje się jeszcze numer vlan`.

`ID portu` = priorytet + numer portu np. 128.1

Multicastowy adres adres rozgłoszeniowy grupy objętej STP `01:80:C2:00:00:00`. Informacje te przetwarza każdy przełącznik z włączoną obsługą STP.

Domyślnie ramki BPDU są wysyłane co dwie sekundy. 2 sekundy to również domyślna wartość dla pakietów typu "Hello".

## 2.4 Odmiany protokołów STP

`STP` - 1998, oryginalna, pierwotna wersja protokołu IEEE 802.1D, nie uwzględnia vlanów. Ulepszona w 2004 -> `802.1D-2004`

`PVSTP+` - Per vlan STP, rozwiązanie Cisco, dla każdego vlanu pracuje osobna instancja STP. Obsługuje funkcje tj. PortFast, Uplink Fast, BPDU guard. Dzięki temu można stosować `load balancing`.

`RSTP` - Rapid STP, IEEE 802.1w rozwinięcie STP, zapewniające osiągnięcie szybszej zbieżności.

`Rapid PVSTP+` - tworzenie osobnej instancji RSTP dla vlanów, rozwiązanie Cisco

`MSTP` - Multiple STP, odpowiednik PVSTP+ i `MISTP`-Multiple Instance SPT od Cisco, opracowany jako standart IEEE. Pozwala na przypisanie wielu sieci VLAN do tej samej instancji RSTP

Potrzebne zasoby vs czas osiągnięcia zbierzności

STP per vlan - duże zapotrzebowanie na zasoby
Rapid - szybki czas osiągnięcia zbieżności

>Od IOS 15.0 przełączniki mają `domyślnie włączoną obsługę PVST+`, obsługują STP w wersji z 2004 i można jest skonfigurować pracy z RSTP.

## 2.4 PVST+

Cechy STP dla sieci CST:
- brak możliwości równoważenia obciążenia
- obliczana jest jedna instancja STP, zatem oszczędzamy CPU

PVSTP+ pozwala na równomierne rozłożenie obciążenia w warstwie 2, co wynika z możliwości uruchomienia różnych instancji STP na różnych przełącznikach

>W środowisku Cisco PVST+ można dostroić parametry drzewa rozpinającego, tak że przez poszczególne łącza trunk przekazywany jest ruch z połowy sieci VLAN

Cechy PVSTP+:
- możliwość optymalizacji, równoważenia obciążenia
- uruchomienie osobnych instancji wpływa negatywnie na zajętość pasma i zwiększenie obciążenia CPU we wszystkich przełącznikach w sieci

Stany portów PVST+:
- blokowanie - odbiera tylko ramki BPDU
- nasłuchiwanie - w celu ustalenia ścieżki do mostu głównego
- uczenie - zapisywanie adresów MAC
- przekazywanie - przekazuje wszystkie ramki
- wyłączony - administracyjnie wyłączony 

Budowa topologii wolnej od pętli w PVST+:
- wybór mostu głównego
- wybór portu głównego - port o najniższym koszcie do mostu głównego
- wybór portu desygnowanego
- porty alternatywne - wszystkie inne, znajdują się w stanie blokowania

`BID dla vlan 2` = priorytet + 2

32770 = 32768 + 2

## 2.5 Konfiguracja PVST+

Konfiguracja identyfikatora mostu

```
S1(config)# spanning-tree vlan 1 root primary
```

```
S1(config)# spanning-tree vlan 1 priority 24567
```

Zapasowy most główny

```
S1(config)# spanning-tree vlan 1 root secondary
```

Sprawdzenie identyfikatora mostu

```
S1(config)# show spanning-tree
```

Konfiguracja PortFast i BPDU Guard

BPDU Guard
>W prawidłowo skonfigurowanej sieci, ramki BPDU nie powinny pojawić się na portach typu PortFast. Pojawienie się tam, tych ramek, oznaczałoby iż do tego portu podpięty jest inny przełącznik, co mogłoby spowodować powstanie pętli. Przełączniki Cisco posiadają mechanizm zwany BPDU guard. Jeżeli funkcja ta jest włączona, to po pojawieniu się ramki BPDU na danym porcie, zostanie on natychmiast wyłączony(stan error-disabled). Musi zostać manualnie włączony przez administratora `no shutdown`.

PortFast a DHCP
>Technologia PorFast okazuje się być pomocna w przypadku działania protokołu DHCP. Gdy funkcja PortFast nie jest stosowana, komputer PC może wysłać żądanie DHCP, zanim port znajdzie się w stanie przekazywania; wskutek tego host może nie uzyskać adresu IP ani innych przydatnych informacji.

```
S1(config)# interface f0/1
S1(config-if)# spanning-tree portfast
S1(config-if)# spanning-tree bpduguard enable
```

Włączenie PortFast na wszystkich interfejsów nie będącymi łączami typu trunk.

```
S1(config)# spanning-tree portfast default
```

Włączenie na wszystkich portach z PortFast funkcji BPDU Guard

```
S1(config)# spanning-tree bpduguard default
```

Weryfikacja

```
S1# show running-config
```

## 2.6 Rapid PVST+

Role portów:
- główny
- desygnowany
- alternatywny
- brzegowy
- zapasowy

Stany portów w Rapid PVSTP+:
- odrzucenie, discarding
- uczenie
- przekazywanie

Cechy Rapid PVST+:
- przyspieszenie procesu ponownego obliczania drzewa rozpinającego
- szybsza zbieżność poprawnie skonfigurowanej sieci, nawet do kiluset ms
- preferowany protokół do zapobiegania powstawania pętli na poziomie warstwy 2
- korzysta z ramek BPDU w wersji 2
- ramki BPDU są używane jako mechanizm keep-alive, utrata 3 ramek jest traktowana jako utrata połączenia między mostami, przełącznikami

`Port brzegowy` - port, który z założenia nigdy nie ma być połączony z przełącznikiem. Po włączeniu od razu przechodzi w tryb przekazywania, w PVSTP+ funkcja ta jest nazywana `PortFast`.

>`Nie zaleca się` konfiguracji portu jako brzegowy, w przypadku gdy podpięty ma być do niego inny przełącznik. Może mieć to negatywny wpływ na działanie RSTP: wystąpienie tymczasowych pętli lub opóźnienia w czasie osiągnięcia zbieżności.

Typy połączeń, dla portów nie-brzegowych:
- `punkt - punkt` - port działa w trybie full-duplex, zazwyczaj łączy 2 przełączniki
- `współdzielony` - działa w trybie half-duplex, połączenia przełącznik z hubem

Typy połączeń najbardziej są wykorzystywane przez porty desygnowane, do szybkiego przejścia w stan przekazywania.

## 2.7 Konfiguracja Rapid PVST+

>Instancja STP tworzona jest w momencie przypisania sieci VLAN do określonego portu, a usuwana gdy na żadnym porcie nie został skonfigurowany dany VLAN.

>`Konfiguracja interfejsu jako łącza typu punkt-punkt, nie jest konieczna` z uwagi na fakt, że łącza współdzielone są dziś bardzo rzadko spotykane. W większości przypadków, różnica w konfiguracji pomiędzy PVST+ a Rapid PVST+, ogranicza się do wydania polecenia spanning-tree mode rapid-pvst.

```
S1(config)# spanning-tree mode rapid-pvst
S1(config)# interface f0/1
S1(config)# spanning-tree link-type point-to-point
```

Reset STP

```
S1# clear spanning-tree detected-protocols
```

## 2.8 Problemy konfiguracji STP

1. Zapoznanie się z topologią warstwy 2, za pomocą dokumentacji lub polecenia `show cdp neighbors`
2. Ustal oczekiwane ścieżki w warstwie 2, wiedząc, który przełącznik jest mostem głównym
3. Sprawdź, który z przełączników jest mostem głównym, `show spanning-tree vlan`
4. Sprawdź, stan w jakim znajdują się porty używając powyższego polecenia
   
Często problemem mogą być rozbieżności między stanem oczekiwanym, a aktualnym. Wynika to często z pominięcia projektowania działania STP

Wyświetlenie informacji dla STP dla wszystkich sieci vlan 

```
S1# show spanning-tree vlan 
S1# show spanning-tree vlan 10
```

Awarie STP:
- błędne blokowanie portu znajdującego się w stanie przekazywania, utrata części danych
- błędne przełączenie portu w tryb przekazywania, powstanie pętli, fluktuacje tablicy adresów MAC, w wyniku przeciążenia sieci może być uniemożliwiony dostęp zdalny

## 2.9 Protokoły zwielokrotniania rotera pierwszego przeskoku

Koncepcja alternatywnych bram głównych i routera wirtualnego.

Wirtualny adres IP i adres MAC.

>Poprzez współdzielenie adresów IP i MAC, dwa lub więcej routerów może działać jako jeden router wirtualny. 

>Adres IP routera wirtualnego pełni rolę bramy domyślnej dla wszystkich stacji roboczych w danym segmencie sieci.

>Różnica w funkcjonalności pomiędzy przełącznikami wielowarstwowymi a routerami w warstwie dystrybucyjnej. Zazwyczaj, przełączniki wielowarstwowe wykorzystywane są jako bramy domyślne dla każdej sieci VLAN

>W sieciach przełączanych, każdy z użytkowników `otrzymuje tylko jeden adres bramy domyślnej`.

Awaryjne przełączanie routera:
1. Router w stanie standby(oczekiwanie,gotowość), przestaje otrzymywać pakiety Hello od routera aktywnego
2. Router standby zmienia stan na aktywny, przejmując adres IP i MAC

`FHRP` - First Hop Redundancy Protocol, grupa protokołów, której zadaniem jest wybór aktywnego routera, a w momencie awarii określa, który router zapasowy ma przejąć jego rolę

Protokoły FHRP:
- `HSRP` - Hot Standby Router Protocol, wyznacza urządzenie aktywne i urządzenie/a zapasowe(standby). Urządzenie zapasowe monitoruje status routera aktywnego, tak aby w razie awarii urządzenia aktywnego możliwie szybko przejąć jego rolę
- `HSRP dla IPv6`
- `VRRPv2` - odpowiedniek HSRP, jednak jest on nie zastrzeżony
- `GLBP` - Gateway Load Balancing Protocol, zabezpiecza ruch sieciowy przed awarią pojednyczego routera, dodatkowo zapewnia rozłożenie obciążenia między routerami działającymi w grupie
    Jeden wirtualny adres IP i kilka wirtualnych adresów MAC.
- `GLBP dla IPv6`
- `IRDP` - ICMP Router Discovery Protocol, ustandaryzowane GLBP, 

Wirtualny adres ip wprowadzamy jest na obu routerach, reszta jest opcjonalna
```
R1(config)# interface g0/1
R1(config-if)# standby 1 ip 192.168.1.254
R1(config-if)# standby 1 priority 150
R1(config-if)# standby 1 preempt

R1# show standby
```

Na drugim routerze ustawamy wirtaulne IP i metodę balansowania ruchu
```
R1(config)# interface g0/1
R1(config-if)# glbp 1 ip 192.168.1.254
R1(config-if)# glbp 1 preempt
R1(config-if)# glbp 1 priority 150
R1(config-if)# glbp 1 load-balancing round-robin

R1# show glbp
```

Wyłączenia HSRP/GLBP

```
R1(config-if)# no standby 1

R1(config-if)# no glbp 1
```

---

<div>
<a href="chapter-01.md">Prev: Chapter 1</a>
</div>
<div align="right">
<a href="chapter-03.md">Next: Chapter 3</a>
</div>