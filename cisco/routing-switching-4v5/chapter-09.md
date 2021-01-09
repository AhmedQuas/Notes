### Spis treści
- [Chapter 9: Rozwiązywanie problemów z siecią](#chapter-9-rozwiązywanie-problemów-z-siecią)
  - [9.1 Rozwiązywanie problemów przy użyciu podejścia systematycznego](#91-rozwiązywanie-problemów-przy-użyciu-podejścia-systematycznego)
  - [9.2 Rozwiązywanie problemów z siecią](#92-rozwiązywanie-problemów-z-siecią)

# Chapter 9: Rozwiązywanie problemów z siecią

## 9.1 Rozwiązywanie problemów przy użyciu podejścia systematycznego

>Dokumentacja sieci pozwala administratorom w sposób efektywny diagnozować i usuwać problemy z działaniem sieci, w oparciu o projekt sieci oraz oczekiwaną wydajność w normalnych warunkach pracy. Cała dokumentacja powinna być przechowywana w jednym miejscu, nie zapominając o przygotowaniu kopii i trzymaniu jej w innym miejscu.

Dokumentacja powinna zawierać:
- pliki konfiguracyjne urządzeń sieciowych,
- schemat topologii fizycznej i logicznej,
- podstawowy poziom wydajności, jako efekt monitorowania sieci mający na celu ustalenie poziomu odniesienia

Stan odniesienia:
- określenie natężenia podczas normalnej pracy sieci,
- określenie wzorców ruchu,
- określenie typów danych których zmiana nas interesuje,
- identyfikowanie ważnych urządzeń i portów,
- wyodrębnij cechy, które uznajesz za istotnie w kontekście działania sieci,
- określenie czasu trwania pomiaru stanu odniesienia,

Polecenia służące do zbierania danych:
- ping, traceroute, telnet,
- show {ip|ipv6} interface {brief},
- show {ip|ipv6} route,
- show cdp neighbor detail,
- show version,
- show interfaces,
- show arp,
- show ipv6 neighbors
- show running-config,
- show port,
- show vlan,
- show tech-support - wykonuje wiele poleceń show,
- show ip cache flow

W dużych sieciach te operacje się automatyzuje za pomocą specjalnego oprogramowania np `Fluke Network SuperAgent`.

Etapy rozwiązywania problemów:
1. Zbieranie objawów
   - zbieranie informacji
   - określanie właściciela
   - zawężenie zakresu badań
   - zebrania symptomów z podejrzanych urządzeń
   - dokumentacja objawów   
2. Wyizolowanie problemów
3. Wdrożenie czynności naprawczych - czy rozwiązywać problem teraz, czy może lepiej nie w czasie największego natężenia ruchu, budowa dróg zastępczych na czas naprawy, krytyczność awarii
4. Określenie czy problem został rozwiązany
5. Udokumentowanie przyczyny i sposobu rozwiązania problemu

Izolowanie problemu z wykorzystaniem modelu warstwowego polega na porównanie cech charakterystycznych usterki do logicznych warstw sieci w celu wyizolowania i rozwiązania problemu.

Routery i przełączniki wielowarstwowe czasami są zaliczane do warstwy 4 ze względu na obsługę ACL.

Metody rozwiązywania problemów z sieciami w oparciu o model warstwowy:
- `bottom-up` - wstępująca, najczęściej problemy występują w dolnych warstwach, dużo urządzeń do sprawdzenia, dużo papierkowej roboty,
- `top-down` - zstępująca, rozpoczynamy od sprawdzenia każdej aplikacji sieciowej,
- `divide et impera` - dziel i rządź - wybieramy jedną z warstw i sprawdzamy czy wszyskie pod nią działają
- `shoot-from-the-hip` - losowa, rozwiązywanie problemów w drodze dedukcji bazując na objawach,
- `spot-the-difference` - rozpoznawanie różnic, między produkcją, a środowiskiem testowym
- `move-the-problem` - podmiana, tymczasowa zmiana problematycznego urządzenia na inne, działające prawidłowo. Jeśli awaria zniknie, oznacza to że problem był w usuniętym urządzeniu.

## 9.2 Rozwiązywanie problemów z siecią

Narzędzia:
- `NMS` - Network Management System, klasa systemów umożliwiających monitorowanie na poziomie urządzenia, konfiguracji oraz zarządzania usterkami np. WhatsUp Gold, HPBTO, SolarWinds
- bazy wiedzy, google, dokumentacje, fora i Tools & Resources
- narzędzia do tworzenia stanu odniesienia i automatycznej dokumentacji np. SolarWinds LANsurveyor i CyberGauge
- analizatory protokołó na hostach - Wireshark, Cisco IOS EPC
- rozwiązania sprzętowe:
  - `NAM`, Network Analysis Module, modułowy analizator sieci, urządzenie pokazujące ruch wychodzący z lokalnych i zdalnych przełączników, routerów w formacie graficznym
  - `multimetry` - mierzy wartości elektryczne natężenia prądu, napięcia i rezystancji
  - `testery kabli` - testują transmisję danych w kablach w celu znalezienia uszkodzonych przewodów, skrzyżowanych przewodów i zwarć 
  - reflektometry TDR, optyczny reflektometr(OTDR)
  - `analizator okablowania` - testuje i certyfikuje kable miedziane i światłowodowe, przeznaczone dla różnych usług i standardów za pomocą urządzenia przenośnego
  - `przenośne analizatory sieci` - wykrywanie konfiguracji VLAN, średnie i szczytowe wykorzystanie pasma za pomocą urządzenia przenośnego
- wykorzystanie serwera Syslog

Objawy i przyczyny problemów z siecią:
- `warstwa fizyczna`:
  - objawy:
    - wydajność niższa od poziomu odniesienia,
    - utrata łączności,
    - zatory lub przeciążenia w sieci,
    - wysokie użycie procesora,
    - komunikaty błędów

  - przyczyny:
    - problemy z zasilaniem,
    -  usterki sprzętowe - Jabber
    -  błędy okablowania - poluzowane okablowanie,
    -  tłumienie - zbyt długi kabel nie spełniający norm,
    -  szum - zakłócenia EM, EMI,
    -  błędy konfiguracji interfejsu,
    -  przekroczenie limitów projektowych,
    -  przeciążenie CPU

    >Jabber jest często określany jako stan, w którym urządzenie sieciowe wysyła w sposób ciągły losowe, nic nie znaczące dane do sieci. Do przyczyn stanu jabber można zaliczyć na przykład niewłaściwe lub uszkodzone pliki sterownika, nieprawidłowe okablowanie lub problemy z uziemieniem.

- `warstwa łącza danych`:
  - objawy:
    - nieprawidłowe funkcjonowanie lub brak łączności w warstwie sieci lub wyższej,
    - wydajność sieci poniżej poziomów odniesienia:
      - ramki docierają do celu, ale droga nie jest optymalna,
      - odrzucanie niektórych ramek
    - nadmierne rozgłoszenia - duże domeny rozgłoszeniowe, pętle STP, migotanie trasy(naprzemienne stany włączenia i wyłączenia)
    - komunikaty konsoli - problemy z interpretacją ramki, brak ramek podtrzymania aktywności(keepalive), komunikat o nieaktywnym protokole łącza(line protocol down)
  - przyczyny:
    - błędy enkapsulacji - różne rodzaje enkapsulacji między urządzeniami,
    - błędy mapowania adresu - arp spoofing, zmiana adresów mac, a w cache są informacje nieaktualne,
    - błędy ramek - zakłócenia na łączu, zła konfuguracja zegara, nieprawidłowo zaprojektowane kable,
    - błędy i pętle STP

- `warstwa sieci`:
  - objawy:
    - awaria sieci,
    - wydajność poniżej optimum
  - przyczyny:
    - ogólne problemy z siecią,
    - problemy z łącznością - problemy ze sprzętem, łącznością, ISP i zasilaniem,
    - problem z ustanowieniem sąsiedztwa,
    - baza danych topologii,
    - tablica routingu

- `warstwa transportowa`:
  - ACL:
    - wybór strumienia ruchu - nieprawidłowe rozmieszczenie ACL(interfejs i kierunek ruchu)
    - kolejność wpisów kontroli dostępu - od najbardziej szczegółowych do najbardziej ogólnych,
    - domyślny (niejawny) wpis blokady całego ruchu,
    - adresy i maski blankietowe
    - wybór protokołu warstwy transportowej,
    - porty źródłowe i docelowe,
    - użycie słowa kluczowego `established`,
    - rzadko uzywane protokoły,
  - NAT:
    - DHCP i BOOTP,
    - DNS i WINS,
    - SNMP,
    - protokoły tunelowania i szyfrowania

      Rozwiązanie problemów często ogranicza się do odpowieniej konfiguracji DHCP relay i odpowiedniego przekierowania portów

- warstwa aplikacji:
  - SSH/Telnet
  - HTTP(S),
  - FTP, TFTP,
  - SMTP, POP3,
  - SNMP,
  - DNS
  - NFS - Network File Systme

Dodatkowe polecenia przydatne podczas rozwiązywania problemów:
- show process cpu,
- show memory,
- show interfaces

>Mimo, jak powszechnie wiadomo, usługa serwera telnet nasłuchuje na dobrze znanym porcie 23 a programy klienckie telnet łączą się na tym porcie domyślnie, można wykorzystać je (programy klienckie) do połączeń na dowolnym innym porcie TCP, który chcemy przetestować, po prostu podając ten numer portu. W ten sposób możemy uzyskać informację, czy połączenie jest nawiązane (o czym świadczy słowo "Open", pojawiające się w wyniku), zostało odrzucone (refused), czy też wygasło (timed out). 

>Niektóre aplikacje, używające protokołu sesji, w którym stosowane są znaki ASCII, mogą nawet wyświetlić odpowiedni banner, może być także możliwość wywołania pewnych odpowiedzi z serwera poprzez wpisanie odpowiednich poleceń; przykładami takich protokołów są SMTP, FTP i HTTP.

Lokalne mapowanie nazw na adresy

```
R1(config)# ip host R1 192.168.1.1 
```



---

<div>
<a href="chapter-08.md">Prev: Chapter 8</a>
</div>