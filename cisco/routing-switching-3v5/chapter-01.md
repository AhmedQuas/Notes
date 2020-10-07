### Spis treści
- [Chapter 1: Wprowadzenie do skalowania sieci](#chapter-1-wprowadzenie-do-skalowania-sieci)
  - [1.1 Implementacja projektu sieci](#11-implementacja-projektu-sieci)
  - [1.2 Rozszerzanie sieci](#12-rozszerzanie-sieci)
  - [1.3 Dobór urządzeń sieciowych](#13-dobór-urządzeń-sieciowych)

# Chapter 1: Wprowadzenie do skalowania sieci

## 1.1 Implementacja projektu sieci

Projektując sieć trzba pamiętać o nie tylko o spełnieniu podstawowych podstawowych funkcji, trzeba wybiec bardziej w przyszłość i pamiętać o tym, aby sieć była:
- `skalowalna` - umożliwająca łatwą rozbudowę
- `o dużym stopniu dostępności` - odporna na awarie

Wymaganie stawiane siecią przedsiębiorstwa:
- obsługa krytycznych aplikacji
- obsługa sieci konwergentnych
- zapewniać scentralizowaną kontrolę administracyjną
- wspierać różnorodne potrzeby biznesowe
- ograniczanie domen awarii

Uzyskanie odpowiedniego poziomu niezawodności zapewnia stosowanie wysokiej jakości sprzętu, nadmiarowości, zapasowe zasilanie UPS,

>Optymalizację przepustowości w sieci można uzyskać przez odpowiednie zorganizowanie ruchu tak, aby nie był przesyłany do tych segmentów, do których nie musi lub nie powinien trafić.

Sposobem zapewnienia odpowiedniej organizacji sieci jest `trój warstwowy hierarchiczny model sieci`. Składa się on z następujących warst:
- `dostępu` - zapewnia łączność użytkownikom końcowym
- `dystybucji` - przesyła ruch z jednej sieci lokalnej do innej
- `rdzenia` - sieć szkieletowa o wysokich prędkościach, łączy rozproszone sieci

W mniejszych przedsiębiorstwach może zostać wykorzystany model 2-poziomowy:
- `warstwa dostępu`
- `zmarginalizowana warstwa szkieletowa` - połączenie warstwy rdzenia i dystrybucji

Zastosowanie takiego modelu pozwala zmniejszyć złożoność sieci i obniżyć koszty sieci.

Architektura Cisco Enterprise wprowadza podział sieci na funkcjonalne komponenty, przy zachowaniu struktury warstwowej, w jej skład wchodzą:
- `Kampus przedsiębiorstwa` - Enterprise Campus, tu mamy te nasze wcześniejsze warstwy, dodatkowo może zawierać moduł farmy serwerów, centrum danych czy moduł usług(VoIP)
- `Urzadzenia brzegowe przedsiębiorstwa` - Enterprise Edge, odpowiadają za połączenie sieci przedsiębiorstwa z siecią ISP, obsługa VPN do zdalnych lokacji, QoS i bezpieczeństwo
- `Granica ISP` - Service Provider Edge, zapewnia dostęp do Internetu, telefonie PSNT, usługi WAN
- `Moduły zdalne`

`Domena awarii` - obszar, w którym sieć jest narażona na uszkodzenia w przypadku gdy wystąpią problemy z urządzeniem sieciowym lub usługą sieciową. Ograniczamy je stosując wysokiej klasy urządzenia i łącza nadmiarowe. Mniejsze domeny nie obniżają drastycznie produktywności firmy i ułatwiają analizę i rozwiązywanie błędów 

## 1.2 Rozszerzanie sieci

>W hierarchicznym modelu projektowania, `najłatwiej` i stosunkowo najtaniej jest `kontrolować rozmiar domeny awarii w warstwie dystrybucji`. W warstwie dystrybucji, błędy sieci mogą ograniczać się do mniejszych obszarów, a co za tym idzie, do mniejszej liczby użytkowników. Gdy w warstwie dystrybucji stosujemy urządzenia warstwy 3, każde z nich pełni funkcję routera (bramy domyślnej) dla ograniczonej liczby użytkowników w warstwie dostępu.

Podstawowa strategia projektowania sieci:
- używaj sprzętu modularnego lub urządzeń połączonych w klastry(kilka urządzeń pracujących jako jedno urządzenie), tak aby mogły być w prosty sposób rozbudowane
- projekt oprzyj o hierarchiczny model sieci, który pozawala na łatwe dodawanie, aktualizowanie, modyfikowanie bez wpływu na pozostałe obszary funkcjonalne sieci
- adresacja IPv4 i IPv6 spełniająca założenia sieci hierarchicznej, eliminując w ten sposób konieczność ponownej adresacji
- korzystajmy z przełączników L3, zmniejszają domeny rozgłoszeniowe, filtrują i ograniczają ruch do warstwy rdzenia sieci
- połączenia nadmiarowe redundancja ma na celu zminimalizowanie ryzyka wystąpienia pojedynczego punktu awarii. Metody tworzenia nadmiarowości:
  - duplikacja urządzeń
  - tworzenie nadmiarowych tras - dodatkowe fizyczne połączenia między urządzeniami. Uwaga tutaj mogą powstać zapętlenia w 2 warstwie, rozwiązaniem problemu może być zastosowanie protokoły STP - blokowanie nadmiarowych ścieżek do momentu, w którym będzie potrzebna np z powodu awarii ścieżki głównej.
- łącza wielokrotne - EtherChannel, połączenia agregowane lub rozkładanie obciążenia na równe trasy
- połączenia bezprzewowdowe, jako rozszerzenia funkcjonalności sieci
- wykorzystywanie skalowalnych protokołów routingu, mniejsze rozmiary tablic routingu, ograniczanie przesyłania aktualizacji routingu

`Zwiększanie przepustowości` - eliminacja wąskich gardeł, poprzez stosowanie łączy o dużej przepustowości, bądź wykorzystanie technologii EtherChannel, pozwalającej na agregację kilku łączy fizycznych w jedno logiczne

## 1.3 Dobór urządzeń sieciowych

Kategorie przełączników przeznaczonych dla firm:
- `kampusowe LAN` - skalowanie sieci, mogą być stosowane w każdej warstwie, seria  Cisco 2960, 3560, 3750, 3850, 4500, 6500, oraz 6800.
- `zarządzane z chmury` - cloud-managed switches, Cisco Meraki umożliwiają grupowanie urządzeń w wirtualne stosy, konfiguracja i monitorowanie tysięcy portów przełączników poprzez sieć WWW, bez konieczności interwencji personelu
- `dla centrum danych` - zapewniają skalowalność, ciągłość działania i elastyczną przesyłania danych, do tej kategorii należą przełączniki serii Cisco Nexus oraz Cisco Catalyst 6500
- `dla dostawców usług, ISP`  
  - przełączniki agregacyjne - zbierają cały ruch na granicy sieci
  - przełączniki dostępowe dla Ethernet - inteligentne aplikacje, zunifikowane usługi, wirtualizację, zintegrowane zabezpieczenia i uproszczone zarządzanie
- `sieci wirtualne` - platformy sieci wirtualnych Cisco Nexus, wirtualna inteligencja

Główne czynniki, które mają wpływ na wybór typu przełącznika:
- koszt
- gęstość portów - ilość dostępnych portów przypadających na jeden przełącznik, uwzglęnij kwestię zatorów na łączach uplink
- moc
- niezawodność
- szybkość portu, szybkość przekazywania ramek, przepustowość
- bufory ramek
- skalowalność

> W warstwie dostępu można zastosować tańsze przełączniki, o niższych parametrach, natomiast w warstwach rdzenia i dystrybucji powinny być stosowane przełączniki droższe, z lepszymi parametrami, w których szybkość przekazywania ma większy wpływ na wydajność sieci.

>`PoE` - Power over Ethernet, technologia umożliwiająca zasilanie urządzeń bezpośrednio za pomocą okablowania sieci Ethernet. Żółty kolor obramówki interfejsu to PoE?

Przełączniki wielowarstwowe - przełączniki warstwy 3 = tradycyjny przełącznik + możliwość tworzenia tablic routingu i obsługa niektórych protokołów routingu

Rola i wymagania stawiane routerom:
- zapewniają połączenie do Internetu przez ISP
- tłumaczą sygnał między różnymi rodzajami mediów np. Ethernet a Serial
- przekazywanie pakietów do innych sieci
- wybór trasy altenatywnej
- zmniejszają ruch rozgłoszeniowy
- łączą ze sobą sieci znajdujące się w odległych lokalizacjach
- zapewniają większe bezpieczeństwo
- grupują logicznie użytkowników np. działy i zasoby do których muszą mieć dostęp

Kategorie routerów:
- `dla oddziałów` - branch routers
- `brzegowe`
- `dostawców usług sieciowych`

Routery podbnie jak przełączniki mogą występować w budowie stałej i modułowej.

Sposoby zarządzania urządzeniami Cisco:
- w paśmie, wewnątrz-pasmowe, wymaga co najmniej jednego działającego interfejsu sieciowego urządzenia
  - Telnet
  - SSH
  - HTTP
- poza pasmem, zewnętrzno-pasmowe
  - port konsoli
  - AUX
  - klient terminala


Router, polcenie show:
- związane z routingiem
  - show ip protocols
  - show ip route
  - show ip ospf neighbour
- związane z interfejsami
  - show interfaces
  - show ip interfaces
  - show ip interface brief
  - show protocols
- inne
  - show cdp neighbour

Switch, polecenie show:
- show port-security
- show port-security address
- show interfaces
- show mac-address-table
- show cdp neighbour

---

<div>
<a href="../routing-switching-2v5/chapter-11.md">Prev: Chapter 2.11</a>
</div>
<div align="right">
<a href="chapter-02.md">Next: Chapter 2</a>
</div>