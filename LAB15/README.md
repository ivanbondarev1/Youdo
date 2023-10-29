# **Лабораторная работа. "Поиск и устранение неполадок базового EIGRP для IPv4 и IPv6"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/Cisco%20Packet%20Tracer%2028.10.2023%2011_22_42.png?raw=true)

## **Задачи**
+ ### Часть 1. Построение сети и загрузка настроек устройств   
+ ### Часть 2. Отладка соединений на уровне 3  
+ ### Часть 3. Поиск и устранение неполадок в EIGRP для IPv4  
+ ### Часть 4. Поиск и устранение неполадок в EIGRP для IPv6  


## **Решение**
## **Часть 1. Построение сети и загрузка настроек устройств**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Загрузите настройки маршрутизатора. **
### **Шаг 4. Сохраните текущую конфигурацию на всех маршрутизаторах. **

### R1:
```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#service password-encryption
Router(config)#hostname R1
R1(config)#enable secret class
R1(config)#no ip domain lookup
R1(config)#ipv6 unicast-routing
R1(config)#interface GigabitEthernet0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#duplex auto
R1(config-if)#speed auto
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#ipv6 address 2001:DB8:ACAD:A::1/64
R1(config-if)#ipv6 eigrp 1
R1(config-if)#no shutdown

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R1(config-if)#interface Serial0/0/0
R1(config-if)#bandwidth 128
R1(config-if)#ip address 192.168.21.1 255.255.255.252
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#ipv6 address 2001:DB8:ACAD:12::1/64
R1(config-if)#ipv6 eigrp 1
R1(config-if)#clock rate 128000
This command applies only to DCE interfaces
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
R1(config-if)#interface Serial0/0/1
R1(config-if)#ip address 192.168.13.1 255.255.255.252
R1(config-if)#ipv6 address FE80::1 link-local
R1(config-if)#ipv6 address 2001:DB8:ACAD:31::1/64
R1(config-if)#ipv6 eigrp 1
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R1(config-if)#router eigrp 1
R1(config-router)#network 192.168.1.0
R1(config-router)#network 192.168.12.0 0.0.0.3
R1(config-router)#network 192.168.13.0 0.0.0.3
R1(config-router)#passive-interface GigabitEthernet0/0
R1(config-router)#eigrp router-id 1.1.1.1
R1(config-router)#ipv6 router eigrp 1
R1(config-rtr)#no shutdown
R1(config-rtr)#exit
R1(config)#banner motd @Unauthorized Access is Prohibited!@
R1(config)#line con 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#transport input all
R1(config-line)#end
R1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]

```


### R2:

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#service password-encryption
Router(config)#hostname R2
R2(config)#enable secret class
R2(config)#no ip domain lookup
R2(config)#ipv6 unicast-routing
R2(config)#interface GigabitEthernet0/0
R2(config-if)#ip address 192.168.2.1 255.255.255.0
R2(config-if)#duplex auto
R2(config-if)#speed auto
R2(config-if)#ipv6 address FE80::2 link-local
R2(config-if)#ipv6 address 2001:DB8:ACAD:B::2/64
R2(config-if)#ipv6 eigrp 1
R2(config-if)#interface Serial0/0/0
R2(config-if)#ip address 192.168.12.2 255.255.255.252
R2(config-if)#ipv6 address FE80::2 link-local
R2(config-if)#ipv6 address 2001:DB8:ACAD:12::2/64
R2(config-if)#ipv6 eigrp 1
R2(config-if)#no shutdown

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up
R2(config-if)#interface Serial0/0/1
R2(config-if)#bandwidth 128
R2(config-if)#ip address 192.168.23.1 255.255.255.0
R2(config-if)#ipv6 address FE80::2 link-local
R2(config-if)#ipv6 address 2001:DB8:ACAD:23::2/64
R2(config-if)#ipv6 eigrp 1
R2(config-if)#clock rate 128000
This command applies only to DCE interfaces
R2(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#router eigrp 1
R2(config-router)#network 192.168.12.0 0.0.0.3
R2(config-router)#network 192.168.23.0 0.0.0.3
R2(config-router)#passive-interface GigabitEthernet0/0
R2(config-router)#eigrp router-id 2.2.2.2
R2(config-router)#ipv6 router eigrp 1
R2(config-rtr)#no shutdown
R2(config-rtr)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::1 (Serial0/0/0) is up: new adjacency

R2(config-rtr)#passive-interface GigabitEthernet0/0
R2(config-rtr)#banner motd @Unauthorized Access is Prohibited!@
R2(config)#line con 0
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#logging synchronous
R2(config-line)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#transport input all
R2(config-line)#end
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]


