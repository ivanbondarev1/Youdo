# **Лабораторная работа. "Настройка расширенных функций EIGRP для IPv4"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB14/Cisco%20Packet%20Tracer%2029.10.2023%2015_45_36.png?raw=true)

## **Задачи**
+ ### Часть 1. Создание сети и настройка базовых параметров устройств    
+ ### Часть 2. Настройка EIGRP и проверка подключения  
+ ### Часть 3. Настройка суммирования для EIGRP   
+ ### Часть 4. Настройка и распространение статического маршрута по умолчанию   
+ ### Часть 5. Выполнение точной настройки EIGRP 


## **Решение**
## **Часть 1. Построение сети и загрузка настроек устройств**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Выполните запуск и перезагрузку маршрутизаторов.**
### **Шаг 4. Настройте базовые параметры каждого маршрутизатора.**

### R1:
```
Router(config)#host R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
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
R2(config-line)#exit
R2(config)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#int g0/0
R2(config-if)#ip addr 192.168.2.1 255.255.255.0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R2(config-if)#int s0/0/0
R2(config-if)#ip addr 192.168.12.2 255.255.255.252
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#int s0/0/1
R2(config-if)#ip addr 1
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed sta
R2(config-if)#ip addr 192.168.23.1 255.255.255.252
R2(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#


```

### R3:

```

Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#host R3
R3(config)#no ip domain-lookup
R3(config)#enable secret class
R3(config)#line con 0
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#exit
R3(config)#line vty 0 4
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#exit
R3(config)#int g0/0
R3(config-if)#ip addr 192.168.3.1 255.255.255.0
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R3(config-if)#int s0/0/0
R3(config-if)#ip addr 192.168.13.2 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R3(config-if)#int s0/0/1
R3(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R3(config-if)#ip addr 192.168.23.2 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

R3(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R3(config-if)#
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#

```



## **Часть 2. Настройка EIGRP и проверка подключения**

### **Шаг 1. Настройте EIGRP.**


### a. На маршрутизаторе R1 настройте маршрутизацию EIGRP с номером автономной системы (AS) 1 для всех сетей с прямым подключением. Запишите использованные команды в поле ниже.  


### R1:

```
R1(config)#router eigrp 1
R1(config-router)#net 192.168.1.0
R1(config-router)#net 192.168.12.0 0.0.0.3
R1(config-router)#net 192.168.13.0 0.0.0.3
```

### b.	Для интерфейса локальной сети маршрутизатора R1 отключите передачу пакетов приветствия (hello) EIGRP. Ниже напишите команду, которую вы использовали. 

### R1:

```
R1(config-router)#passive-interface g0/0
```

### c.	На маршрутизаторе R1 настройте пропускную способность для интерфейса S0/0/0 равной 1024 Кбит/с, а для интерфейса S0/0/1 равной 64 Кбит/с. Запишите использованные команды в поле ниже. Примечание. Команда bandwidth влияет только на расчёт метрики EIGRP, а не на фактическую пропускную способность последовательного канала. 

### R1:

```
R1(config)#int s0/0/0
R1(config-if)#ban
R1(config-if)#bandwidth 1024
R1(config-if)#int s0/0/1
R1(config-if)#ban
R1(config-if)#bandwidth 64
```

### d. На маршрутизаторе R2 настройте маршрутизацию EIGRP с идентификатором AS 1 для всех сетей, отключите передачу пакетов приветствия (hello) EIGRP для интерфейса локальной сети и задайте пропускную способность для интерфейса S0/0/0 равной 1024 Кбит/с. 


### R2:


```
R2(config)#router eigrp 1
R2(config-router)#pass
R2(config-router)#passive-interface g0/0
R2(config-router)#int s0/0/0
R2(config-if)#ban
R2(config-if)#bandwidth 1024
```

### e. На маршрутизаторе R3 настройте маршрутизацию EIGRP с идентификатором AS 1 для всех сетей, отключите передачу пакетов приветствия (hello) EIGRP для интерфейса локальной сети и задайте пропускную способность для интерфейса S0/0/0 равной 64 Кбит/с. 


### R3:

```
R3(config)#router eigrp 1
R3(config-router)#pass
R3(config-router)#passive-interface g0/0
R3(config-router)#int s0/0/0
R3(config-if)#ban
R3(config-if)#bandwidth 64
R3(config-if)

```

