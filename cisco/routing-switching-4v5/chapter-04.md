### Spis treści
- [Chapter 4: Frame Relay](#chapter-4-frame-relay)
  - [4.1 Wprowadzenie do FR](#41-wprowadzenie-do-fr)
  - [4.2 Zaawansowane pojęcia związane z FR](#42-zaawansowane-pojęcia-związane-z-fr)
  - [4.3 Konfiguracja FR](#43-konfiguracja-fr)
  - [4.4 Rozwiązywanie problemów z łącznością](#44-rozwiązywanie-problemów-z-łącznością)

# Chapter 4: Frame Relay

## 4.1 Wprowadzenie do FR

Dobra alternetywa dla drogich dedykowanych lini dzierżawionych. Obecnie wypierany jest przez metro Ethernet. W przeciwieństwie do łącz dzierżawionych, FR wymaga tylko jednego łącza do dostawcy. FR został wynaleziony jako prostsza wersja X.25 do stosowania na interfejsach ISDN.

Korzyści wynikające ze stosowania FR:
- zapewnia większą przepustowość, niezawodność i elastryczność niż linie dzierżawione
- jeden interfejs do połączenia z wieloma sieciami
- wykorzystanie stałych wirtualnych obwodów(PVC), które są tworzą logiczną ścieżkę na łączu pomiędzy miejscem początkowego połączenia FR, a zakończeniem połączenia FR
- bardziej opłacalne rozwiązanie:
  - płacimy tylko za pętlę lokalną i pasmo, a nie całe fizyczne połączenie. Odległość nie ma znaczenia. Potrzeba tylko jednego łącza fizycznego, aby połączyć się z siecią rozległą FR
  - możliwośc definiowania przepustowości z gradacją co 4 kb/s, a nie 64 kb/s
  - możliwość dzielenia pasma przez większą liczbę klientów
- znacznie większa elastyczność projektowanych sieci


Połączenie przez sieć FR pomiędzy urządzeniami DTE jest wirtualnym obwodem(VC) - połączenie jest logiczne, dane są przesyłane jednego końca do drugiego bez zestawianie bezpośredniego obwodu elektrycznego

Tryby zestawianie wirtualnych obwodów:
- `SVC` - Switched Virtual Circuit, przełączane wirtualne obwody, obwód ustawiany dynamiczni przez wysyłanie sygnałów do sieci
- `PVC` - Pernamet Virtual Circuit, skonfigurowane wcześniej przez dostawcę usług, później pracują tylko w trybie przesyłu danych lub bezczynności

>Niektóre publikacje nazywają PVC jako prywatny wirtualny obwód. `PVC są częściej implementowane niż SVC.`

>Obwody wirtualne zapewniają dwukierunkową komunikację z jednego urządzenia do drugiego. Są zdefiniowane przez identyfikatory DLCI(są unikatowe tylko w kanale fizycznym, nie są unikalene w FR, mogą się powtórzyć). Identyfikator DLCI jest przechowywany w polu adresowym każdej transmitowanej ramki aby wskazać sieci gdzie ramka powinna być skierowana. Dostawca usługi Frame Relay przydziela wartości DLCI. Zwykle wartości DLCI od 0 do 15 oraz od 1008 do 1023 są zarezerwowane. Tak więc, usługodawcy przydzielają wartości DLCI od 16 do 1007 włącznie.

FR wykorzystuje multipleksowanie statystyczne - pozwala na transmisję jednej ramki w danym czasie, ale zarówno pozwala na obsługę wielu logicznych połączeń, które będą przesyłane przez pojedyncze fizyczne połączenie.

`FRAD` - Frame Relay Access Device, urządzenie dostępow FR

>FRAD lub router podłączony do sieci Frame Relay mogą mieć stworzonych wiele obwodów wirtualnych (VC) do wielu punktów końcowych. Wiele obwodów VC na pojedynczym fizycznym łączu są rozróżnialne ponieważ każdy ma swój własny identyfikator DLCI

Enkapsulacja FR opisana w ITU Q.922-A:
- `Flaga` - znaczenie jak w PPP, oznacza początek i koniec ramki. 0x7E -> 01111110
- `Adres`:
  - `DLCI` - Data Link Conncetion Identifier, 10-bitowa wartość reprezentująca wirtualne połączenie pomiędzy urządzeniem DTE i przełącznikiem FR
    >Każde wirtualne połączenie, które jest multipleksowane na fizycznym kanale reprezentowane jest przez unikalną wartość DLCI.
  - `C/R` - obecnie pole nie zdefiniowane
  - `EA` - Extended Address, jeśli równy 1 to obecny bajt jest jako ostatni oktet DLCI
  - `Congestion Control` - 3-bitowe pole  informujące o zatorze:
    - FECN
    - BECN
    - DE - '1'->CRI powyżej ustawionego limitu, można odrzucić ramkę
- `Dane`
- `FCS`
- `Flaga`

>Frame Relay nie powiadamia źródło kiedy ramka jest odrzucana. Obsługa błędów pozostawiona jest wyższym warstwom modelu OSI.

Topologie FR:
- `Hub & Spoke` - zwana topologią gwiazdy, 
  - Hub - punkt centralny
  - Spoke - połączenie fizyczne, pojedynczy obwód wirtualny
- `Full mesh` - każde urządzenie jest połączone z każdym
- `Mesh` - FR ma może teoretycznie, maksymalnie obsługiwać 1000 obwodów wirtualnych przypadających na łącze 

Sposoby mapowania adresów warstwy 3 na identyfikatory DLCI:
- `Odwzorowanie dynamiczne`
  - `Inverse ARP` - pozwala na odwzorowanie obwodów wirtualnych(VC) na adresy sieciowe np. IP. To nie jest broadcast, router już zna DLCI, a chce poznać jeszcze IP
- `Odwzorowanie statyczne`
  
  ```
  R1(config)# interface s0/0/0
  R1(config-if)# encapsulation frame-relay
  R1(config-if)# no frame-relay inverse-arp
  R1(config-if)# frame-relay map ip 10.1.1.2. 106 [broadcast] [ietf] cisco
  ```
  ietf - gdy łączymy się z innym routerem niż Cisco. Rodzaj enkapsulacji: cisco lub ietf
  broadcast - umożliwia przekazywanie rozgłoszeń routingu dynamicznego

Sprawdzenie odwzorowań FR

```
R1# show frame-relay map
```

`LMI` - Local Management Interface, mechanizm podtrzymywania(keepalive), który udostępnia informacje o stanie połączenia FR między routerm i przełącznikiem FR. Wykorzystywane w innych sieciach niż ISDN. Kontrola pozwala upewnić się, że łącza są w stanie przesłać dane. LMI jest definicją komunikatów przesyłanych pomiędzy DTE i DCE.

Opcjonalne rozszerzenia LMI:
- komunikat statusu obwodu wirtualnego
- komunikacja grupowa(multicast)
- adresacja globalna
- prosta kontrola przepływów

Typy interfejsu LMI:
- CISCO
- ANSI
- Q.933-A

Ręczne ustawianie LMI na wybranym interfejsie

```
R1(config-if)# frame-relay lmi-type {ciso|ansi|q933a}
```

Domyślnie czas keepalive wynosi 10s, duże rozbieżności między urządzeniami mogą spowodować, że urządzenie zostanie błędnie uznane za nieaktywne

```
R1(config-if)# keepalive 10[s]
```

## 4.2 Zaawansowane pojęcia związane z FR

`Access rate` - szybkość dostępu odnosi się do przepustowości portu, wyznaczana przez taktowanie na przełączniku Frame Relay.

>Z punktu widzenia klienta usługodawca dostarcza połączenie szeregowe lub łącze do sieci Frame Relay poprzez linie dzierżawione.

`CIR` - Committed Information Rate, gwarantowana szybkość transmisji. CIR to ilość danych, które sieć musi odebrać z obwodu dostępowego

>Usługodawca gwarantuje, że klient będzie mógł przesyłać dane z prędkością wynoszącą CIR. Wszystkie ramki otrzymane z równą lub mniejszą prędkością są akceptowane.

>CIR określa maksymalną średnią wartość szybkości transmisji danych, które sieć zobowiązuje się dostarczyć w normalnych warunkach.

W FR klient płaci za:
- przepustowość, szybkość dostępu,
- obwód PVC,
- CIR

>`Nadsubskrypcja` - dostawcy usług sprzedają więcej pasma niż mają bazując na założeniu, że nie każdy będzie chciał korzystać ze swojego pełnego pasma przez cały czas. Sytuacja podobna jak z liniami lotniczymi

Skokowa nadmiarowa transmisja - bursting

Z drugiej strony klienci nie mają górnego limitu i jeśli obciążenie sieci na to pozwala na to, że klienci mogą przekraczać limit CIR. Urządzenie może przekroczyć CIR aż do wysokości szybkości portu(przez 3-4s).

Kontrola przepływu, powiadamianie o zatorze -> FECN, BECN.

>Bit FECN(Forward) ustawiany jest w każdej ramce, którą przełącznik otrzymuje na przeciążonym łączu.

>Bit BECN(Backward) jest ustawiany na każdej ramce, którą przełącznik umieszcza na przeciążonym łączu.

>Nagłówek ramki zawiera również bit DE, który oznacza ruch o mniejszym znaczeniu, który może być odrzucony podczas występowania zatorów w sieci.

## 4.3 Konfiguracja FR

Przepustowość na interfejsie ustawia się bardziej z powowdu, że niektóre protokoły routingu wykorzystują tą wartość w momencie obliczania metryki trasy.

```
R1(config)# interface s0/0/0
R1(config-if)# bandwidth 64
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# ipv6 address {global and linklocal}
R1(config-if)# encapsulation frame-relay
R1(config-if)# no shutdown
```

Odwzorowanie statyczne

```
R1(config)# frame-relay ip map 10.0.0.2 106 broadcast
R1(config)# frame-relay ip map 2001:dead:beef:1::2 106
R1(config)# frame-relay ip map fe80::2 106 broadcast 
```

>Frame Relay, ATM oraz X.25 są sieciami nierozgłoszeniowymi z wielodostępem (non-broadcast multiaccess) i są oznaczane skrótem NBMA. Sieci NBMA pozwalają tylko na transfer danych od jednego urządzenia do drugiego czy to przez obwód wirtualny (VC) czy przez urządzenie przełączające. Sieci NBMA nie obsługują ruchu komunikacji grupowej (multicast) czy rozgłoszeniowej (broadcast), tak więc pojedynczy pakiet nie może dotrzeć do wielu adresatów. To wymaga ręcznej replikacji pakietu to wszystkich adresatów. `Użycie opcji broadcast jest uproszczonym sposobem przekazywania aktualizacji routingu.`

Problemy z osiągalnością sąsiadów:
- >`Split horizon` - mechanizm zabezpieczający przed powstanie pętli dla protokołów routingu dynamicznego wektora odległości takich jak EIGRP i RIP. Nie jest ona stosowana do protokołów routingu stanu łącza. Reguła ta redukuje pętle routingu przez zapobieganie wysyłaniu aktualizacji routingu przez ten sam interfejs, na którym aktualizacja ta została otrzymana.

    Może powstać problem z przesyłaniem aktualizacji z routera Hub -> centrali

- `wykrywanie DR i BDR` - brak rozgłoszenia w sieci NBMA = nie można automatycznie wykryć sąsiadów

Rozwiązanie problemów z osiągalnością:
- `wyłączenie split horizon` - możliwa tylko dla IP, jednak wtedy mogą powstawać pętle routingu
- `stosowanie topologii full mesh`
- `wykorzystanie podinterfejsów` - podzielenie jednego fizycznego interfejsu na kilka logicznych

Tryby pracy subinterfejsów:
- `punkt-punkt` - pojedynczy indetyfikator DLCI, nie podlega on regule split horizon
- `wielopunktowy` - działa podbnie jak fizyczny interfejs w FR w NBMA

Podinterfejsy

```
R1(cofig-if)# encapsulation fr && no shutdown
R1(cofig-if)# interface s0/0/0.106
R1(cofig-subif)# ip address && bandwidth
R1(cofig-subif)# frame-relay interface dlci
```

>Usuń adres warstwy sieciowej przypisany do interfejsu fizycznego. Jeżeli fizyczny interfejs ma adres, ramki nie są odbierane przez lokalne podinterfejsy.

>`To dostawca usługi Frame Relay przydziela numery DLCI`

Czasem trzeba restartować interfejs, bo może nie zastosować zmian właśnie wprowadzonych.

## 4.4 Rozwiązywanie problemów z łącznością

>Frame Relay dla IPv6 używa Inverse Neighbor Discovery (IND) w celu uzyskania adresu IPv6 warstwy 3 na podstawie DLCI warstwy 2.

```
R1# show interfaces s0/0/0
R1# show frame-relay lmi
R1# show frame-relay pvc [interface] 102 
R1# clear counters
R1# clear frame-relay inarp
R1# show frame-relay map
R1# debug frame-relay lmi
```

Pole status podczas `debug frame-relay lmi`:
- `0x0` - przełącznik ma skonfigurowany ten DLCI, ale z dziwnych powodów nie może on zostać użyty. Przyczyną może być tutaj wyłączenie drugiego końca obwodu
- `0x2` - ok, wszystko jest skonfigurowane poprawnie
- `0x4` - przełącznik nie ma skonfigurowanego DLCI, ale był on kiedyś skonfigurowany. Jedną z przyczyn może być zmiana identyfikatorów DLCI na routerze

---

<div>
<a href="chapter-03.md">Prev: Chapter 3</a>
</div>
<div align="right">
<a href="chapter-05.md">Next: Chapter 5</a>
</div>