```

### R3:

```

Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#service password-encryption
Router(config)#hostname R3
R3(config)#enable secret class
R3(config)#no ip domain lookup
R3(config)#interface GigabitEthernet0/0
R3(config-if)#ip address 192.168.3.1 255.255.255.0
R3(config-if)#duplex auto
R3(config-if)#speed auto
R3(config-if)#ipv6 address FE80::3 link-local
R3(config-if)#ipv6 address 2001:DB8:ACAD:C::3/64
R3(config-if)#ipv6 eigrp 1
% IPv6 routing not enabled
R3(config-if)#interface Serial0/0/0
R3(config-if)#ip address 192.168.13.2 255.255.255.252
R3(config-if)#ipv6 address FE80::3 link-local
R3(config-if)#ipv6 address 2001:DB8:ACAD:13::3/64
R3(config-if)#ipv6 eigrp 1
% IPv6 routing not enabled
R3(config-if)#no shutdown

R3(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R3(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R3(config-if)#interface Serial0/0/1
R3(config-if)#bandwidth 128
R3(config-if)#ip address 192.168.23.2 255.255.255.252
R3(config-if)#ipv6 address FE80::3 link-local
R3(config-if)#ipv6 address 2001:DB8:ACAD:23::3/64
R3(config-if)#ipv6 eigrp 1
% IPv6 routing not enabled
R3(config-if)#no shutdown

R3(config-if)#router eigrp 1
R3(config-router)#network 192.168.3.0
R3(config-router)#network 192.168.13.0 0.0.0.3
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is up: new adjacency

R3(config-router)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R3(config-router)#passive-interface GigabitEthernet0/0
R3(config-router)#eigrp router-id 3.3.3.3
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is down: route configuration changed

%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is up: new adjacency

R3(config-router)#banner motd @Unauthorized Access is Prohibited! @
R3(config)#line con 0
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#logging synchronous
R3(config-line)#line vty 0 4
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#transport input all
R3(config-line)#end
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#

```



## **Часть 2. Поиск и устранение неполадок связи на уровне 3**

### **Шаг 1. Убедитесь, что интерфейсы, указанные в таблице адресации, активны и настроены с правильными IP-адресами.**


### a. Выполните команду show ip interface brief на всех маршрутизаторах, чтобы убедиться, что все интерфейсы находятся в рабочем состоянии (up/up). Запишите полученные результаты. 


### ***R1 – Все интерфейсы up/up***
### ***R2 – G0/0 is administratively down***
### ***R3 – G0/0 is administratively down*** 

### R1:

```
R1#sh ip int brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.1.1     YES manual up                    up 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
Serial0/0/0            192.168.21.1    YES manual up                    up 
Serial0/0/1            192.168.13.1    YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down

```

### R2:

```
R2#sh ip int brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.2.1     YES manual administratively down down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
Serial0/0/0            192.168.12.2    YES manual up                    up 
Serial0/0/1            192.168.23.1    YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down

```


### R3:

```
R3#sh ip int brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     192.168.3.1     YES manual administratively down down 
GigabitEthernet0/1     unassigned      YES unset  administratively down down 
Serial0/0/0            192.168.13.2    YES manual up                    up 
Serial0/0/1            192.168.23.2    YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```

### b.	Введите команду show run interface, чтобы проверить назначения IP-адресов на всех интерфейсах маршрутизаторов. Сравните IP-адреса интерфейсов с данными из таблицы адресации и проверьте назначения маски подсети. Убедитесь, что для IPv6 был назначен адрес типа link-local. Запишите полученные результаты. 

### ***R1 – S0/0/0 неправильный ip адрес должен быть 192.168.12.1, S0/0/1 неправильный ipv6 адрес должен быть 2001:DB8:ACAD:13::1/64***
### ***R2 – S0/0/1 неправильная маска подсети должна быть 255.255.255.252***
### ***R3 – всё правильно***

### R1:

```
R1#sh run
Building configuration...

interface Serial0/0/0
 bandwidth 128
 ip address 192.168.21.1 255.255.255.252
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:12::1/64
 ipv6 eigrp 1
!
interface Serial0/0/1
 ip address 192.168.13.1 255.255.255.252
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:31::1/64
 ipv6 eigrp 1
```

### R2:

```
R2#sh run
!
interface Serial0/0/0
 ip address 192.168.12.2 255.255.255.252
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:12::2/64
 ipv6 eigrp 1
 clock rate 128000  
!
interface Serial0/0/1
 bandwidth 128
 ip address 192.168.23.1 255.255.255.0
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:23::2/64
 ipv6 eigrp 1
 clock rate 128000  
```



### R3:

```
R3#sh run

interface Serial0/0/0
 ip address 192.168.13.2 255.255.255.252
 ipv6 address FE80::3 link-local
 ipv6 address 2001:DB8:ACAD:13::3/64
 ipv6 eigrp 1
 clock rate 2000000
!
interface Serial0/0/1
 bandwidth 128
 ip address 192.168.23.2 255.255.255.252
 ipv6 address FE80::3 link-local
 ipv6 address 2001:DB8:ACAD:23::3/64
 ipv6 eigrp 1
 clock rate 2000000
!

```



### c.	Введите команду show interfaces interface-id для проверки настройки пропускной способности на последовательных интерфейсах. Запишите полученные результаты. 

### ***R1 – S0/0/1 неправильная пропускная способность 1544 должна быть 128***
### ***R2 – S0/0/0 неправильная пропускная способность 1544 должна быть 128***
### ***R3 – S0/0/0 неправильная пропускная способность 1544 должна быть 128***

### R1:

### s0/0/1
```
R1#sh interfaces s0/0/1
Serial0/0/1 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 192.168.13.1/30
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
```


### R2:

### s0/0/0

```
R2#sh interfaces s0/0/0
Serial0/0/0 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 192.168.12.2/30
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
```

### R3:

### s0/0/0

```
R3#sh interfaces s0/0/0
Serial0/0/0 is up, line protocol is up (connected)
  Hardware is HD64570
  Internet address is 192.168.13.2/30
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
     reliability 255/255, txload 1/255, rxload 1/255

```


### d. Введите команду show controllers interface-id, чтобы убедиться, что на всех последовательных интерфейсах DCE установлена тактовая частота 128 кбит/с. Введите команду show interfaces interface-id для проверки настройки пропускной способности на последовательных интерфейсах. Запишите полученные результаты

### ***R1 – правильная тактовая частота на S0/0/0***
### ***R2 – правильная тактовая частота на S0/0/1***
### ***R3 – неправильная тактовая частота 2000000 на S0/0/0 должна быть 128000***

### R3:

```
R3#show controllers s0/0/0
Interface Serial0/0/0
Hardware is PowerQUICC MPC860
DCE V.35, clock rate 2000000

```

### e.	Устраните все обнаруженные неполадки. Запишите команды, используемые для исправления неполадок. 

### R1:

```
R1(config)#interface s0/0/0
R1(config-if)#ip addr 192.168.12.1 255.255.255.252
R1(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.12.2 (Serial0/0/0) is up: new adjacency

R1(config-if)#int s0/0/1
R1(config-if)#ban
R1(config-if)#bandwidth 128
R1(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.2 (Serial0/0/1) is down: interface down

R1(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.2 (Serial0/0/1) is up: new adjacency

R1(config-if)#no ipv6 address 2001:db8:acad:31::1/64
R1(config-if)#ipv6 address 2001:db8:acad:13::1/64
R1(config-if)#end
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
R2(config)#int g0/0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R2(config-if)#
R2(config-if)#int s0/0/0
R2(config-if)#ban
R2(config-if)#bandwidth 128
R2(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.12.1 (Serial0/0/0) is down: interface down

R2(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.12.1 (Serial0/0/0) is up: new adjacency

R2(config-if)#int s0/0/1
R2(config-if)#ip addr 192.168.23.1 255.255.255.252
R2(config-if)#end
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
R3(config)#int g0/0
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R3(config-if)#int s0/0/0
R3(config-if)#ban
R3(config-if)#bandwidth 128
R3(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is down: interface down

R3(config-if)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.13.1 (Serial0/0/0) is up: new adjacency

R3(config-if)#clo
R3(config-if)#clock r
R3(config-if)#clock rate 128000
R3(config-if)#end
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#

```



### **Шаг 2: Проверьте наличие подключения на уровне 3.**


### PC-A(ping PC-B, PC-C)

```

C:\>ping 192.168.2.3

Pinging 192.168.2.3 with 32 bytes of data:

Reply from 192.168.1.1: Destination host unreachable.
Request timed out.
Reply from 192.168.1.1: Destination host unreachable.
Reply from 192.168.1.1: Destination host unreachable.

Ping statistics for 192.168.2.3:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>ping 192.168.3.3

Pinging 192.168.3.3 with 32 bytes of data:

Reply from 192.168.3.3: bytes=32 time=12ms TTL=126
Reply from 192.168.3.3: bytes=32 time=10ms TTL=126
Reply from 192.168.3.3: bytes=32 time=10ms TTL=126
Reply from 192.168.3.3: bytes=32 time=13ms TTL=126

Ping statistics for 192.168.3.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 13ms, Average = 11ms

C:\>
```

### PC-B(ping PC-C):

```
C:\>ping 192.168.3.3

Pinging 192.168.3.3 with 32 bytes of data:

Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 192.168.3.3:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>
```

## **Часть 3: Поиск и устранение неполадок в EIGRP для IPv4**


### **Шаг 1. Протестируйте сквозное соединение по IPv4.**


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/15.docx%20-%20Word%2029.10.2023%2010_40_18.png?raw=true)


### **Шаг 2. Убедитесь, что всем интерфейсам назначен протокол EIGRP для IPv4.**

### a.	Введите команду show ip protocols, чтобы убедиться, что EIGRP работает и объявляются все сети. Также эта команда позволяет убедиться, что идентификатор маршрутизатора настроен правильно, а интерфейсы локальной сети настроены как пассивные. Запишите полученные результаты. 

### ***R1 – router id настроен правильно и все сети анонсируются***
### ***R2 – router id настроен правильно, но не анонсируется сеть 192.168.2.0/24 и g0/0 не настроен как пассивный***
### ***R3 –  router id и пассивные интерфейсы настроены правильно, но не анонсируется сеть 192.168.23.0/24***


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
    Router-ID: 1.1.1.1
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
    192.168.13.2    90            7610       
    192.168.12.2    90            9332       
  Distance: internal 90 external 170
```

### R2:

```
R2#sh ip protocols 

Routing Protocol is "eigrp  1 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 1
  EIGRP-IPv4 Protocol for AS(1)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 2.2.2.2
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
     192.168.12.0/30
     192.168.23.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.12.1    90            9333       
  Distance: internal 90 external 170

```

### R3:

```
R3#sh ip protocols 

Routing Protocol is "eigrp  1 " 
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Default networks flagged in outgoing updates  
  Default networks accepted from incoming updates 
  Redistributing: eigrp 1
  EIGRP-IPv4 Protocol for AS(1)
    Metric weight K1=1, K2=0, K3=1, K4=0, K5=0
    NSF-aware route hold timer is 240
    Router-ID: 3.3.3.3
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
     192.168.13.0/30
  Passive Interface(s): 
    GigabitEthernet0/0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.13.1    90            7610       
  Distance: internal 90 external 170
```

### b.	Внесите необходимые изменения на основе выходных данных команды show ip protocols. Запишите команды, используемые для исправления неполадок.


### R2:

```
R2(config)#router eigrp 1
R2(config-router)#net 192.168.2.0 0.0.0.255
R2(config-router)#pass
R2(config-router)#passive-interface g0/0
R2(config-router)#end
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
R3(config)#router eigrp 1
R3(config-router)#net
R3(config-router)#network 192.168.23.0 0.0.0.3
R3(config-router)#
%DUAL-5-NBRCHANGE: IP-EIGRP 1: Neighbor 192.168.23.1 (Serial0/0/1) is up: new adjacency

R3(config-router)#end
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#

```



### c.	Повторно введите команду show ip protocols, чтобы убедиться в эффективности выполненных изменений. 



### **Шаг 3. Проверьте данные соседнего устройства EIGRP.**


### a.	Введите команду show ip eigrp neighbor, чтобы убедиться, что между соседними маршрутизаторами сформировались отношения смежности EIGRP. 

### R1:

```
R1#sh ip eigrp neighbors 
IP-EIGRP neighbors for process 1
H   Address         Interface      Hold Uptime    SRTT   RTO   Q   Seq
                                   (sec)          (ms)        Cnt  Num
0   192.168.13.2    Se0/0/1        12   00:33:43  40     1000  0   13
1   192.168.12.2    Se0/0/0        11   00:33:41  40     1000  0   11

```


### R2:

```
R2#sh ip eigrp neighbors 
IP-EIGRP neighbors for process 1
H   Address         Interface      Hold Uptime    SRTT   RTO   Q   Seq
                                   (sec)          (ms)        Cnt  Num
0   192.168.12.1    Se0/0/0        14   00:34:21  40     1000  0   8
1   192.168.23.2    Se0/0/1        13   00:07:34  40     1000  0   14

```


### R3:

```
R3#sh ip eigrp neighbors 
IP-EIGRP neighbors for process 1
H   Address         Interface      Hold Uptime    SRTT   RTO   Q   Seq
                                   (sec)          (ms)        Cnt  Num
0   192.168.13.1    Se0/0/0        12   00:34:01  40     1000  0   7
1   192.168.23.1    Se0/0/1        11   00:07:12  40     1000  0   12

```

### b.	Устраните все найденные неполадки. 

<неполадок нет>





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


### **Шаг 4. Проверьте информацию о маршрутах EIGRP для IPv4.**

### a.	Введите команду show ip route eigrp, чтобы убедиться, что у каждого маршрутизатора есть маршруты EIGRP для IPv4 ко всем несмежным сетям

### R1:

```
R1#sh ip route eigrp 
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.2.0/24 [90/20514560] via 192.168.12.2, 00:11:58, Serial0/0/0
D    192.168.3.0/24 [90/20514560] via 192.168.13.2, 00:37:35, Serial0/0/1
     192.168.23.0/30 is subnetted, 1 subnets
D       192.168.23.0 [90/21024000] via 192.168.12.2, 00:37:33, Serial0/0/0
                     [90/21024000] via 192.168.13.2, 00:10:46, Serial0/0/1

```

### R2:

```
R2#sh ip route eigrp 
D    192.168.1.0/24 [90/20514560] via 192.168.12.1, 00:37:09, Serial0/0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
D    192.168.3.0/24 [90/20514560] via 192.168.23.2, 00:10:22, Serial0/0/1
     192.168.13.0/30 is subnetted, 1 subnets
D       192.168.13.0 [90/21024000] via 192.168.12.1, 00:37:09, Serial0/0/0
                     [90/21024000] via 192.168.23.2, 00:10:22, Serial0/0/1

```

### R3:

```
R3#sh ip route eigrp 
D    192.168.1.0/24 [90/20514560] via 192.168.13.1, 00:38:09, Serial0/0/0
D    192.168.2.0/24 [90/20514560] via 192.168.23.1, 00:11:20, Serial0/0/1
     192.168.12.0/30 is subnetted, 1 subnets
D       192.168.12.0 [90/21024000] via 192.168.13.1, 00:38:09, Serial0/0/0
                     [90/21024000] via 192.168.23.1, 00:11:20, Serial0/0/1


```




![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/15.docx%20-%20Word%2029.10.2023%2011_14_01.png?raw=true)


### b.	Если какие-то данные маршрутизации пропущены, исправьте эти неполадки. (всё на месте)


### **Шаг 5. Проверьте сквозное подключение IPv4.**


### PC-A:

```
C:\>ping 192.168.2.3

Pinging 192.168.2.3 with 32 bytes of data:

Request timed out.
Reply from 192.168.2.3: bytes=32 time=13ms TTL=126
Reply from 192.168.2.3: bytes=32 time=10ms TTL=126
Reply from 192.168.2.3: bytes=32 time=11ms TTL=126

Ping statistics for 192.168.2.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 13ms, Average = 11ms

C:\>ping 192.168.3.3

Pinging 192.168.3.3 with 32 bytes of data:

Request timed out.
Reply from 192.168.3.3: bytes=32 time=12ms TTL=126
Reply from 192.168.3.3: bytes=32 time=12ms TTL=126
Reply from 192.168.3.3: bytes=32 time=14ms TTL=126

Ping statistics for 192.168.3.3:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 12ms, Maximum = 14ms, Average = 12ms

C:\>

```


### PC-B:

```
C:\>ping 192.168.3.3

Pinging 192.168.3.3 with 32 bytes of data:

Reply from 192.168.3.3: bytes=32 time=12ms TTL=126
Reply from 192.168.3.3: bytes=32 time=15ms TTL=126
Reply from 192.168.3.3: bytes=32 time=2ms TTL=126
Reply from 192.168.3.3: bytes=32 time=11ms TTL=126

Ping statistics for 192.168.3.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 2ms, Maximum = 15ms, Average = 10ms

C:\>

```



## **Часть 4. Поиск и устранение неполадок в EIGRP для IPv6**


### **Шаг 1. Протестируйте сквозное подключение IPv6.**

### PC-A:

```
C:\>ping 2001:DB8:ACAD:B::B

Pinging 2001:DB8:ACAD:B::B with 32 bytes of data:

Reply from 2001:DB8:ACAD:B::B: bytes=32 time=13ms TTL=126
Reply from 2001:DB8:ACAD:B::B: bytes=32 time=7ms TTL=126
Reply from 2001:DB8:ACAD:B::B: bytes=32 time=13ms TTL=126
Reply from 2001:DB8:ACAD:B::B: bytes=32 time=10ms TTL=126

Ping statistics for 2001:DB8:ACAD:B::B:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 7ms, Maximum = 13ms, Average = 10ms

C:\>ping 2001:DB8:ACAD:C::C

Pinging 2001:DB8:ACAD:C::C with 32 bytes of data:

Reply from 2001:DB8:ACAD:A::1: Destination host unreachable.
Reply from 2001:DB8:ACAD:A::1: Destination host unreachable.
Reply from 2001:DB8:ACAD:A::1: Destination host unreachable.
Reply from 2001:DB8:ACAD:A::1: Destination host unreachable.

Ping statistics for 2001:DB8:ACAD:C::C:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>

```

### **Шаг 2. Убедитесь, что одноадресная маршрутизация IPv6 включена на всех маршрутизаторах.**

### **На R3 не включен ipv6 unicast routing**


### a.Простым способом проверки включения IPv6-маршрутизации на маршрутизаторе является использование команды show run | section ipv6 unicast. При добавлении этого канала в команду show run команда ipv6 unicast-routing отображается, если маршрутизация IPv6 включена

### R1:

```
R1#show run | section ipv6 unicast
ipv6 unicast-routing
R1#
```


### R2:

```
R2#show run | section ipv6 unicast
ipv6 unicast-routing
R2#

```

### R3:

```
R3#show run | section ipv6 unicast
R3#

```

### b. If IPv6 unicast routing is not enabled on one or more routers, enable it now. Record the commands that were used to correct the issues.

### R3:

```
R3(config)#ipv6 unicast-routing
```

### **Шаг 3. Убедитесь, что всем интерфейсам назначен протокол EIGRP для IPv6.**

### a.	Введите команду show ipv6 protocols и убедитесь, что идентификатор маршрутизатора настроен правильно. Также эта команда позволяет убедиться, что интерфейсы локальной сети настроены как пассивные.

### ***R1 – router id настроен неправильно и g0/0 не настроен как пассивный***
### ***R2 – router id настроен неправильно***
### ***R3 – EIGRP для ipv6 не настроен на этом роутере***


### R1:

```
R1#sh ipv6 protocols 
IPv6 Routing Protocol is "connected"
IPv6 Routing Protocol is "ND"
IPv6 Routing Protocol is "eigrp 1"
  EIGRP metric weight K1=1, K2=0, K3=1, K4=0, K5=0
  EIGRP maximum hopcount 100
  EIGRP maximum metric variance 1
  Interfaces:
    GigabitEthernet0/0
    Serial0/0/0
    Serial0/0/1
Redistributing: eigrp 1
  Maximum path: 16
  Distance: internal 90 external 170
```

### R2

```
R2#sh ipv6 protocols 
IPv6 Routing Protocol is "connected"
IPv6 Routing Protocol is "ND"
IPv6 Routing Protocol is "eigrp 1"
  EIGRP metric weight K1=1, K2=0, K3=1, K4=0, K5=0
  EIGRP maximum hopcount 100
  EIGRP maximum metric variance 1
  Interfaces:
    GigabitEthernet0/0 (passive)
    Serial0/0/0
    Serial0/0/1
Redistributing: eigrp 1
  Maximum path: 16
  Distance: internal 90 external 170
```

### R3

```
R3#sh ipv6 protocols 
IPv6 Routing Protocol is "connected"
IPv6 Routing Protocol is "ND"

```

### b.	Сделайте необходимые изменения конфигурации. Запишите команды, используемые для исправления неполадок.

### R1:

```
R1(config)#ipv6 router eigrp 1
R1(config-rtr)#ei
R1(config-rtr)#eigrp rou
R1(config-rtr)#eigrp router-id 1.1.1.1
R1(config-rtr)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::2 (Serial0/0/0) is down: route configuration changed

R1(config-rtr)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::2 (Serial0/0/0) is up: new adjacency

R1(config-rtr)#pass
R1(config-rtr)#passive-interface g0/0
R1(config-rtr)#end
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
R2(config)#ipv6 router eigrp 1
R2(config-rtr)#ei
R2(config-rtr)#eigrp rou
R2(config-rtr)#eigrp router-id 2.2.2.2
R2(config-rtr)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::1 (Serial0/0/0) is down: route configuration changed

R2(config-rtr)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::1 (Serial0/0/0) is up: new adjacency

R2(config-rtr)#
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
R3(config)#ipv6 router eigrp 1
R3(config-rtr)#eigrp router-id 3.3.3.3
R3(config-rtr)#pass
R3(config-rtr)#passive-interface g0/0
R3(config-rtr)#no sh
R3(config-rtr)#int s0/0/0
R3(config-if)#ipv6 ei
R3(config-if)#ipv6 eigrp 1
R3(config-if)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::1 (Serial0/0/0) is up: new adjacency

R3(config-if)#int s0/0/1
R3(config-if)#ipv6 ei
R3(config-if)#ipv6 eigrp 1
R3(config-if)#
%DUAL-5-NBRCHANGE: IPv6-EIGRP 1: Neighbor FE80::2 (Serial0/0/1) is up: new adjacency

R3(config-if)#end
R3#
%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#
```

### c.	Повторно введите команду show ipv6 protocols, чтобы убедиться в правильности сделанных изменений.(Всё правильно)


### **Шаг 4. Убедитесь, что все маршрутизаторы обладают правильной информацией об отношениях смежности с соседними маршрутизаторами.**

### a.	Введите команду show ipv6 eigrp neighbor, чтобы убедиться, что между соседними маршрутизаторами сформировались отношения смежности. 

### R1:

```
R1#sh ipv6 eigrp neighbors 
IPv6-EIGRP neighbors for process 1
H   Address                 Interface     Hold   Uptime   SRTT   RTO  Q  Seq
                                          (sec)           (ms)       Cnt Num
0   Link-local address:       Se0/0/0      14   00:05:03  40     1000  0   24
    FE80::2                   
1   Link-local address:       Se0/0/1      14   00:03:27  40     1000  0   13
    FE80::3                   


```


### R2:

```
R2#sh ipv6 eigrp neighbors 
IPv6-EIGRP neighbors for process 1
H   Address                 Interface     Hold   Uptime   SRTT   RTO  Q  Seq
                                          (sec)           (ms)       Cnt Num
0   Link-local address:       Se0/0/0      10   00:05:24  40     1000  0   24
    FE80::1                   
1   Link-local address:       Se0/0/1      13   00:03:38  40     1000  0   14
    FE80::3                   


```



### R3:


```
R3#sh ipv6 eigrp neighbors 
IPv6-EIGRP neighbors for process 1
H   Address                 Interface     Hold   Uptime   SRTT   RTO  Q  Seq
                                          (sec)           (ms)       Cnt Num
0   Link-local address:       Se0/0/0      14   00:04:12  40     1000  0   25
    FE80::1                   
1   Link-local address:       Se0/0/1      11   00:04:02  40     1000  0   25
    FE80::2                   
```


### b.	Устраните все оставшиеся неполадки в отношениях смежности EIGRP. (всё работает)


### **Шаг 5. Проверьте сведения маршрутизации EIGRP для IPv6.**

### a.	Введите команду show ipv6 route eigrp и убедитесь в наличии маршрутов EIGRP для IPv6 ко всем несмежным сетям. 

### R1:

```
R1#sh ipv6 route 
IPv6 Routing Table - 9 entries
Codes: C - Connected, L - Local, S - Static, R - RIP, B - BGP
       U - Per-user Static route, M - MIPv6
       I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       D - EIGRP, EX - EIGRP external
C   2001:DB8:ACAD:A::/64 [0/0]
     via GigabitEthernet0/0, directly connected
L   2001:DB8:ACAD:A::1/128 [0/0]
     via GigabitEthernet0/0, receive
D   2001:DB8:ACAD:B::/64 [90/20514560]
     via FE80::2, Serial0/0/0
C   2001:DB8:ACAD:12::/64 [0/0]
     via Serial0/0/0, directly connected
L   2001:DB8:ACAD:12::1/128 [0/0]
     via Serial0/0/0, receive
C   2001:DB8:ACAD:13::/64 [0/0]
     via Serial0/0/1, directly connected
L   2001:DB8:ACAD:13::1/128 [0/0]
     via Serial0/0/1, receive
D   2001:DB8:ACAD:23::/64 [90/21024000]
     via FE80::2, Serial0/0/0
     via FE80::3, Serial0/0/1
L   FF00::/8 [0/0]
     via Null0, receive

```

### R2:

```
R2#sh ipv6 route 
IPv6 Routing Table - 9 entries
Codes: C - Connected, L - Local, S - Static, R - RIP, B - BGP
       U - Per-user Static route, M - MIPv6
       I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       D - EIGRP, EX - EIGRP external
D   2001:DB8:ACAD:A::/64 [90/20514560]
     via FE80::1, Serial0/0/0
C   2001:DB8:ACAD:B::/64 [0/0]
     via GigabitEthernet0/0, directly connected
L   2001:DB8:ACAD:B::2/128 [0/0]
     via GigabitEthernet0/0, receive
C   2001:DB8:ACAD:12::/64 [0/0]
     via Serial0/0/0, directly connected
L   2001:DB8:ACAD:12::2/128 [0/0]
     via Serial0/0/0, receive
D   2001:DB8:ACAD:13::/64 [90/21024000]
     via FE80::1, Serial0/0/0
     via FE80::3, Serial0/0/1
C   2001:DB8:ACAD:23::/64 [0/0]
     via Serial0/0/1, directly connected
L   2001:DB8:ACAD:23::2/128 [0/0]
     via Serial0/0/1, receive
L   FF00::/8 [0/0]
     via Null0, receive

```


### R3:

```
R3#sh ipv6 route 
IPv6 Routing Table - 10 entries
Codes: C - Connected, L - Local, S - Static, R - RIP, B - BGP
       U - Per-user Static route, M - MIPv6
       I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea, IS - ISIS summary
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
       ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       D - EIGRP, EX - EIGRP external
D   2001:DB8:ACAD:A::/64 [90/20514560]
     via FE80::1, Serial0/0/0
D   2001:DB8:ACAD:B::/64 [90/20514560]
     via FE80::2, Serial0/0/1
C   2001:DB8:ACAD:C::/64 [0/0]
     via GigabitEthernet0/0, directly connected
L   2001:DB8:ACAD:C::3/128 [0/0]
     via GigabitEthernet0/0, receive
D   2001:DB8:ACAD:12::/64 [90/21024000]
     via FE80::1, Serial0/0/0
     via FE80::2, Serial0/0/1
C   2001:DB8:ACAD:13::/64 [0/0]
     via Serial0/0/0, directly connected
L   2001:DB8:ACAD:13::3/128 [0/0]
     via Serial0/0/0, receive
C   2001:DB8:ACAD:23::/64 [0/0]
     via Serial0/0/1, directly connected
L   2001:DB8:ACAD:23::3/128 [0/0]
     via Serial0/0/1, receive
L   FF00::/8 [0/0]
     via Null0, receive
```


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/1.png?raw=true)






### **Шаг 6. Протестируйте сквозное подключение IPv6.**


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/3.png?raw=true)









