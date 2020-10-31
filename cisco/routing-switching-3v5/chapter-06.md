### Spis treści
- [Chapter 6: Wieloobszarowy protokół OSPF](#chapter-6-wieloobszarowy-protokół-ospf)
  - [6.1 Zaawansowana konfiguracja jednoobszarowego OSPF](#61-zaawansowana-konfiguracja-jednoobszarowego-ospf)
  - [6.2 Działanie wieloobszarowego OSPF LSA](#62-działanie-wieloobszarowego-ospf-lsa)
  - [6.3 Tablica routingu i rodzaje tras w OSPF](#63-tablica-routingu-i-rodzaje-tras-w-ospf)
  - [6.4 Konfigurowanie wieloobszarowego OSPF](#64-konfigurowanie-wieloobszarowego-ospf)
  - [6.5 Sumaryzacja tras OSPF](#65-sumaryzacja-tras-ospf)

# Chapter 6: Wieloobszarowy protokół OSPF

## 6.1 Zaawansowana konfiguracja jednoobszarowego OSPF

Wieloobszarowy OSPF - dzielenie 

Obszar 0 - obszar szkieletowy, wszystkie inne obszary łączą się z nim.

Podział na obszary, wprowadzenie struktury hierarchicznej zapewnia nam skalowalność i większą efektywność.

>Obszar OSPF to grupa routerów, współdzielących te same informacje stanu łącza w bazach danych LSDB.

Problemy dla dużych obszarów protokołu OSPF:
- duża tablica routingu i brak sumaryzacji tras
- duża LSDB, baza danych stanu łącza
- częste obliczanie algorytmu SPF - praktycznie przy każdej zmianie topologii sieci(dodanie, usunięcie, modyfikacja łącza)

Zalety podziału na obszary dużej sieci OSPF:
- `mniejsze tablice routingu` - sumaryzacja między obszarami
- `zredukowana liczba aktualizacji stanu łącza` - mniej routerów wymienia pakiety LSA
- `zmniejszona częstotliwość obliczeń algorytmu SPF` - zalewanie LSA zatrzymuje się na granicach obszaru

Dwuwarstwowa hierarchia w obszarach OSPF:
- `obszar szkieletowy`, tranzytowy, rdzeń, obszar 0 - szybkie i sprawne przemieszczanie pakietów IP, nie ma tutaj użytkowników końcowych, łączą się one z innymi obszarami OSPF
- `obszar zwykły` - łączy użytkowników końcowych i zasoby sieciowe, ruch z innych obszarów musi przejść przez obszar tranzytowy, taki ruch jest określany jak `ruch międzyobszarowy`

Wytyczne dotyczące tworzenia stref:
- mniej niż 50 routerów w obszarze
- router w max 3 obszarach
- każdy pojedynczy router nie powinien mieć więcej niż 60 sąsiadów(switch i Ethernet)

Rodzaje routerów w wieloobszarowym OSPF:
- `router wewnętrzny` - interfejsy tego routera należą do jednego obszaru
- `router rdzeniowy` - router w obszarze szkieletowym
- `router ABR` - Area Border Router, interfejsy tego routera należą do wielu obszarów, prowadzi oddzielne LSDB dla każdego obszaru. Mogą być skonfigurowane do sumaryzacji informacji o routingu LSDB
  
    >Routery ABR rozpowszechniają informacje o routingu do obszaru szkieletowego. Routery szkieletowe następnie przesyłają informacje do innych routerów ABR.

- router ASBR - Autonomous System Boundary Router, router mający co najmniej jeden interfejs podłączony do zewnętrznej sieci(inny system autonomiczny)
  
    >Router ASBR może importować informacje o sieci nie obsługującej OSPF i odwrotnie, za pomocą procesu zwanego redystrybucją trasy.

>Redystrybucja w wieloobszarowym OSPF występuje, gdy `ASBR` łączy różne domeny routingu (np. EIGRP i OSPF) i konfiguruje je do wymiany i ogłaszania informacji o routingu pomiędzy nimi.

## 6.2 Działanie wieloobszarowego OSPF LSA

>Każda realizacja wieloobszarowego OSPF musi obsługiwać LSA 1 - LSA 5

Typy komunikatów OSPF LSA:
- `1, router LSA` - router rozgłasza bezpośrednio podłączone łącza z obsługą OSPF, komunikaty rozgłaszane w ramach obszaru
- `2, network LSA` - wykorzystywane przez sieci wielodostępowe i nierozgłoszeniowych sieci wielodostępowych(NBMA), gdzie wybierany jest DR. Zapewniają innym routerom informacje o sieciach wielodostępowych, generowane przez DR
- `3, summary LSA` - routery ABR używają ich do ogłaszania sieci z innych obszarów, domyślnie nie sumaryzowane
- `4, summary LSA` - stosowane do powiadamiania innych obszarów o obecności router ASBR i podawania trasy do ASBR, generowane przez routery ABR
- `5, AS External LSA` - trasy do sieci spoza systemu autonomicznego OSPF, pochodzą od ASBR i są rozgłaszane do całego systemu autonomicznego
- 6, multicast OSPF LSA
- 7, zdefiniowane dla obszarów NSSA(Not-So-Stubby-Areas)
- 8, atrybuty zewnętrzne LSA dla protokołu BGP
- 9, 10 i 11, opaque LSA - do przyszłych zastosowań

## 6.3 Tablica routingu i rodzaje tras w OSPF

- `O` - trasa wewnątrz obszaru OSPF
- `O IA` - Inter Area, trasy sumaryzacyjne
- `O E1 / O E2` - zewnętrzne trasy typu 1 lub 2, powstają z zewnętrznych LSA

Uzyskiwanie zbieżności OSPF:
1. Wyznacz trasy OSPF wewnątrz obszaru
2. Obliczenie najlepszej ścieżki do tras międzyobszarowych OSPF
3. Wyznacz najlepszą trasę prowadzącą do zewnętrznych sieci nie obsługujących protokołu OSPF

>Po konwergencji, router może komunikować się z każdą siecią w ramach lub na zewnątrz systemu autonomicznego OSPF.

## 6.4 Konfigurowanie wieloobszarowego OSPF

Plan implementacji wieloobszarowego OSPF:
1. Zbierz parametry i wymagania dotyczące sieci
2. Określ parametry OSPF
   - Plan adresowania IP
   - Obszary OSPF
   - Topologia sieci
3. Konfiguracja OSPF
4. Weryfikacja

## 6.5 Sumaryzacja tras OSPF

Sumaryzacja pozwala utrzymywać małe tablice routingu, poprzez konsolidację wielu tras w jedno ogłoszenie.

Sposoby konfiguracji sumaryzacji tras:
- `sumaryzacja tras pomiędzy obszarami` - odbywa się na routerach ABR i ma zastosowanie do tras z wewnątrz każdego obszaru

  ```
  R1(config-router)# area {area-id} range {ip address} {mask} 
  ```

- `sumaryzacja tras zewnętrznych` - charakterystyczna dla tras zewnętrznych, które są wstrzyknięte do OSPF przez redystrybucję tras

  >Sumaryzacja tras zewnętrznych jest konfigurowana na routerze ASBR za pomocą polecenia trybu konfiguracji routera summary-address address mask .

>`Router ABR` może sumować tylko trasy, które znajdują się w ramach obszarów połączonych do routera ABR.

Wpis sumaryzacyjny na wprowadzonym routerze jest wyświetlany jako Null0, i automatycznie odrzucany, tak aby nie powstawały pętle routingu


Weryfikacja stanu OSPF:

```
R1# show ip ospf neighbour
R1# show ip protocols
R1# show ip ospf
R1# show ip ospf interface
R1# show ip ospf interface brief
R1# show ip route ospf
R1# show ip ospf database
```

---

<div>
<a href="chapter-05.md">Prev: Chapter 5</a>
</div>
<div align="right">
<a href="chapter-07.md">Next: Chapter 7</a>
</div>