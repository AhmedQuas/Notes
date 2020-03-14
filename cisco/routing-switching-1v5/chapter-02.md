### Spis treści
- [Chapter 2: Konfiguracja sieciowego systemu operacyjnego](#chapter-2-konfiguracja-sieciowego-systemu-operacyjnego)
  - [2.1 Wprowadzenie & ISO Bootcamp](#21-wprowadzenie--iso-bootcamp)
    - [2.1.1 OS](#211-os)
    - [2.1.1 Metody dostępu do IOS](#211-metody-dost%c4%99pu-do-ios)
    - [2.1.2 Struktura poleceń](#212-struktura-polece%c5%84)
    - [2.1.3 Polecenie show](#213-polecenie-show)
  - [2.2 Podstawowa konfiguracja](#22-podstawowa-konfiguracja)
    - [2.2.1 Host name](#221-host-name)
    - [2.2.2 Hasła do trybów konfiguracji](#222-has%c5%82a-do-tryb%c3%b3w-konfiguracji)

# Chapter 2: Konfiguracja sieciowego systemu operacyjnego

## 2.1 Wprowadzenie & ISO Bootcamp
Domowy router pełni funkcję następujących użądzeń:
- `router` - przekazuje pakiety między sieciami
- `switch` - przełącznik, łączy urządzenia końcowe używając kabli sieciowych
- `wireless AP` - bezprzewodowy punkt dostępowy, to nic innego jak nadajnik radiowy, który łączy bezprzewodowe urządzenia końcowe
- `firewall` - sprzętowa zapora ogniowa, zabezpieczająca/ograniczająca ruch wychodzący i przychodzący

`IOS` - Internetwork Operating System - nazwa sieciowych systemów operacyjnych stosowanych w urządzeniach Cisco. Wersja IOS jest zależna od typu urządzenia i wymaganych funkcji.

Plik IOS ma kilkaset MB i jest przechowywany w nieulotnej pamięci Flash. Jeśli trzeba to dane mogą być nadpisane (uaktualnienie wersji, zapis konfiguracji)
>Możliwym jest, że najnowsze wersje IOS mogą wymagać więcej pamięci RAM i flash niż może być zainstalowane na niektórych urządzeniach.

Funkcje IOS:
- zapewnienie bezpieczeństwa sieci
- adresacja IP interfejsów fizycznych i wirtualnych
- modyfikacja konfiguracji
- `routing`, trasowanie
- obsługa `QoS`
- wsparcie dla technologi zarządzania siecią

### 2.1.1 OS
Podczas uruchamiania komputera system operacyjny jest ładowany z dysku twardego do pamięci RAM.

`Powłoka` - shell, interfejs między aplikacjami, a użytkownikiem. Zadania mogą być wykonywane za pośrednictwem `CLI` (Commadn Line Interface) lub `GUI` (Graphical User Interface)

`Jądro` - kernel, zapewnia komunikację między sprzętem, a użytkownikiem, dodatkowo zarządza zasobami sprzętowymi

`Sprzęt` - głównie elektronika, czyli fizyczna część komputera

### 2.1.1 Metody dostępu do IOS

- `Konsola` - metoda poza-pasmowego dostępu do urządzenia. Zaletą jest fakt, że urządzenie jest dostępne dla administratora nawet gdy nie jest jeszcze skonfigurowane (nadanie IP i inne bajery). Rodzaj połączenia wykorzystywany do wstępnej konfiguracji i w momentach gdy usługi sieciowe przestały działać prawidłowo i dostęp zdalny jest nie możliwy
    >Domyślnie, konsola przekazuje komunikaty generowane w czasie uruchamiania urządzenia, komunikaty dotyczące procesu debugowania oraz komunikaty o błędach.

    >W razie zagubienia hasła istnieje specjalny zestaw procedur pozwalający na ominięcie hasła i przywrócenie dostępu do urządzenia. Należy również zwrócić uwagę, iż urządzenie powinno znajdować się w zamkniętym pomieszczeniu lub szafie (RACK), aby zapobiec nieautoryzowanemu dostępowi fizycznemu.

- `Telnet` - pozawala na ustanowienie nieszyfrowanej, zdalnej sesji CLI poprzez sieć przy wykorzystaniu wirtualnego interfejsu. Do połączenia musi być skonfigurowany przynajniej jeden interfejs

- `SSH` - Secure Shell, zapewnia podobne funkcje do telnet, dodatkiem jest szyfrowanie transmisji i silniejsze uwierzytelnianie użytkownika 

- `port AUX` - starszy sposób wykorzystujący telefoniczne połączenie za pomocą modemu podłączonego do portu AUX. Nie jest to połączenie zdalne, tylko bezpośrednie, więc i tutaj można nie mieć żadnej konfiguracji jak w połączeniu konsolowym

Programy emulujące terminal:
- `PuTTY`
- `Tera Term`

Znaki zachęty i tryby w IOS:
- `>` - user EXEC, użytkownika, pozawala na użycie poleceń ograniczonych do monitoringu, `view-only mode`, zwany inaczej trybem podglądu
- `#` - privileged EXEC, uprzywilejowany, monitorowanie, konfiguracja i zarządzanie urządzeniem. Wyszystkie pozostłe tryby są dostępne z tego poziomu
- `(config)#` - global config, konfiguracja globalna. Konfiguracja dotyczny urządzenia jako całości
- `(config-if)#` - inne, tryby konfiguracji interfejsu. Konfiguracje poszczególnych części lub funkcji urządzenia. Przykładowo:
  - tryb interfejsu np. Fa0/0
  -  tryb lini dostępu - konfiguracja fizycznych i wirtualnych lini dostępowych (konsola, AUX i VTY)

>Hierarchiczna struktura trybów pozwala na zwiększenie bezpieczeństwa dotyczącego dokonywania zmian w konfiguracji urządzenia. Oznacza to, że przy przechodzeniu do innego poziomu w hierarchicznej strukturze trybów konfiguracji może być wymagana dodatkowa autoryzacja użytkownika.

Przejścia między trybami:
```
enable                  #user   -> priv 
configure terminal      #priv   -> (config)
exit                    # lvl--

disable                 #priv   -> user
end => CTRL + Z         # *     -> priv
line vty 0 4            #config -> config-line
```
### 2.1.2 Struktura poleceń

Każda komenda może być wykonana we odpowiednim trybie konfiguracyjnym.

IOS w poleceniach nie rozróżnia wielkość liter.

```
Switch>ping 192.168.1.1
```
```
Switch>show running-config
```

|               |            |
|   ---         |       ---  |
|Switch>        |znak zachęty|
|ping           |polecenie   |
|               |spacja      |
|192.168.1.1    |argument/y  |

Zamiast argumentu może pojawić się tutaj `predefiniowane słowo kluczowe` np. running-config.
Argument to zmienna dostarczona przez użytkownika.

Dokumentacja online -> `Cisco IOS Command Reference`

Ułatwienia w podawaniu poleceń:
- `?` - podpowiedzi (pomoc kontekstowa), dostępne argumenty dla danego polecenia. Wyświetla listę wszystkich dostępnych komend w danym trybie

- `Enter` - weryfikowanie składni. Możliwe błędy:
    
  - `Incomplete command` - polecenie wymaga dodatkowych argumentów
  - `Ambiguous command` - liczba wprowadzonych znaków nie pozwala na rozpoznanie polecenia
  - `Invalid input detected at ^ marker` - wskazuje miejsce, w którym polecenie okazało się niezrozumiałe dla interpretera poleceń
- skróty i hot-keys:
  - `Tab` - uzupełnia nazwę polecenia
  - `Ctrl+Shift+6` - przerywa wykonywanie polecenia
  - `Ctrl + Z / C` - z trybu konfiguracji przechodzi do privileged EXEC
  - `Strzałka w górę / Ctrl + P` - poprzednie polecenie w historii poleceń
  - `Strzałka w dół / Ctrl + N` - następne polecenie w historii poleceń 
  
  Przegadanie poleceń, które zwracaja kilka stron "--More--":
  - `Enter` - następna linia
  - `Spacja` - następna strona
  - `Dowolny klawisz` - zakończenie wyświetlania

Polecenia moga być skracane, do znaków które je jednoznacznie identyfikuja.
```
Switch>sh int      #show interface
```

### 2.1.3 Polecenie show

`Switch#show ?`

```
Switch#show version
Switch#show running-config     #RAM config
Switch#show startup-config     #NVRAM config
Switch#show interfaces
Switch#show interface fastethernet 0/1
Switch#show flash

#Mniej popularne

Switch#show arp
Switch#show mac-address-table
Switch#vlan
Switch#show processes
Switch#show cdp neighbors
```

Switch#`show version`
 - `Software version` - wersja IOS w Flash
 - `Bootstrap version` - wersja programu rozruchowego, pamięci BootROM
 - `System up-time`
 - `System restart info` - przyczyna i sposób ostaniego restartu
 - `Software Image Name` - nazwa IOS w Flash
 - `Router type and processor type`
 - `Memory type and allocation (shared/main)`
 - `Software features`
 - `Hardware interfaces`
 - `Configuration register`

>Przełącznik Cisco jest jednym z najprostszych urządzeń, które można skonfigurować w sieci. Wynika to z faktu, że nie wymagane są żadne czynności konfiguracyjne w celu uruchomienia tego urządzenia. W najbardziej podstawowym stanie przełącznik może być podłączony bez konfigurowania, a będzie przełączał dane pomiędzy podłączonymi urządzeniami.

## 2.2 Podstawowa konfiguracja

### 2.2.1 Host name

A po co to komu?
>Wyobraź sobie, że w sieci uruchomiono kilka przełączników i wszystkie zostały nazwane z domyślną nazwą "Switch" (jak pokazano na rysunku). Może to powodować znaczne zamieszanie podczas konfiguracji i zarządzania sieci.

>Dobrą praktyką jest tworzenie nazw w tym samym czasie co schemat adresowania.

```
Switch#configure terminal
Switch (config)#hostaname sw1
sw1 (config)#no hostaname       #remove hostname
Switch (config)#
```
`no` command -> odwołanie skutków polecenia

### 2.2.2 Hasła do trybów konfiguracji

>Dobrą praktyką jest ograniczanie fizycznego dostępu do urządzeń sieciowych poprzez zamykanie ich w szafach. Jednak podstawową ochroną przed nieautoryzowanym dostępem są ustawione hasła do urządzeń.

Możliwe rodzje haseł:
- `Enable password` - ograniaczenie dostępu do prvileged EXEC

- `Enable secret` - ograniaczenie dostępu do prvileged EXEC, **szyfrowane**
  ```
  Switch#configure terminal
  Switch (config)#enable secret class
  Switch (config)#exit
  Switch#disable
  Switch>enable
  Password:
  ```

- `Console password` - ograniczenie dostępu do łącza konsolowego
    ```
  Switch#configure terminal
  Switch (config)#line console 0
  Switch (config-line)#password cisco
  Switch (config-line)#login            #wymusza wymóg uwierzytelnienia
  ```

- `VTY password` - ogranicza dostęp przez Telnet
  Domyślnie przełączniki Cisco wspierają 16 lini Telnet, natomiast router 5 (0-4)
  ```
  Switch#configure terminal
  Switch (config)#line vty 0 15
  Switch (config-line)#password cisco
  Switch (config-line)#login            #wymusza wymóg uwierzytelnienia
  ```
  **Uwaga haczyk!**

  Jak przez pomyłkę wpiszemy `Switch (config-line)#no login` w konfiguracji lini VTY, to usuniemy wymóg uwierzytelnienia dla Telnet co nie jest dobrym rozwiązaniem.

>Dobrą praktyką jest stosowanie różnych haseł do każdego z tych poziomów dostępu.

**Szyforwanie haseł, żeby nie były widoczne w plain text w podczas `show running-config`**

Słabe szyfrowanie nieszyfrowanych haseł zapisanych w pliku konfiguracyjnym.

```
  Switch (config)#service password-encryption
  Switch (config)#exit
  Switch#show running-config
```

### 2.2.3 Banner MOTD

`MOTD` - message of the day

>Banery mogą być istotnym elementem procesu prawnego w sytuacji, gdy ktoś jest ścigany za włamanie do urządzenia. Niektóre systemy prawne nie pozwalają na ściganie, a nawet monitoring użytkowników, bez wcześniejszej widocznej informacji o tym fakcie.

```
Switch (config)#banner motd # Authorized access only! #
```
`#` - to znak ograniczający, nie jest on częścią wiadomości

### 2.2.4 Zapisywanie konfiguracji do NVRAM

`running-config` jest czyszczony po restartcie, wtedy _startup-config_ ląduje w RAM i jest naszym _running-config_.

`startup-config` jest w NVRAM

Kopiowanie bieżącej konfiguracji do nieulotniej NVRAM:
```
Switch# copy running-config startup-config
```

Cofnięcie zmian w bieżącej konfiguracji. Skopiowanie istniejacej konfiguracji do pamięci RAM:
```
Switch# copy startup-config running-config
```

Przywrócenie domyślnej konfiguracji

```
Switch# erase startup-config
Switch# delete vlan.dat       # tego już nie trzeba 
Switch# reload
```

>Uwaga! Należy zachować ostrożność podczas korzystania z komendy erase . Komenda ta może być użyta do usunięcia każdego pliku z urządzenia. Niewłaściwe użycie polecenia może usunąć obraz systemu IOS lub inny istotny plik.

Konfiguracja może być również zapisana w postaci przechwyconego tekstu z programu programu, który emuluje nam terminal. Oczywiście pod warunkiem, że program obsługuje daną funkcję (Capture Text, a później show running config)

## 2.3 Porty i adresacja

>Adres IPv4 najczęściej przedstawiany jest w notacji kropkowo-dziesiętnej, w której jest reprezentowany przez cztery liczby dziesiętne z zakresu od 0 do 255.

>Z adresem IP musi być również skonfigurowana maska sieci. Jest on specjalnym typem adresu IPv4, który, w połączeniu z adresem IP, określa, do której podsieci stanowiącej element większej sieci przynależy dane urządzenie.

>Adres IP może być przydzielony zarówno do portu fizycznego jak i interfejsu wirtualnego urządzenia. Wirtualny interfejs oznacza, że nie ma fizycznego sprzętu na urządzeniu, z którym ten interfejs jest związany.

Ethernet jest powszechnie używany w sieciach LAN.

>Żeby kabel można było podłączyć do portu Ethernet urządzenia, to musi on być zakończony złączem RJ-45.

Przełączniki posiadają jeden lub więcej wirtualnych interfejsów (SVI).

>SVI jest tworzony wyłącznie przez oprogramowanie. Wirtualny interfejs daje możliwość zdalnego zarządzania przełącznikiem przez sieć przy użyciu protokołu IPv4. Domyślnym interfejsem SVI jest VLAN1.

Ustawianie IP i maski podsieci na SVI:
```
Switch (config)# interface vlan 1
Switch (config-if)# ip address 192.168.1.10 255.255.255.0
Switch (config-if)# no shutdown   #int state up
```
>Adres bramy domyślnej jest adresem IP interfejsu routera używanego dla ruchu opuszczającego sieć lokalną

>Adres serwera DNS jest adresem IP serwera systemu nazw domen (DNS), który jest używany do tłumaczenia adresów domenowych na adresy IP

>DHCP (Dynamic Host Configuration Protocol) umożliwia automatyczną konfigurację adresu IPv4 dla każdego urządzenia końcowego w sieci obsługującego protokół DHCP.

`ipconfig /all` - polecenie służące do wyświetlenia konfiguracji kart sieciowych w Windows

`ifconfig` - polecenie służące do wyświetlenia konfiguracji kart sieciowych w Linux

`Konflikt adresów IP` - co najmniej 2 hosty mają ten sam adres IP

`Statyczne a dynamiczne nadawanie adresu IP`
>Zazwyczaj statyczne adresy IP są przydzielane dla serwerów i drukarek w małych i średnich sieciach biznesowych podczas, gdy urządzenia pracowników wykorzystują adresy IP przydzielone przez serwer DHCP.

## 2.4 Testowanie połączenia

Testowanie loopback (pętli zwrotnej)

>Adres pętli zwrotnej 127.0.0.1 jest zdefiniowany przez stos protokołów TCP/IP jako adres zarezerwowany, który przekazuje pakiety z powrotem do hosta.

> Pomyślne zakończenie komendy ping oznacza, że karta sieciowa, sterowniki i protokół TCP/IP działają poprawnie.

```
C:\> ping 127.0.0.1
```

Weryfikacja interfejsów przełącznika:

```
Switch1# show ip interface brief
```

Testowanie połączenia end-to-end

```
C:\> ping 192.168.1.10
```