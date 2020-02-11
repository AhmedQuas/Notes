### Sections
- [Wprowadzenie](#wprowadzenie)
- [Hierarchia katalogów, system plików](#hierarchia-katalog%c3%b3w-system-plik%c3%b3w)
- [`ls` i podstawowe uprawnienia pod lupą](#ls-i-podstawowe-uprawnienia-pod-lup%c4%85)
- [Działania na plikach i katalogach](#dzia%c5%82ania-na-plikach-i-katalogach)
- [To Do](#to-do)

## Wprowadzenie
`powłoka systemowa` - program przyjmujący polecenia z klawiatury i przekazujący je do wykonania przez system operacyjny. Powłoki dzielimy na tekstowe (CLI) i graficzne  (GUI). W Linux domyślną powłoką tekstową jest **bash** (Bourne Again Shell), ulepszona wersja **sh**.

Terminal \ konsola uruchamiany w środowisku graficznym to tak naprawdę **emulator terminalu**. Przełączanie między terminalami wirtualnymi `Ctrl+Alt+F1-6`. Powrót do środowiska graficznego `Alt+F7`. 

Kopiowanie do konsoli `Ctrl+Shift+C` i wklejanie `Ctrl+Shift+V`.

`znak zachęty` - command prompt, pojawia się gdy powłoka oczekuje na wprowadzenie polecenia. Znak zachęty może być konfigurowany względem wlasnych upodobań, jednak zazwyczaj występuje w formacie user@host current_dir $/#. Gdzie:
- $ zwykły użytkownik
- \# użytkownik uprzywilejowany **root** 

Naciśnięcie strzałki w górę/dół powoduje wyświetlenie poprzedniego/następnego polecenia. Wydane wcześniej polecenia można zobaczyć używając polecenia `history` lub przeglądając zawartość pliku `~/.bash_history`. Czasem jednak ten plik nie zawiera historii naszej obecnej sesji (zależnie od dystrybucji).

Większość poleceń składa się z poniższej struktury:

polecenie -opcje/przełączniki argument/y , np
```
ls -la /home/user
```
Zazwyczaj przełącznika to jedna litera np `ls -a`

Istnieje możliwość stosowania  długiego formatu np `ls --all`

Dosyć często powtarzające się przełączki:
- `-h` human readable format
- `-r` reverse
- `-v` verbose mode, wyrzuca więcej informacji na ekaran, bardziej gadatliwy

Zakończenie sesji terminala `exit`.

Polecenia związane z datą: `date`, `cal`.

Polecenia związane z zasobami komputera: `df`, `free`, `top`.

Nawigacja po systemie plików:
- `pwd` - print working directory, analogicznie do `echo $PWD`
- `cd` - change directory `cd - ` powrót do wcześniejszego, poprzedniego katalogu, analogicznie do `cd $OLDPWD`
- `ls` - wypisuje, listuje zawartość katalogu

Do wyświetlania zawartości plików:
- `cat` - wyświetla cały plik, niektórych konsolach może uciąć część pliku (najprawdopodobniej ze względu na brak scroll`a)
- `more` - pozawaka tylko na przewijanie w dół. Klawisze
  - spacja, q - kolejna strona, zakończ działanie programu
- `less` - pozwana na przewijanie góra, dół. Klawisze:
  - spacja, q 
  - PageUp PageDown - strona wyżej, niżej
  - Strzałka góra, dół -linijka niżej, wyżej
  - /pattern - wyszukiwanie znaku, wzoru
  - n - next, następne wystąpienie szukanej frazy
  - h - wyświetlenie pomocy

Pliki konfiguracyjne to głównie pliki tekstowe. Popularne edytory tekstu:
- `vi`
- `nano`

---

## Hierarchia katalogów, system plików
System plików stosowany w Linux to **ext** (Extended File System). Obecnie stosowany jest ext4.

W Linux istnieje jeden, główny katalog, bez względu na liczbę dysków, zamontowanych urządzeń itp. Rozróżniamy ścieżki:
- bezwzględne - absolute, czyli takie, które zaczynają się od rootfs`a
- względne - relative, względem katalogu, w którym obecnie się znajdujemy, występują tu też symbole tj.
  -  . - oznacza obecny katalog 
  -  .. - oznacza nadrzędny katalog, jakby rodzic obecnego katalogu

`~` - oznacza katalog domowy użytkownika

`~user` - katalog domowy użytkownika _user_

Pliki, których nazwa zaczyna się od `.` to ukryte pliki do wylistowania trzeba użyć `ls -a`.

W nazwach poleceń i plików jest rozróżniana wielkość liter. Rozszerzenia plików nie określa typu pliku, typ pliku może być określony np na podstawie [magic number](https://en.wikipedia.org/wiki/List_of_file_signatures). Do określenia typu pliku służu polecenie `file`

**/** - root directory, rootfs, katalog główny

**Katalogi które warto znać**
- `/bin` - pliki binarne (wykonywalne, programy) najbardziej podstawowych narzędzi systemowych, część z nich jest wymagana do uruchomienia systemu
- `/boot` - pliki niezbędne do uruchomienia systemu, obraz dysku RAM wraz ze sterwonikami potrzebnymi w czasie rozruchu/bootowania, GRUB (proram rozuchowy).
- `/dev` - tutaj znajdują się dowiązania do urządzeń rozpoznanych przez jądro (duża ilość urządzeń I/O o ile nie wszystkie)
- `/etc` - większość plików konfiguracyjnych systemu i zainstalowanych usług, aplikacji
- `/home` - katalogi domowe zwykłych użytkowników, tylko tu zwykły użytkownik bez dodatkowych praw tworzyć/modyfikować swoje pliki, katalogi 
- `/lib` - biblioteki współdzielone między różnymi użytkownikami i aplikacjami. Podobne do DLL w Windows
- `/media` - tutaj montowane są nośniki wyjmowane (pendrive, CD-ROM)
- `/mnt` - miejsce montowane dysków twardych
- `/proc` - zawiera dane o uruchomionych procesach, wirtualny system plików będący interfejsem do kernela
- `/root` - katalog domowy użytkownika uprzywilejowanego, root`a
- `/sbin` - pliki wykonywalne, które są dostępne tylko dla administratora, użytkownika uprzywilejowanego
- `/tmp` - przechowuje tymczasowe pliki, w zależności od dystrybucji może być czyszczony przy zamknięciu lub uruchomieniu systemu
- `/usr` - tutaj mogą być instalowane programy wykorzystywane do pracy przez zwykłego użytkownika
- `/var` - pliki, które często ulegają zmianą. Przykładowo pliki wykorzystywane przez serwer www,  `/var/log` znajdują się logi 

---

## `ls` i podstawowe uprawnienia pod lupą

Listowanie wszystkich plików (w tym też ukrytych) `ls -a`

Listowanie zawrtości dowlnego katalogu np `ls /tmp`

Listowanie zawrtości kilku katalogów np `ls ~/my_dir /tmp`

Więcej szczegółów, uprawnienia, typ pliku, daty modyfikacji `ls -l`

Szczegóły o obecnym katalogu `ls -dl`

Numer inode (wyjaśniony przy dowiązaniach)  `ls -i`

```
-rwxr-xr-x 1 user user 13216 2020-02-09 file_name 
```
1. Typ pliku, tutaj jest to zwykly plik
2. Uprwanienia dostępu do pliku (uprawnienia dla właściciela, grupy i reszty użytkowników)
   - r - read - 4
   - w - write - 2
   - x - execute - 1
3. Liczba twardych dowiązań (hard link)
4. Nazwa właściciela
5. Nazwa grupy, której dotyczą uprawnienia
6. Rozmiar pliku w bajtach
7. Data ostatniej modyfikacji
8. Nazwa pliku

Typy plików:
- `-` - zwykły plik, regularny
- `d` - directory, katalog
- `l` - symlink, dowiązanie symboliczne
- `c` - character device, urządzenia strumieniowe
- `b` - block device, urządzenia blokowe
- `s` - socket, gniazdo sieciowe (komunikacja 2 kierunkowa?)
- `p` - named pipe (jak potok?)
 
Urządzenia strumieniowe i blokowe znajdziemy  w katalogu **/dev**. 

Warto tutaj wspomnieć o **inode number**, jest to wpis w tablicy inode reprezentujący metadane o każdym pliku, katalogu znajdującym się w systemie. Wpis ten zawiera dane takie jak: typ pliku, uprawnienia, liczbę hard-linków, id ownera i grupy, rozmiar, time stamp (data utworzenia, modyfikacji itp) i wiele innych metadanych. Numer inode jednoznacznie identyfikuje plik w systemie. Do wyświetlenia numeru inode można wykorzystać polecenia `stat` lub `ls -i`, [więcej o inode](https://linoxide.com/linux-command/linux-inode/)

Zwykły plik to taki jakby wskaźnik do danego numeru inode.

Jeśli plik ma typ **l** to jest to dowiązanie symbliczne. Jest to twór podobny do skrótów znanych z Windowsa. Wskazuje on na inną nazwę pliku. Czyli wskazuje na nazwę pliku, który wskazuje na konkretny inode number. 

Przykład zastosowania: w skrypcie podajemy nazwę programu (dowiązanie symboliczne), który mamy uruchomić. W przypadku gdy nazwa progarmu ulega zmianie (nowa wersja) to jedyne co musimy zrobić to uaktualnić nasze dowiązanie, nie musimy wprowadzać zmian we wszystkich skryptach wykorzystujących ten program. Podobnie wygląda sytuacja z współdzielonymi zasobami.

softlink->file->inode

Hard link, czyli dowiązanie twarde

---

## Działania na plikach i katalogach
Podstawowe polecenia do manipulowania plikami i katalogami:
- `cp` - kopiowanie plików i katalogów
  - `r` - recursive, do kopiowania katalogów
  - `u` -update, copy only when the SOURCE file is newer or missing  to the destiantion
- `ln` - tworzenie symlink i hardlink`ów
- `mv` - przenoszenie, zmiana nazwa plików i katalogów
- `mkdir` - tworzenie katalogów (`mkdir dir1...` - kolejne katalogi po spacji)
  - `-p` - no error if existing, make parent directories as needed
- `rm` - usuwanie plików i katalogów, **w Linux nie ma kosza, bin czy trash**
  - `r` - do usuwania katalogów
  - `f` - force, usuwa bez potwierdzenia

```
cp source destination
```
```
mv old_location new_location
```
**Dobra praktyka**
>Jeśli przy rm korzystamy z wildcard, zaleca się sprawdzić wyniki polecenia ls z tym wieloznacznikiem. Pomoże to uniknąć problemów, gdzie między * a resztą wieloznacznika damy spację, taki wildcard **może nam usunąć całą zawartość obecnego katalogu**

Przy manipulowaniu plikami i katalogami przydatna może okazać się znajomość wieloznaczników, wildcards, pomagają wyszukać, zarządzać wieloma plikami o nazwach spełniających dany wzorzec. Trochę podobne do RegEx. Podstawowe wildcard`sy to:
- `*` oznacza dowolne znaki
- `?` oznacza dowolny znak (pojedynczy)
- `[abc]` oznacza znak a,b lub c
- `[!abc]` negacja, zaprzeczenie
- `[[:class:]]` znak należący do danej klasy

Popularne klasy znaków, to samo w RegEx

|Oznaczenie     | Oznacza dowolne...        |
|   ---         |       ---                 |
|[:alnum:]      |znaki alfanumeryczne       |
|[:alpha:]      |znaki alfabetyczne         |
|[:digit:]      |cyfry, liczby              |
|[:lower:]      |małe litery                |
|[:upper:]      |duże litery                |
|[![:alpha:]]   |negacja [:alpha:]          |

Przykłady:
- i*.html
- *[[:digit:]qaz]

---

## To Do
Uzupełnić o:
- proces bootowania bios->bootloader
- różnice między initd a systemd, grub1/2
- zawratosc /etc/passwd
- czym jest kernel
- opisac dowiazanie twarde