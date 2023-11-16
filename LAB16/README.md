# **Лабораторная работа. "НАСТРОЙКА БАЗОВОГО ПРОТОКОЛА OSPFV2 ДЛЯ ОДНОЙ ОБЛАСТИ"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/Cisco%20Packet%20Tracer%2015.11.2023%2010_50_50.png?raw=true)

## **Цели:**
+ ### Часть 1. Построение сети и проверка соединения  
+ ### Часть 2. Настройка обеспечения избыточности на первом хопе с помощью HSRP 
+ ### Часть 3. Изменение значения ID маршрутизатора
+ ### Часть 4. Настройка пассивных интерфейсов OSPF
+ ### Часть 5. Изменение метрик OSPF

## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Выполните инициализацию и перезагрузку машрутизаторов.**
### **Шаг 4. Настройте базовые параметры каждого маршрутизатора.**

### R1:

```
Router(config)#host R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#exit
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#banner motd @Unauthorized Access is Prohibited!@
R1(config)#int g0/0
R1(config-if)#ip addr 192.168.1.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R1(config-if)#int s0/0/0
R1(config-if)#ip addr 192.168.12.1 255.255.255.252
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
R1(config-if)#int s0/0/1
R1(config-if)#ip addr 192.168.13.1 255.255.255.252
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R1(config-if)#
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#
```


### R2:

```
Router(config)#host R2
R2(config)#no ip domain-lookup
R2(config)#enable secret class
R2(config)#line con 0
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#logging synchronous
R2(config-line)#exit
R2(config)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#banner motd @Unauthorized Access is Prohibited!@
R2(config)#int g0/0
R2(config-if)#ip addr 192.168.2.1 255.255.255.0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R2(config-if)#int s0/0/0
R2(config-if)#clock rate 128000
R2(config-if)#ip addr 192.18.12.2 255.255.255.252
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#int s0/0/1
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R2(config-if)#ip addr 192.168.23.1 255.255.255.252
R2(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#copy run st
Destination filename [startup-config]? 
Building configuration
```

### R3:

```
Router(config)#host R3
R3(config)#no ip domain-lookup
R3(config)#enable secret class
R3(config)#line con 0
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#logging synchronous
R3(config-line)#exit
R3(config)#line vty 0 4
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#exit
R3(config)#banner motd @Unauthorized Access is Prohibited!@
R3(config)#int g0/0
R3(config-if)#ip addr 192.168.3.1 255.255.255.0
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R3(config-if)#int s0/0/0
R3(config-if)#clo
R3(config-if)#clock r
R3(config-if)#clock rate 128000
R3(config-if)#ip addr 192.168.13.2 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R3(config-if)#int s0/0/1
R3(config-if)#clock rate 128000
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R3(config-if)#ip addr 192.168.23.2 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

R3(config-if)#
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R3#
```


## **Часть 2. Настройка и проверка маршрутизации OSPF**


### **Шаг 1. Настройте маршрутизацию OSPF на маршрутизаторе R1.**

### a. Используйте команду router ospf в режиме глобальной конфигурации, чтобы активировать OSPF на маршрутизаторе R1.

### R1:

```
R1(config)#router ospf 1

```

### b. Используйте команду network для сетей маршрутизатора R1. Используйте идентификатор области, равный 0.

### R1:

```	
R1(config-router)#net 192.168.1.0 0.0.0.255 area 0
R1(config-router)#net 192.168.12.0 0.0.0.3 area 0
R1(config-router)#net 192.168.13.0 0.0.0.3 area 0
```

### **Шаг 2. Настройте OSPF на маршрутизаторах R2 и R3.**

### R2:

```
R2(config)#router ospf 1
R2(config-router)#net 192.168.2.0 0.0.0.255 area 0
R2(config-router)#net 192.168.12.0 0.0.0.3 area 0
04:29:46: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.13.1 on Serial0/0/0 from LOADING to FULL, Loading Done

R2(config-router)#net 192.168.23.0 0.0.0.3 area 0
R2(config-router)#


```

### R3:

```
R3(config)#router ospf 1
R3(config-router)#net 192.168.3.0 0.0.0.255 area 0
R3(config-router)#net 192.168.13.0 0.0.0.3 area 0
04:30:57: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.13.1 on Serial0/0/0 from LOADING to FULL, Loading Done

R3(config-router)#net 192.168.23.0 0.0.0.3 area 0
R3(config-router)#

```

