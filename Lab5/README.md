# Лабораторная работа. EIGRP
[**Файл EVE-NG**](https://github.com/oreshinog/OTUSProf/blob/main/Lab5/EVE/otusprof_lab5.zip)
# Топология
![Топология](https://github.com/oreshinog/OTUSProf/blob/main/Lab5/Img/Img00.png)

# Цели

Настроить EIGRP в С.-Петербург;
Использовать named EIGRP

Описание/Пошаговая инструкция выполнения домашнего задания:
В офисе С.-Петербург настроить EIGRP.
R32 получает только маршрут по умолчанию.
R16-17 анонсируют только суммарные префиксы.
Использовать EIGRP named-mode для настройки сети.

## 1.Настройка named EIGRP

```
R18(config)#router eigrp SPB
R18(config-router)#address-family ipv4 unicast autonomous-system 1
R18(config-router-af)#eigrp router-id 10.254.6.1
R18(config-router-af)#network 0.0.0.0
R18(config-router-af)#af-interface default
R18(config-router-af-interface)#shutdown
R18(config-router-af-interface)#exi
R18(config-router-af)#af-int e0/0
R18(config-router-af-interface)#no shutdown
R18(config-router-af-interface)#af-int e0/1
R18(config-router-af-interface)#no shutdown
R18(config-router-af-interface)#exi
R18(config-router-af)#topology base
R18(config-router-af-topology)#redistribute static
R18(config-router-af-topology)#exi
R18(config-router-af)#exi
```

```
R17(config)#router eigrp SPB
R17(config-router)#address-family ipv4 unicast autonomous-system 1
R17(config-router-af)#eigrp router-id 10.254.6.2
R17(config-router-af)#network 0.0.0.0
R17(config-router-af)#af-int e0/1
R17(config-router-af-interface)#summary-address 10.6.0.0 255.255.0.0
R17(config-router-af-interface)#summary-address 172.16.0.0 255.255.255.0
R17(config-router-af-interface)#summary-address 10.254.6.0 255.255.255.0
```

```
SW9(config)#router eigrp SPB
SW9(config-router)#address-family ipv4 unicast autonomous-system 1
SW9(config-router-af)#eigrp router-id 10.254.6.5
SW9(config-router-af)#network 0.0.0.0
SW9(config-router-af)#af-int def
SW9(config-router-af-interface)#shut
SW9(config-router-af)#af-int e1/0
SW9(config-router-af-interface)#no shut
SW9(config-router-af)#af-int e0/3
SW9(config-router-af-interface)#no shut
SW9(config-router-af)#af-int loop1
SW9(config-router-af-interface)#no shut
```

```
SW10(config)#router eigrp SPB
SW10(config-router)#address-family ipv4 unicast autonomous-system 1
SW10(config-router-af)#eigrp router-id 10.254.6.5
SW10(config-router-af)#network 0.0.0.0
SW10(config-router-af)#af-int def
SW10(config-router-af-interface)#shut
SW10(config-router-af)#af-int e1/0
SW10(config-router-af-interface)#no shut
SW10(config-router-af)#af-int e0/3
SW10(config-router-af-interface)#no shut
SW10(config-router-af)#af-int loop1
SW10(config-router-af-interface)#no shut
```

```
R16(config)#ip prefix-list default_only seq 5 permit 0.0.0.0/0
R16(config)#router eigrp SPB
R16(config-router)#address-family ipv4 unicast autonomous-system 1
R16(config-router-af)#topology base
R16(config-router-af-topology)#distribute-list prefix default_only out e0/3
R16(config-router-af-topology)#exi
R16(config-router-af)#eigrp router-id 10.254.6.3
R16(config-router-af)#network 0.0.0.0
R16(config-router-af)#af-int e0/1
R16(config-router-af-interface)#summary-address 10.6.0.0 255.255.0.0
R16(config-router-af-interface)#summary-address 172.16.0.0 255.255.255.0
R16(config-router-af-interface)#summary-address 10.254.6.0 255.255.255.0

```

```
R32(config)#router eigrp SPB
R32(config-router)#address-family ipv4 unicast autonomous-system 1
R32(config-router-af)#eigrp router-id 10.254.6.4
R32(config-router-af)#network 0.0.0.0
```

## 2.Итог

### R32 получает только маршрут по умолчанию.

```
R32#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.89 to network 0.0.0.0

D*EX  0.0.0.0/0 [170/2048000] via 172.16.0.89, 00:33:09, Ethernet0/0
```

### R16-17 анонсируют только суммарные префиксы.

```
R18#sh eigrp address-family ipv4 1 topology
EIGRP-IPv4 VR(SPB) Topology Table for AS(1)/ID(10.254.6.1)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - reply Status, s - sia Status

P 172.16.0.80/30, 1 successors, FD is 131072000
        via Connected, Ethernet0/1
P 172.16.0.0/24, 2 successors, FD is 196608000
        via 172.16.0.82 (196608000/131072000), Ethernet0/1
        via 172.16.0.86 (196608000/131072000), Ethernet0/0
P 0.0.0.0/0, 1 successors, FD is 131072000
        via Rstatic (131072000/0)
P 10.254.6.0/24, 2 successors, FD is 131153920
        via 172.16.0.82 (131153920/163840), Ethernet0/1
        via 172.16.0.86 (131153920/163840), Ethernet0/0
P 172.16.0.84/30, 1 successors, FD is 131072000
        via Connected, Ethernet0/0
```
