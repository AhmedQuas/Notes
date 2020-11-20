### Spis treści
- [Chapter 8: Zaawansowana konfiguracja i rozwiązywanie problemów z EIGRP](#chapter-8-zaawansowana-konfiguracja-i-rozwiązywanie-problemów-z-eigrp)
  - [8.1 Sumaryzacja tras](#81-sumaryzacja-tras)
  - [8.2 Rozgłaszanie trasy domyślnej](#82-rozgłaszanie-trasy-domyślnej)
  - [8.3 Dostrajanie interfejsów EIGRP](#83-dostrajanie-interfejsów-eigrp)
  - [8.4 Zabezpieczanie EIGRP](#84-zabezpieczanie-eigrp)
  - [8.4 Rozwiazywanie problemów zwiazanych z EIGRP](#84-rozwiazywanie-problemów-zwiazanych-z-eigrp)

# Chapter 8: Zaawansowana konfiguracja i rozwiązywanie problemów z EIGRP

## 8.1 Sumaryzacja tras

Automatyczna sumaryzacja jest domyślnie wyłączona

Włączenie automatycznej sumaryzacji

```
R1(config)# router eigrp 1
R1(config-router)# auto-summary
```

>Pakiety, pasujące do trasy z interfejsem wyjścia Null0, są odrzucane.

Ręczna sumaryzacja tras

```
R1(config)# int s0/1/0
R1(config-if)# ip summary-address eigrp 1 192.168.0.0 255.255.252.0
```

Dla IPv6
```
R1(config-if)# ipv6 summary-address eigrp 1 2001:db8:acad::/48
```

## 8.2 Rozgłaszanie trasy domyślnej

```
R1(config)# ip route 0.0.0.0 0.0.0.0 s0/0/1
R1(config)# router eigrp 1
R1(config-router)# redistribute static
```

Oznaczenie w tablicy routingu odnoszące się do EIGRP:
- `D` - trasa pozyskana z aktualizacji routingu EIGRP
- `*` - trasa kandyduje do roli trasy domyślnej
- `EX` -trasa jest zewnętrzną EIGRP

Dla IPv6

```
R1(config)# ip route ::/0 s0/0/1
R1(config)# ipv6 router eigrp 1
R1(config-router)# redistribute static
```

## 8.3 Dostrajanie interfejsów EIGRP

>Domyślnie EIGRP używa jedynie do 50% przepustowości interfejsu dla informacji EIGRP. Zapobiega to nadużywaniu przez proces EIGRP łącza ze szkodą dla routingu normalnego ruchu.

Konfiguracja procentu przepustowości

```
R1(config-if)# ip bandwidth-percent eigrp as_number 50(%)
```

Dla IPv6

```
R1(config-if)# ipv6 bandwidth-percent eigrp as_number 50(%)
```

Przedziały czasowe między pakietami Hello i licznik Hold

```
R1(config-if)# ip hello-interval eigrp 1 10
R1(config-if)# ip hold-time eigrp 1 30
```

hold-time > hello-interval

>Interwał czasowy Hello `nie musi być taki sam dla obu routerów`, aby utworzyć relację przylegania(ang. adjacency) EIGRP.

Dla ipv6 analogicznie jak w przykładzie z bandwidth-percent.

Tryby przełączania pakietów w przypadku rozkładania obciążenia:
- `procesowe`, process switching - równoważenie obciążenia na ścieżkach równych kosztów następuje dla każdego pakietu z osobna
- `szybkie`, fast switching - równoważenie obciążenia na ścieżkach równych kosztów następuje na podstawie miejsca przeznaczenia

>Cisco Express Forwarding (`CEF`) może wykonywać równoważenie obciążenia zarówno dla poszczególnych pakietów jak i dla miejsc przeznaczenia.

>`Domyślnie` system Cisco IOS pozwala na rozkładanie obciążenia przy użyciu maksymalnie `czterech` tras o równym koszcie

Zmiana liczby tras na których można rozkładać obciążenie.

```
R1(config-router)# maximum-paths [1-32] 
```

>Jeżeli wartość maximum-paths wynosi 1, to równoważenie obciążenia jest wyłączone.

Nierównomierne rozkładanie obciążenia tras

```
R1(config-router)# variance [number]
```

Rozdzielanie ruchu proporcjonalnie do kosztu trasy

```
R1(config-router)# traffic-share balanced
```

## 8.4 Zabezpieczanie EIGRP

>Administratorzy sieci muszą mieć świadomość, że routery są narażone na ataki tak samo jak urządzenia użytkowników końcowych. Korzystając z oprogramowania, takiego jak np. Wireshark, można odczytać informacje przesyłane pomiędzy routerami. 

Konsekwencje ataków polegających na fałszowaniu informacji routingu:
- przekierowanie ruchu w cele stworzenia pętli routingu,
- przekierowanie ruchu do monitorowania na niezabezpieczonej lini,
- przekierowanie ruchu w celu jego odrzucenia


Uwierzytelnianie MD5 pozwala routerom porównać podpisy, które powinny być takie same, potwierdzając swoje pochodzenie(legalne źródło).

Konfiguracja uwierzytelniania MD5:
- tworzenie pęku kluczy i klucza,
- konfiguracja uwierzytelniania EIGRP do używania tego pęku kluczy

```
R1(config)# key chain name-of-chain
R1(config-keychain)# key key-id
R1(config-keychain-key)# key-string key-string-text

R1(config)# interface s0/0/0
R1(config-if)# ip authentication mode eigrp 1 md5
R1(config-if)# ip authentication key-chain eigrp 1 name-of-chain
```

## 8.4 Rozwiazywanie problemów zwiazanych z EIGRP

Przyczyny problemów z ustanowieniem przyległości z sasiadami EIGRP:
- wyłaczony interfejs,
- routery nie sa w tym samym AS,
- interfejsy nie zostały dołaczone do odpowiedniego procesu EIGRP,
- jeden z interfejsów jest skonfigurowany jako pasywny,
- niepoprawnie skonfigurowane uwierzytelnienie,
- niezgodne wartości parametrów K, z których jest liczona metryka

Relacja sasiedzka została nawiazana poprawnie, ale wciaż nie ma połaczenia:
- sieci nie zostały rozgłoszone,
- interfejsy pasywne(to nie tylko LAN, ale też bezpieczeństwo),
- blokady na ACL,
- automatyczna sumaryzacja powoduje niespójność routingu w sieciach nie ciagłych
- nieodpowiednia przepustowość ustawiona na interfejsie

>Pojawiający się komunikat o treści `not on common subnet` oznacza, że na jednym z dwóch interfejsów sąsiedzkich EIGRP skonfigurowano nieprawidłowy adres IPv4.

>Określenie “IP Routing is NSF aware”, widoczne w górnej części wyniku, odnosi się do techniki przekazywania ciągłego (ang. Nonstop Forwarding, NSF). Funkcja ta pozwala sparowanemu routerowi EIGRP przejąć informacje routingu rozgłaszane przez router, na którym wystąpiła awaria i kontynuować pracę przy użyciu tych informacji, do momentu aż router ten uzyska pełną funkcjonalność i będzie w stanie wymieniać informacje routingu.

```
R1# show ip eigrp neighbors
R1# show ip route {eigrp}
R1# show ip protocols
R1# show ip interface brief
R1# show ip eigrp interfaces
R1# show running-config | section eigrp 1
```

---

<div>
<a href="chapter-07.md">Prev: Chapter 7</a>
</div>
<div align="right">
<a href="chapter-09.md">Next: Chapter 9</a>
</div>