### Spis treści
- [Chapter 1: Hierarchiczne podejście do projektowania sieci](#chapter-1-hierarchiczne-podejście-do-projektowania-sieci)
  - [1.1 Przegląd hierarchicznego projektowania sieci](#11-przegląd-hierarchicznego-projektowania-sieci)
  - [1.2 Architektura Cisco Enterprise](#12-architektura-cisco-enterprise)

# Chapter 1: Hierarchiczne podejście do projektowania sieci

## 1.1 Przegląd hierarchicznego projektowania sieci

Trójwarstwowy hierarchiczny model sieci składa się z następujących warstw:
- `szkieletowej` - zapewnia szybki transport między przełącznikami dystrybucji w ramach kampusu korporacji, składa się z urządzeń sieciowych dużej prędkości. Rdzeń powinien być wysoce dostępny i nadmiarowy. Warstaw ta powinna zawierać:
  - szybkie przełączanie,
  - nadmiarowość i odporność na błędy,
  - skalowanie - szybszy sprzęt, bez zwiększania jego ilości
  - pakiety nie powinny być filtrowane pod kątem QoS, bezpieczeństwa i innych spraw, po prostu ma być to szybkie
  
- `dystrybucji` - zapewnia łączność opartą na polityce i kontroluje granice między warstwą dostępu i warstwą szkieletową, jest centralnym punktem w szafach kablowych, segmentuje grupy robocze i izoluje problemy sieciowe. Jeden przełącznik z warstwy dystrybucji może świadczyć usługi dla wielu przełączników z warstwy dostępu. Warstwa ta zapewnia:
  - agregację połączeń sieci LAN i WAN,
  - polityki bezpieczeństwa(ACL) i filtrowanie,
  - usługi routingu między sieciami LAN, VLAN i między domenami routingu(EIGRP, OSPF),
  - redundancję i równoważenie obciążenia,
  - kontrolę domeny rozgłoszeniowej, punkt demarkacyjny pomiędzy domenami rozgłoszeniowymi
  - granica agregacji i sumaryzacja trasu w uplink

- `dostępu` - zapewnia użytkownikom końcowym dostęp do sieci. Warstwa ta oferuje: 
  - przełączanie w warstwie 2, 
  - wysoką dostępność, 
  - zabezpieczenie portów, 
  - klasyfikacja QoS, 
  - kontrolę protokołu ARP, 
  - wirtualne listy kontroli dostępu(VACL), 
  - STP, 
  - PoE,
  - pomocnicze sieci VLAN i VoIP

>Korzyścią z dzielenia płaskiej sieci na mniejsze, łatwiejsze w zarządzaniu bloki jest to, że ruch lokalny pozostaje lokalnym. Tylko ruch przeznaczony dla innych sieci jest przenoszony do wyższej warstwy.

`Architektura zapadniętego szkieletu` - zmarginalizowana warstwa szkieletowa, rdzeń uproszczony(`collapsed core`) mode, w kótrym elementy warstwy szkieletowej i dystrybucyjnej są połączone w jednym przełączniku fizycznym.

>Główną przyczyną stosowania uproszczonego projektu rdzenia jest zmniejszenie kosztów sieci, przy jednoczesnym zachowaniu większości korzyści, jakie daje trójwarstwowy model hierarchiczny.

Podział sieci ze względu na liczbę urządzeń:
- mała sieć -> x < 200,
- sieć średniej wielkości -> 200 < x < 1000 urządzeń
- duża sieć -> x > 1000

`CCDA` - Cisco Certified Design Associate, certyfikat dla osób, które zajmują się projektowaniem podstawowych sieci kampusu, centrum danych, bezpieczeństwa, przenoszących głos i sieci bezprzewodowych.

Zasady projektowania sieci:
- `hierarchia` - dzielenie złożonego problemu na mniejsze części,
- `modułowość`,
- `odporność` - awarie, ataki, ekstremalne obciążenie ruchem,
- `elastyczność` - możliwość zmiany części sieci

## 1.2 Architektura Cisco Enterprise

>Modułowa konstrukcja sieci dzieli sieci na różne funkcjonalne moduły sieciowe, a każdy z nich jest nakierowany na konkretne miejsce lub cel w sieci.

Zalety podejści modułowego:
- usterki występujące w module mogą być oddzielone od pozostałej części sieci(większa dostępność i szybsze wykrywanie problemów)
- zmiany w sieci, uaktualnienia mogą być wykonane stopniowo, w sposób kontrolowany
- moduł może być aktualizowany, zastąpiony przez inny moduł, który będzie pełnił taką samą rolę
- bezpieczeństwo realizowane w sposób modułowy pozwala na większą kontrolę bezpieczeństwa

>Modułowe podejście do projektowania sieci dzieli dalej trójwarstwową konstrukcję hierarchiczną, wyróżniając konkretne bloki lub obszary modułowe.

Podstawowe moduły sieci:
- `dostępowo-dystrybucyjny` - podstawowy element sieci kampusowej
- `usługi` - ogólny moduł zapewniający dostęp do usług: bezprzewodowe kontrolery(LWAPP), ujednolicone usługi komunikacyjne, bramy zabezpieczeń itp.
- `centrum danych` - farmy serwerów, moduł odpowiedzialny za zarządzanie i eksploatację wielu serwerów danych(przechowywanie i przetwarzanie)
- `urządzenia brzegowe korporacji` - moduł odpowiedzialny za połączenie Internetu z siecią WAN w korporacji 

Obszar funkcjonalny <=> moduł

Architektury Cisco Enterprise - model, który uwzględnia potrzebę modułowości w sieci, w ramach tej architektury wyróżnia się następujące moduły:
- `kampus przedsiębiorstwa` - połączenie wielu sieci LAN, opiera się na hierarchicznym modelu trójwarstwowym, moduł składa się z:
  - warstwy dostępowej,
  - warstwy dystrybucji,
  - warstwy szkieletowej,
  - wewnętrznej farmy serwerów(wewnętrzne serwery email, DNS, drukarki, pliki i aplikacje)
- `urządzenia brzegowe przedsiębiorstwa` - zapewniają dostęp do usług zewnętrznych:
  - sieci e-commerce i serwery,
  - łączność z Internetem i strefa DMZ,
  - zdalny dostęp i VPN,
  - WAN(MPLS, Metro Ethernet, SONET, SDH, Frame Relay, ATM, DSL i sieci bezprzewodowe)
- `granicę ISP` - urządzenia brzegowe dostawcy ISP:
  - ISP,
  - usługi WAN,
  - usługi PSNT - Public Switched Telephone Network 
- `moduły dodatkowe`:
  - centrum danych - pełni funkcje takie same jak data center kampusu, lecz jest połozony w odległym miejscu. Zapewnia odzyskiwanie danych i ciągłość pracy korporacji
  - oddział korporacji - zdalne odległe oddziały,
  - telepracownik korporacji - praca zdalna, pracownik mobilny, rozproszone geograficznie miejsca

Rodzaje połączeń z ISP:
- `single-homed` - połączenie pojedyncze do jednego ISP
- `dual-homed` - dwa lub więcej połączeń do jednego dostawcy ISP
- `multihomed` - połączenie do dwóch lub więcej ISP
- `dual-multihomed` - zdublowane połączenia do dwóch lub więcej ISP

Wyzwania i w trendy IT:
- `BYOD` - Bring Your Own Device,
- Praca grupowa online,
- Komunikacja wideo,
- Chmura obliczeniowa

Architektury, które mają sprostać nowym wymaganiom:
- `Cisco Borderless Network` - sieć bez granic, pozwala na połączenie się szybko i bezpiecznie do sieci korporacyjnej korzystając z BYOD. Architektura ta obejmuje: łączność bezprzewodową, routing, przełączanie, funkcje bezpieczeństwa. Architektura obejmuje następujące zestawy usług:
  - usługi klienckie,
  - usługi w sieci bez granic
- `Cisco Collaboration` - Architekura współpracy grupowej, pozwala na zwiększenie wydajności pracowników, składa się z następujących warstw:
  - `aplikacje i urządzenia` - WebEx, Telepresence,
  - `usługi do współpracy grupowej`
  - `infrastruktura sieciowa i grupowa`
- `Cisco Data Center/Virtualization` - Architektura centrów danych i wirtualizacji, obejmuje platformę sieciową, przetwarzanie, przechowywanie danych i wirtualizację. Składa się z następujących komponentów:
  - `Cisco Unified Management` - ujednolicone zarządzanie, zautomatyzowany system zarządzania zasobami zarówno fizycznymi jak i wirtualnymi
  - `Unified Fabric` - ujednolicona struktura, szybko skalowalna architektura sieci oraz bezpieczeństwo ruchu sieciowego
  - `Unified Computing` - modułowe i bezstanowe urządzenia obliczeniowe 

`TCO` - Total Cost of Ownership - całkowity koszt posiadania

>Cisco Unified Computing System (`Cisco UCS`) jest zbudowany z serwerów typu blade, serwerów montowanych w szafie, połączeń strukturalnych w ramach Unified Fabric oraz kart interfejsów wirtualnych (`VIC`).



---

<div>
<a href="../routing-switching-3v5/chapter-09.md">Prev: Chapter 3.9</a>
</div>
<div align="right">
<a href="chapter-02.md">Next: Chapter 2</a>
</div>