### Spis treści
- [Chapter 9: Licencjonowanie i obrazy IOS](#chapter-9-licencjonowanie-i-obrazy-ios)
  - [9.1 Zarządzanie plikami systemowymi IOS](#91-zarządzanie-plikami-systemowymi-ios)
  - [9.2 Zarządzanie obrazami oprogramowania Cisco IOS](#92-zarządzanie-obrazami-oprogramowania-cisco-ios)
  - [9.3 Licencjonowanie oprogramowania](#93-licencjonowanie-oprogramowania)
  - [9.4 Weryfikacja i zarządzanie licencją](#94-weryfikacja-i-zarządzanie-licencją)

# Chapter 9: Licencjonowanie i obrazy IOS

## 9.1 Zarządzanie plikami systemowymi IOS

Cisco IOS - Internetwork Operation System, oprogramownie stosowane w routerach i przełącznikach Cisco. IOS to nie tylko system operacyjny, ale również zbiór pakietów routingu, przełączania, oraz innych technologii zebranych w jeden wielozadaniowy system operacyjny. W zależności od wersji oprogramowania IOS mamy dostęp do różnych funkcji sprzętu sieciowego.

>Wersja oprogramowania IOS wydanego po 12.4 to 15.0. Wersje 13 i 14 nie istnieją.

IOS jest podzielony na rodziny i serie.

W skład rodziny wchodzi wiele wersji wydań oprogramowania IOS, które:
- dzielą kod bazowy,
- stosują się do pokrewnych platform sprzętowych,
- pokrywają się w zakresie obsługi - gdy jeden przestaje być wspierany to na jego miejsce pojawia się kolejny, który jest wspierany

>Przykłady wersji oprogramowania IOS w ramach rodziny programu, to m.in.: 12.3, 12.4, 15.0, i 15.1.

 IOS 12.4, rodzaje serii:
 - `główna` - otrzymuje przeważnie poprawki błędów oprogramowania. Wydania tej serii są oznaczane jako `MD` - Maintenance Deployment, wydania wdrażające utrzymanie
 - `T` - techniczna, otrzymuje te same poprawki co wersja główna +  funkcje wspierające nowe oprogramowanie i sprzęt. Wydania są oznaczane jako `ED` - Early Deployment, wydania wczesnego wdrażania. Na bazie tej serii powstaje nowa seria główna
 - `S` - service provider train, seria zawiera konkretne funkcje zaprojektowane do spełnienia wymagan usługodawcy

>Każda poprawka, która jest wprowadzona w wydaniu serii T powinna zostać wprowadzona `w następnym wydaniu` wersji głównej.
```
12.4(21 a)
  |   |  |_ identyfikator poprawki
  |   |____ identyfikator utrzymania
  |________ numer serii
```
```
12.4(20)T 1 
  |   | | |_ identyfikator poprawki
  |   | |___ identyfikator serii 
  |   |_____ identyfikator utrzymania
  |_________ numer serii

```

>Począwszy od wydania rodziny oprogramowania Cisco IOS Software 12.4, SSH jest dostępny we wszystkich obrazach.

Pakiety IOS nie będące premium:
- `IP Base`
- `IP Voice` - VoIP, VoFR, telefonia IP, połączenie danych i głosu
- `Advanced Security` - VPN, IOS z Firewall, IDS/IPS, IPSec, 3DES i inne funkcje zabezpiczeń
- `SP` - Service Provider Services, SSH/SSL, ATM, VoATM, `MPLS`(Multiprotocol Label Switching) do VoIP
- `Enterprise Base` - Appletalk, IPX i IBM Support

Pakiety premium:
- `Advanced Enterprise Services` - pełna funkcjonalność oprogramowania IOS
- `Enterprise Services` - Enterprise Base + Service Provider Services
- `Advanced IP Services` - Advanced Security + Service Provider Services

> `Cisco Feature Navigator` jest narzędziem używanym do znajdywania odpowiedniego systemu operacyjnego Cisco w zależności od wymaganych technologii i funkcji

12.4 vs 15.0:
- nowe funkcje i wsparcie sprzętowe
- rozszerzona funkcja zgodności z innymi głównymi wersjami IOS
- przewidywana polityka wydań i harmonogram poprawek polityka proaktywnego wsparcia indywidualnych wydań
- uproszczona numeracja wydań
- jaśniejsze wytyczne dotyczące wdrażania oprogramowania i migracji

>Wydania Cisco IOS od wersji 15.0 obejmują:
>- dziedziczenie funkcji z oprogramowania wersji głównej Cisco IOS Software wydań 12.4T i 12.4;
>- nowe funkcje wydawane są dwa lub trzy razy w ciągu roku, dostarczane kolejno w pojedynczej serii;
>- wydania EM są dostępne co 16 lub 20 miesięcy i zawierają nowe funkcje;
>- wydania T dla najnowszych funkcji i wsparcia sprzętu są dostępne dla następnych wydań EM i zostają udostępnione na stronie Cisco.com;
>- poprawki wydań M i T zawierają tylko poprawki błędów.

>Zamiast dzielić na osobne serie, wersja główna i seria T oprogramowania Cisco IOS Software 15 będzie miał rozszerzoną wersję utrzymania (wydanie EM) (ang. extended maintenance) oraz wersję standardową utrzymania (wydanie T). Wraz z nowym modelem wydań IOS, wersję główną wydania Cisco IOS 15 określa się jako serię M.

`EM` - Extended Maintenance, rozszerzona wersji utrzymania

```
15.0(1)M 1
 | | | | |_ numer poprawki utrzymania
 | | | |___ M=wydanie rozszerzonego utrzymania
 | | |_____ numer wydania nowej funkcjonalności
 | |_______ numer wydania drugorzędowego
 |_________ numer wydania głównego
```

M = EM

```
15.1(1)T 1
 | | | | |_ numer poprawki utrzymania
 | | | |___ T=wydanie standardowego utrzymania
 | | |_____ numer wydania nowej funkcjonalności
 | |_______ numer wydania drugorzędowego
 |_________ numer wydania głównego
```

Wydanie T - krótkie wersjie wdrożeniowe, nowe funkcje, obsługę sprzętu(zanim następne wydanie EM stanie się dostępne) i regularną naprawę błędów przez poprawki utrzymania oraz krytyczne wsparcie poprawek dla błędów wpływających na sieć, takich jak kwestie `PSIRT` (ang. Product Security Incident Report Team).

Kupując router dostajemy pojedynczy, uniwersalny obraz Cisco IOS, a licencja służy do włączania konkretnych zestawów funkcji pakietów.

Typy uniwersalnych obrazów:
- `universalk9` - oferuje wszystkie funkcje oprogramowania IOS tj silne funkcje kryptograficzne, IPSec VPN, SSL VPN oraz technologię Secure Unified Communications
- `universalk9_npe` - niektóre kraje mają wymagania importowe, które wymagają, aby platforma nie obsługiwała żadnych mocnych funkcji kryptgraficznych, szczególnie kryptografii ruchu.

>Każdy klucz jest unikalny dla danego urządzenia i jest otrzymywany od firmy Cisco, dostarczając identyfikator produktu i numer seryjny routera oraz klucz aktywacji produktu (`PAK`) (ang. Product Activation Key). Klucz dostarcza firma Cisco w chwili zakupu oprogramowania. Pakiet funkcji IP Base jest instalowany domyślnie.

Rodzaje pakietów dla IOS 15.0:
- `IP Base` - IP Base
- `Security` - SEC, Advanced Security
- `Unified Communication` - UC, IP Voice
- `Data` - Enterprise Base

`show flash` - wyświetla pliki przechowywane w pamięci flash, między innymi pliki obrazu systemu

```
c2800nm-advipservicesk9-mz.124-6.T.bin
```

Konwencja nazewnicza obrazów Cisco IOS:
- `c2800nm` - platforma, na jakiej działa obraz - Cisco 2800 z modułem sieciowym
- `advipservicesk9` - określa zestaw funkcji, w tym przypadku odnosi się do zaawansowanego zestawu funkcji usług IP(obejmuje zaawansowane funkcje bezpieczeństwa, dostawcę usług i IPv6)
- `mz` - określa gdzie działa obraz i czy plik jest skompresowany. W tym przypadku wskazuje, że plik działa z pamięci RAM i jest skompresowany. Inne miejsca gdzie może działać obraz:
  - `f` - pamięć flash,
  - `m` - pamięć RAM,
  - `r` - pamięć ROM,
  - `l` - pamięć przenośna 
  
  Sposoby kompresji:
  - `z` - zip,
  - `x` - mzip
- `124-6.T` - format obrazu dla 12.4(6)T - numer serii, wersji utrzymania i id obrazu
- `bin` - rozszerzenie pliku, w tym przypadku oznacza, że plik wykonywalny jest plikiem binarnym

`SPA` - obraz podpisany cyfrowo

## 9.2 Zarządzanie obrazami oprogramowania Cisco IOS

>Dobrą praktyką dla każdej sieci jest zachowanie kopii zapasowej obrazu oprogramowania w przypadku, gdy obraz systemu na routerze zostanie uszkodzony lub przypadkowo usunięty.

Utworzenie kopii systemu IOS i przesłanie go na serwer TFTP:
1. upewnij się że istnieje dostęp do serwera sieciowego TFTP, wykonaj `ping`
2. sprawdź czy na TFTP jest wystarczająco dużo miejsca, rozmiar pliku obrazu IOS można sprawdzić za pomocą `show flash0:` - rozmiar podawany w B
3. `copy flash0: tftp`
4. podanie nazwy pliku źródłowego, adresu IP zdalnego hosta i nazwę pliku docelowego 

Wgrywanie nowej wersji IOS z serwera TFTP:
1. wybranie i pobranie odpowiedniego obrazu Cisco IOS z cisco.com
2. sprawdź ping i czy jest wymagana ilość miejsca na flash0 -> `show flash0:`
3. `copy tftp flash0:`
4. podanie adresu IP zdalnego hosta, nazwę pliku źródłowego i docelowego
5. wskazanie w boot system nazwę i lokalizację obrazu IOS do załadowania, zapisanie bieżącej konfiguracji i przeładowanie urządzenia
  ```
  R1(config)# boot system flash0://c1900-universalk9-mz.SPA.152-4.M3.bin
  R1# copy running-config startup-config 
  R1# reload
  ```
6. weryfikacja, czy operacja powiodła się, poleceniem `show version`

## 9.3 Licencjonowanie oprogramowania

>Cisco IOS 15.0 zawiera zestaw funkcji na wiele platform, aby uprościć proces wyboru obrazów. Czyni to poprzez dostarczanie podobnych funkcji niezależnie od platformy. `Każde urządzenie jest dostarczane z tym samym uniwersalnym obrazem`. Pakiety technologiczne są włączone w obrazie uniwersalnym za pomocą kluczy licencyjnych `Cisco Software Activation`.

> Licencja IP Base jest warunkiem dla instalacji licencji Data, Security i Unified Communications.

[Więcej o licencjach pakietów technologicznych](https://static-course-assets.s3.amazonaws.com/ScaN50PL/course/module9/index.html#9.2.1.1)

Weryfikacja funkcji i pakietów technologii obsługiwanych na routerze

```
R1# show licence feature
```

>Gdy dostarczony jest nowy router, jest na nim fabrycznie zainstalowany obraz oprogramowania i odpowiednie stałe licencje dla pakietów i funkcji określonych przez klienta.

>Router posiada także licencję próbną, zwaną także jako licencja czasową, dla większości pakietów i funkcji obsługiwanych na określonym routerze.

Kroki wymagane do stałej aktywacji nowego pakietu oprogramowania lub funkcji na routerze:
1. zakup pakietu lub funkcji i otrzymanie `PAK`
   >Certyfikaty roszczenia oprogramowania (ang. `Software Claim Certificates`) są używane do licencji, które wymagają aktywacji oprogramowania. Certyfikat roszczeniowy dostarcza klucz aktywacji produktu (`PAK`) dla licencji i ważne informacje dotyczące umowy licencyjnej użytkownika (`EULA`)
2. pozyskanie licencji(`powiązanie PAK z UDI`), co pozwala na otrzymanie pliku licencji
   Plik licencji jest określany jako `Software Activation License` - Licencja aktywująca oprogramowanie. Licencję można uzyskać za pomocą:
   - `CLM` - Cisco License Manager, aplikacja pomagająca realizować wdrożenia wielu licencji
   - `Portal Cisco License Registration` - uzyskanie i rejestrowanie indywidualnych licencji na IOS

    Oba sposoby wymagają podania numeru klucza `PAK`(otrzymywanego podczas zakupu) i unikalnego identyfikatora urządzenia (`UDI` - Unique Device Identifier, jest to kombinacja ID produktu(`PID`, typ urządzenia), numeru seryjnego urządzenia(`SN`, 11 cyfr) i numeru urządzenia)

    Później klient dostaje maila zawierającego plik licencji(XML z rozszerzeniem .lic)

3. instalacja licencji
   ```
   R1# license install flash0:securityk9-CISCO1941-FHH12250057.xml
   R1# reload
   ```


>Klucz PAK jest `11-cyfrowym` alfanumerycznym kluczem stworzonym przez dział produkcji firmy Cisco. Definiuje on zestaw funkcji związanych z kluczem PAK. Klucz PAK `nie jest związany z konkretnym urządzeniem`, dopóki nie zostanie utworzona licencja. Można zakupić klucz PAK, który generuje dowolną liczbę licencji.

Wyświetlenie numeru UDI w IOS

```
R1# show license udi
```

>Stała licencja jest licencją, która nigdy nie wygasa. Są ważne dla do końca trwałości urządzenia. Aktualizacja do nowej wersji IOS nie powoduje problemów z zakupionymi wcześniej fukcjami. Jest to najczęściej używany typ licencji, kiedy mamy zamiar dokupić zestaw funkcji do urządzenia.

Typy licencji:
- `permanent` - stała
- `evaluation` - próbna 

## 9.4 Weryfikacja i zarządzanie licencją

Weryfikacja licencji

```
R1# show license
```

Aktywacja licencji próbnej z prawem do użytkowania

>Licencje próbne z prawem do użytkowania (RTU) (ang. Right-To-Use) po 60 dniach. Licencja próbna jest dobra na okres próbny 60 dni. Po 60 dniach, licencja ta automatycznie przechodzi w licencję z prawem do użytkowania (RTU).

```
R1(config)# license accept end user agreement
R1(config)# license boot module module_name technology-package package_name
R1# reload
R1# show license
```

Nazwy pakietów technologicznych:
- ipbasek9 - pakiet technologii IP Base,
- securityk9 - pakiet technologii Security
- datak9 - pakiet technologii Data
- uck9 - pakiet Unified Communications

Kopia zapasowa licencji

```
R1# license save flash0:licenses.lic
R1# show flash0:
```

Deinstalacja licencji = wyłączenie pakietu technologicznego + usunięcie licencji

```
R1(config)# license boot module module_name technology-package package_name disable
R1# reload
R1# license clear feature_name
R1(config)# no license boot module module_name technology-package package_name disable
```

>Niektóre licencje nie mogą zostać usunięte, takie jak na przykład licencje wbudowane. Tylko licencje dodane za pomocą polecenia license install są usuwane. Licencje próbne nie są usuwane.



---

<div>
<a href="chapter-08.md">Prev: Chapter 8</a>
</div>
<div align="right">
<a href="../routing-switching-4v5/chapter-01.md">Next: Chapter 4.1</a>
</div>