### **Шаг 3: Проверьте информацию о соседях и маршрутизации OSPF.**

### a. Используйте команду show ip ospf neighbor для проверки списка смежных маршрутизаторов на каждом маршрутизаторе в соответствии с топологией.

### R1:


```
R1#sh ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.23.1      0   FULL/  -        00:00:33    192.168.12.2    Serial0/0/0
192.168.23.2      0   FULL/  -        00:00:38    192.168.13.2    Serial0/0/1

```

### b. Выполните команду show ip route, чтобы убедиться, что в таблицах маршрутизации всех маршрутизаторов отображаются все сети.

### R1:

```
R1#sh ip route 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0
O    192.168.2.0/24 [110/65] via 192.168.12.2, 00:03:21, Serial0/0/0
O    192.168.3.0/24 [110/65] via 192.168.13.2, 00:01:14, Serial0/0/1
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.12.0/30 is directly connected, Serial0/0/0
L       192.168.12.1/32 is directly connected, Serial0/0/0
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.13.0/30 is directly connected, Serial0/0/1
L       192.168.13.1/32 is directly connected, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0/30 [110/128] via 192.168.12.2, 00:01:14, Serial0/0/0
                        [110/128] via 192.168.13.2, 00:01:14, Serial0/0/1

```
### ***Какую команду вы бы использовали, чтобы видеть только маршруты OSPF в таблице маршрутизации? Ответ: show ip route ospf***

### **Шаг 4. Проверьте настройки протокола OSPF.**

### R1:

```
R1#sh ip protocols 

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 192.168.13.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    192.168.12.0 0.0.0.3 area 0
    192.168.13.0 0.0.0.3 area 0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.13.1         110      00:01:54
    192.168.23.1         110      00:01:38
    192.168.23.2         110      00:01:38
  Distance: (default is 110)


```

### **Шаг 5: Проверьте данные процесса OSPF.**

### R1:

```
R1#sh ip ospf 
 Routing Process "ospf 1" with ID 192.168.13.1
 Supports only single TOS(TOS0) routes
 Supports opaque LSA
 SPF schedule delay 5 secs, Hold time between two SPFs 10 secs
 Minimum LSA interval 5 secs. Minimum LSA arrival 1 secs
 Number of external LSA 0. Checksum Sum 0x000000
 Number of opaque AS LSA 0. Checksum Sum 0x000000
 Number of DCbitless external and opaque AS LSA 0
 Number of DoNotAge external and opaque AS LSA 0
 Number of areas in this router is 1. 1 normal 0 stub 0 nssa
 External flood list length 0
    Area BACKBONE(0)
        Number of interfaces in this area is 3
        Area has no authentication
        SPF algorithm executed 8 times
        Area ranges are
        Number of LSA 3. Checksum Sum 0x00be1f
        Number of opaque link LSA 0. Checksum Sum 0x000000
        Number of DCbitless LSA 0
        Number of indication LSA 0
        Number of DoNotAge LSA 0
        Flood list length 0

```


### **Шаг 6: Проверьте настройки интерфейса OSPF.**

### a. Выполните команду show ip ospf interface brief, чтобы отобразить сводку об интерфейсах, на которых активирован алгоритм OSPF(не работает в pkt).



### b. Для того чтобы увидеть более подробные данные об интерфейсах, на которых активирован OSPF, выполните команду show ip ospf interface.

### R1:

```
R1#sh ip ospf interface 

GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.1.1/24, Area 0
  Process ID 1, Router ID 192.168.13.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 192.168.13.1, Interface address 192.168.1.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:05
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
Serial0/0/0 is up, line protocol is up
  Internet address is 192.168.12.1/30, Area 0
  Process ID 1, Router ID 192.168.13.1, Network Type POINT-TO-POINT, Cost: 64
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:08
  Index 2/2, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 192.168.23.1
  Suppress hello for 0 neighbor(s)
Serial0/0/1 is up, line protocol is up
  Internet address is 192.168.13.1/30, Area 0
  Process ID 1, Router ID 192.168.13.1, Network Type POINT-TO-POINT, Cost: 64
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:08
  Index 3/3, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 192.168.23.2
  Suppress hello for 0 neighbor(s)

```



