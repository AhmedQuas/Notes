### Spis treści
- [Chapter 1: Poznawanie sieci](#chapter-1-poznawanie-sieci)
  - [1.1 Połączenia globalne](#11-po%c5%82%c4%85czenia-globalne)
  - [1.2 Sieci LAN, WAN i Internet](#12-sieci-lan-wan-i-internet)
    - [1.2.1 Składniki sieci](#121-sk%c5%82adniki-sieci)
      - [1.2.1.1 Sieciowe urządzenia pośredniczące](#1211-sieciowe-urz%c4%85dzenia-po%c5%9brednicz%c4%85ce)
      - [1.2.1.2 Media sieciowe](#1212-media-sieciowe)
      - [1.2.1.3 Inne pojęcia](#1213-inne-poj%c4%99cia)
    - [1.2.2 Sieci LAN, WAN i Internet](#122-sieci-lan-wan-i-internet)
    - [1.2.3 Dostęp do internetu](#123-dost%c4%99p-do-internetu)
  - [1.3 Sieć jako plarforma](#13-sie%c4%87-jako-plarforma)
    - [1.3.1 Sieć niezawodna](#131-sie%c4%87-niezawodna)
  - [1.4 Zmiana środowiska sieci](#14-zmiana-%c5%9brodowiska-sieci)
    - [1.4.1 Nowe trendy](#141-nowe-trendy)
    - [1.4.2 Bezpieczeństwo sieciowe](#142-bezpiecze%c5%84stwo-sieciowe)

# Chapter 1: Poznawanie sieci
>Badanie zagrożeń bezpieczeństwa i nauka technik obrony, powinna rozpocząć się od dogłębnego zrozumienia zasad przełączania i routowania.

## 1.1 Połączenia globalne
`IoE` - Internet of Everything - skupia ludzi, procesy i dane, aby wszystko to za pomocą sieci stało się bardziej istotne i cenne.

Formy komunikacji:
- komunikatory internetowe (IM - Instant messaging)
- media społecznościowe
- narzędzia pracy grupowej
- blogi - indywidualny dziennik
- wiki - strony internetowe pozwalające grupie ludzi wspólnie je edytować
- publikacje internetowe (podcast)
- udostępnianie plików P2P

Rodzaje sieci:
- małe sieci domowe
- sieci małego biura/biuro w domu, `SOHO` (Small office/home office)
- sieci średnie do wielkich (np. w obrębie budynku)
- sieci rozległe (WAN - Wide Area Network)

`Host` to każde urządzenie biorące udział w komunikacji sieciowej.

`Klient` - host posiadający odpowiednie oprogramowanie do wysyłania zapytań oraz wyświetlania informacji uzyskanych od serwera (w ramach danej usługi).

`Serwer` - host z zainstalowanym odpowiednim oprogramowaniem, który świadczy usługi dla klienta/ów

`Peer-to-Peer` - host działa równocześnie jako klient i serwer (jakoś podobnie mamy w torrentach)

## 1.2 Sieci LAN, WAN i Internet
Kategorie składników sieci:
- urządzenia
- media
- usługi 

`Urządzenia końcowe` - zapewniają interfejs między człowiekiem a siecią np. komputery, durkarki, VoIP, urządzenia mobilne
### 1.2.1 Składniki sieci
#### 1.2.1.1 Sieciowe urządzenia pośredniczące
`Urządzenia pośredniczące` - łączą urządzenia końcowe:
- urządzenia dostępowe (switch, AP)
- urządzenia łączące sieci (routery)
- urządzenia zapewnijące bezpieczeństwo (firewall)

Funkcje:
- regenerują i przekazują sygnały
- utrzymują informacje o istniejących trasach w sieci
- powiadamiają urządzenia o błędach i awariach w komunikacji (np. TTL--)
- jeśli wystąpi awaria to kierują alternatywnymi ścieżkami
- obsługują `QoS` (Quality of Service)
- umożliwiają (permit) lub blokują (deny) przepływ danych (np firewall)

#### 1.2.1.2 Media sieciowe
`Medium` - jest to kanał, którym przesyłana jest wiadomość:
- miedź (cooper)
- włókna szklane (fiber optic)
- transmisja bezprzewodowa (fala elektromagnetyczna, zakres fal widzialnych i podczerwieniu)

Kryteria wyboru odpowiedniego miedium:
- odległość - na jaką długość możemy poprawnie transmitować sygnał
- otoczenie (wpływ zakłóceń EM, otwarta przestrzeń, budynek itp)
- prędkość transmisji
- koszt medium, instalacji i jego eksploatacji

#### 1.2.1.3 Inne pojęcia

`Karta sieciowa` (NIC - Network Interface Card) - zapewnia fizyczne połączenie z siecią dla hosta

`Port fizyczny` - wtyczka lub gniazdo w urządzeniu sieciowym do którego podłączone jest medium, czyli bardziej jako fizyczny port

`Interfejs` - bardziej w odnisieniu do portu widzianego w systemie operacyjnym

`Topologia fizyczna` - przedstawia umiejscowienie fizyczne (uwzględnia rozkład pomieszczeń) urządzeń pośredniczących, rozplanowanie przewodów (czy jak kto woli kabli)

`Topologia logiczna` - identyfikuje urządzenia, porty i adresowanie (adres IP, maski, podział na podsieci)

### 1.2.2 Sieci LAN, WAN i Internet
`LAN` - Local Area Network:
- łączy sieci na małym obszarze np. dom, szkoła, biuro
- wysokie przepustowości w ramach swojej infrastruktury
  
`WAN` - Wide Area Network:
- łączy ze sobą sieci LAN
- zazwyczaj oferuje wolniejsze połączenie

`MAN` - Metropolitan Area Network

`WLAN` - Wireless Area Network

`SAN` - Storage Area Network - rozwiązanie specjalne zaprojektowane pod serwery plików:
- przechowywanie danych
- replikacja
- wysokiej klasy serwery
- macierze dyskowe
- technologia Fiber Channel (szybkie połączenia między węzłami sieci)

`internet` - sieć łącząca wiele sieci, połączenie wielu routerów

`Internet` - połączenie sieci komputerów lub sieci WWW

`intranet` - sieci LAN i WAN należące do firmy, dostęp do niej mają wyłącznie członkowie, pracownicy i osoby upoważnione. Zazwyczaj intranet jest dostępny z sieci wewnętrzej (LAN) organizacji, lub po nawiązaniu połączenia przez VPN

`extranet` - budowana głównie po to, aby zapewnić osobom trzecim bezpieczny dostęp do danych firmowych.
Przykłady:
- dostęp do stanu magazynu dla wykonawców czy dostawców
- system rezerwacji wizyt

### 1.2.3 Dostęp do internetu
`ISP` - Internet Service Provider, dostawca usług sieciowych

Metody połączenia z Internetem:
- Łącze kablowe - oferowane przez dostawców usług telewizyjnych. Tym samym przewodem przesyłana jest telewizja i internet, szerokopasmowe połaczenie.
- `DSL` - Digital Subscriber Line - łącze działa za pomocą lini telefonicznej (podział lini na 3 kanały: rozwowa, upload, download danych), szerokopasmowy dostęp. Duże znaczenie ma dystans do centrali telefonicznej. Technologia wymaga specjalnego modemu, który separuje sygnał DSL od sygnału telefonicznego
- Technologia komórkowa - dostępna wszędzie tam gdzie mamy zasięg telefonii, wydajność zależy głównie od telefonu i BTS (przekaźnika). Doskonale sprawdza się w miejscach gdzie nie dostępnych innych możliwości, bądź jesteśmy ciągle w ruchu
- Technologia satelitarna - wymaga bezpośredniej łączności z satelitą (utrudnione w terenach zalesionych, lub gdzie występują inne naziemne przeszkody). Prędkość zależy od warunków, wysoki koszt sprzętu.
- Połączenie telefoniczne `dial up` - linia telefoniczna + modem. Nawiązanie połączenia to wykonanie połączenia na numer telefoniczny ISP. Niskie przepustowości, mogą być przydatne w czasie podróży.

Metody połączenia z Internetem dla firm:
- dedykowane łącza dzierżawione - dla prywatnych transmisji, między oddzielonymi geograficznie biurami (Linie T1, T3, E1, E3)
- Metro Ethernet - dedykowane połącznie między klientem a ISP, w formie łącz miedzianych i światłowodowych
- `DSL` - `ADSL` - asymetryczne, większy download niż upload. `SDSL` - symetryczne upload = download
- Technologie satelitarne

## 1.3 Sieć jako plarforma

`Sieć konwergentna` - sieć powstała w procesie 
konsolidacji niezależnych sieci (m.i.n.: telefonii, 
telewizji). Dzisiaj jedna sieć (jedna infrastruktura, 
te same reguły i standardy) pozwala na działanie wielu usług.

### 1.3.1 Sieć niezawodna

Każda sieć powinna spełniać 4 poniższe cechy:

1. `Tolerancja błędu` - zapwenienie architektury odpornej na błędy, lub ograniczenie zasięgu awarii. Zapewnienie nadmiarowych ścieżek, `redundancja` - nadmiarowość.

    `Sieć z przełączaniem obowdów` - tutaj celem jest utrzymanie istniejących połączeń niż zestawianie nowych. Zestawienie połączenia oznacza blokadę zasobów, a ich mamy skończoną ilość, a wszystko kosztuje, więc nie jest to optymalna technologia dla internetu. Bardziej wykorzystywana w systemie telefonicznym.

    `(Bezpołączeniowa) sieć z przełączaniem pakietów` - pojedyncza widomość jest dzielona na wiele mniejszych zawierających dane adresowe, więc te mniejsze części zwana pakietami mogą być wysyłane różnymi drogami. W całość są łączone w miejscu docelowym. W każdym miesjcu (węźle) sieci dynamicznie jest podejmowana decyzja o dalszej trasie pakietu. W przypadku awarii retransmitujemy kilka utraconych pakietów, a nie całą wiadomość.


2. `Skalowalność` - możliwość szybkiej rozbudowy bez negatywnego wpływu na jakość świadczonych usług. Jest to również zdolność akceptacji nowych rozwiązań, aplikacji.

    >Przestrzeganie standardów pozwala producentom sprzętu i oprogramowania koncentrować się na rozwoju i doskonaleniu produktów w obszarze wydajności i pojemności, wiedząc że nowe produkty mogą się zintegrować i ulepszyć istniejącą infrastrukturę.

3. `QoS (Quality of Service, jakość usług)` - gdy chcemy wysłać więcej danych niż mamy dostępnej szerokości pasma, trzeba jakoś to wszystko kolejkować. Niektóre usługi powinny mieć wyższy priorytet(transmisja głosowa), podział na klasy, kategorie, faworyzowanie pakietów o wyższym priorytecie. QoS to również zarządzanie parametrami opóźnień i strat pakietów. Przepełnienie kolejki to automatyczne odrzucanie pakietów, które do niej przychodzą.
  
    `Przeciążenie` - stan, w którym zapotrzebowanie na zasoby sieciowe przewyższa dostępną pojemność sieci

4. `Bezpieczeństwo` - podjęcia środków, które mają na celu zapobieanie:
     - nieuprawnionemu dostępowi, ujawnieniu poufnych danych
     - kradzieży informacji, tożsamości
     - nieuprawnionej modyfikacji infromacji
     - przeprowadzeniu ataku `DoS` (Denial of Service)
  
    Podstawowe filary bezpieczeństwa informacji, `CIA triad`:
      - `Poufność` - dostęp do danych tylko dla upoważnionych odbiorców (silne uwieżytelnianie, trudne hasła, zaszyfrowana komunikacja i dane)
      - `Integralność` - zapewnienie, że informacja nie została zmieniona podczas transmisji, określenie czy pakiet nie został zmodyfikowany podczas transmisji (sumy kontrolne lub szyfrowanie)
      - `Dostępność` - zapewnienie dostępu do usługi dla uprawnionych/autoryzowanych użytkowników. Realizowane przez nadmiarową infrastrukturę, ograniczanie skutków awarii (jeśli wystąpi) i posiadanie oprogramowania, sprzętu, który pomaga wykrywać, odpierać, przeciwdziałać różnym formom ataku. 

Struktura internetu, czyli kto komu płaci:
- `ISP lvl 1` - udostępnia połączenia na poziomie narodowym i międzynarodowym (Verizon, Sprint, AT&T, NTT, bezprzewodowy WAN), sieć szkieletowa internetu  

- `ISP lvl 2` - zapewnia dostęp do internetu na poziomie regionalnym, płacą ISP lvl 1 za dostęp do internetu. Połączenia między sieciami na tym poziomie są realizowane w sposób bezpośredni, dzięki temu odciążamy szkielet sieci (lvl 1)

- `ISP lvl 3` - lokalni dostawcy internetu, podłączają oni bezpośrednio użytkowników końcowych, płacą tym do których są przyłączeni, czyli zazwyczaj lvl 2

>Popularne usługi mogą być powielane w różnych regionach, powstrzymując ruch przed przechodzeniem do sieci szkieletowych wyższego poziomu.

## 1.4 Zmiana środowiska sieci

### 1.4.1 Nowe trendy

`BYOD` - Bring Your Our Device, użytkownik końcowy korzysta z dowolnego urządzenia, daje to dużą swobodę korzystania ze swoich prywatnych narzędzi, oprogramowania. Dowolne urządzenie może być używane gdziekolwiek i bez znaczenia do kogo one należy

`Praca grupowa online` -  nikt nie jest ograniczony do fizycznej lokalizacji, większa elastyczność, mniejsze koszty dla firmy i teoretycznie większa wydajność zespołu

`Komunikacja wideo` - jeszcze większa możliwość integracji pracowników, tworzenie multimedialnych materiałów reklamowych, ograniczenie liczby podróży służbowych w celu przeprowadzenia spotkania. _Cisco TelePresence_

`VoD` - Video-on-Demand, możliwość oglądania interesujących nas programów, filmów kiedy i gdzie chcemy

`Chmura obliczeniowa` - Cloud Computing, wykorzystywanie oprogramowania i zasobów sprzętowych dostarczonych jako usługa w sieci (AWS, Azure). Zazwyczaj płaci się za wykorzystane zasoby, `pay-per-use`. Klient nie musi posiadać super wydajnego sprzętu, nie musi też płacić za licencje na oprogramowanie na swój sprzęt.

Korzyści korzystania z chmury obliczeniowej:
- elastyczność organizacji - dostęp praktycznie z każdego miejsca na ziemi
- zwinna i szybka rozbudowa infrastruktury
- zmniejszczenie kosztów infrastruktury
- tworzenie nowych modeli biznesowych
- możliwość chwilowego zwiększenia mocy obliczeniowej (np. okres świąt)

`Centra danych` - pozwalają na funkcjonowanie chmur obliczeniowych. Duże obiety z ogronmą liczbą serwerów i dodatkowymi elementami:
- nadmiarowe połączenia teleinformatyczne
- skupisko współpracujących wirtualnych serwerów o dużych mocach obliczeniowych (zwana `klastrami`, `farmami serwerów`)
- redundancja danych, `SAN` 
- nadmiarowe i zapasowe źródła zasilania `UPS` (Uninterruptible Power Supply)
- kontrola warunków otczenia. Odpowiednia klimatyzacja, wentylacja, systemy przeciwpożarowe
- urządzenia i oprogramowanie do zapewnienia bezpieczeństwa

---
Trendy sieciowe dla domu

`Inteligentny budynek` - integracja urządzeń codziennego użytku z innymi urządzeniami, w celu większej automatyzacji, możliwości zdalnego sterowania pewnymi urządzeniami

`Komunikacja z wykorzystaniem sieci elektroenergetycznej` - możliwość podłączenia urządzenia wszędzie tam, gdzie jest gniazdko elektryczne, bez nowych przewodów

`WISP` - Wireless ISP, bezprzewodowy dostawca internetu. Rodzaj dostawcy usług internetowych, który za pomocą dedykowanego punktu bezprzewodowego lub hot spota podłącza abonentów do sieci

`Bezprzewodowa transmisja szerokopasmowa` - ta sama technologia dostępu do internetu jak w przypdaku smartfonów i operatora sieci komórkowej

### 1.4.2 Bezpieczeństwo sieciowe

Zagrożenia zewnętrzne:
- `wirusy, robaki i konie trojańskie` - złośliwy kod i oprogramowanie działające na urządzeniu użytkownika
- `spyware i adware` - wyświetlanie niechcianych reklam, potajemne zbieranie informacji o użytkowniku
- `zero-day` - atak wykorzystujący świeżo odkrytą lukę, podatność, na którą nie ma jeszcze aktualizacji
- `ataki hakerów` - ataki na urządzenia lub zasoby przeprowadzony przez osobę, często też z pomocą zautomatyzowanych skryptów
- `Dos` - Denial of Service, spowolnienie, wykorzystanie zasobów, czasem prowadzi to do zatrzymania urządzenia/programu
- `przechwycenie i kradzież danych` - przechwycenie prywatnych informacji 
- `kradzież tożsamości` - kradzież danych do logowania, w celu dostępu do prywatnych danych.

>Równie ważnym jest aby wziąć pod uwagę wewnętrzne źródła zagrożenia. Istnieje wiele przypadków, które pokazują iż najczęściej winni naruszeniu bezpieczeństwa sieci są użytkownicy wewnętrzni. Tego typu incydenty są spowodowane np. zagubionymi lub skradzionymi urządzeniami, przypadkowymi błędami pracowników, a w środowisku biznesowym możemy nawet mówić o celowych i szkodliwych działaniach przez niektórych zatrudnionych. Ewolucja strategii BYOD powoduje to, iż dane firmowe są znacznie bardziej podatne na wyciek, dlatego też przy opracowywaniu polityk bezpieczeństwa, bardzo ważnym jest uwzględnienie zarówno wewnętrznych jak i zewnętrznych źródeł zagrożenia.

Rozwiązania bezpieczeństwa:
>Nie istnieje jedno rozwiązanie, które byłoby wstanie ochronić sieć przed wszystkimi istniejącymi zagrożeniami, dlatego też, zabezpieczenia powinny być implementowane w wielu warstwach, stosując przy tym więcej niż jeden system zabezpieczeń.

Metody ochrony:
- `oprogramowanie antywirusowe i wykrywające programy szpiegowskie` - ochrona użytkowników końcowych
- `zapora filtrująca` - blokuje nieautoryzowany dostęp do sieci (router firewall) lub systemu operacyjnego (pc firewall)
- `dedykowane zapory ogniowe` - pozwala filtrować duże ilości ruchu z większą dokładnością
- `ACL` - Access Control List, filtrowanie dostępu np. po MAC
- `IPS` - Intrusion Prevention System, identyfikacja różnego rodzaju zagrożeń, na podstawie reguł i podejrzanych zachowań
- `VPN` - Virtual Private Network, umożliwiają bezpieczny dostęp do sieci firmowej dla zdalnych pracowników  

>Dodatkowo, bezpieczeństwo musi być implementowane w taki sposób, aby mogło być skalowalne i gotowe na zmiany w ciągle zmieniających się trendach sieciowych.