### **Шаг 2. Проверьте соединение.**

### **На R2 и R3 не настроен router eigrp и не аносируются никакие сети**


### R2:

```
R2(config)#router eigrp 1
R2(config-router)#net 192.168.2.0 0.0.0.255
R2(config-router)#net 192.168.12.0 0.0.0.3
R2(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.12.1 (Serial0/0/0) is up: new adjacency

R2(config-router)#net 192.168.23.0 0.0.0.3
R2(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.23.2 (Serial0/0/1) is up: new adjacency

R2(config-router)#

```

### R3:

```
R3(config)#router eigrp 1
R3(config-router)#net
R3(config-router)#network 192.168.3.0 0.0.0.255
R3(config-router)#network 192.168.13.0 0.0.0.3
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is up: new adjacency

R3(config-router)#network 192.168.23.0 0.0.0.3
R3(config-router)#
```

### PC-A(ping PC-C, PC-B):

```
C:\>ping 192.168.3.3

Pinging 192.168.3.3 with 32 bytes of data:

Request timed out.
Reply from 192.168.3.3: bytes=32 time=17ms TTL=125
Reply from 192.168.3.3: bytes=32 time=16ms TTL=125
Reply from 192.168.3.3: bytes=32 time=18ms TTL=125

Ping statistics for 192.168.3.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 16ms, Maximum = 18ms, Average = 17ms

C:\>ping 192.168.2.3

Pinging 192.168.2.3 with 32 bytes of data:

Request timed out.
Reply from 192.168.2.3: bytes=32 time=10ms TTL=126
Reply from 192.168.2.3: bytes=32 time=11ms TTL=126
Reply from 192.168.2.3: bytes=32 time=11ms TTL=126

Ping statistics for 192.168.2.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 11ms, Average = 10ms

C:\>
```

## **Часть 3.Настройка EIGRP для автоматического суммирования**


### Шаг 1. Настройте EIGRP для автоматического суммирования. 

### a. Выполните команду show ip protocols на роутере R1. Как по умолчанию настроено автоматическое суммирование в EIGRP? 

### ***По умолчанию автоматическое суммирование выключено***

### R1:

```
R1#sh ip protocols 

Routing Protocol is "eigrp  1 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 1
  EIGRP-IPv4 Protocol for AS(1)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 192.168.1.1
    Topology : 0 (base)
      Active Timer: 3 min
      Distance: internal 90 external 170
      Maximum path: 4
      Maximum hopcount 100
      Maximum metric variance 1

  Automatic Summarization: disabled
  Automatic address summarization: 
  Maximum path: 4
  Routing for Networks:  
     192.168.1.0
     192.168.12.0/30
     192.168.13.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.13.2    90            1293990    
    192.168.12.2    90            1574434    
  Distance: internal 90 external 170


```



### b. Настройте loopback-адреса на R1. 

### R1:

```
R1(config)#int lo1

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R1(config-if)#ip addr 192.168.11.1 255.255.255.252
R1(config-if)#int lo5

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback5, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback5, changed state to up

R1(config-if)#ip addr 192.168.11.5 255.255.255.252
R1(config-if)#int lo9

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback9, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback9, changed state to up

R1(config-if)#ip addr 192.168.11.9 255.255.255.252
R1(config-if)#int lo13

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback13, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback13, changed state to up

R1(config-if)#ip addr 192.168.11.13 255.255.255.252
R1(config-if)#

```



### c.	Добавьте соответствующие инструкции network для процесса EIGRP на маршрутизаторе R1. Запишите использованные команды в поле ниже. 

### R1:

```
R1(config)#router eigrp 1
R1(config-router)#net 192.168.11.0 0.0.0.3
R1(config-router)#net 192.168.11.4 0.0.0.3
R1(config-router)#net 192.168.11.8 0.0.0.3
R1(config-router)#net 192.168.11.12 0.0.0.3
```



### d.	На маршрутизаторе R2 выполните команду show ip route eigrp. Как сети loopback представлены в результатах этой команды? 

### ***Эти сети представлены как доступные по EIGRP через соседа 192.168.12.1***


### R2:

