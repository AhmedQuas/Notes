### Spis treści
- [Chapter 7: Zabezpieczanie połączeń pomiędzy lokalizacjami](#chapter-7-zabezpieczanie-połączeń-pomiędzy-lokalizacjami)
  - [7.1 Witualne sieci prywatne](#71-witualne-sieci-prywatne)
  - [7.2 Tunele GRE](#72-tunele-gre)
  - [7.3 Wprowadzenie do IPsec](#73-wprowadzenie-do-ipsec)
  - [7.4 Zdalny dostęp](#74-zdalny-dostęp)

# Chapter 7: Zabezpieczanie połączeń pomiędzy lokalizacjami

## 7.1 Witualne sieci prywatne

>Sieć VPN jest to prywatna sieć utworzona poprzez tunelowanie w sieci publicznej, najczęściej Internecie.

Pierwsze sieci VPN były związane z tunelami IP bez uwierzytelniania i szyfrowania danych, np opierały się na tunelu GRE. Obecnie stosuje się bezpieczniejsze rozwiązanie np IPsec VPN.

>`Cisco ASA` - Adaptive Security Appliance,samodzielne urządzenie, które łączy w jednym obrazie oprogramowania zaporę firewall, koncentrator sieci VPN oraz funkcje zapobiegania włamaniom.

Zalety VPN:
- oszczędności,
- skalowalność,
- kompatybilność z technologiami szerokopasmowymi,
- bezpieczeństwo

Rodzaje sieci VPN:
- `site-to-site` - statycznie skonfigurowane, łączy całe sieci, hosty wewnętrzne nie wiedzą, że sieć VPN istnieje. Hosty wysyłają normarny ruch TCP/IP przez "bramę VPN", które enkapsuluje i szyfruje ruch. Ten rodzaj sieci jest klasycznym rozszerzeniem sieci WAN(połączenia dzierżawione, Frame Relay)
- `zdalnego dostępu` - dynamicznie konfigurowane(włączane/wyłączane), wspierają architekturę klient/serwer, są używane do łączenia poszczególnych hostów, które muszą uzyskać bezpieczny dostęp do sieci firmy przez Internet. Wymagana jest odpowiednia konfiguracja serwera VPN i klienta VPN.
    
    >Zaszyfrowane dane są wysyłane przez Internet do bramy VPN na brzegach sieci docelowej. W momencie otrzymania ruchu, brama VPN zachowuje się tak, jak dla sieci VPN typu site-to-site.

## 7.2 Tunele GRE

`GRE` - Genric Routing Encapsulation, rozwiązanie Cisco, protokół, który może enkapsulować wiele różnych typów pakietów warstwy sieci wewnątrz tuneli IP. Tworzy on wirtualne połączenie typu punkt-punkt do routerów Cisco w odległych punktach w sieci IP.

Jest to  przykład podstawowego, niezabezpieczonego protokołu tunelowania sieci VPN typu site-to-site.

```
+----+-----+----+-----+------+
| IP | GRE | IP | TCP | Data |
+----+-----+----+-----+------+
           |Oryginalny pakiet|
           +-----------------+
```

Pole GRE składa się z pola:
- `flagi` - identyfikuje obecność opcjonalnych nagłówków 
- `typ protokołu` - określa rodzaj ładunku(EtherType) np 0x800 to IPv4 

Protokół transportowany(pasażer): IPv4, IPv6, AppleTalk, DECnet, IPX

Protokół transportujący(nośnik) np. GRE

>Protokół transportujący, taki jak IP, jest protokołem, który niesie ze sobą protokół transportowany

Cechy GRE:
- zdefinowany jako standard IETF(RFC 2784)
- w zewnętrznym nagłówku IP, wartość 47 jest stosowana w polu protokół, aby wskazać, że następny to nagłówek GRE
- enkapsulacja wykorzystuje pole typu protokołu w nagłówku GRE do wspierania enkapsulacji jakiegokolwiek protokołu OSI warstwy 3(rodzaje protokołów - RFC 1700, Ether Types)
- jest bezstanowy, domyślnie nie posiada żadnych mechanizmów kontroli przepływu
- nie zawiera silnych mechanizmów bezpieczeństwa w celu ochrony danych
- nagłówek dodaje co najmniej 24 bajty dodatkowego narzutu
- pozwala na tunelowanie ruchu multicast

Konfigurowanie tunelu GRE

>GRE jest używany do tworzenia tunelu VPN pomiędzy dwoma miejscami. W celu wprowadzenia tunelu GRE, administrator sieci musi najpierw poznać adresy IP miejsc docelowych. Po tym następuje właściwa konfiguracja tunelu GRE

```
R1(config)# interface Tunnel0
R1(config-if)# tunnel mode gre ip
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# tunnel source 209.165.201.1
R1(config-if)# tunnel destination 198.133.219.87

R1(config)# router ospf 1
R1(config)# network 192.168.10.0 0.0.0.255 area 0
```

```
R2(config)# interface Tunnel0
R2(config-if)# tunnel mode gre ip #tunnel type gre over ip
R2(config-if)# ip address 192.168.10.2 255.255.255.0
R2(config-if)# tunnel source 198.133.219.87
R2(config-if)# tunnel destination 209.165.201.1

R2(config)# router ospf 1
R2(config)# network 192.168.10.0 0.0.0.255 area 0
```

`ip address` - adres związany z interfejsem tunelowym, odnosi się do sieci IP specjalnie stworzonej na potrzeby tunelu GRE
`tunnel {source|destination}` - adres powiązany z fizycznie skonfigurowanymi interfejsami

>Minimalna konfiguracja wymaga określenia źródła tunelu i adresów docelowych. Także podsieć IP musi być skonfigurowana tak, aby zapewnić łączność IP w całym połączeniu tunelowym.

Weryfikacja GRE:
- show ip interface brief | include Tunnel
- show interface Tunnel 0
- show ip ospf neighbor
- 

>Protokół liniowy na interfejsie tunelu GRE jest włączony tak długo, jak istnieje trasa do miejsca przeznaczenia tunelu. Przed wprowadzeniem tunelu GRE, musi istnieć połączenie IP między adresami IP fizycznych interfejsów na przeciwnych końcach potencjalnych tuneli GRE.

>GRE jest uważany za sieć VPN, ponieważ jest to prywatna sieć, która jest utworzona przez tunelowanie w sieci publicznej.

## 7.3 Wprowadzenie do IPsec

>Sieci VPN zawierające protokół IPsec oferują elastyczne i skalowalne połączenia. Połączenia typu site-to-site są w stanie zapewnić bezpieczne, szybkie i niezawodne połączenia zdalne. Stosując sieci IPsec VPN, informacje z sieci prywatnej są bezpiecznie transportowane przez sieć publiczną.

IPsec - zbiór otwartych standardów, które określają zasady bezpiecznej komunikacji. Opiera się na istniejących algorytmach nie jest związany z konkretnymi rozwiązaniami dotyczącymi szyforwania, uwierzytelniania, kluczowania i innymi technologiami bezpieczeństwa.

>IPsec pracuje w warstwie sieci, chroniąc i uwierzytelniając pakiety IP pomiędzy uczestniczącymi urządzeniami IPsec, zwanymi także jako `peerami`. Protokół IPsec zabezpiecza trasę pomiędzy parą bram, parą hostów lub pomiędzy bramą i hostem. W rezultacie, protokół ten może chronić praktycznie cały ruch aplikacji, ponieważ ochrona może być realizowana od warstwy 4 do warstwy 7.

>Wszystkie implementacje protokołu IPsec posiadają nagłówek warstwy 3. zapisany zwykłym tekstem, a więc nie występują problemy z routingiem. IPsec działa we wszystkich protokołach warstwy 2, takich jak Ethernet, ATM lub Frame Relay.

Cechy IPsec:
- struktura otwrtych standardów, niezależnych rozwiązań
- zapewnia:
  -  poufność(`szyfrowanie`), stopień ochrony zależy od długości klucza algorytmu kodowania oraz złożoności tego algorytmu.
  -  integralność(`sumy kontrolne`)
  -  uwierzytelnienie źródła(`IKE` - Internet Key Exchange: login, hasło, `OTP`, biometria, klucz współdzielony(`PSK`), certyfikaty cyfrowe, `DSA`),
  -  `zabezpiecza przed powtarzaniem`(porównanie liczby sekwencji odebranych pakietów z przesuwanym oknem na hoście docelowym lub bramie bezpieczeństwa)
- działa w warstwie sieciowej, chroniąc i uwierzytelniając pakiety IP

Zbiór protokołów IPsec

Szyfrowanie:
- symetryczne:
  - kryptografia klucza symetrycznego,
  - wymagają współdzielonego tajnego klucza(szyfrowanie i deszyfrowanie)
  - zazwyczaj używane do szyfrowania wiadomości,
  - DES, 3DES, AES
- asymetryczne, szyfrowanie z kluczem tajnym:
  - wykorzystuje kryptografię klucza publicznego,
  - szyfrowanie i deszyfrowanie wiąże się z użyciem innego klucza
  - stosowane w certyfikacji cyfrowej i zarządzaniu kluczami
  - RSA

>Największe bezpieczeństwo dla szyfrowania IPsec sieci VPN pomiędzy urządzeniami Cisco zapewnia 256-bitowa wersja algorytmu AES. Ponadto zostały złamane 512-bitowe i 768-bitowe klucze RSA (Rivest-Shamir-Adleman), a firma Cisco zaleca stosowanie kluczy 2048-bitowych z opcją RSA podczas stosowania w fazie uwierzytelniania IKE.

Algorytm Diffie-Hellman(DH, obecnie część IPsec) pozwala obu stronom utworzyć współdzielony tajny klucz, który jest wykorzystywany do szyfrowania i algorytmów funkcji skrótu. Algorytm DH określa metody wymiany kluczy publicznych, które zapewniają sposób ustalenia współdzielonego tajnego klucza dla dwóch stron, który znają tylko one, mimo że komunikują się ze sobą przez niezabezpieczony kanał.

>Również protokół znany jako OAKLEY korzysta z algorytmu DH. OAKLEY jest używany przez protokół IKE, który jest częścią ogólnej struktury o nazwie Internet Security Association and Key Management Protocol (ISAKMP).

Algorytmy kodu HMAC:
- MD5
- SHA

>Kod uwierzytelniania wiadomości z kluczem tajnym (HMAC) (Hash-based Message Authentication Code) jest mechanizmem uwierzytelniania wiadomości przy użyciu funkcji skrótu. Kluczowany HMAC jest algorytmem spójności danych, który gwarantuje integralność wiadomości.

Główne grupy protokołów IPsec:
- `AH` - Authentication Header, wykorzystywany gdy poufność(szyfrowanie) nie jest wymagana. Zapewnia uwierzytelnienie i integralność danych. Wiadomość jest transportowana w postaci tekstu jawnego, otwartego(clear text)
- `ESP` - Encapsulation Security Payload, poufność, uwierzytelnienie przez szyfrowanie, integralność. ESP uwierzytelnia wewnętrzny pakiet IP i nagłówek ESP

Zbiór protokołów na które składa się IPsec:
- `protokoł IPsec`{ AH|ESP|ESP+AH }
- `poufności`(jeśli wybrano ESP) { DES|3DES|AES|SEAL }
- `integralności` { MD5|SHA }
- `uwierztelnienie` { PSK|RSA }
- `Diffie-Hellman` { DH1|DH2|...|DH24 }

>IPSec jest to połączeniem tych cegiełek, które zapewniają poufność, integralność i opcje uwierzytelniania dla sieci VPN.

## 7.4 Zdalny dostęp

Sposoby wdrażania sieci VPN zdalnego dostępu:
- `SSL` - Secure Socket Layer, pozwala na łączność z dowolnego miejsca nie tylko z zasobów zarządzanych przez firmę, ale także z komputerów należących do pracowników. Rozwiązanie Cisco SSL VPN oferuję następujące funkcje:
  - dostęp oparty o sieć Web bez oprogramowania klienckiego, dostęp do sieci bez zainstalowanego oprogramowania stacjonarnego
  - ochrona przed wirusami, robakami, programami szpiegującymi i hakerami na połączeniach sieci VPN przez integrację sieci i bezpieczeństwa punktów końcowych na platformie Cisco SSL VPN
  - zastosowanie jednego urządzenia dla sieci SSL VPN i IPsec VPN

  >`Cisco IOS SSL VPN` to technologia, która umożliwia zdalny dostęp za pomocą przeglądarki internetowej oraz natywne szyfrowanie SSL w przeglądarce internetowej. Ewentualnie można zapewnić zdalny dostęp przy użyciu oprogramowania `Cisco AnyConnect Secure Mobility Client`.

  Rozwiązania bazujące na Cisco ASA:
  - `Cisco AnyConnect Secure Mobility Client z SSL`, wymaga klienta Cisco AnyConnect Client
  - `Cisco Secure Mobility Clientless SSL VPN`, wymaga przeglądarki internetowej, urządzenie klienckie nie musi być zarządzane przez korporację

- `IPsec` - IP security oparte o rozwiązania Cisco Easy VPN, które:
  - negocjuje parametry tunelu,
  - ustanawia tunele według ustawionych parametrów,
  - uwierzytelnia użytkowników poprzez nazwy uzytkowników, grup i hasła
  - zarządza kluczami bezpieczeństwa do szyfrowania i deszyfrowania,
  - uwierzytelnia, szyfruje i odszyfrowuje dane przez tunel
  
  Rozwiązanie Cisco Easy VPN składa się z:
  - `serwer Cisco Easy VPN` - pozwala na korzystanie z oprogramowania klienta VPN na komputerach do tworzenie bezpiecznych tuneli IPsec w celu uzyskania dostępu do intranetu
  - `Cisco Easy VPN Remote` - pozwala klientom oprogramowania na działanie jako zdalni klienci VPN. Urządzenia te mogą odbierać politykę bezpieczeństwa z serwera Cisco Easy VPN, minimalizując wymagania dotyczące konfiguracji sieci VPN w oddalonej lokalizacji. Idealne rozwiązanie dla oddalonych biur z niewielkim wsparciem IT, lub w miejscach gdzie niepraktyczne jest indywidualne skonfigurowanie wielu zdalnych urządzeń
  - `klient Cisco Easy VPN` - pozwala organizacjom na ustalenie szyfrowanych tuneli sieci VPN typu end-to-end dla bezpieczenej łączności pracowników

>Po tym, gdy oddalony klient zainicjuje połączenie tunelu VPN, serwer Cisco Easy VPN `wypycha politykę IPsec do klienta`, minimalizując wymagania konfiguracyjne w oddalonej lokalizacji.

IPsec przewyższa SSL w:
- liczbie aplikacji, które wspiera,
- mocy szyfrowania,
- mocy uwierzytelniania,
- ogólnym bezpieczeństwie

>Gdy problemem jest bezpieczeństwo, IPsec jest najlepszym wyborem. Jeśli podstawowymi kwestiami jest wsparcie i łatwość wdrożenia, należy rozważyć SSL.

>IPsec i SSL są komplementarne, ponieważ rozwiązują różne problemy. W zależności od swoich potrzeb, organizacja może wdrożyć jeden protokół lub oba na raz.

---

<div>
<a href="chapter-06.md">Prev: Chapter 6</a>
</div>
<div align="right">
<a href="chapter-08.md">Next: Chapter 8</a>
</div>