# **Лабораторная работа. "Базовая настройка протокола EIGRP для IPv4"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/Cisco%20Packet%20Tracer%2027.10.2023%2013_29_54.png?raw=true)

## **Задачи**
+ ### Часть 1. Построение сети и проверка соединения  
+ ### Часть 2. Настройка маршрутизации EIGRP 
+ ### Часть 3. Проверка маршрутизации EIGRP 
+ ### Часть 4. Настройка пропускной способности и пассивных интерфейсов 


## **Решение**
## **Часть 1. Построение сети и проверка соединения **

### **Шаг 1. Подключите кабели в сети в соответствии с топологией.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Выполните запуск и перезагрузку маршрутизаторов.**
### **Шаг 4. Настройте базовые параметры каждого маршрутизатора.**

### R1:
```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#host
Router(config)#hostname R1
R1(config)#no ip domain-lo
R1(config)#no ip domain-lookup 
R1(config)#int g0/0
R1(config-if)#ip addr 192.168.1.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R1(config-if)#int s0/0/0
R1(config-if)#ip addr 10.1.1.1 255.255.255.252
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
R1(config-if)#int s0/0/1
R1(config-if)#ip addr 10.3.3.1 255.255.255.252
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R1(config-if)#exit
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#exit
R1(config)#ena
R1(config)#enable se
R1(config)#enable secret class
R1(config)#banner motd *STAY_OUT*
R1(config)#
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
Router(config)#host
Router(config)#hostname R2
R2(config)#int g0/0
R2(config-if)#ip addr 192.168.2.1 255.255.255.0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R2(config-if)#int s0/0/0
R2(config-if)#ip addr 10.1.1.2 255.255.255.252
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#int s0/0/1
R2(config-if)#ip addr 10
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed st
R2(config-if)#ip addr 10.2.2.2 255.255.255.252
R2(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#exit
R2(config)#no ip domain-lo
R2(config)#no ip domain-lookup 
R2(config)#line
R2(config)#line vty 0 4
R2(config-line)#pass
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#logging synchronous
R2(config-line)#exit
R2(config)#en
R2(config)#ena
R2(config)#enable se
R2(config)#enable secret class
R2(config)#banner motd *STAY_OUT*
R2(config)#
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R2#


```

### R3:

```

Router(config)#ho
Router(config)#hostname R3
R3(config)#no ip dom
R3(config)#no ip domain-lo
R3(config)#no ip domain-lookup 
R3(config)#int g0/0
R3(config-if)#ip addr 192.168.3.1 255.255.255.0
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R3(config-if)#int s0/0/0
R3(config-if)#ip addr 10.3.3.2 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R3(config-if)#int s0/0/1
R3(config-if)#ip 
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to 
R3(config-if)#ip addr 10.2.2.1 255.255.255.252
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

R3(config-if)#exit
R3(config)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R3(config)#line vty 0 4
R3(config-line)#pass
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#logging synchronous
R3(config-line)#exit
R3(config)#enab
R3(config)#enable se
R3(config)#enable secret class
R3(config)#banner motd *STAY_OUT*
R3(config)#
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#

```

### PC-A:

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/PC-A%2027.10.2023%2013_51_56.png?raw=true)

### PC-B:

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/PC-B%2027.10.2023%2013_52_25.png?raw=true)

### PC-C:

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/PC-C%2027.10.2023%2013_52_57.png?raw=true)

### **Шаг 5: Проверьте соединение.**


## **Часть 2. Настройка маршрутизации EIGRP**

### **Шаг 1. Включите маршрутизацию EIGRP на маршрутизаторе R1. Используйте номер автономной системы 10.**


### **Шаг 2. Объявите напрямую подключенные сети на маршрутизаторе R1, используя шаблонную маску.**

### R1:

```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#rou
R1(config)#router ei
R1(config)#router eigrp 10
R1(config-router)#network 10.1.1.0 0.0.0.3 
R1(config-router)#network 192.168.1.0 0.0.0.255 
R1(config-router)#network 10.3.3.0 0.0.0.3
R1(config-router)#
```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2015_42_57.png?raw=true)




### **Шаг 3. Включите маршрутизацию EIGRP и объявите напрямую подключенные сети на маршрутизаторах R2 и R3.**

### R2:

```
R2(config)#router ei
R2(config)#router eigrp 10
R2(config-router)#net 10.1.1.0 0.0.0.3
R2(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.1.1.1 (Serial0/0/0) is up: new adjacency

R2(config-router)#net 10.2.2.0 0.0.0.3
R2(config-router)#net 192.168.2.0 0.0.0.255
```
### R3:

