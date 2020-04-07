### Spis treści
- [Chapter 9: Podział sieci na podsieci](#chapter-9-podzia%c5%82-sieci-na-podsieci)
  - [9.1  Zalety podziału na podsieci](#91-zalety-podzia%c5%82u-na-podsieci)
  - [9.2 Schematy adresowania](#92-schematy-adresowania)
  - [9.3 Podział sieci IPv6 na podsieci](#93-podzia%c5%82-sieci-ipv6-na-podsieci)

# Chapter 9: Podział sieci na podsieci

## 9.1  Zalety podziału na podsieci

`Sieć płaska` - wszystkie urządzenia sieciowe są podłączone w ramach jednej sieci IP.

Segmentowanie sieci, poprzez podział na mniejsze przestrzenie adresowe jest nazywany podziałem na podsieci.

Stosując podział na podsieci ograniczamy rozmiar domeny rozgłoszeniowej, a otrzymany ruch przetwarza mniej urządzeń. Podział ten może być dokonany ze względów:
- `geograficznych`
- `jednostkę organizacyjną`
- `typu urządzeń` (serwery, drukarki)

>`Uwaga:` Podsieć jest odpowiednikiem sieci i określenia te mogą być używane zamiennie. Większość sieci jest podsiecią, jakiegoś większego bloku adresowego.

>Urządzenia znajdujące się w tej samej podsieci muszą wykorzystywać odpowiedni dla danej podsieci adres, maskę podsieci oraz bramę domyślną.

Określenie rozmiaru podsieci wymaga podania liczby hostów, które będą się w niej znajdowały, czli innymi słowy określenia wymaganej liczby unikalnych adresów IP.

Mając określoną liczbę wszystkich hostów, które będą pracowały we wszystkich podsieciach możemy określić z jakiej przestrzeni adresowej adresów prywatnych możemy skorzystać.

Dobrą praktyką jest też spójna adresacja np:
- pierwszy możliwy adres hosta to adres bramy domyślnej
- ostatnie możliwe w podsieci to adresy adresy statyczne: drukarki, serwery itp.

>Najczęściej `adres routera` jest najniższym lub najwyższym użytecznym adresem w sieci.

>Podsieci IPv4 są tworzone poprzez użycie jednego lub większej ilości bitów z części hosta jako bitów części sieci. Realizowane jest to poprzez `wydłużenie maski` dzięki pożyczeniu bitów z części hosta adresu IP w celu utworzenia dodatkowych bitów dla części sieci. Im więcej bitów z części hosta jest pożyczonych, tym więcej podsieci możne zostać zdefiniowanych. Każdy następny pożyczony bit oznacza podwojenie liczby dostępnych podsieci.

>Bity mogą być pożyczone jedynie z części hosta adresu IP. Część sieci adresu IP jest przydzielana przez dostawcę usług i nie może być zmieniona.

Liczba możliwych podsieci 2^n (n->liczba pożyczonych bitów)

Liczba możliwych hostów 2^n-2 (n-> liczba bitów pozostających w części hosta), zawsze odchodzi nam adres sieci i adres rozgłoszeniowy.

Tradycyjny podział sieci na podsieci o stałej długości jest niezbyt dobry dla sieci, o różnej ilości hostów. Szczególnie widać to w momencie tworzenia podsieci między dwoma routerami.

Ten sposób adresowania powoduje znacznie marnowanie części dostępnej puli adresowej, poprzez niewykorzystanie często dużej części adresów.

>`Nieefektywne wykorzystanie adresów`, jest cechą charakterystyczną dla tradycyjnego podziału klasowych sieci na podsieci.

W celu uniknięcia marnowania adresów wprowadzono podział wykorzystujący zmienną długość maski, `VLSM` - Variable Length Subnet Mask.

## 9.2 Schematy adresowania

Podczas planowania zasad przydzielania adresów należy rozważyć następujące aspekty:
- `zapobieganiu duplikacji adresów`
- `udostępnianiu usług oraz sprawowaniu nad nimi kontroli dostępu`
- `monitorowaniu bezpieczeństwa oraz kontroli wydajności`

Co należy uwzględnić podczas tworzenia schematu adresacji:
- `adresacja klientów`
- `adresy dla serwerów i urządzeń peryferyjnych`
- `adresy hostów, które są osiągalne z Internetu`
- `adresy urządzeń pośredniczących`
- `adresacja bram sieciowych i firewall'i`

## 9.3 Podział sieci IPv6 na podsieci

Podział sieci IPv6 jest realizowany w celach logicznego podziału sieci, zachowania pewnej hierarchii. Tutuj nie musimy się matrwić o marnowanie przestrzeni adresowej.

16 bitów na określenie identyfikatora podsieci. 

2001:0DB8:ACAD:`0000`::/64

2001:0DB8:ACAD:`0001`::/64

Wykorzytanie identyfikatora interfejsu do podziału na posieci.

Jak nie dzielimy `nibbla`, półbajtu, pojedyncza cyfra szesnastkowa to sprawa jest prosta.


---

<div>
<a href="chapter-08.md">Prev: Chapter 8</a>
</div>
<div align="right">
<a href="chapter-10.md">Next: Chapter 10</a>
</div>