### Spis treści
- [Chapter 6: Rozwiązania szerokopasmowe](#chapter-6-rozwiązania-szerokopasmowe)
  - [6.1 Praca zdalna](#61-praca-zdalna)
  - [6.2 Porównianie rozwiązań szerokopasmowych](#62-porównianie-rozwiązań-szerokopasmowych)
  - [6.3 Przegląd PPPoE](#63-przegląd-pppoe)

# Chapter 6: Rozwiązania szerokopasmowe

## 6.1 Praca zdalna

Korzyści pracy zdalnej:
- dla pracodowacy:
  - poprawa wydajności pracowników
  - niższe koszty i wydatki
  - łatwiejsza rekrutacja i retencja kadr
  - ograniczone nieobecności
  - zwiększone morale pracowników
  - postawa obywatelska - ekologia, lepsza jakość usług
  - lepsza obsługa klienta
- społeczności i rządu:
  - zmniejszenie infrastruktury
  - zmniejsza migrację zarobkową
  - polepszenie warunków życia podmiejskiego
- indywidualne:
  - produktywność
  - elastyczność
  - oszczędności
  - więcej czasu dla domu i rodziny

Negatywne aspekty pracy zdalnej:
- ze strony pracodawcy:
  - śledzenie postępów pracowników
  - konieczność wdrożenia nowego systemu zarządzania
- ze strony pracownika:
  - poczucie izolacji
  - połączenia niskopasmowe
  - przeszkody i czynniki rozpraszające

Typy zdalnych połączeń:
- `łącza szerokopasmowe` - odnosi się do systemów komunikacyjnych umożliwiających transmisję usług tj.: dane, dźwięk i wideo. Transmisja odbywa się za pośrednictwem różnych technologii: DSL, światłowód, kable koncentryczne, sieć bezprzewodowa, łącza satelitarne itp.
- `IPsec VPN` - najpopularniejsze rozwiązanie dla telepracowników, umożliwia bezpieczne połączenia VPN poprzez Internet. Połączenia `site-to-site` są w stanie zapewnić bezpieczne, szybkie, elastyczne, skalowalne i  niezawodne połączenia
- `Tradycyjne technologie WAN warstwy drugiej` - rozwiązania zdalnego połączenia, którego bezpieczeństwo zależy od operatora, zaliczamy do nich takie technologie jak: Frame Relay, ATM i łącze dzierżawione

Wymagania dla pracy zdalnej:
- `komponenty biura domowego` - komputer, łącze szerokopasmowe(punkt dostępu sieci bezprzewodowej), router lub klient VPN
- `komponenty po stronie korporacji` - routery, koncentratory VPN, urządzenia wielofunkcyjne i urządzenia uwierzytelniające połączenia

>IPSec działa w warstwie sieci (a więc tam gdzie pakiety są przetwarzane).

>VPN można traktować jako prywatną sieć realizowaną w publicznej infrastrukturze.

Najpopularniejsze formy dostępu szerokopasmowego:
- łącze kablowe
- DSL
- bezprzewodowa sieć szerokopasmowa

## 6.2 Porównianie rozwiązań szerokopasmowych

`Łącze kablowe` - kable koncentryczne(podstawowe medium w sieciach telewizji kablowej), światłowodowe(sieć hybrydowa HFC),

Podział widma EM:
- `pobieranie` - kanały telewizyjne i dane(50 - 860 MHz)
- `wysyłanie` - 5 do 42 MHz

`DOCSIS` - Data-over-Cable Service Interface Specifiacation - określa wymagania dotyczące komunikacji i działania wsperanych interfejsów dla systemu danych na kablu(1 i 2 - MAC(TDMA,S-CDMA - kodowe, rozpraszanie widma) warstwa ISO/OSI).

Urządzenia wymagane do pracy w sieci kablowej:
- `CMTS` - Cable Modem Termination System, umieszczony w stacji czołowej operatora
- `CM` - Cable Modem - pełniące funkcję zakończenia abonenckiego

`DSL` - sposób na zapewnienie szybkich połączeń wykorzystując miedzianą infrastrukturę kablową(telefonia)

Podział widma w ADSL:
- POTS - rozmowy 300Hz - 3(w Polsce do 3.4)kHz
- upstream - 26 - 138 kHz
- downstream - 138 - 1100 kHz

Urządzenia wymagane do korzystania z DSL:
- `transceiver` - zwykle jest to modem DSL, łączy komputer z DSL
- `DSLAM` - znajduje się w CO(Central Office) łączy wiele połączeń od klientów w jedno duże zagregowane łącze, które jest podłączone daleje do Internetu

>Przewagą posiadaną przez DSL nad technologią kablową jest to, że DSL nie jest medium współdzielonym, gdyż każdy użytkownik ma oddzielne bezpośrednie połączenie z DSLAM. Dodawanie użytkowników nie utrudnia działania, chyba że połączenie Internetowe DSLAM do usługodawcy internetowego lub Internetu jest wysycone.

>Dostawca usług przy użyciu filtrów lub rozdzielaczy oddziela w siedzibie klienta kanał POTS od modemu ADSL. Taka konfiguracja gwarantuje nieprzerwane, regularne usługi telefoniczne nawet jeśli ADSL ulegnie awarii.

Sposoby rozdzielanie ADSL od głosu:
- `mikro filtr` - pasywny filtr dolnoprzepustowy
- `rozdzielacz POTS` - pasywne urządzenie znajdujące się w centrali operatora i czasem w siedzibie klienta

Bezprzewodowe sieci szerokopasmowe:
- `miejska sieć WiFi` - mesh
- `WiMAX` - Worldwide Interoperability for Microwave Access:
  - stacja nadawcza(podobna do tych znanych z telefonii komórkowych), może ona pokryć zasięgiem obszar o powierzchni 7500 km^2. Doskonałe rozwiązanie na obszarach miejskich poza zasięgiem lini kablowych i DSL
- `telefonia komórkową`
- `Internet satelitarny`

>Hotspot to obszar objęty jednym lub więcej punktem dostępowym.

## 6.3 Przegląd PPPoE

>Najczęściej używanym przez ISP protokołem L2 jest PPP (Point-to-Point).

Technologie wspierające PPP:
- analogowe modemy
- ISDN
- DSL

PPPoE - PPP over Etheret - umożliwia przesyłanie ramek PPP wewnątrz ramek Ethernet.

Konfiguracja PPPoE

>Aby pomieścić nagłówki PPPoE, maksymalna jednostka transmisji (MTU) powinna być ustawiona na 1492, w porównaniu z wartością domyślną 1500.

```
R1(config)# interface dialer 1
R1(config-if)# ip mtu 1492 #domyślne 1500
R1(config-if)# ip address negotiated
R1(config-if)# encapsulation ppp

R1(config-if)# dialer pool 1
R1(config-if)# ppp authentication chap callin
R1(config-if)# ppp chap hostname R1
R1(config-if)# ppp chap password r1-pppoe-password
R1(config-if)# no shutdown

R1(config)# interface g0/0
R1(config-if)# no ip address
R1(config-if)# pppoe enable
R1(config-if)# pppoe-client dial-pool-number 1
``` 

---

<div>
<a href="chapter-05.md">Prev: Chapter 5</a>
</div>
<div align="right">
<a href="chapter-07.md">Next: Chapter 7</a>
</div>