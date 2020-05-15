### Spis treści
- [Chapter 5: Routing między sieciami VLAN](#chapter-5-routing-mi%c4%99dzy-sieciami-vlan)
  - [5.1 Konfigurowanie routingu między sieciami VLAN](#51-konfigurowanie-routingu-mi%c4%99dzy-sieciami-vlan)
  - [5.2 Rozwiązywanie problemów z routingiem między sieciami VLAN](#52-rozwi%c4%85zywanie-problem%c3%b3w-z-routingiem-mi%c4%99dzy-sieciami-vlan)
  - [5.3 Przełączanie w warstwie 3](#53-prze%c5%82%c4%85czanie-w-warstwie-3)

# Chapter 5: Routing między sieciami VLAN

## 5.1 Konfigurowanie routingu między sieciami VLAN

Sieci VLAN są wykorzystywane do segmentowania sieci przełączanych. 

>`VLAN jest domeną rozgłoszeniową`, dlatego komputery w różnych sieciach VLAN nie mogą się ze sobą komunikować bez zastosowania routingu. Aczkolwiek urządzenia które umożliwiają routowanie w warstwie 3, takie jak router lub przełącznik wielowarstwowy, mogą być wykorzystane do wykonywania routingu.

Możliwości rozwiązania zadania routingu między sieciami VLAN:
- każdy fizyczny interfejs routera należy do innej sieci VLAN, połączenia w router-switch realizowane są w trybie access
- router na patyku - jeden interfejs fizyczny kieruje ruch do wielu sieci VLAN, połączenie switch-router realizowane jest jako trunk, max 50 sieci VLAN
- przełączniki wielowarstwowe, przełączniki warstwy 3, metoda najbardziej skalowalna z wymienionych

>Przełącznik wielowarstwowy może być traktowany jako urządzenie warstwy 2, które jest posiada niektóre funkcje routingu.

Włączenie trybu trunk

```
S1 (config-if)# switchport mode trunk
```

Utworzenie i konfiguracja podinterfejsu
```
R1 (config-if)# interfece g0/0.10
R1 (config-if)# encapsulation dot1q 10
R1 (config-if)# ip address 192.168.10.1 255.255.255.0
R1 (config-if)# interface g0/0
R1 (config-if)# no shutdown
```

>Numer podinterfejsu można dowolnie konfigurować, ale zalecane jest zwykle by był równy numerowi sieci VLAN. 

>`Pamietaj`, że gdy interfejs fizyczny jest włączony za pomocą polecenia no shutdown to wszystkie skonfigurowane w nim podinterfejsy będą także włączone.

Routing między podinterfejsami działa domyślnie, nie musi być w żaden sposób konfigurowany. Do wyświetlenia dodatkowych informacji o podinterfejsach może posłużyć

```
R1 # show vlans
```

## 5.2 Rozwiązywanie problemów z routingiem między sieciami VLAN

>Podczas używania tradycyjnego modelu routingu między sieciami VLAN należy sprawdzać, czy porty przełącznika połączone z interfejsami routera są skonfigurowane do obsługi właściwych sieci VLAN. Jeżeli port przełącznika nie jest skonfigurowany dla konkretnej sieci VLAN, to urządzenia w tym VLAN-ie nie będą mogły połączyć się z interfejsem routera i dlatego nie będą mogły przesyłać danych do innych sieci VLAN.

```
S1# ping
S1# tracert
S1# show interfaces g0/0 switchport
S1# show vlan
S1# show ip interface brief
S1# show running-config
S1# show ip route
```

Najczęstsze problemy:
- problemy dotyczące portów przełącznika
- weryfikowanie konfiguracji przełącznika
- problemy dotyczące konfiguracji interfejsu
- weryfikowanie konfiguracji routera
- błędy adresacji IP i maski podsieci

## 5.3 Przełączanie w warstwie 3

Przełączniki warswt 3 charakteryzują się większą przepustowością pakietów, liczoną w milionach na sekundę w porównaniu do 100k-1mln dla tradycyjnych przełączników.

Przełączniki wielowarstowe obsługują następujące interfejsy warstwy 3:
- `routed port`, port routujący interfejs podobny do portu na routerach, jednak nie obsługują podinterfejsów
- `SVI`, switch virtual interface

Dlaczego warto konfigurować SVI:
- zapewniamy bramę domyślą dla sieci VLAN
- łączność warstwy 3 z przełącznikiem
- możliwość obsługi protokołu routingu i konfiguracji mostu

Zalety przełączników 3 warstwy:
- rozwiązanie szybsze niż router-on-a-stick
- nie trzeba łączyć się z routerem, żeby wykonać routowanie
- interfejs nie jest ograniczony do pojednyczego łącza, można wykorzystać `EtherChannel` do uzyskania większego pasma
- znacznie mniejsze opóźnienia

Jedyną wadą tych przełączników jest wysoka cena.

Konfiguracja portów rutowalnych
```
S1 (config-if)# no switchport
```

Zalety portów routowalnych:
- możemy posiadać jednocześnie SVI i porty routowalne
- przesyłanie ruchu między warstwą 2 i 3 odbywa się za pomocą rozwiązań sprzętowych co znacznie przyspiesza wykonanie routingu

`SDM` - (Cisco) Switch Database Manager

Wyświetlanie opcji szablonu SDM
```
S1 (config)# sdm prefer ?
```

Włączenie routingu na przełączniku wielowarstwowym

>Uwaga: Polecenie ip routing jest automatycznie aktywowane na routerach Cisco, natomiast adekwatne polecenie dla IPv6 ipv6 unicast-routingjest domyślnie nieaktywne w routerach i przełącznikach Cisco.

---

<div>
<a href="chapter-04.md">Prev: Chapter 4</a>
</div>
<div align="right">
<a href="chapter-06.md">Next: Chapter 6</a>
</div>