### **Шаг 7: Проверьте наличие сквозного соединения.**




## **Часть 3: Изменение значения ID маршрутизатора**

### **Шаг 1: Измените идентификаторы маршрутизатора, используя loopback-адреса.**

### a. Назначьте IP-адрес loopback 0 для маршрутизатора R1.

### R1:

```
R1(config)#int lo0

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

R1(config-if)#ip addr 1.1.1.1 255.255.255.255

```


### b. Назначьте IP-адреса loopback 0 для маршрутизаторов R2 и R3. Используйте IP-адрес 2.2.2.2/32 для R2 и 3.3.3.3/32 для R3.


### R2:

```
R2(config)#int lo0

R2(config-if)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

R2(config-if)#ip addr 2.2.2.2 255.255.255.255
R2(config-if)#

```


### R3:

```
R3(config)#int lo0

R3(config-if)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

R3(config-if)#ip addr 3.3.3.3 255.255.255.255
R3(config-if)#

```

### c. Сохраните текущую конфигурацию в загрузочную на всех трёх маршрутизаторах.

### R1, R2, R3:

```
R1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
```


### d. Для того чтобы идентификатор маршрутизатора получил значение loopback-адреса, необходимо перезагрузить маршрутизаторы. Выполните команду reload на всех трёх маршрутизаторах. Нажмите клавишу Enter, чтобы подтвердить перезагрузку.


### e. После перезагрузки маршрутизатора выполните команду show ip protocols, чтобы просмотреть новый идентификатор маршрутизатора

### R1:

```
R1#sh ip protocols 

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 1.1.1.1
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    192.168.12.0 0.0.0.3 area 0
    192.168.13.0 0.0.0.3 area 0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    1.1.1.1              110      00:00:06
    2.2.2.2              110      00:00:06
    3.3.3.3              110      00:00:06
  Distance: (default is 110)
```

### f. Выполните show ip ospf neighbor, чтобы отобразить изменения идентификатора маршрутизатора для соседних маршрутизаторов.

### R1:

```
R1#sh ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
3.3.3.3           0   FULL/  -        00:00:38    192.168.13.2    Serial0/0/1
2.2.2.2           0   FULL/  -        00:00:31    192.168.12.2    Serial0/0/0
```

### **Шаг 2: Измените идентификатор маршрутизатора R1 с помощью команды router-id.**




### a. Чтобы переназначить идентификатор маршрутизатора, выполните команду router-id 11.11.11.11 на маршрутизаторе R1. Обратите внимание на уведомление, которое появляется при выполнении команды router-id.

### R1:

```
R1(config)#router ospf 1
R1(config-router)#rou
R1(config-router)#router-id 11.11.11.11
R1(config-router)#Reload or use "clear ip ospf process" command, for this to take effect


R1(config-router)#
R1#
```


### b. Вы получите уведомление о том, что для того, чтобы изменения вступили в силу, вам необходимо либо перезагрузить маршрутизатор, либо использовать команду clear ip ospf process. Выполните команду clear ip ospf process на всех трёх маршрутизаторах. Введите yes, чтобы подтвердить сброс, и нажмите клавишу Enter.

### R1, R2, R3:

```
R1#clear ip ospf process 
Reset ALL OSPF processes? [no]: yes

R1#
04:33:30: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.1 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:33:30: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.1 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached

04:33:30: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:33:30: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R1#
04:33:34: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from LOADING to FULL, Loading Done

R1#
04:33:41: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.1 on Serial0/0/0 from LOADING to FULL, Loading Done

R1#

```


### c. Для маршрутизатора R2 настройте идентификатор 22.22.22.22, а для маршрутизатора R3 — идентификатор 33.33.33.33. Затем используйте команду clear ip ospf process, чтобы сбросить процесс маршрутизации OSPF.

### R2:

