### Spis treści
- [Chapter 6: Routing statyczny](#chapter-6-routing-statyczny)
  - [6.1 Routing statyczny](#61-routing-statyczny)
  - [6.2 Konfigurowanie tras statycznych](#62-konfigurowanie-tras-statycznych)
    - [6.2.1 IPv4](#621-ipv4)
    - [6.2.2 IPv6](#622-ipv6)
  - [6.3 Adresowanie klasowe, CIDR i VLSM](#63-adresowanie-klasowe-cidr-i-vlsm)
  - [6.4 Konfiguracja sumarycznych i pływających tras statycznych](#64-konfiguracja-sumarycznych-i-p%c5%82ywaj%c4%85cych-tras-statycznych)
  - [6.5 Rozwiązywanie problemów](#65-rozwi%c4%85zywanie-problem%c3%b3w)

# Chapter 6: Routing statyczny

## 6.1 Routing statyczny

Sposoby uzyskiwania informacji o sieciach odległych:
- ręczenie, manualnie, statyczne wpisy w tablicy routingu
- dynamicznie - trasy dodawane automacztynie za pomocą protokołów routingu, występuje tutaj również aktualizacje dostępnych tras

Zalety routing statycznego:
- trasy nie są rozgłaszane, większe bezpieczeństwo sieci
- mniejsze obciążenie obliczeniowe, nic nie rozgłasza, więc nie marnuje dostępnego pasma
- droga jest zawsze znana

Wady routingu statycznego:
- czasochłonna konfiguracja i utrzymanie
- konfigurując trasę zawsze można popełnić błąd
- zmiana tras wymaga każdorazowej ingerencji administratora
- kompletnie nie jest skalowalny
- proprawna implementacja routingu statycznego wymaga znajomości topologii całej sieci

Wady jednej metody są zaletami drugiej.

>Dystans administracyjny (AD) trasy statycznej wynosi 1. Dlatego `trasa statyczna ma pierwszeństwo przed wszystkimi trasami dynamicznymi`.

Zastosowania tras statycznych:
- tablice routingu w małych sieciach, których rozbudowa nie jest przewidywalna
- routing z/do sieci szczątkowych, `stub network` - czyli takich, do których prowadzi jedna trasa
- konfigurowanie trasy domyślnej, bramy ostatniej szansy, wykorzystywanej w przypadku gdy router nie znajdzie lepszej trasy dla pakietu

Rodzaje tras statycznych:
- `standardowa trasa statyczna` - głównie routing do stub network
- `domyślna trasa statyczna` - taka do której pasują wszystkie pakiety, brama ostatniej szansy `0.0.0.0/0` jako adres sieci docelowej. Stosujemy w przypadkach:
  - router jest połączony tylko z jednym routerem
  - wykorzystywany gdy żaden inny adres nie pasuje w tablicy routingu
  - router brzegowy do sieci ISP

- `sumaryczna trasa statyczna` - zastępujemy wiele tras jedną, która jest sumaryzacją pozostałych, często wiele podobnych tras statycznych używa tego samego interfejsu wyjściowego
- `floating static route` - pływająca trasa statyczna, służące do zapewnienia rezerwowej trasy. Nic innego jak zwykła trasa statyczna z większym dystansem administracyjnym AD, jest wykorzystywana jak droga otrzymana dynamicznie nie jest dostępna

## 6.2 Konfigurowanie tras statycznych

### 6.2.1 IPv4

Trasy statyczne dodawane są za pomocą polecenia `ip route`.

`no ip route` - usuwanie statycznych wpisów

Następny przeskok może być konfigurowany jako adres ip i/lub nazwa interfejsu wyjściowego.

Rodzaj konfiguracji miejsca docelowego określa jeden z trzech następujących rodzajów tras:
- `trasa następnego przeskoku` - adres IP dla następnego przezkoku, zwana czasem wpisem rekurencyjnym
    ```
    ip route 192.168.1.0 255.255.255.0 10.0.0.2
    ```

    `10.0.0.2` - adres IP następnego przeskoku

    >Każda trasa, która odwołuje się tylko do adresu IPv4 następnego skoku, a nie do interfejsu wyjściowego, wymaga znalezienia w tablicy routingu innej trasy z adresem IPv4, która ma interfejs wyjściowy.

    Cisco Express Forwarding, CEF pozwala uniknąć zasobożernego przeszukiwania rekurencyjnego
- `bezpośrednio podłączona trasa statyczna` - podajemy nazwę interfejsu wyjściowego routera
    
    Tutaj wszystko odbywa się w ramach jednego cyklu wyszukiwania

    ```
    ip route 192.168.1.0 255.255.255.0 s0/0/0
    ```

- `w pełni określona trasa statyczna` - adres IP + nazwa interfejsu wyjściowego
    ```
    ip route 192.168.1.0 255.255.255.0 s0/0/0 10.0.0.2
    ```

Weryfikowanie wprowadzonych tras statycznych:
- `show ip route`
- `show ip route static`
- `show ip route 192.168.1.1`
- ping
- tracert

Konfigurowanie tras domyślnych IPv4

```
ip route 0.0.0.0 0.0.0.0 {interface | ip-address} 
```
`FIB` - Forwarding Information Base

### 6.2.2 IPv6

Włączenie routingu unicast IPv6
```
R1 (config)# ipv6 unicast-routing
```

W IPv6 wyróżniamy identyczne rodzaje tras jak w IPv4.
- next hop
    ```
    ip route 2001:DB8:ACAD:2::/64 2001:DB8:ACAD:4::2
    ```
- directly connected
    ```
    ip route 2001:DB8:ACAD:2::/64 s0/0/0
    ```
- w pełni określona trasa statyczna - dla IPv6 jest wymagana jeśli podajemy adres typu link-local
    ```
    ip route 2001:DB8:ACAD:2::/64 s0/0/0 fe80::2
    ```

Weryfikowanie wprowadzonych tras statycznych:
- `show ipv6 route`
- `show ipv6 route static`
- `show ipv6 route 192.168.1.1`
- ping
- tracert

Domyślna trasa statyczna IPv6

```
ipv6 route ::/0 {interface | ip-address}
```

## 6.3 Adresowanie klasowe, CIDR i VLSM

`Adresowanie klasowe` - wykorzystujemy domyślne maski, więc aktualizacje routingu w swoich aktualizacjach nie przesyłają masek podsieci. Router stosuje domyślną maskę na podstawie wartości pierwszego oktetu.

Przykładem protokołu routingu, który wykorzystuje adresowanie klasowe jest RIPv1.

Jest tutaj też wykorzystywana sumaryzacja tras, jednak nie potrafią wysyłać supersieci.

Adresacja wykorzystująca domyślne maski powoduje ogromne marnotrawstwo przestrzeni adresowej.

`Adresowanie z wykorzystaniem CIDR`, podział na równe podsieci, wykorzystywanie maski. Metoda CIDR wspiera :
- `sumaryzację tras` - agregację prefix\`u
- `tworzenie supersieci` - występuje, gdy maska ​​sumarycznej trasy jest mniejsza od wartości domyślnej klasowej maski.

>**Uwaga:** Należy zapamiętać, że supersieć to zawsze trasa sumaryczna, ale trasa sumaryczna nie zawsze jest supersiecią.

>`Supersieć` sumaryzuje kilka adresów sieciowych z maskami, które są mniejsze niż maska klasowa.

Sumaryzacja tras powoduje zmniejszenie rozmiaru tablicy tras routingu, co znacznie przyspiesza proces przeszukiwania.

>Gdy w tablicy routingu znajduje się supersieć, na przykład jako trasa statyczna, to klasowy protokół routingu nie będzie umieszczał tej trasy w swoich aktualizacjach routingu.

`FLSM` - Fixed Length Subnet Masking, tradycyjna metoda dzielenia na podsieci o stałej długości masek, zazwyczaj to również nie jest optymalne rozwiązanie.

`VLSM` - jest najbardziej wydajną metodą dzielenia sieci na podsieci

## 6.4 Konfiguracja sumarycznych i pływających tras statycznych

>`Sumaryzacja tras`, zwana również agregacją tras, jest procesem ogłaszania ciągłego zbioru adresów jako pojedynczego adresu z maską mniejszą od dostępnej, krótszej maski podsieci. Zbiór adresów używa tego samego interfejsu wyjściowego

Sposób obliczania:
1. Zapisanie adresów sieci binarnie
2. Wyznaczenie liczby zgodnych bitów, od lewej strony wszystkie bity zgodne
3. `Sumaryczny adres sieciowy to taki`, który powstał przez przepisanie zgodnych bitów, a różniące się ma wypłenone zerami

```
ip route 172.16.0.0 255.255.252.0 192.168.1.2
```

Sumaryzacja adresów w IPv6 jest wykonywana tak samo, dodatkowo musimy tutaj wykonać konwersję z hex na bin i póżniej na odwrót.

Pływająca trasa statyczna - trasa alternatywna

Domyślne dystanse administracyjne dla wybranych protokołów routingu:
- `EIGRP` - 90
- `IGRP` - 100
- `OSPF` - 110
- `IS-IS` - 115
- `RIP` - 120

Konfigurowanie trasy alternatywnej - polega na skonfigurowaniu zwykłej trasy statycznej z podaniem na końcu odpowiedniej wartości dystansu administracyjnego.

ip route 192.168.1.0 255.255.255.0 10.0.0.2 `10`

```
ip route 192.168.1.0 255.255.255.0 10.0.0.2 10
```

## 6.5 Rozwiązywanie problemów

Stan sieci może ulec zmianie z przyczyny:
- awarii interfejsu
- zbyt dużego ruchu na łączach
- przerwanie połączenia przez dostawcę usług
- nieprawidłową konfigurację wprowadzoną przez administratora

Polecenia IOS pomocne do rozwiązywania problemów:
- `ping`
- `traceroute`
- `show ip route`
- `show ip interface brief`
- `show cdp neighbours detail`

---

<div>
<a href="chapter-05.md">Prev: Chapter 5</a>
</div>
<div align="right">
<a href="chapter-07.md">Next: Chapter 7</a>
</div>