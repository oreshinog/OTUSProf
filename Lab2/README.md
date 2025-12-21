# Лабораторная работа. Проектирование сети
[**Файл EVE-NG**](https://github.com/oreshinog/OTUSProf/blob/main/Lab2/EVE/otusprof_lab2.zip)
# Топология
![Топология](https://github.com/oreshinog/OTUSProf/blob/main/Lab2/Img/Img00.png)

# Цели

Настроить политику маршрутизации в офисе Чокурдах;
Распределить трафик между 2 линками.

В этой самостоятельной работе мы ожидаем, что вы самостоятельно:

Настроите политику маршрутизации для сетей офиса.
Распределите трафик между двумя линками с провайдером.
Настроите отслеживание линка через технологию IP SLA.(только для IPv4)
Настройте для офиса Лабытнанги маршрут по-умолчанию.
План работы и изменения зафиксированы в документации .

## 1. Чокурдах

Траффик 2-х VLAN распределён между двумя линкам, у одного приоритетен e0/0, у второго - e0/1, настроено отслеживание линка, в случае потери связи - траффик пойдёт через оставшийся интерфейс.

```
ip access-list standard VLAN2-WAN-OUT
 permit 10.5.0.0 0.0.0.255
ip access-list standard VLAN3-WAN-OUT
 permit 10.5.1.0 0.0.0.255

ip sla 1
 icmp-echo 172.16.0.69 source-ip 172.16.0.70
 frequency 5
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-echo 172.16.0.65 source-interface Ethernet0/1
 frequency 5
ip sla schedule 2 life forever start-time now

track 1 ip sla 1 reachability
 delay down 10 up 5

track 2 ip sla 2 reachability
 delay down 10 up 5

route-map PBR permit 10
 description balance VLAN2
 match ip address VLAN2-WAN-OUT
 set ip next-hop verify-availability 172.16.0.69 1 track 1
 set ip next-hop verify-availability 172.16.0.65 2 track 2

route-map PBR permit 15
 description balance VLAN3
 match ip address VLAN3-WAN-OUT
 set ip next-hop verify-availability 172.16.0.65 1 track 2
 set ip next-hop verify-availability 172.16.0.69 2 track 1
```

```
R28# sh route-map
route-map PBR, permit, sequence 10
  Match clauses:
    ip address (access-lists): VLAN2-WAN-OUT
  Set clauses:
    ip next-hop verify-availability 172.16.0.69 1 track 1  [down]
    ip next-hop verify-availability 172.16.0.65 2 track 2  [down]
  Policy routing matches: 0 packets, 0 bytes
route-map PBR, permit, sequence 15
  Match clauses:
    ip address (access-lists): VLAN3-WAN-OUT
  Set clauses:
    ip next-hop verify-availability 172.16.0.65 1 track 2  [down]
    ip next-hop verify-availability 172.16.0.69 2 track 1  [down]
  Policy routing matches: 0 packets, 0 bytes
```


## 2. Лабытнанги

```
ip route 0.0.0.0 0.0.0.0 172.16.0.61
```