```
R2(config)#router ospf 1
R2(config-router)#rou
R2(config-router)#router-id 22.22.22.22
R2(config-router)#Reload or use "clear ip ospf process" command, for this to take effect


R2(config-router)#
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#cle
R2#clear ip os
R2#clear ip ospf pr
R2#clear ip ospf process 
Reset ALL OSPF processes? [no]: yes

R2#
04:58:10: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:58:10: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached

04:58:10: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:58:10: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R2#
04:58:12: %OSPF-5-ADJCHG: Process 1, Nbr 192.168.23.2 on Serial0/0/1 from LOADING to FULL, Loading Done

R2#
04:58:14: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from LOADING to FULL, Loading Done

R2#

```

### R3:

```
R3(config)#router ospf 1
R3(config-router)#rou
R3(config-router)#router-id 33.33.33.33
R3(config-router)#Reload or use "clear ip ospf process" command, for this to take effect


R3(config-router)#
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#cle
R3#clear ip os
R3#clear ip ospf pr
R3#clear ip ospf process 
Reset ALL OSPF processes? [no]: yes

R3#
04:58:47: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:58:47: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached

04:58:47: %OSPF-5-ADJCHG: Process 1, Nbr 22.22.22.22 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:58:47: %OSPF-5-ADJCHG: Process 1, Nbr 22.22.22.22 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R3#

```

### d. Выполните команду show ip protocols, чтобы проверить изменился ли идентификатор маршрутизатора R1.

### R1:

```
R1#sh ip protocols 

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 11.11.11.11
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.1.0 0.0.0.255 area 0
    192.168.12.0 0.0.0.3 area 0
    192.168.13.0 0.0.0.3 area 0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    11.11.11.11          110      00:00:11
    22.22.22.22          110      00:00:05
    33.33.33.33          110      00:00:05
    192.168.13.1         110      00:28:11
    192.168.23.1         110      00:02:54
    192.168.23.2         110      00:01:10
  Distance: (default is 110)

```

### e. Выполните команду show ip ospf neighbor на маршрутизаторе R1, чтобы убедиться, что новые идентификаторы маршрутизаторов R2 и R3 содержатся в списке.

### R1:

```
R1#sh ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
22.22.22.22       0   FULL/  -        00:00:30    192.168.12.2    Serial0/0/0
33.33.33.33       0   FULL/  -        00:00:35    192.168.13.2    Serial0/0/1

```

## **Часть 4: Настройка пассивных интерфейсов OSPF**


## **Шаг 1: Настройте пассивный интерфейс.**


### a. Выполните команду show ip ospf interface g0/0 на маршрутизаторе R1. Обратите внимание на таймер, указывающий время получения очередного пакета приветствия. Пакеты приветствия отправляются каждые 10 секунд и используются маршрутизаторами OSPF для проверки работоспособности соседних устройств.

### R1:

```
R1#sh ip ospf interface g0/0

GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.1.1/24, Area 0
  Process ID 1, Router ID 11.11.11.11, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 11.11.11.11, Interface address 192.168.1.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:05
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
R1#

```


### b. Выполните команду passive-interface, чтобы интерфейс G0/0 маршрутизатора R1 стал пассивным.

### R1:

```
R1(config)#router ospf 1
R1(config-router)#pass
R1(config-router)#passive-interface g0/0
R1(config-router)#
```

### d. Выполните команду show ip route на маршрутизаторах R2 и R3, чтобы убедиться, что маршрут к сети 192.168.1.0/24 по-прежнему доступен.

### R2:

```
R2#sh ip route 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     2.0.0.0/32 is subnetted, 1 subnets
C       2.2.2.2/32 is directly connected, Loopback0
O    192.168.1.0/24 [110/65] via 192.168.12.1, 00:07:36, Serial0/0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.2.0/24 is directly connected, GigabitEthernet0/0
L       192.168.2.1/32 is directly connected, GigabitEthernet0/0
O    192.168.3.0/24 [110/65] via 192.168.23.2, 00:02:55, Serial0/0/1
     192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.12.0/30 is directly connected, Serial0/0/0
L       192.168.12.2/32 is directly connected, Serial0/0/0
     192.168.13.0/30 is subnetted, 1 subnets
O       192.168.13.0/30 [110/128] via 192.168.12.1, 00:07:36, Serial0/0/0
                        [110/128] via 192.168.23.2, 00:07:36, Serial0/0/1
     192.168.23.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.23.0/30 is directly connected, Serial0/0/1
L       192.168.23.1/32 is directly connected, Serial0/0/1

```