```
R3(config)#router ei
R3(config)#router eigrp 10
R3(config-router)#net 192.168.3.0 0.0.0.255
R3(config-router)#net 10.3.3.0 0.0.0.3
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.3.3.1 (Serial0/0/0) is up: new adjacency

R3(config-router)#net 10.2.2.0 0.0.0.3
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.2.2.2 (Serial0/0/1) is up: new adjacency

R3(config-router)#

```
### **Шаг 4. Проверьте сквозное подключение.** 


## **Часть 3: Проверка маршрутизации EIGRP**

### **Шаг 1. Анализ таблицы соседних устройств EIGRP.**

### R1:

```
R1#show ip eigrp neighbors 
IP-EIGRP neighbors for process 10
H   Address         Interface      Hold Uptime    SRTT   RTO   Q   Seq
                                   (sec)          (ms)        Cnt  Num
0   10.1.1.2        Se0/0/0        14   00:08:57  40     1000  0   16
1   10.3.3.2        Se0/0/1        10   00:05:05  40     1000  0   17

```

### **Шаг 2. Проанализируйте таблицу IP-маршрутизации EIGRP.**

### R1:

```
R1#show ip eigrp neighbors 
IP-EIGRP neighbors for process 10
H   Address         Interface      Hold Uptime    SRTT   RTO   Q   Seq
                                   (sec)          (ms)        Cnt  Num
0   10.1.1.2        Se0/0/0        14   00:08:57  40     1000  0   16
1   10.3.3.2        Se0/0/1        10   00:05:05  40     1000  0   17

R1#show ip route eigrp 
     10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
D       10.2.2.0/30 [90/2681856] via 10.1.1.2, 00:10:32, Serial0/0/0
                    [90/2681856] via 10.3.3.2, 00:06:40, Serial0/0/1
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.2.0/24 [90/2172416] via 10.1.1.2, 00:05:36, Serial0/0/0
D    192.168.3.0/24 [90/2172416] via 10.3.3.2, 00:06:52, Serial0/0/1

```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2015_59_53.png?raw=truee)


### **Шаг 3. Проанализируйте таблицу соседних устройств EIGRP.**

### R1:

```
R1#show ip eigrp topology 
IP-EIGRP Topology Table for AS 10/ID(192.168.1.1)

Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
       r - Reply status

P 10.1.1.0/30, 1 successors, FD is 2169856
         via Connected, Serial0/0/0
P 10.2.2.0/30, 2 successors, FD is 2681856
         via 10.1.1.2 (2681856/2169856), Serial0/0/0
         via 10.3.3.2 (2681856/2169856), Serial0/0/1
P 10.3.3.0/30, 1 successors, FD is 2169856
         via Connected, Serial0/0/1
P 192.168.1.0/24, 1 successors, FD is 5120
         via Connected, GigabitEthernet0/0
P 192.168.2.0/24, 1 successors, FD is 2172416
         via 10.1.1.2 (2172416/5120), Serial0/0/0
P 192.168.3.0/24, 1 successors, FD is 2172416
         via 10.3.3.2 (2172416/5120), Serial0/0/1
```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2016_15_55.png?raw=true)






### **Шаг 4. Проверьте параметры маршрутизации EIGRP и объявленные сети.**

### R1:

```
R1#show ip protocols

Routing Protocol is "eigrp  10 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 10
  EIGRP-IPv4 Protocol for AS(10)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 10.1.1.1
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
     10.1.1.0/30
     192.168.1.0
     10.3.3.0/30
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    10.1.1.2        90            746545     
    10.3.3.2        90            977773     
  Distance: internal 90 external 170

```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2016_39_22.png?raw=true)


## **Часть 4. Настройка пропускной способности и пассивных интерфейсов**

### **Шаг 1: Изучите текущие настройки маршрутизации.**

### R1:
```
R1#show interface s0/0/0 
Serial0/0/0 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 10.1.1.1/30
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
  5 minute input rate 105 bits/sec, 0 packets/sec
  5 minute output rate 104 bits/sec, 0 packets/sec
     809 packets input, 48744 bytes, 0 no buffer
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     964 packets output, 57941 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
     DCD=up  DSR=up  DTR=up  RTS=up  CTS=up


```
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2016_51_03.png?raw=true)

### **Шаг 2. Измените пропускную способность на маршрутизаторах.**

### R1:

```
R1(config)#int s0/0/0
R1(config-if)#ban
R1(config-if)#bandwidth 2000
R1(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.1.1.2 (Serial0/0/0) is down: interface down

R1(config-if)#int 
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.1.1.2 (Serial0/0/0) is up: new adjacenc
               ^
% Invalid input detected at '^' marker.
	
R1(config-if)#int s0/0/1
R1(config-if)#ban
R1(config-if)#bandwidth 64
R1(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.3.3.2 (Serial0/0/1) is down: interface down

%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.3.3.2 (Serial0/0/1) is up: new adjacency

R1(config-if)#

```
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2027.10.2023%2017_00_18.png?raw=true)

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

     10.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
