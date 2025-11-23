# Лабораторная работа. Проектирование сети
[**Файл Cisco Packet Tracer**](https://github.com/oreshinog/OTUS/blob/main/Labs/Lab02/Pkt/lab02.pkt)
# Топология
![Топология](https://github.com/oreshinog/OTUSProf/blob/main/Lab1/Img/Img00.png)

# Цели
В данной самостоятельной работе необходимо распланировать адресное пространство
Настроить IP на всех активных портах для дальнейшей работы над проектом
Адресное пространство должно быть задокументировано

Разработаете и задокументируете адресное пространство для лабораторного стенда.
Настроите ip адреса на каждом активном порту
Настроите каждый VPC в каждом офисе в своем VLAN.
Настроите VLAN/Loopbackup interface управления для сетевых устройств
Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано
Используете IPv4. IPv6 по желанию

## 1. Адресация

|Объект|Устройство|Порт|IP-адрес|Маска подсети|
|--|--|--|--|--|
|Москва|R12|e0/2|172.16.0.1|255.255.255.252
|Москва|R14|e0/0|172.16.0.2|255.255.255.252
|Москва|R12|e0/3|172.16.0.5|255.255.255.252
|Москва|R15|e0/1|172.16.0.6|255.255.255.252
|Москва|R13|e0/2|172.16.0.9|255.255.255.252
|Москва|R15|e0/0|172.16.0.10|255.255.255.252
|Москва|R13|e0/3|172.16.0.13|255.255.255.252
|Москва|R14|e0/1|172.16.0.14|255.255.255.252
|Москва|R19|e0/0|172.16.0.17|255.255.255.252
|Москва|R14|e0/3|172.16.0.18|255.255.255.252
|Москва|R20|e0/0|172.16.0.21|255.255.255.252
|Москва|R15|e0/3|172.16.0.22|255.255.255.252
|Киторн|R22|e0/0|172.16.0.25|255.255.255.252
|Москва|R14|e0/2|172.16.0.26|255.255.255.252
|Киторн|R22|e0/1|172.16.0.29|255.255.255.252
|Ламас|R21|e0/1|172.16.0.30|255.255.255.252
|Ламас|R21|e0/0|172.16.0.33|255.255.255.252
|Москва|R15|e0/2|172.16.0.34|255.255.255.252
|Киторн|R22|e0/2|172.16.0.37|255.255.255.252
|Триада|R23|e0/0|172.16.0.38|255.255.255.252
|Ламас|R21|e0/2|172.16.0.41|255.255.255.252
|Триада|R24|e0/0|172.16.0.42|255.255.255.252
|Триада|R24|e0/2|172.16.0.45|255.255.255.252
|Триада|R23|e0/2|172.16.0.46|255.255.255.252
|Триада|R23|e0/1|172.16.0.49|255.255.255.252
|Триада|R25|e0/0|172.16.0.50|255.255.255.252
|Триада|R25|e0/2|172.16.0.53|255.255.255.252
|Триада|R26|e0/2|172.16.0.54|255.255.255.252
|Триада|R26|e0/0|172.16.0.57|255.255.255.252
|Триада|R24|e0/1|172.16.0.58|255.255.255.252
|Триада|R25|e0/1|172.16.0.61|255.255.255.252
|Лабытнанги|R27|e0/0|172.16.0.62|255.255.255.252
|Триада|R25|e0/3|172.16.0.65|255.255.255.252
|Чокурдах|R28|e0/1|172.16.0.66|255.255.255.252
|Триада|R26|e0/1|172.16.0.69|255.255.255.252
|Чокурдах|R28|e0/0|172.16.0.70|255.255.255.252
|Триада|R24|e0/3|172.16.0.73|255.255.255.252
|С.-Петербург|R18|e0/2|172.16.0.74|255.255.255.252
|Триада|R26|e0/3|172.16.0.77|255.255.255.252
|С.-Петербург|R18|e0/3|172.16.0.78|255.255.255.252
|С.-Петербург|R18|e0/1|172.16.0.81|255.255.255.252
|С.-Петербург|R17|e0/1|172.16.0.82|255.255.255.252
|С.-Петербург|R18|e0/0|172.16.0.85|255.255.255.252
|С.-Петербург|R16|e0/1|172.16.0.86|255.255.255.252
|С.-Петербург|R16|e0/3|172.16.0.89|255.255.255.252
|С.-Петербург|R32|e0/0|172.16.0.90|255.255.255.252
|Москва|R12|e0/0|172.16.0.93|255.255.255.252
|Москва|SW4|e1/0|172.16.0.94|255.255.255.252
|Москва|R12|e0/1|172.16.0.97|255.255.255.252
|Москва|SW5|e1/1|172.16.0.98|255.255.255.252
|Москва|R13|e0/1|172.16.0.101|255.255.255.252
|Москва|SW4|e1/1|172.16.0.102|255.255.255.252
|Москва|R13|e0/0|172.16.0.105|255.255.255.252
|Москва|SW5|e1/0|172.16.0.106|255.255.255.252
|Москва|SW4|vlanif2|10.0.0.252|255.255.255.0
|Москва|SW5|vlanif2|10.0.0.253|255.255.255.0
|Москва|SW4|vlanif3|10.0.1.252|255.255.255.0
|Москва|SW5|vlanif3|10.0.1.253|255.255.255.0
|Москва|SW4|vlanif3-VRRP|10.0.1.254|255.255.255.255
|Москва|SW5|vlanif3-VRRP|10.0.1.254|255.255.255.255
|Москва|SW4|vlanif2-VRRP|10.0.0.254|255.255.255.255
|Москва|SW5|vlanif2-VRRP|10.0.0.254|255.255.255.255
|Москва|SW3|vlanif2|10.0.0.249|255.255.255.0
|Москва|SW2|vlanif2|10.0.0.248|255.255.255.0
|С.-Петербург|R17|e0/0|172.16.0.109|255.255.255.252
|С.-Петербург|SW9|e0/3|172.16.0.110|255.255.255.252
|С.-Петербург|R17|e0/2|172.16.0.113|255.255.255.252
|С.-Петербург|SW10|e1/0|172.16.0.114|255.255.255.252
|С.-Петербург|R16|e0/2|172.16.0.117|255.255.255.252
|С.-Петербург|SW9|e1/0|172.16.0.118|255.255.255.252
|С.-Петербург|R16|e0/0|172.16.0.121|255.255.255.252
|С.-Петербург|SW10|e0/3|172.16.0.122|255.255.255.252
|С.-Петербург|SW10|vlanif2|10.6.0.252|255.255.255.0
|С.-Петербург|SW9|vlanif2|10.6.0.253|255.255.255.0
|С.-Петербург|SW9|vlanif2-VRRP|10.6.0.254|255.255.255.255
|С.-Петербург|SW10|vlanif2-VRRP|10.6.0.254|255.255.255.255
|С.-Петербург|SW9|vlanif3-VRRP|10.6.1.254|255.255.255.255
|С.-Петербург|SW10|vlanif3-VRRP|10.6.1.254|255.255.255.255
|Чокурдах|R28|e0/2.2|10.5.0.254|255.255.255.0
|Чокурдах|R28|e0/2.3|10.5.1.254|255.255.255.0
|Чокурдах|SW29|vlanif2|10.5.0.253|255.255.255.0

###Адреса клиентов

|Объект|Устройство|Порт|IP-адрес|Маска подсети|Шлюз|
|--|--|--|--|--|--|
|Москва|VPC1|eth0(vlan3)|10.0.1.1|255.255.255.0|10.0.1.254|
|Москва|VPC7|eth0(vlan3)|10.0.1.2|255.255.255.0|10.0.1.254|
|С.-Петербург|VPC8|eth0(vlan3)|10.6.1.1|255.255.255.0|10.1.1.254|
|С.-Петербург|VPC|eth0(vlan3)|10.6.1.2|255.255.255.0|10.1.1.254|
|Чокурдах|VPC30|eth0(vlan3)|10.5.1.1|255.255.255.0|10.2.1.254|
|Чокурдах|VPC31|eth0(vlan3)|10.5.1.2|255.255.255.0|10.2.1.254|

###Loopback адреса

|Объект|Устройство|IP-адрес|Маска подсети
|--|--|--|--|
|Москва|R14|10.254.0.1|255.255.0.0
|Москва|R15|10.254.0.2|255.255.0.0
|Москва|R12|10.254.0.3|255.255.0.0
|Москва|R13|10.254.0.4|255.255.0.0
|Москва|R19|10.254.0.5|255.255.0.0
|Москва|R20|10.254.0.6|255.255.0.0
|Москва|SW4|10.254.0.7|255.255.0.0
|Москва|SW5|10.254.0.8|255.255.0.0
|Киторн|R22|10.254.1.1|255.255.0.0
|Ламас|R22|10.254.2.1|255.255.0.0
|Триада|R23|10.254.3.1|255.255.0.0
|Триада|R24|10.254.3.2|255.255.0.0
|Триада|R25|10.254.3.3|255.255.0.0
|Триада|R26|10.254.3.4|255.255.0.0
|Лабытнанги|R27|10.254.4.1|255.255.0.0
|Чокурдах|R28|10.254.5.1|255.255.0.0
|С.-Петербург|R18|10.254.6.1|255.255.0.0
|С.-Петербург|R17|10.254.6.2|255.255.0.0
|С.-Петербург|R16|10.254.6.3|255.255.0.0
|С.-Петербург|R32|10.254.6.4|255.255.0.0
|С.-Петербург|SW9|10.254.6.5|255.255.0.0
|С.-Петербург|SW10|10.254.6.6|255.255.0.0

## 2. Типовая настройка оборудования

### Маршрутизатор

```
R14#sh run
Building configuration...

Current configuration : 1258 bytes
!
! Last configuration change at 09:28:17 UTC Sun Nov 23 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname R14
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$dVpI$aSCU3zmyuyn/FksMaM3kM0
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
no ip domain lookup
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
redundancy
!
interface Loopback1
 ip address 10.254.0.1 255.255.0.0
!
interface Ethernet0/0
 description R12-0/2
 ip address 172.16.0.2 255.255.255.252
!
interface Ethernet0/1
 description R13-0/3
 ip address 172.16.0.14 255.255.255.252
!
interface Ethernet0/2
 description R22-0/0
 ip address 172.16.0.26 255.255.255.252
!
interface Ethernet0/3
 description R19-0/0
 ip address 172.16.0.18 255.255.255.252
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!

control-plane
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 104D000A0618
 logging synchronous
 login
line aux 0
line vty 0 4
 password 7 05080F1C2243
 login
 transport input none
!
!
end

```

### Маршрутизатор Чокурдах

```
R28#sh run
Building configuration...

Current configuration : 1602 bytes
!
! Last configuration change at 11:02:58 UTC Sun Nov 23 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname R28
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$/0TT$7FdKbNoS0cIEFx6HrGuJ60
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
no ip domain lookup
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
redundancy
!
interface Loopback1
 ip address 10.254.5.1 255.255.0.0
!
interface Ethernet0/0
 description R26-0/1
 ip address 172.16.0.70 255.255.255.252
!
interface Ethernet0/1
 description R25-0/3
 ip address 172.16.0.66 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/2.2
 description Management
 encapsulation dot1Q 2
 ip address 10.5.0.254 255.255.255.0
!
interface Ethernet0/2.3
 description Computers
 encapsulation dot1Q 3
 ip address 10.5.1.254 255.255.255.0
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
control-plane
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 13061E010803
 logging synchronous
 login
line aux 0
line vty 0 4
 password 7 060506324F41
 login
 transport input none
!
!
end
```



### L2/L3 коммутатор

```
SW4#sh run
Building configuration...

Current configuration : 2268 bytes
!
! Last configuration change at 08:49:47 UTC Sun Nov 23 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW4
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$H.Dd$1gSPgP9ZB0v/MR5palY5e0
!
no aaa new-model
!
no ip domain-lookup
ip cef
no ipv6 cef
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
interface Loopback1
 ip address 10.254.0.7 255.255.0.0
!
interface Port-channel1
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
!
interface Ethernet0/0
 description SW3-0/0
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
!
interface Ethernet0/1
 description SW2-0/1
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
!
interface Ethernet0/2
 description PortChannel1-SW5-e0/2
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet0/3
 description PortChannel1-SW5-e0/3
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
 channel-group 1 mode active
!
interface Ethernet1/0
 description R12-0/0
 no switchport
 ip address 172.16.0.94 255.255.255.252
 duplex auto
!
interface Ethernet1/1
 description R13-0/1
 no switchport
 ip address 172.16.0.102 255.255.255.252
 duplex auto
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan2
 ip address 10.0.0.252 255.255.255.0
 vrrp 2 ip 10.0.0.254
 vrrp 2 priority 120
!
interface Vlan3
 ip address 10.0.1.252 255.255.255.0
 vrrp 3 ip 10.0.1.254
 vrrp 3 priority 120
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
control-plane
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 104D000A0618
 logging synchronous
 login
line aux 0
line vty 0 4
 password 7 070C285F4D06
 login
!
!
end
```

### L2 коммутатор

```
SW3#sh run
Building configuration...

Current configuration : 1361 bytes
!
! Last configuration change at 07:33:11 UTC Sun Nov 23 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
enable secret 5 $1$FC9Y$jrBskz8dc3ZDSFBJ75/aU1
!
no aaa new-model
!
no ip domain-lookup
ip cef
no ipv6 cef
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
interface Ethernet0/0
 description SW4-0/0
 switchport trunk allowed vlan 2,3
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 990
 switchport mode trunk
!
interface Ethernet0/1
!
interface Ethernet0/2
 description VLAN3
 switchport access vlan 3
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface Ethernet0/3
!
interface Ethernet1/0
!
interface Ethernet1/1
!
interface Ethernet1/2
!
interface Ethernet1/3
!
interface Vlan2
 ip address 10.0.0.249 255.255.255.0
 shutdown
!
ip default-gateway 10.0.0.254
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
control-plane
!
banner motd ^C
Unauthorized access is strictly prohibited. ^C
!
line con 0
 password 7 110A1016141D
 logging synchronous
 login
line aux 0
line vty 0 4
 password 7 060506324F41
 login
!
!
end

```