### R3:

```
R3#sh ip route 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     3.0.0.0/32 is subnetted, 1 subnets
C       3.3.3.3/32 is directly connected, Loopback0
O    192.168.1.0/24 [110/65] via 192.168.13.1, 00:08:50, Serial0/0/0
O    192.168.2.0/24 [110/65] via 192.168.23.1, 00:04:06, Serial0/0/1
     192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.3.0/24 is directly connected, GigabitEthernet0/0
L       192.168.3.1/32 is directly connected, GigabitEthernet0/0
     192.168.12.0/30 is subnetted, 1 subnets
O       192.168.12.0/30 [110/128] via 192.168.13.1, 00:04:06, Serial0/0/0
                        [110/128] via 192.168.23.1, 00:04:06, Serial0/0/1
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.13.0/30 is directly connected, Serial0/0/0
L       192.168.13.2/32 is directly connected, Serial0/0/0
     192.168.23.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.23.0/30 is directly connected, Serial0/0/1
L       192.168.23.2/32 is directly connected, Serial0/0/1


```



### **Шаг 2: Настройте маршрутизатор так, чтобы все его интерфейсы были пассивными по умолчанию.**

### a. Выполните команду show ip ospf neighbor на маршрутизаторе R1, чтобы убедиться, что R2 указан в качестве соседа OSPF.

### R1:

```
R1#sh ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
22.22.22.22       0   FULL/  -        00:00:34    192.168.12.2    Serial0/0/0
33.33.33.33       0   FULL/  -        00:00:39    192.168.13.2    Serial0/0/1

```

### b. Выполните команду passive-interface default на R2, чтобы по умолчанию настроить все интерфейсы OSPF в качестве пассивных.

### R2:

```
R2(config)#router ospf 1
R2(config-router)#pass
R2(config-router)#passive-interface de
R2(config-router)#passive-interface default 
R2(config-router)#
05:04:53: %OSPF-5-ADJCHG: Process 1, Nbr 11.11.11.11 on Serial0/0/0 from FULL to DOWN, Neighbor Down: Interface down or detached

05:04:53: %OSPF-5-ADJCHG: Process 1, Nbr 33.33.33.33 on Serial0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R2(config-router)#

```


### c. Повторно выполните команду show ip ospf neighbor на R1. После истечения таймера простоя маршрутизатор R2 больше не будет указан, как сосед OSPF.

### R1:

```
R1#sh ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
33.33.33.33       0   FULL/  -        00:00:32    192.168.13.2    Serial0/0/1
```

### d. Выполните команду show ip ospf interface S0/0/0 на маршрутизаторе R2, чтобы просмотреть состояние OSPF интерфейса S0/0/0.

### R2:

```
R2#sh ip ospf interface s0/0/0

Serial0/0/0 is up, line protocol is up
  Internet address is 192.168.12.2/30, Area 0
  Process ID 1, Router ID 22.22.22.22, Network Type POINT-TO-POINT, Cost: 64
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    No Hellos (Passive interface)
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Suppress hello for 0 neighbor(s)

```

### e. В случае если все интерфейсы маршрутизатора R2 являются пассивными, маршрутизирующая информация объявляться не будет. В этом случае маршрутизаторы R1 и R3 больше не должны иметь маршрут к сети 192.168.2.0/24. Это можно проверить с помощью команды show ip route.



### f. На маршрутизаторе R2 выполните команду no passive-interface, чтобы маршрутизатор отправлял и получал обновления маршрутизации OSPF. После ввода этой команды появится уведомление о том, что на маршрутизаторе R1 были установлены отношения смежности.

### R2:

```
R2(config)#router ospf 1
R2(config-router)#no pas
R2(config-router)#no passive-interface s0/0/0

```

### g. Повторно выполните команды show ip route и show ip ospf neighbor на маршрутизаторах R1 и R3 и найдите маршрут к сети 192.168.2.0/24.

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/8.2.4.5%20Lab%20-%20Configuring%20Basic%20Single-Area%20OSPFv2.pdf%20-%20Word%2016.11.2023%2010_57_05.png?raw=true)


## **Часть 5: Изменение метрик OSPF**


### **Шаг 1: Измените заданную пропускную способность на маршрутизаторах.**

