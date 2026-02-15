# Лабораторная работа. IS-IS
[**Файл EVE-NG**](https://github.com/oreshinog/OTUSProf/blob/main/Lab3/EVE/otusprof_lab3.zip)
# Топология
![Топология](https://github.com/oreshinog/OTUSProf/blob/main/Lab3/Img/Img00.png)

# Цели

Настроить OSPF офисе Москва
Разделить сеть на зоны
Настроить фильтрацию между зонами

Описание/Пошаговая инструкция выполнения домашнего задания:
Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
Настройка для IPv6 повторяет логику IPv4.
План работы и изменения зафиксированы в документации .

## 1. Зона backbone

Все L3 интерфесы установлены в режим Point-to-Point, между маршрутизаторами проброшен дополнительный линк через интерфейс e1/0, для установления соседства в зоне.

|Объект|Устройство|Порт|IP-адрес|Маска подсети|
|--|--|--|--|--|
|Москва|R14|e1/0|172.16.0.125|255.255.255.252
|Москва|R15|e1/0|172.16.0.126|255.255.255.252

Настройки OSPF маршрутизаторов

```
R14(config)#ip route 0.0.0.0 0.0.0.0 172.16.0.25

R14#sh run | s r o
router ospf 1
 router-id 10.254.0.1
 area 101 stub no-summary
 network 10.254.0.1 0.0.0.0 area 0
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.12 0.0.0.3 area 0
 network 172.16.0.16 0.0.0.3 area 101
 network 172.16.0.124 0.0.0.3 area 0
 default-information originate 
```

```
R15(config)#ip route 0.0.0.0 0.0.0.0 172.16.0.33

R15#sh run | s r o
router ospf 1
 router-id 10.254.0.2
 area 102 filter-list prefix Filter101 in
 network 10.254.0.2 0.0.0.0 area 0
 network 172.16.0.4 0.0.0.3 area 0
 network 172.16.0.8 0.0.0.3 area 0
 network 172.16.0.20 0.0.0.3 area 102
 network 172.16.0.124 0.0.0.3 area 0
 default-information originate 
```

## 1. Зона 10

Все L3 интерфесы установлены в режим Point-to-Point, между маршрутизаторами проброшен дополнительный линк через интерфейс e1/0, для установления соседства в зоне.

|Объект|Устройство|Порт|IP-адрес|Маска подсети|
|--|--|--|--|--|
|Москва|R12|e1/0|172.16.0.129|255.255.255.252
|Москва|R13|e1/0|172.16.0.130|255.255.255.252

Настройки OSPF маршрутизаторов и L3 коммутаторов

```
R12#sh run | s r o
router ospf 1
 router-id 10.254.0.3
 network 10.254.0.3 0.0.0.0 area 10
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.4 0.0.0.3 area 0
 network 172.16.0.92 0.0.0.3 area 10
 network 172.16.0.96 0.0.0.3 area 10
 network 172.16.0.128 0.0.0.3 area 10
```

```
R13#sh run | s r o
router ospf 1
 router-id 10.254.0.4
 network 10.254.0.4 0.0.0.0 area 10
 network 172.16.0.8 0.0.0.3 area 0
 network 172.16.0.12 0.0.0.3 area 0
 network 172.16.0.100 0.0.0.3 area 10
 network 172.16.0.104 0.0.0.3 area 10
 network 172.16.0.128 0.0.0.3 area 10
```

```
SW4#sh run | s r o
router ospf 1
 network 10.0.0.0 0.0.0.255 area 10
 network 10.0.1.0 0.0.0.255 area 10
 network 10.254.0.7 0.0.0.0 area 10
 network 172.16.0.92 0.0.0.3 area 10
 network 172.16.0.100 0.0.0.3 area 10
```

```
SW5#sh run | s r o
router ospf 1
 network 10.0.0.0 0.0.0.255 area 10
 network 10.0.1.0 0.0.0.255 area 10
 network 10.254.0.8 0.0.0.0 area 10
 network 172.16.0.96 0.0.0.3 area 10
 network 172.16.0.104 0.0.0.3 area 10
```

Маршруты, на примере R12 и SW4

```
R12#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.6 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 172.16.0.6, 00:05:27, Ethernet0/3
                [110/1] via 172.16.0.2, 00:05:43, Ethernet0/2
      10.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
O        10.0.0.0/24 [110/11] via 172.16.0.98, 01:04:04, Ethernet0/1
                     [110/11] via 172.16.0.94, 01:05:17, Ethernet0/0
O        10.0.1.0/24 [110/11] via 172.16.0.98, 01:04:04, Ethernet0/1
                     [110/11] via 172.16.0.94, 01:05:07, Ethernet0/0
C        10.254.0.0/16 is directly connected, Loopback1
O        10.254.0.1/32 [110/11] via 172.16.0.2, 00:12:23, Ethernet0/2
L        10.254.0.3/32 is directly connected, Loopback1
O        10.254.0.4/32 [110/11] via 172.16.0.130, 01:31:33, Ethernet1/0
O IA     10.254.0.5/32 [110/21] via 172.16.0.2, 00:12:23, Ethernet0/2
O IA     10.254.0.6/32 [110/21] via 172.16.0.6, 00:12:23, Ethernet0/3
O        10.254.0.7/32 [110/11] via 172.16.0.94, 01:28:09, Ethernet0/0
O        10.254.0.8/32 [110/11] via 172.16.0.98, 01:25:41, Ethernet0/1
      172.16.0.0/16 is variably subnetted, 17 subnets, 2 masks
C        172.16.0.0/30 is directly connected, Ethernet0/2
L        172.16.0.1/32 is directly connected, Ethernet0/2
C        172.16.0.4/30 is directly connected, Ethernet0/3
L        172.16.0.5/32 is directly connected, Ethernet0/3
O        172.16.0.8/30 [110/20] via 172.16.0.6, 01:36:54, Ethernet0/3
O        172.16.0.12/30 [110/20] via 172.16.0.2, 00:12:23, Ethernet0/2
O IA     172.16.0.16/30 [110/20] via 172.16.0.2, 00:12:23, Ethernet0/2
O IA     172.16.0.20/30 [110/20] via 172.16.0.6, 00:12:23, Ethernet0/3
C        172.16.0.92/30 is directly connected, Ethernet0/0
L        172.16.0.93/32 is directly connected, Ethernet0/0
C        172.16.0.96/30 is directly connected, Ethernet0/1
L        172.16.0.97/32 is directly connected, Ethernet0/1
O        172.16.0.100/30 [110/20] via 172.16.0.130, 01:38:54, Ethernet1/0
                         [110/20] via 172.16.0.94, 01:28:09, Ethernet0/0
O        172.16.0.104/30 [110/20] via 172.16.0.130, 01:38:54, Ethernet1/0
                         [110/20] via 172.16.0.98, 01:25:41, Ethernet0/1
O        172.16.0.124/30 [110/20] via 172.16.0.6, 01:37:55, Ethernet0/3
                         [110/20] via 172.16.0.2, 00:12:23, Ethernet0/2
C        172.16.0.128/30 is directly connected, Ethernet1/0
L        172.16.0.129/32 is directly connected, Ethernet1/0
```


```
SW4#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.101 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 172.16.0.101, 00:00:01, Ethernet1/1
                [110/1] via 172.16.0.93, 00:00:01, Ethernet1/0
      10.0.0.0/8 is variably subnetted, 12 subnets, 3 masks
C        10.0.0.0/24 is directly connected, Vlan2
L        10.0.0.252/32 is directly connected, Vlan2
C        10.0.1.0/24 is directly connected, Vlan3
L        10.0.1.252/32 is directly connected, Vlan3
C        10.254.0.0/16 is directly connected, Loopback1
O IA     10.254.0.1/32 [110/21] via 172.16.0.101, 00:07:02, Ethernet1/1
                       [110/21] via 172.16.0.93, 00:07:02, Ethernet1/0
O        10.254.0.3/32 [110/11] via 172.16.0.93, 01:22:51, Ethernet1/0
O        10.254.0.4/32 [110/11] via 172.16.0.101, 01:23:01, Ethernet1/1
O IA     10.254.0.5/32 [110/31] via 172.16.0.101, 00:07:02, Ethernet1/1
                       [110/31] via 172.16.0.93, 00:07:02, Ethernet1/0
O IA     10.254.0.6/32 [110/31] via 172.16.0.101, 00:07:02, Ethernet1/1
                       [110/31] via 172.16.0.93, 00:07:02, Ethernet1/0
L        10.254.0.7/32 is directly connected, Loopback1
O        10.254.0.8/32 [110/2] via 10.0.1.253, 00:58:43, Vlan3
                       [110/2] via 10.0.0.253, 00:58:43, Vlan2
      172.16.0.0/16 is variably subnetted, 14 subnets, 2 masks
O IA     172.16.0.0/30 [110/20] via 172.16.0.93, 01:22:51, Ethernet1/0
O IA     172.16.0.4/30 [110/20] via 172.16.0.93, 01:22:51, Ethernet1/0
O IA     172.16.0.8/30 [110/20] via 172.16.0.101, 01:23:01, Ethernet1/1
O IA     172.16.0.12/30 [110/20] via 172.16.0.101, 01:23:01, Ethernet1/1
O IA     172.16.0.16/30 [110/30] via 172.16.0.101, 00:07:02, Ethernet1/1
                        [110/30] via 172.16.0.93, 00:07:02, Ethernet1/0
O IA     172.16.0.20/30 [110/30] via 172.16.0.101, 00:07:02, Ethernet1/1
                        [110/30] via 172.16.0.93, 00:07:02, Ethernet1/0
C        172.16.0.92/30 is directly connected, Ethernet1/0
L        172.16.0.94/32 is directly connected, Ethernet1/0
O        172.16.0.96/30 [110/11] via 10.0.1.253, 00:58:43, Vlan3
                        [110/11] via 10.0.0.253, 00:58:43, Vlan2
C        172.16.0.100/30 is directly connected, Ethernet1/1
L        172.16.0.102/32 is directly connected, Ethernet1/1
O        172.16.0.104/30 [110/11] via 10.0.1.253, 00:58:43, Vlan3
                         [110/11] via 10.0.0.253, 00:58:43, Vlan2
O IA     172.16.0.124/30 [110/30] via 172.16.0.101, 01:23:01, Ethernet1/1
                         [110/30] via 172.16.0.93, 01:22:51, Ethernet1/0
O        172.16.0.128/30 [110/20] via 172.16.0.101, 01:23:01, Ethernet1/1
                         [110/20] via 172.16.0.93, 01:22:51, Ethernet1/0

```


## 1. Зона 101

Настройка маршрутизатора
```
R19#sh run | s r o
router ospf 1
 router-id 10.254.0.5
 area 101 stub no-summary
 network 10.254.0.5 0.0.0.0 area 101
 network 172.16.0.16 0.0.0.3 area 101
```

Маршруты

```
R19#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.18 to network 0.0.0.0

O*IA  0.0.0.0/0 [110/11] via 172.16.0.18, 00:15:09, Ethernet0/0
      10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        10.254.0.0/16 is directly connected, Loopback1
L        10.254.0.5/32 is directly connected, Loopback1
      172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
C        172.16.0.16/30 is directly connected, Ethernet0/0
L        172.16.0.17/32 is directly connected, Ethernet0/0

```


## 1. Зона 102

Настройки маршрутизаторов

```
R20#sh run | s r o
router ospf 1
 network 10.254.0.6 0.0.0.0 area 102
 network 172.16.0.20 0.0.0.3 area 102
```

```
ip prefix-list Filter101 seq 5 deny 172.16.0.16/30
ip prefix-list Filter101 seq 10 deny 10.254.0.5/32
ip prefix-list Filter101 seq 15 permit 0.0.0.0/0 le 32

R15#sh run | s r o
router ospf 1
 router-id 10.254.0.2
 area 102 filter-list prefix Filter101 in
 network 10.254.0.2 0.0.0.0 area 0
 network 172.16.0.4 0.0.0.3 area 0
 network 172.16.0.8 0.0.0.3 area 0
 network 172.16.0.20 0.0.0.3 area 102
 network 172.16.0.124 0.0.0.3 area 0
 default-information originate 
```

Маршруты

```
R20#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is 172.16.0.22 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 172.16.0.22, 00:15:06, Ethernet0/0
      10.0.0.0/8 is variably subnetted, 9 subnets, 3 masks
O IA     10.0.0.0/24 [110/31] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     10.0.1.0/24 [110/31] via 172.16.0.22, 00:22:06, Ethernet0/0
C        10.254.0.0/16 is directly connected, Loopback1
O IA     10.254.0.1/32 [110/21] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     10.254.0.3/32 [110/21] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     10.254.0.4/32 [110/21] via 172.16.0.22, 00:22:06, Ethernet0/0
L        10.254.0.6/32 is directly connected, Loopback1
O IA     10.254.0.7/32 [110/31] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     10.254.0.8/32 [110/31] via 172.16.0.22, 00:22:06, Ethernet0/0
      172.16.0.0/16 is variably subnetted, 12 subnets, 2 masks
O IA     172.16.0.0/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.4/30 [110/20] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.8/30 [110/20] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.12/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
C        172.16.0.20/30 is directly connected, Ethernet0/0
L        172.16.0.21/32 is directly connected, Ethernet0/0
O IA     172.16.0.92/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.96/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.100/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.104/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.124/30 [110/20] via 172.16.0.22, 00:22:06, Ethernet0/0
O IA     172.16.0.128/30 [110/30] via 172.16.0.22, 00:22:06, Ethernet0/0
```