C       10.1.1.0/30 is directly connected, Serial0/0/0
L       10.1.1.1/32 is directly connected, Serial0/0/0
D       10.2.2.0/30 [90/2681856] via 10.1.1.2, 00:00:59, Serial0/0/0
C       10.3.3.0/30 is directly connected, Serial0/0/1
L       10.3.3.1/32 is directly connected, Serial0/0/1
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0
D    192.168.2.0/24 [90/1794560] via 10.1.1.2, 00:00:59, Serial0/0/0
D    192.168.3.0/24 [90/2684416] via 10.1.1.2, 00:00:48, Serial0/0/0


```


+ Измените пропускную способность для последовательных интерфейсов маршрутизаторов R2 и R3. 

### R2:

```
R2(config)#int s0/0/0
R2(config-if)#ban
R2(config-if)#bandwidth 2000
R2(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.1.1.1 (Serial0/0/0) is down: interface down
int 
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.1.1.1 (Serial0/0/0) is up: new adjace
R2(config-if)#int s0/0/1
R2(config-if)#ban
R2(config-if)#bandwidth 2000
R2(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.2.2.1 (Serial0/0/1) is down: interface down

%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.2.2.1 (Serial0/0/1) is up: new adjacency

R2(config-if)#
```

### R3:


```
R3(config)#int s0/0/0
R3(config-if)#band
R3(config-if)#bandwidth 64
R3(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.3.3.1 (Serial0/0/0) is down: interface down

%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.3.3.1 (Serial0/0/0) is up: new adjacency

R3(config-if)#int s0/0/1
R3(config-if)#ban
R3(config-if)#bandwidth 2000
R3(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.2.2.2 (Serial0/0/1) is down: interface down

%DUAL-5-NBRCHANGE: IP-EIGRP 10: Neighbor 10.2.2.2 (Serial0/0/1) is up: new adjacency

R3(config-if)#

```

### **Шаг 3: Проверьте изменения пропускной способности.**

### R1:

```
R1#show interface s0/0/0 
Serial0/0/0 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 10.1.1.1/30
  MTU 1500 bytes, BW 2000 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set, keepalive set (10 sec)
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0 (size/max/drops); Total output drops: 0
  Queueing strategy: weighted fair
  Output queue: 0/1000/64/0 (size/max total/threshold/drops)
     Conversations  0/0/256 (active/max active/max total)
     Reserved Conversations 0/0 (allocated/max allocated)
     Available Bandwidth 1500 kilobits/sec
  5 minute input rate 115 bits/sec, 0 packets/sec
  5 minute output rate 107 bits/sec, 0 packets/sec
     1077 packets input, 64822 bytes, 0 no buffer
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     1216 packets output, 72877 bytes, 0 underruns
     0 output errors, 0 collisions, 0 interface resets
     0 output buffer failures, 0 output buffers swapped out
     0 carrier transitions
     DCD=up  DSR=up  DTR=up  RTS=up  CTS=up
```


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2028.10.2023%2010_06_52.png?raw=true)



### **Шаг 4. Настройте на маршрутизаторах R1, R2 и R3 интерфейс G0/0 как пассивный.**


### R1:
```
R1(config)#router eigrp 10
R1(config-router)#passive-interface g0/0
R1(config-router)#
```

### R2:

```
R2(config)#router eigrp 10
R2(config-router)#passive-interface g0/0
R2(config-router)#
```

### R3:

```
R3(config)#router eigrp 10
R3(config-router)#passive-interface g0/0
R3(config-router)#
```


### **Шаг 5: Проверьте конфигурацию пассивных интерфейсов.**

### R1:

```
R1#sh ip protocols

Routing Protocol is "eigrp  10 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 10
  EIGRP-IPv4 Protocol for AS(10)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 10.1.1.1
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
     10.1.1.0/30
     192.168.1.0
     10.3.3.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    10.3.3.2        90            8149       
    10.1.1.2        90            8950       
  Distance: internal 90 external 170
```

### R2:

```
R2#sh ip protocols 

Routing Protocol is "eigrp  10 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 10
  EIGRP-IPv4 Protocol for AS(10)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 10.1.1.2
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
     10.1.1.0/30
     10.2.2.0/30
     192.168.2.0
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    10.2.2.1        90            6940       
    10.1.1.1        90            8950       
  Distance: internal 90 external 170

```


### R3:

```
R3#sh ip protocols 

Routing Protocol is "eigrp  10 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 10
  EIGRP-IPv4 Protocol for AS(10)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 10.2.2.1
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
     192.168.3.0
     10.3.3.0/30
     10.2.2.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    10.2.2.2        90            6940       
    10.3.3.1        90            8150       
  Distance: internal 90 external 170
```


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB13/13.docx%20-%20Word%2028.10.2023%2010_55_18.png?raw=true)