### a. Выполните команду show interface на маршрутизаторе R1, чтобы просмотреть значение пропускной способности по умолчанию для интерфейса G0/0.

### R1:

```
R1#sh interfaces g0/0
GigabitEthernet0/0 is up, line protocol is up (connected)
  Hardware is CN Gigabit Ethernet, address is 000d.bdaa.3401 (bia 000d.bdaa.3401)
  Internet address is 192.168.1.1/24
  MTU 1500 bytes, BW 1000000 Kbit, DLY 100 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s, media type is RJ45
  output flow-control is unsupported, input flow-control is unsupported
  ARP type: ARPA, ARP Timeout 04:00:00, 
  Last input 00:00:08, output 00:00:05, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0 (size/max/drops); Total output drops: 0
  Queueing strategy: fifo
  Output queue :0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     4 packets input, 512 bytes, 0 no buffer
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     0 watchdog, 1017 multicast, 0 pause input
     0 input packets with dribble condition detected
     212 packets output, 13760 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped out
```


### b. Выполните команду show ip route ospf на R1, чтобы определить маршрут к сети 192.168.3.0/24.

### R1:

```
R1#sh ip route ospf 
O    192.168.2.0 [110/65] via 192.168.12.2, 00:04:12, Serial0/0/0
O    192.168.3.0 [110/65] via 192.168.13.2, 00:08:26, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/128] via 192.168.12.2, 00:04:12, Serial0/0/0
                     [110/128] via 192.168.13.2, 00:04:12, Serial0/0/1


```

### c. Выполните команду show ip ospf interface на маршрутизаторе R3, чтобы определить стоимость маршрутизации для интерфейса G0/0.

### R3:

```
R3#sh ip ospf interface g0/0

GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.3.1/24, Area 0
  Process ID 1, Router ID 33.33.33.33, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 33.33.33.33, Interface address 192.168.3.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:04
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)

```


### d. Выполните команду show ip ospf interface s0/0/1 на маршрутизаторе R1, чтобы просмотреть стоимость маршрутизации для интерфейса S0/0/1.

### R1:

```
R1#sh ip ospf interface s0/0/1

Serial0/0/1 is up, line protocol is up
  Internet address is 192.168.13.1/30, Area 0
  Process ID 1, Router ID 11.11.11.11, Network Type POINT-TO-POINT, Cost: 64
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:06
  Index 3/3, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 33.33.33.33
  Suppress hello for 0 neighbor(s)
```

### e. Выполните команду auto-cost reference-bandwidth 10000 на маршрутизаторе R1, чтобы изменить параметр заданной пропускной способности по умолчанию. С подобной установкой стоимость интерфейсов 10 Гб/с будет равна 1, стоимость интерфейсов 1 Гбит/с будет равна 10, а стоимость интерфейсов 100 Мб/c будет равна 100.

### R1:

```
R1(config)#router ospf 1
R1(config-router)#auto-cost reference-bandwidth 10000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R1(config-router)#
```

### f. Выполните команду auto-cost reference-bandwidth 10000 на маршрутизаторах R2 и R3.

### R2:

```
R2(config)#router ospf 1
R2(config-router)#au
R2(config-router)#auto-cost re
R2(config-router)#auto-cost reference-bandwidth 10000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R2(config-router)#
```

### R3:

```
R3(config)#router ospf 1
R3(config-router)#au
R3(config-router)#auto-cost re
R3(config-router)#auto-cost reference-bandwidth 10000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R3(config-router)#
```

### g. Повторно выполните команду show ip ospf interface, чтобы просмотреть новую стоимость интерфейса G0/0 на R3 и интерфейса S0/0/1 на R1.

### R3:

```
R3#sh ip ospf interface g0/0

GigabitEthernet0/0 is up, line protocol is up
  Internet address is 192.168.3.1/24, Area 0
  Process ID 1, Router ID 33.33.33.33, Network Type BROADCAST, Cost: 100
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 33.33.33.33, Interface address 192.168.3.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:05
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
```

### R1:

```
R1#sh ip ospf interface s0/0/1

Serial0/0/1 is up, line protocol is up
  Internet address is 192.168.13.1/30, Area 0
  Process ID 1, Router ID 11.11.11.11, Network Type POINT-TO-POINT, Cost: 6476
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:08
  Index 3/3, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 33.33.33.33
  Suppress hello for 0 neighbor(s)
R1#
```

