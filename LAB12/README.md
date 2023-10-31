# **Лабораторная работа. "Настройка протоколов HSRP"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB12/Cisco%20Packet%20Tracer%2031.10.2023%2015_51_38.png?raw=true)

## **Цели:**
+ ### Часть 1. Построение сети и проверка соединения  
+ ### Часть 2. Настройка обеспечения избыточности на первом хопе с помощью HSRP 



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
R1(config)#int g0/1
R1(config-if)#ip addr 192.168.1.1 255.255.255.0
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

R1(config-if)#int s0/0/0
R1(config-if)#ip addr 10.1.1.1 255.255.255.252
R1(config-if)#clo
R1(config-if)#clock r
R1(config-if)#clock rate 128000
This command applies only to DCE interfaces
R1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
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
R2(config)#int s0/0/0
R2(config-if)#ip addr 10.1.1.2 255.255.255.252
R2(config-if)#clo
R2(config-if)#clock rate 128000
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

R2(config-if)#int s0/0/1
R2(config-if)#ip addr 
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R2(config-if)#ip addr 10.2.2.2 255.255.255.252
R2(config-if)#clo
R2(config-if)#clock rate 128000
This command applies only to DCE interfaces
R2(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#int lo1

R2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R2(config-if)#ip addr 209.165.200.225 255.255.255.224
R2(config-if)#
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
R3(config)#int g0/1
R3(config-if)#ip addr 192.168.1.3 255.255.255.0
R3(config-if)#no sh

R3(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

R3(config-if)#int s0/0/1
R3(config-if)#ip addr 10.2.2.1 255.255.255.252
R3(config-if)#cl
R3(config-if)#clock rate 128000
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



### **Шаг 5. Настройте базовые параметры каждого коммутатора.**


### S1:


```
Switch(config)#host S1
S1(config)#no ip domain-lookup
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#logging synchronous
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#int vlan 1
S1(config-if)#ip addr 192.168.1.11 255.255.255.0
S1(config-if)#no sh

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
S1(config-if)#
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#
```

### S3:

```
Switch(config)#host S3
S3(config)#no ip domain-lookup
S3(config)#enable sec class
S3(config)#line con 0
S3(config-line)#pass cisco
S3(config-line)#login
S3(config-line)#logging synchronous
S3(config-line)#exit
S3(config)#line vty 0 4
S3(config-line)#pass cisco
S3(config-line)#login
S3(config-line)#exit
S3(config)#int vlan 1
S3(config-if)#ip addr 192.168.1.13 255.255.255.0
S3(config-if)#no sh

S3(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

S3(config-if)#
S3#
%SYS-5-CONFIG_I: Configured from console by console

S3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S3#


```

### **Шаг 6. Проверьте подключение между PC-A и PC-C.**


### ***Отправьте эхо-запрос с компьютера PC-A на компьютер PC-C. Успешно ли выполнен эхо-запрос? Ответ: Да***


### PC-A:

```
C:\>ping 192.168.1.33

Pinging 192.168.1.33 with 32 bytes of data:

Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.33:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```

### **Шаг 7. Настройте маршрутизацию.**

### a. Настройте RIP версии 2 на всех маршрутизаторах. Добавьте все сети, кроме 209.165.200.224/27.


### b. Настройте маршрут по умолчанию на R2, используя Lo1 в качестве интерфейса выхода в сеть 209.165.200.224/27.

### c. На R2 используйте следующие команды для перераспределения маршрута по умолчанию в процессе RIP.


### R1:

```
R1(config)#router rip 
R1(config-router)#network 10.1.1.0
R1(config-router)#network 192.168.1.0

```

### R2:

```
R2(config)#router rip
R2(config-router)#network 10.1.1.0
R2(config-router)#network 10.2.2.0
R2(config-router)#default-information originate
R2(config-router)#exit
R2(config)#ip route 0.0.0.0 0.0.0.0 Loopback1
%Default route without gateway, if not a point-to-point interface, may impact performance
R2(config)#
```

### R3:

```
R3(config)#router rip
R3(config-router)#network 10.2.2.0
R3(config-router)#network 192.168.1.0
```


### **Шаг 8. Проверить подключение**

### a. С PC-A вы должны иметь возможность проверять все интерфейсы на R1, R2, R3 и PC-C. Все ли пинги были успешными?

### ***Все ответы были получены***

### PC-A:

```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

Control-C
^C
C:\>ping 10.1.1.1

Pinging 10.1.1.1 with 32 bytes of data:

Reply from 10.1.1.1: bytes=32 time<1ms TTL=255
Reply from 10.1.1.1: bytes=32 time<1ms TTL=255
Reply from 10.1.1.1: bytes=32 time<1ms TTL=255
Reply from 10.1.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 10.1.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 10.2.2.2

Pinging 10.2.2.2 with 32 bytes of data:

Reply from 10.2.2.2: bytes=32 time=14ms TTL=254
Request timed out.
Reply from 10.2.2.2: bytes=32 time=13ms TTL=254
Reply from 10.2.2.2: bytes=32 time=10ms TTL=254

Ping statistics for 10.2.2.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 14ms, Average = 12ms

C:\>ping 192.168.1.3

Pinging 192.168.1.3 with 32 bytes of data:

Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 10.2.2.1

Pinging 10.2.2.1 with 32 bytes of data:

Reply from 10.2.2.1: bytes=32 time=15ms TTL=255
Reply from 10.2.2.1: bytes=32 time=10ms TTL=255
Reply from 10.2.2.1: bytes=32 time=12ms TTL=255
Reply from 10.2.2.1: bytes=32 time=10ms TTL=255

Ping statistics for 10.2.2.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 10ms, Maximum = 15ms, Average = 11ms

C:\>ping 192.168.1.33

Pinging 192.168.1.33 with 32 bytes of data:

Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128
Reply from 192.168.1.33: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.33:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>

```

### b. С PC-C вы должны иметь возможность проверять все интерфейсы на R1, R2, R3 и PC-A. Все ли пинги были успешными?

### ***Все ответы были получены***


### PC-C:


```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 10.1.1.1

Pinging 10.1.1.1 with 32 bytes of data:

Reply from 10.1.1.1: bytes=32 time=1ms TTL=255
Reply from 10.1.1.1: bytes=32 time=2ms TTL=255
Reply from 10.1.1.1: bytes=32 time=1ms TTL=255
Reply from 10.1.1.1: bytes=32 time=3ms TTL=255

Ping statistics for 10.1.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 3ms, Average = 1ms

C:\>ping 10.1.1.2

Pinging 10.1.1.2 with 32 bytes of data:

Reply from 10.1.1.2: bytes=32 time=5ms TTL=254
Reply from 10.1.1.2: bytes=32 time=1ms TTL=254
Reply from 10.1.1.2: bytes=32 time=2ms TTL=254
Reply from 10.1.1.2: bytes=32 time=4ms TTL=254

Ping statistics for 10.1.1.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 5ms, Average = 3ms

C:\>ping 10.2.2.2

Pinging 10.2.2.2 with 32 bytes of data:

Reply from 10.2.2.2: bytes=32 time=5ms TTL=254
Reply from 10.2.2.2: bytes=32 time=2ms TTL=254
Reply from 10.2.2.2: bytes=32 time=2ms TTL=254
Reply from 10.2.2.2: bytes=32 time=1ms TTL=254

Ping statistics for 10.2.2.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 5ms, Average = 2ms

C:\>ping 192.168.1.3

Pinging 192.168.1.3 with 32 bytes of data:

Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255
Reply from 192.168.1.3: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 10.2.2.1

Pinging 10.2.2.1 with 32 bytes of data:

Reply from 10.2.2.1: bytes=32 time<1ms TTL=255
Reply from 10.2.2.1: bytes=32 time<1ms TTL=255
Reply from 10.2.2.1: bytes=32 time<1ms TTL=255
Reply from 10.2.2.1: bytes=32 time<1ms TTL=255

Ping statistics for 10.2.2.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.1.31

Pinging 192.168.1.31 with 32 bytes of data:

Reply from 192.168.1.31: bytes=32 time<1ms TTL=128
Reply from 192.168.1.31: bytes=32 time<1ms TTL=128
Reply from 192.168.1.31: bytes=32 time<1ms TTL=128
Reply from 192.168.1.31: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.31:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>


```



## **Часть 2. Настройка обеспечения избыточности на первом хопе с помощью HSRP**

### **Шаг 1: Определите путь интернет-трафика для PC-A и PC-C.** 

### a.	В командной строке на PC-A введите команду tracert для loopback-адреса 209.165.200.225 на маршрутизаторе R2. 


### PC-A:

```
C:\>tracert 209.165.200.225

Tracing route to 209.165.200.225 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.1.1
  2   0 ms      1 ms      2 ms      209.165.200.225

Trace complete.
```
### ***Какой путь прошли пакеты от PC-A до 209.165.200.225? Ответ: R1, R2****



### b.	В командной строке на PC-С введите команду tracert для loopback-адреса 209.165.200.225 на маршрутизаторе R2. 

### PC-C:

```
C:\>tracert 209.165.200.225

Tracing route to 209.165.200.225 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.1.3
  2   0 ms      2 ms      0 ms      209.165.200.225

Trace complete.

```

### ***Какой путь прошли пакеты от PC-C до 209.165.200.225? Ответ: R3, R2***


### **Шаг 2. Запустите сеанс эхо-тестирования на PC-A и разорвите соединение между S1 и R1.**

### a. В командной строке на PC-A введите команду ping –t для адреса 209.165.200.225 на маршрутизаторе R2. Убедитесь, что окно командной строки открыто.  

### PC-A:

```
C:\>ping -t 209.165.200.225

Pinging 209.165.200.225 with 32 bytes of data:

Reply from 209.165.200.225: bytes=32 time=5ms TTL=254
Reply from 209.165.200.225: bytes=32 time=2ms TTL=254

Ping statistics for 209.165.200.225:
    Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 2ms, Maximum = 5ms, Average = 3ms

Control-C
^C
C:\>

```

### b. В процессе эхо-тестирования отсоедините кабель Ethernet от интерфейса F0/5 на S1. Отключение интерфейса F0/5 на S1 приведет к тому же результату. Что произошло с трафиком эхо-запросов? 

### ***После того, как кабель был отсоединен от F0/5 на S1, проверка связи завершилась неудачей.***


### PC-A:

```
C:\>ping -t 209.165.200.225

Pinging 209.165.200.225 with 32 bytes of data:

Reply from 209.165.200.225: bytes=32 time=2ms TTL=254
Reply from 209.165.200.225: bytes=32 time=2ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.
Request timed out.

Ping statistics for 209.165.200.225:
    Packets: Sent = 16, Received = 3, Lost = 13 (82% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 2ms, Average = 1ms

Control-C
^C
C:\>


```

### c. Повторите шаги 2a и 2b на PC-C и S3. Отсоедините кабель от интерфейса F0/5 на S3(получлилось всё то же самое.).

### ***Результаты были такими же, как и на PC-A. После того, как кабель Ethernet был отсоединен от F0/5 на S3, проверка связи завершилась неудачей.***


### d. Повторно подсоедините кабели Ethernet к интерфейсу F0/5 или включите интерфейс F0/5 на S1 и S3, соответственно. Повторно отправьте эхо-запросы на 209.165.200.225 с компьютеров PC-A и PC-C, чтобы убедиться в том, что подключение восстановлено.


### **Шаг 3: Настройте HSRP на R1 и R3.**

### a.	Настройте протокол HSRP на маршрутизаторе R1. 


### R1:

```
R1(config)#int g0/1
R1(config-if)#standby version 2
R1(config-if)#standby 1 ip 192.168.1.254
R1(config-if)#standby 1 pri 150
R1(config-if)#standby 1 pree
R1(config-if)#standby 1 preempt 
R1(config-if)#

```

### b.	Настройте протокол HSRP на маршрутизаторе R3.

```
R3(config)#int g0/1
R3(config-if)#standby version 2
R3(config-if)#standby 1 ip 192.168.1.254
```


### c.	Проверьте HSRP, выполнив команду show standby на R1 и R3. 

### R1:

```
R1#sh standby 
GigabitEthernet0/1 - Group 1 (version 2)
  State is Active
    7 state changes, last state change 01:03:48
  Virtual IP address is 192.168.1.254
  Active virtual MAC address is 0000.0C9F.F001
    Local virtual MAC address is 0000.0C9F.F001 (v2 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 0.6 secs
  Preemption enabled
  Active router is local
  Standby router is unknown, priority 100 (expires in 1 sec)
  Priority 150 (configured 150)
  Group name is hsrp-Gig0/1-1 (default)
```

### R3:

```
R3#sh standby 
GigabitEthernet0/1 - Group 1 (version 2)
  State is Standby
    11 state changes, last state change 01:21:25
  Virtual IP address is 192.168.1.254
  Active virtual MAC address is 0000.0C9F.F001
    Local virtual MAC address is 0000.0C9F.F001 (v2 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 2.823 secs
  Preemption disabled
  Active router is 192.168.1.1, priority 150 (expires in 7 sec)
    MAC address is 0000.0C9F.F001
  Standby router is local
  Priority 100 (default 100)
  Group name is hsrp-Gig0/1-1 (default)
```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB12/2.4.3.4%20Lab%20-%20Configuring%20HSRP%20and%20GLBP.pdf%20-%20Word%2031.10.2023%2017_13_20.png?raw=true)


### d.	Используйте команду show standby brief на R1 и R3, чтобы просмотреть сводку состояния HSRP. Пример выходных данных приведен ниже. 

### R1:

```
R1#sh standby brief 
                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    150 P Active   local           192.168.1.3     192.168.1.254
```

### R3:

```
R3#sh standby brief 
                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    100   Standby  192.168.1.1     local           192.168.1.254  
```

### e.	Измените адрес шлюза по умолчанию для PC-A, PC-C, S1 и S3. Какой адрес следует использовать? Ответ: 192.168.1.254(На пк такой же адрес шлюза сделать)




### S1:

```
S1(config)#ip default-gateway 192.168.1.254
```

### S3:

```
S3(config)#ip default-gateway 192.168.1.254
```


### f. Проверьте новые настройки. Отправьте запрос как от PCA, так и от PCC на обратный адрес R2. Успешны ли пинги?

### PC-A:

```
C:\>ping 209.165.200.225

Pinging 209.165.200.225 with 32 bytes of data:

Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=10ms TTL=254
Reply from 209.165.200.225: bytes=32 time=12ms TTL=254

Ping statistics for 209.165.200.225:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 12ms, Average = 6ms

C:\>

```
### PC-C:


```
C:\>ping 209.165.200.225

Pinging 209.165.200.225 with 32 bytes of data:

Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=10ms TTL=254
Reply from 209.165.200.225: bytes=32 time=12ms TTL=254

Ping statistics for 209.165.200.225:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 12ms, Average = 6ms

C:\>


```


### **Шаг 4: Запустите сеанс эхо-тестирования на PC-A и разорвите соединение с коммутатором, подключенным к активному маршрутизатору HSRP (R1).**


### a.	В командной строке на PC-A введите команду ping –t для адреса 209.165.200.225 на маршрутизаторе R2. Убедитесь, что окно командной строки открыто. 


### b.	Во время отправки эхо-запроса отсоедините кабель Ethernet от интерфейса F0/5 на коммутаторе S1 или выключите интерфейс F0/5. Что произошло с трафиком эхо-запросов? Ответ: несолько пакетов было потеряно, но связь восстановилась 


### PC-A:

```
C:\>ping -t 209.165.200.225

Pinging 209.165.200.225 with 32 bytes of data:

Reply from 209.165.200.225: bytes=32 time=16ms TTL=254
Reply from 209.165.200.225: bytes=32 time=16ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Request timed out.
Request timed out.
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254
Reply from 209.165.200.225: bytes=32 time=1ms TTL=254

Ping statistics for 209.165.200.225:
    Packets: Sent = 8, Received = 6, Lost = 2 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 16ms, Average = 6ms

Control-C
^C
C:\>
```


### **Шаг 5: Проверьте настройки HSRP на маршрутизаторах R1 и R3.** 

### a.	На коммутаторах R1 и R3 выполните команду show standby brief. Какой маршрутизатор является активным? Ответ: R3 - активный маршрутизатор

### R1:

```
R1#sh standby brief 
                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    150 P Init     unknown         unknown         192.168.1.254  

```
### R3:

```
R3#sh standby brief 
                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    100   Active   local           unknown         192.168.1.254  

```

### b.	Повторно подсоедините кабель, соединяющий коммутатор и маршрутизатор, или включите интерфейс F0/5. Повторно подсоедините кабель между коммутатором и маршрутизатором или включите интерфейс F0/5. Теперь, какой маршрутизатор является активным? Объяснять.

### ***R1 стал активным маршрутизатором, поскольку функция приоритетного вытеснения включена и у R1 более высокий приоритет***


### **Шаг 6: Измените приоритеты HSRP.**

### a. Измените приоритет HSRP на 200 на R3. Какой маршрутизатор является активным? Ответ: R1(команда sh standby brief на R1)

### R3:

```
R3(config)#int g0/1
R3(config-if)#st
R3(config-if)#standby 1 pri
R3(config-if)#standby 1 priority 200
```



### b. Выполните команду для изменения активного маршрутизатора на R3 без изменения приоритета. Какую команду вы использовали?


### R3:

```
R3(config)#int g0/1
R3(config-if)#sta
R3(config-if)#standby 1 pre
R3(config-if)#standby 1 preempt 
R3(config-if)#
 %HSRP-6-STATECHANGE: GigabitEthernet0/1 Grp 1 state Standby -> Active

R3(config-if)#

```


### c. Используйте команду show, чтобы убедиться, что R3 является активным маршрутизатором.


### R3:

```
R3#sh standby brief 
                     P indicates configured to preempt.
                     |
Interface   Grp  Pri P State    Active          Standby         Virtual IP
Gig0/1      1    200 P Active   local           192.168.1.1     192.168.1.254  

```



![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB12/2.4.3.4%20Lab%20-%20Configuring%20HSRP%20and%20GLBP.pdf%20-%20Word%2031.10.2023%2017_44_43.png?raw=true)