```
R2#sh ip route eigrp
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

D    192.168.1.0/24 [90/3014400] via 192.168.12.1, 00:12:37, Serial0/0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.3.0/24 [90/2172416] via 192.168.23.2, 00:12:31, Serial0/0/1
     192.168.11.0/30 is subnetted, 4 subnets
D       192.168.11.0/30 [90/3139840] via 192.168.12.1, 00:01:05, Serial0/0/0
D       192.168.11.4/30 [90/3139840] via 192.168.12.1, 00:00:54, Serial0/0/0
D       192.168.11.8/30 [90/3139840] via 192.168.12.1, 00:00:47, Serial0/0/0
D       192.168.11.12/30 [90/3139840] via 192.168.12.1, 00:00:40, Serial0/0/0
     192.168.13.0/30 is subnetted, 1 subnets
D       192.168.13.0/30 [90/41024000] via 192.168.12.1, 00:12:37, Serial0/0/0
                        [90/41024000] via 192.168.23.2, 00:12:31, Serial0/0/1



```






### e.	На маршрутизаторе R1 выполните команду auto-summary в рамках процесса EIGRP. Как изменилась таблица маршрутизации на R2? 

### ***Произошла суммаризация сети 192.168.11.0 по классовой маске***

### R1:

```
R1(config)#router eigrp 1
R1(config-router)#aut
R1(config-router)#auto-summary 
R1(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.12.2 (Serial0/0/0) resync: summary configured

%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.2 (Serial0/0/1) resync: summary configured
```



### R2:

```
R2#sh ip route eigrp 
D    192.168.1.0/24 [90/3014400] via 192.168.12.1, 00:00:03, Serial0/0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.3.0/24 [90/2172416] via 192.168.23.2, 00:06:22, Serial0/0/1
D    192.168.11.0/24 [90/3139840] via 192.168.12.1, 00:00:03, Serial0/0/0
     192.168.12.0/24 is variably subnetted, 3 subnets, 3 masks
D       192.168.12.0/24 [90/41536000] via 192.168.23.2, 00:00:03, Serial0/0/1
     192.168.13.0/24 is variably subnetted, 2 subnets, 2 masks
D       192.168.13.0/24 [90/41024000] via 192.168.12.1, 00:00:03, Serial0/0/0
D       192.168.13.0/30 [90/41024000] via 192.168.23.2, 00:06:22, Serial0/0/1
     192.168.33.0/28 is subnetted, 1 subnets
D       192.168.33.0 [90/2297856] via 192.168.23.2, 00:06:22, Serial0/0/1

```







### f. Повторите подэтапы с b по e, добавив интерфейсы обратной связи, сети процессов EIGRP и автоматическое объединение маршрутов на R3.

### R3:

```
R3#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R3(config)#int lo1

R3(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R3(config-if)#ip addr 192.168.33.1 255.255.255.252
R3(config-if)#int lo5

R3(config-if)#
%LINK-5-CHANGED: Interface Loopback5, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback5, changed state to up

R3(config-if)#ip addr 192.168.33.5 255.255.255.252
R3(config-if)#int lo9

R3(config-if)#
%LINK-5-CHANGED: Interface Loopback9, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback9, changed state to up

R3(config-if)#ip addr 192.168.33.9 255.255.255.252
R3(config-if)#int lo13

R3(config-if)#
%LINK-5-CHANGED: Interface Loopback13, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback13, changed state to up

R3(config-if)#ip addr 192.168.33.13 255.255.255.252
R3(config-if)#exit
R3(config)#rou
R3(config)#router ei
R3(config)#router eigrp 1
R3(config-router)#net 192.168.33.0 0.0.0.3
R3(config-router)#net 192.168.33.4 0.0.0.3
R3(config-router)#net 192.168.33.8 0.0.0.3
R3(config-router)#net 192.168.33.12 0.0.0.3
R3(config-router)#aut
R3(config-router)#auto-summary 
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.23.1 (Serial0/0/1) resync: summary configured

%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) resync: summary configured

R3(config-router)#
```

### R2(суммарный маршрут до 192.168.33.0 на R3):