### h. Повторно выполните команду show ip route ospf, чтобы просмотреть новую суммарную стоимость для маршрута 192.168.3.0/24 (10 + 6476 = 6486).

### R1:

```
R1#sh ip route ospf 
O    192.168.2.0 [110/6576] via 192.168.12.2, 00:04:05, Serial0/0/0
O    192.168.3.0 [110/6576] via 192.168.13.2, 00:03:26, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/12952] via 192.168.12.2, 00:03:26, Serial0/0/0
                     [110/12952] via 192.168.13.2, 00:03:26, Serial0/0/1
```

### i. Для того чтобы восстановить заданную пропускную способность до значения по умолчанию, на всех трёх маршрутизаторах выполните команду auto-cost reference-bandwidth 100.

### R1:

```
R1(config)#router ospf 1
R1(config-router)#auto-cost reference-bandwidth 100
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R1(config-router)#

```
### R2:

```
R2(config)#router ospf 1
R2(config-router)#auto-cost reference-bandwidth 100
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R2(config-router)#

```

### R3:

```
R3(config)#router ospf 1
R3(config-router)#auto-cost reference-bandwidth 100
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R3(config-router)#

```
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/8.2.4.5%20Lab%20-%20Configuring%20Basic%20Single-Area%20OSPFv2.pdf%20-%20Word%2016.11.2023%2011_17_25.png?raw=true)

### **Шаг 2: Измените пропускную способность для интерфейса.**

### a. Выполните команду show interface s0/0/0 на маршрутизаторе R1, чтобы просмотреть установленное значение пропускной способности на интерфейсе S0/0/0. Реальная скорость передачи данных на этом интерфейсе, установленная командой clock rate, составляет 128 Кб/с, при этом установленное значение пропускной способности по-прежнему равно 1544 Кб/с.

### R1:

```
R1#sh interfaces s0/0/0
Serial0/0/0 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 192.168.12.1/30
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set, keepalive set (10 sec)
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0 (size/max/drops); Total output drops: 0
  Queueing strategy: weighted fair
  Output queue: 0/1000/64/0 (size/max total/threshold/drops)
     Conversations  0/0/256 (active/max active/max total)
     Reserved Conversations 0/0 (allocated/max allocated)
     Available Bandwidth 1158 kilobits/sec
  5 minute input rate 57 bits/sec, 0 packets/sec
  5 minute output rate 61 bits/sec, 0 packets/sec
     654 packets input, 46188 bytes, 0 no buffer
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     684 packets output, 48080 bytes, 0 underruns
     0 output errors, 0 collisions, 1 interface resets
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
     DCD=up  DSR=up  DTR=up  RTS=up  CTS=up


```

### b. Выполните команду show ip route ospf на маршрутизаторе R1, чтобы просмотреть суммарную стоимость для маршрута к сети 192.168.23.0/24 через интерфейс S0/0/0. Обратите внимание, что к сети 192.168.23.0/24 есть два маршрута с равной стоимостью (128): один через интерфейс S0/0/0, другой через интерфейс S0/0/1.

### R1:

```
R1#sh ip route ospf 
O    192.168.2.0 [110/164] via 192.168.12.2, 00:01:19, Serial0/0/0
O    192.168.3.0 [110/164] via 192.168.13.2, 00:01:19, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/6540] via 192.168.12.2, 00:01:19, Serial0/0/0
                     [110/6540] via 192.168.13.2, 00:01:19, Serial0/0/1

```
### c.	Выполните команду bandwidth 128, чтобы установить на интерфейсе S0/0/0 пропускную способность равную 128 Кб/с.

### R1:

```
R1(config)#int s0/0/0
R1(config-if)#ban
R1(config-if)#bandwidth 128

```


### d. Повторно выполните команду show ip route ospf. В таблице маршрутизации больше не отображается маршрут к сети 192.168.23.0/24 через интерфейс S0/0/0. Это связано с тем, что оптимальный маршрут с наименьшей стоимостью проложен через S0/0/1.

### R1:

