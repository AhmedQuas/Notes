### Spis treści
- [Chapter 3: Agregacja łaczy](#chapter-3-agregacja-łaczy)
  - [3.1 Podstawowe pojęcia agregacji łaczy](#31-podstawowe-pojęcia-agregacji-łaczy)
  - [3.2 Konfiguracja agregacji łączy](#32-konfiguracja-agregacji-łączy)
  - [3.3 Weryfikacja i rozwiązywanie problemów z EtherChannel](#33-weryfikacja-i-rozwiązywanie-problemów-z-etherchannel)

# Chapter 3: Agregacja łaczy

## 3.1 Podstawowe pojęcia agregacji łaczy

`Agregacja łaczy` - technika pozwalajaca na utworzenie jednego logicznego połaczenia za pomoca wielu fizycznych połaczeń między urzadzeniami. Pozwla to na rozłożenia obciażenia między połaczenia fizyczne.

Dodawanie szybszych fizycznych portów jest często kosztowną operacją, z tego względu zazwyczaj bardziej ekonomiczne jest logiczne złączenie kilku portów fizycznych za pomocą EtherChannel. 

Interfejs logiczny - `port channel`

Zalety EtherChannel:
- większość konfiguracji wykonujemy na jednym logicznym interfejsie - `port channel`, a nie na każdym fizycznym interfejsie z osobna
- technologia bazuje na obecnych portach przełącznika, nie trzeba nic dokupować
- wspiera równoważenie obciążenia w ramach tego samego EtherChannel
- STP nie blokuje połączeń które są częścią tego samego łącza EtherChannel, identyfikuje to jako jedno połączenie
- zapewnia nadmiarowość, awaria jednego interfejsu, który jest częścią port channel nie powoduje zmian w topologii sieci

>`Typy interfejsów nie mogą być łączone`. Na przykład, Fast Ethernet i Gigabit Ethernet nie mogą być połączone w jednym EtherChannel.

>Obecnie każdy EtherChannel może składać się z `maksymalnie ośmiu` zgodnie skonfigurowanych portów Ethernet.
Co przekłada się na:
- 8 x Fa = 800 Mb/s
- 8 x Gi = 8 Gb/s

>Przełącznik Cisco IOS może obecnie obsługiwać `sześć kanałów` EtherChannel.

EtherChannel tworzy relacje jeden-do-jeden. Oznacza to, że jedno połączenie EtherChannel łączy tylko dwa urządzenia np switch-to-switch.
`Oczywiście indywidualna konfiguracja musi być zgodna na obu urządzeniach` tj. duplex, prędkość, vlan, trunk itp.

Kanały EtherChannel warstwy 2, a kanały EtherChannel warstwy 3 na przełącznikach wielowarstwowych.

Sposoby tworzenia kanałów EtherChannel:
- `statyczny` - 
- `PAgP` - Port Aggregation Protocol, protokół Cisco, pomaga w automatycznym tworzeniu połączeń EtherChannel, zajmuje się negocjacją kształtu kanału, zarządza kanałem, wykrywa awarie fizycznych portów. 
    
    Tryby portokołu:
    - `On` - uruchamia interfejs bez protokoły PAgp, zatem nie ma też wymiany tych pakietów, strona ta nie negocjuje, po prostu umieszcza interfejs w EtherChannel bez negocjacji
    - `PAgP desirable` - stan aktywnej negocjacji, wysyła pakiety PAgP
    - `PAgP auto` - stan pasywnej negocjacji, interfejs odpowiada na pakiety PAgP, jednak nie inicjuje negocjacji PAgP
  
  Tryby muszą być z obu stron kompatybilne.

- `LACP` - Link Aggregation Control Protocol, standard `IEEE 802.3ad, 802.1AX`, również umożliwia negocjację warunków utworzenia kanału. Ze względu, że jest to standard to może pracować w środowiskach wielu dostawców
    
    Tryby protokołu LACP:
    - `On` - uruchamia interfejs bez protokołu LACP, nie ma wymiany pakietów LACP, bezwarunkowo tworzy EtherChannel
    - `LACP active` - stan aktywnej negocjacji
    - `LACP passive` - stan biernej negocjacji, port odpowiada na pakiety LACP, ale nie incjuje negocjacji pakietów LACP

    Tu też tryby muszą być zgodne.

## 3.2 Konfiguracja agregacji łączy

Co musi być zgodne, aby utworzyć EtherChannel:
- możliwość obsługi EtherChannel przez wszystkie porty wchodzące w jego skład
- zgodna szybkość i tryb duplex\`u
- interfejsy muszą być przypisane do tej samej sieci VLAN lub przypisane jako łącze trunk
- łącza trunk muszą przekazywać ten sam zakres sieci VLAN

>Po tym, gdy interfejs kanału portów jest skonfigurowany, każda zastosowana konfiguracja na interfejsie `kanału portów` również ma wpływ pojedyncze interfejsy. Jednak konfiguracje zastosowane do poszczególnych interfejsów nie wpływają na interfejs kanału portu. W związku z tym, `dokonywanie zmian w konfiguracji interfejsu`, który jest częścią łącza EtherChannel może spowodować problemy ze zgodnością interfejsów.

`EtherChannel jest domyślnie wyłączony.`

Konfiguracja EtherChannel

```
S1(config)# interface range f0/1-2
S1(config-if-range) channel-group 1 mode active

S1(config)# interface port-channel 1
S1(config-if)#
```

## 3.3 Weryfikacja i rozwiązywanie problemów z EtherChannel

(SU) - kanał w użyciu

```
S1# show interface port-channel
S1# show etherchannel summary
S1# show etherchannel port-channel
S1# show interfaces etherchannel
```

Rozwiązywanie problemów z EtherChannel:
- rozbieżności w konfiguracji interfejsów:
  - prędkość
  - duplex
  - vlan
  - trunk i przekazywane vlany
- nie kompatybilne tryby protokołów

---

<div>
<a href="chapter-02.md">Prev: Chapter 2</a>
</div>
<div align="right">
<a href="chapter-04.md">Next: Chapter 4</a>
</div>