```
R2#sh ip route eigrp 
D    192.168.1.0/24 [90/3014400] via 192.168.12.1, 00:15:28, Serial0/0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.3.0/24 [90/2172416] via 192.168.23.2, 00:03:19, Serial0/0/1
D    192.168.11.0/24 [90/3139840] via 192.168.12.1, 00:15:28, Serial0/0/0
     192.168.12.0/24 is variably subnetted, 3 subnets, 3 masks
D       192.168.12.0/24 [90/41536000] via 192.168.23.2, 00:03:19, Serial0/0/1
D    192.168.13.0/24 [90/41024000] via 192.168.12.1, 00:15:28, Serial0/0/0
                     [90/41024000] via 192.168.23.2, 00:03:19, Serial0/0/1
     192.168.23.0/24 is variably subnetted, 3 subnets, 3 masks
D       192.168.23.0/24 [90/41536000] via 192.168.12.1, 00:03:19, Serial0/0/0
D    192.168.33.0/24 [90/2297856] via 192.168.23.2, 00:03:19, Serial0/0/1


```

## **Часть 4: Настройка и распространение статического маршрута по умолчанию**


### a.	Настройте loopback-адрес на R2. 

### R2:

```
R2(config)#int lo1

R2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R2(config-if)#ip addr 192.168.22.1 255.255.255.252
R2(config-if)#


```

### b.	Настройте статический маршрут по умолчанию с выходным интерфейсом Lo1.  


### R2:

```
R2(config)#ip route 0.0.0.0 0.0.0.0 lo1
%Default route without gateway, if not a point-to-point interface, may impact performance

```

### c.	Выполните команду redistribute static в рамках процесса EIGRP, чтобы распространить статический маршрут по умолчанию на другие участвующие маршрутизаторы. 

### R2:

```
R2(config)#router eigrp 1
R2(config-router)#red
R2(config-router)#redistribute st
R2(config-router)#redistribute static 

```


### d.	Выполните команду show ip protocols на маршрутизаторе R2, чтобы убедиться, что статический маршрут распределяется. 

### R2:

```
R2#sh ip protocols 

Routing Protocol is "eigrp  1 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 1, static 
  EIGRP-IPv4 Protocol for AS(1)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 192.168.2.1
    Topology : 0 (base)
      Active Timer: 3 min
      Distance: internal 90 external 170
      Maximum path: 4
      Maximum hopcount 100
      Maximum metric variance 1

  Automatic Summarization: disabled
  Automatic address summarization: 
  Maximum path: 4
  Routing for Networks:  
     192.168.2.0
     192.168.12.0/30
     192.168.23.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.23.2    90            7922       
    192.168.12.1    90            8087       
  Distance: internal 90 external 170


```


### e. На маршрутизаторе R1 выполните команду show ip route eigrp | include 0.0.0.0, чтобы просмотреть инструкции, относящиеся к маршруту по умолчанию. Как статический маршрут по умолчанию представлен в результатах этой команды? Укажите административную дистанцию (AD) распространяемого маршрута. 

### ***Маршрут представлен как изученный по EIGRP и доступный через соседа 192.168.12.2. AD для внешних маршутов EIGRP = 170***


### R1:

```
R1#show ip route eigrp | include 0.0.0.0
D*EX 0.0.0.0/0 [170/4291840] via 192.168.12.2, 00:04:14, Serial0/0/0
```




## **Часть 5. Подгонка EIGRP**


### Шаг 1. Настройте параметры использования пропускной способности для EIGRP. 



### Шаг 2. Настройте интервал отправки пакетов приветствия (hello) и таймер удержания для EIGRP. 

### a. На маршрутизаторе R2 выполните команду show ip eigrp interfaces detail (такой команды нет в pkt), чтобы просмотреть интервал приветствия и удержания для EIGRP. 


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB14/14.docx%20-%20Word%2031.10.2023%2014_24_02.png?raw=true)


### b.	Для интерфейсов S0/0/0 и S0/0/1 маршрутизатора R1 настройте интервал приветствия равным 60 секунд, а таймер удержания равным 180 секунд, именно в этом порядке (в pkt нельзя настроить hold timer). 


### c.	Для последовательных интерфейсах маршрутизаторов R2 и R3 настройте интервал приветствия равным 60 секунд, а таймер удержания равным 180 секунд. 

### d.	На маршрутизаторе R2 выполните команду show ip eigrp interfaces detail, чтобы проверить конфигурацию. 

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB14/99.png?raw=true)



