```
R1#sh ip route ospf 
O    192.168.2.0 [110/129] via 192.168.13.2, 00:00:16, Serial0/0/1
O    192.168.3.0 [110/65] via 192.168.13.2, 00:25:20, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/128] via 192.168.13.2, 00:00:16, Serial0/0/1

```


### e. Выполните show ip ospf interface brief. Стоимость для интерфейса S0/0/0 изменилась с 64 на 781, что является более точным представлением стоимости скорости канала.(такой команды нет в pkt)


### f. Измените пропускную способность для интерфейса S0/0/1 на значение, установленное для интерфейса S0/0/0 маршрутизатора R1.

### R1:

```
R1(config)#int s0/0/1
R1(config-if)#bandwidth 128
R1(config-if)#

```

### g. Повторно выполните команду show ip route ospf, чтобы просмотреть суммарную стоимость обоих маршрутов к сети 192.168.23.0/24. Обратите внимание, что к сети 192.168.23.0/24 есть два маршрута с одинаковой стоимостью (845): один через интерфейс S0/0/0, другой через интерфейс S0/0/1

### R1:

```
R1#sh ip route ospf 
O    192.168.2.0 [110/782] via 192.168.12.2, 00:00:01, Serial0/0/0
O    192.168.3.0 [110/782] via 192.168.13.2, 00:00:01, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/845] via 192.168.12.2, 00:00:01, Serial0/0/0
                     [110/845] via 192.168.13.2, 00:00:01, Serial0/0/1
```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/8.2.4.5%20Lab%20-%20Configuring%20Basic%20Single-Area%20OSPFv2.pdf%20-%20Word%2016.11.2023%2012_01_41.png?raw=true)

### h. Выполните команду show ip route ospf на R3. Суммарная стоимость сети 192.168.1.0/24 по-прежнему равна 65. В отличие от команды clock rate, команду bandwidth следует выполнить на каждом конце последовательного канала.


### R3:

```
R3#sh ip route ospf 
O    192.168.1.0 [110/65] via 192.168.13.1, 00:27:36, Serial0/0/0
O    192.168.2.0 [110/65] via 192.168.23.1, 00:27:36, Serial0/0/1
     192.168.12.0/30 is subnetted, 1 subnets
O       192.168.12.0 [110/128] via 192.168.23.1, 00:02:35, Serial0/0/1

```


### i. Выполните команду bandwidth 128 на всех остальных последовательных интерфейсах в топологии.


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/8.2.4.5%20Lab%20-%20Configuring%20Basic%20Single-Area%20OSPFv2.pdf%20-%20Word%2016.11.2023%2011_57_30.png?raw=true)

### **Шаг 3: Измените стоимость маршрута.**


### a. Введите команду show ip route ospf на маршрутизаторе R1.

### R1:
```
R1#sh ip route ospf 
O    192.168.2.0 [110/782] via 192.168.12.2, 00:30:28, Serial0/0/0
O    192.168.3.0 [110/782] via 192.168.13.2, 00:30:28, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/1562] via 192.168.12.2, 00:27:47, Serial0/0/0
                     [110/1562] via 192.168.13.2, 00:27:47, Serial0/0/1
```

### b.	Выполните команду ip ospf cost 1565 на интерфейсе S0/0/1 маршрутизатора R1. Стоимость 1565 является выше суммарной стоимости маршрута, проходящего через R2 (1562).

```
R1(config)#interface s0/0/1
R1(config-if)#ip ospf cost 1565

```

### c. Повторно выполните команду show ip route ospf на R1, чтобы отобразить изменения в таблице маршрутизации. Теперь все маршруты OSPF для маршрутизатора R1 направляются через маршрутизатор R2.

```

R1#sh ip route ospf 
O    192.168.2.0 [110/782] via 192.168.12.2, 00:32:09, Serial0/0/0
O    192.168.3.0 [110/1563] via 192.168.12.2, 00:00:31, Serial0/0/0
     192.168.23.0/30 is subnetted, 1 subnets
O       192.168.23.0 [110/1562] via 192.168.12.2, 00:00:31, Serial0/0/0
```



![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB16/8.2.4.5%20Lab%20-%20Configuring%20Basic%20Single-Area%20OSPFv2.pdf%20-%20Word%2016.11.2023%2012_27_04.png?raw=true)









































































