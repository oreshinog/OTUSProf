# Лабораторная работа. IS-IS
[**Файл EVE-NG**](https://github.com/oreshinog/OTUSProf/blob/main/Lab4/EVE/otusprof_lab4.zip)
# Топология
![Топология](https://github.com/oreshinog/OTUSProf/blob/main/Lab4/Img/Img00.png)

# Цели

Настроить IS-IS офисе Триада

Описание/Пошаговая инструкция выполнения домашнего задания:
Настроите IS-IS в ISP Триада.
R23 и R25 находятся в зоне 2222.
R24 находится в зоне 24.
R26 находится в зоне 26.

## 1. R23 и R25

### R23

```
R23(config)#router isis
R23(config-router)#net 49.2222.1025.4310.0000.00
R23(config-router)#is-type level-2-only
R23(config-router)#log-adjacency-changes
R23(config-router)#exi
R23(config)#int loop1
R23(config-if)#ip router isis
R23(config)#int e0/1
R23(config-if)#ip router isis
R23(config)#int e0/2
R23(config-if)#ip router isis
```


### R25

```
R25(config)#router isis
R25(config-router)#net 49.2222.1025.4330.0000.00
R25(config-router)#is-type level-2-only
R25(config-router)#log-adjacency-changes
R25(config-router)#exi
R25(config)#int loop1
R25(config-if)#ip router isis
R25(config)#int e0/1
R25(config-if)#ip router isis
R25(config)#int e0/2
R25(config-if)#ip router isis
```

### 2. R24

```
R24(config)#router isis
R24(config-router)#net 49.0024.1025.4320.0000.00
R24(config-router)#is-type level-2-only
R24(config-router)#log-adjacency-changes
R24(config-router)#exi
R24(config)#int loop1
R24(config-if)#ip router isis
R24(config)#int e0/0
R24(config-if)#ip router isis
R24(config)#int e0/2
R24(config-if)#ip router isis
```

### 3. R26

```
R26(config)#router isis
R26(config-router)#net 49.0026.1025.4340.0000.00
R26(config-router)#is-type level-2-only
R26(config-router)#log-adjacency-changes
R26(config-router)#exi
R26(config)#int loop1
R26(config-if)#ip router isis
R26(config)#int e0/0
R26(config-if)#ip router isis
R26(config)#int e0/2
R26(config-if)#ip router isis
```

## Итог

### R23
```
R23#sh isis topology


IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  --
R24                  10         R24                  Et0/2       aabb.cc01.8020
R25                  30         R24                  Et0/2       aabb.cc01.8020
R26                  20         R24                  Et0/2       aabb.cc01.8020

R23#sh isis database verbose

IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00           * 0x00000007   0x72EF        900               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R23
  IP Address:   10.254.3.1
  Metric: 10         IS R24.02
  Metric: 10         IP 10.254.3.1 255.255.255.255
  Metric: 10         IP 172.16.0.36 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
R24.00-00             0x00000017   0x2BB9        898               0/0/0
  Area Address: 49.0024
  NLPID:        0xCC
  Hostname: R24
  IP Address:   10.254.3.2
  Metric: 10         IS R24.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.2 255.255.255.255
  Metric: 10         IP 172.16.0.40 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.72 255.255.255.252
R24.02-00             0x00000004   0x1809        894               0/0/0
  Metric: 0          IS R24.00
  Metric: 0          IS R23.00
R25.00-00             0x00000013   0x8F4D        1108              0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R25
  IP Address:   10.254.3.3
  Metric: 10         IS R26.02
  Metric: 10         IP 10.254.3.3 255.255.255.255
  Metric: 10         IP 172.16.0.48 255.255.255.252
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.60 255.255.255.252
  Metric: 10         IP 172.16.0.64 255.255.255.252
R26.00-00             0x00000014   0xA7CF        494               0/0/0
  Area Address: 49.0026
  NLPID:        0xCC
  Hostname: R26
  IP Address:   10.254.3.4
  Metric: 10         IS R26.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.4 255.255.255.255
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.68 255.255.255.252
  Metric: 10         IP 172.16.0.76 255.255.255.252
R26.02-00             0x0000000C   0x2B8D        416               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R25.00
R26.04-00             0x0000000F   0xA51E        798               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R24.00
```

### R24

```
R24#sh isis topology


IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  10         R23                  Et0/2       aabb.cc01.7020
R24                  --
R25                  20         R26                  Et0/1       aabb.cc01.a000
R26                  10         R26                  Et0/1       aabb.cc01.a000

R24#sh isis data verbose

IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x00000007   0x72EF        668               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R23
  IP Address:   10.254.3.1
  Metric: 10         IS R24.02
  Metric: 10         IP 10.254.3.1 255.255.255.255
  Metric: 10         IP 172.16.0.36 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
R24.00-00           * 0x00000017   0x2BB9        670               0/0/0
  Area Address: 49.0024
  NLPID:        0xCC
  Hostname: R24
  IP Address:   10.254.3.2
  Metric: 10         IS R24.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.2 255.255.255.255
  Metric: 10         IP 172.16.0.40 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.72 255.255.255.252
R24.02-00           * 0x00000004   0x1809        666               0/0/0
  Metric: 0          IS R24.00
  Metric: 0          IS R23.00
R25.00-00             0x00000013   0x8F4D        879               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R25
  IP Address:   10.254.3.3
  Metric: 10         IS R26.02
  Metric: 10         IP 10.254.3.3 255.255.255.255
  Metric: 10         IP 172.16.0.48 255.255.255.252
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.60 255.255.255.252
  Metric: 10         IP 172.16.0.64 255.255.255.252
R26.00-00             0x00000015   0xA5D0        975               0/0/0
  Area Address: 49.0026
  NLPID:        0xCC
  Hostname: R26
  IP Address:   10.254.3.4
  Metric: 10         IS R26.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.4 255.255.255.255
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.68 255.255.255.252
  Metric: 10         IP 172.16.0.76 255.255.255.252
R26.02-00             0x0000000D   0x298E        1009              0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R25.00
R26.04-00             0x0000000F   0xA51E        569               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R24.00
```

### R25

```
R25#sh isis topology


IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  30         R26                  Et0/2       aabb.cc01.a020
R24                  20         R26                  Et0/2       aabb.cc01.a020
R25                  --
R26                  10         R26                  Et0/2       aabb.cc01.a020

R25#sh isis data ver

IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x00000007   0x72EF        606               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R23
  IP Address:   10.254.3.1
  Metric: 10         IS R24.02
  Metric: 10         IP 10.254.3.1 255.255.255.255
  Metric: 10         IP 172.16.0.36 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
R24.00-00             0x00000017   0x2BB9        608               0/0/0
  Area Address: 49.0024
  NLPID:        0xCC
  Hostname: R24
  IP Address:   10.254.3.2
  Metric: 10         IS R24.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.2 255.255.255.255
  Metric: 10         IP 172.16.0.40 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.72 255.255.255.252
R24.02-00             0x00000004   0x1809        605               0/0/0
  Metric: 0          IS R24.00
  Metric: 0          IS R23.00
R25.00-00           * 0x00000013   0x8F4D        826               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R25
  IP Address:   10.254.3.3
  Metric: 10         IS R26.02
  Metric: 10         IP 10.254.3.3 255.255.255.255
  Metric: 10         IP 172.16.0.48 255.255.255.252
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.60 255.255.255.252
  Metric: 10         IP 172.16.0.64 255.255.255.252
R26.00-00             0x00000015   0xA5D0        917               0/0/0
  Area Address: 49.0026
  NLPID:        0xCC
  Hostname: R26
  IP Address:   10.254.3.4
  Metric: 10         IS R26.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.4 255.255.255.255
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.68 255.255.255.252
  Metric: 10         IP 172.16.0.76 255.255.255.252
R26.02-00             0x0000000D   0x298E        952               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R25.00
R26.04-00             0x0000000F   0xA51E        512               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R24.00
```

### R26

```
R26#sh isis topology


IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  20         R24                  Et0/0       aabb.cc01.8010
R24                  10         R24                  Et0/0       aabb.cc01.8010
R25                  10         R25                  Et0/2       aabb.cc01.9020
R26                  --

R26#sh isis data ver

IS-IS Level-2 Link State Database:
LSPID                 LSP Seq Num  LSP Checksum  LSP Holdtime      ATT/P/OL
R23.00-00             0x00000007   0x72EF        526               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R23
  IP Address:   10.254.3.1
  Metric: 10         IS R24.02
  Metric: 10         IP 10.254.3.1 255.255.255.255
  Metric: 10         IP 172.16.0.36 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
R24.00-00             0x00000017   0x2BB9        528               0/0/0
  Area Address: 49.0024
  NLPID:        0xCC
  Hostname: R24
  IP Address:   10.254.3.2
  Metric: 10         IS R24.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.2 255.255.255.255
  Metric: 10         IP 172.16.0.40 255.255.255.252
  Metric: 10         IP 172.16.0.44 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.72 255.255.255.252
R24.02-00             0x00000004   0x1809        525               0/0/0
  Metric: 0          IS R24.00
  Metric: 0          IS R23.00
R25.00-00             0x00000013   0x8F4D        742               0/0/0
  Area Address: 49.2222
  NLPID:        0xCC
  Hostname: R25
  IP Address:   10.254.3.3
  Metric: 10         IS R26.02
  Metric: 10         IP 10.254.3.3 255.255.255.255
  Metric: 10         IP 172.16.0.48 255.255.255.252
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.60 255.255.255.252
  Metric: 10         IP 172.16.0.64 255.255.255.252
R26.00-00           * 0x00000015   0xA5D0        838               0/0/0
  Area Address: 49.0026
  NLPID:        0xCC
  Hostname: R26
  IP Address:   10.254.3.4
  Metric: 10         IS R26.02
  Metric: 10         IS R26.04
  Metric: 10         IP 10.254.3.4 255.255.255.255
  Metric: 10         IP 172.16.0.52 255.255.255.252
  Metric: 10         IP 172.16.0.56 255.255.255.252
  Metric: 10         IP 172.16.0.68 255.255.255.252
  Metric: 10         IP 172.16.0.76 255.255.255.252
R26.02-00           * 0x0000000D   0x298E        872               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R25.00
R26.04-00           * 0x0000000F   0xA51E        432               0/0/0
  Metric: 0          IS R26.00
  Metric: 0          IS R24.00
```