# **Лабораторная работа. "Разработка и внедрение схемы адресации VLSM"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB18/Cisco%20Packet%20Tracer%20-%20D__Учеба_Сетевой%20инженер_Youdo_2022_18.pkt%2016.11.2023%2017_04_50.png?raw=true)

## **Цели:**
+ ### Часть 1. Изучение требований к сети  
+ ### Часть 2. Разработка схемы адресации VLSM 
+ ### Часть 3. Подключение и настройка IPv4-сети


## **Решение**

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB18/18.docx%20-%20Word%2016.11.2023%2016_58_08.png?raw=true)

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB18/18.docx%20-%20Word%2016.11.2023%2016_58_52.png?raw=true)

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB18/18.docx%20-%20Word%2016.11.2023%2016_59_08.png?raw=true)

## **Часть 3: Подключение и настройка сети IPv4**


### **Шаг 1: Создайте сеть в соответствии с изображенной на схеме топологией.**

### **Шаг 2: Настройте основные параметры для каждого коммутатора.**

### BR1:

```
Router(config)#host BR1
BR1(config)#no ip domain-lookup
BR1(config)#enable secret class
BR1(config)#line con 0
BR1(config-line)#password cisco
BR1(config-line)#login
BR1(config-line)#logging synchronous
BR1(config-line)#exit
BR1(config)#line vty 0 4
BR1(config-line)#password cisco
BR1(config-line)#login
BR1(config-line)#exit
BR1(config)#service password-encryption
BR1(config)#banner motd @Unauthorized Access is Prohibited!@
BR1(config)#end
BR1#copy run start
%SYS-5-CONFIG_I: Configured from console by console

BR1#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
BR1#

```
### BR2:

```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#host BR2
BR2(config)#no ip domain-lookup
BR2(config)#enable secret class
BR2(config)#line con 0
BR2(config-line)#password cisco
BR2(config-line)#login
BR2(config-line)#logging synchronous
BR2(config-line)#exit
BR2(config)#line vty 0 4
BR2(config-line)#password cisco
BR2(config-line)#login
BR2(config-line)#exit
BR2(config)#service password-encryption
BR2(config)#banner motd @Unauthorized Access is Prohibited!@
BR2(config)#end
BR2#copy run start
%SYS-5-CONFIG_I: Configured from console by console

BR2#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
BR2#

```

### HQ:

```
Router(config)#host HQ
HQ(config)#no ip domain-lookup
HQ(config)#enable secret class
HQ(config)#line con 0
HQ(config-line)#password cisco
HQ(config-line)#login
HQ(config-line)#logging synchronous
HQ(config-line)#exit
HQ(config)#line vty 0 4
HQ(config-line)#password cisco
HQ(config-line)#login
HQ(config-line)#exit
HQ(config)#service password-encryption
HQ(config)#banner motd @Unauthorized Access is Prohibited!@
HQ(config)#end
HQ#copy run start
%SYS-5-CONFIG_I: Configured from console by console

HQ#copy run start
Destination filename [startup-config]? 
Building configuration...
[OK]
HQ#


```

### **Шаг 3: 	Настройте интерфейсы на каждом маршрутизаторе.**

### **Шаг 4: 	Сохраните конфигурацию на всех устройствах.**

### HQ:


```
HQ(config)#int g0/0
HQ(config-if)#ip addr 172.16.128.1 255.255.192.0
HQ(config-if)#no sh

HQ(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

HQ(config-if)#int g0/1
HQ(config-if)#ip addr 172.16.192.1 255.255.224.0
HQ(config-if)#no sh

HQ(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

HQ(config-if)#int s0/0/0
HQ(config-if)#ip addr 172.16.254.1 255.255.255.252
HQ(config-if)#cl
HQ(config-if)#clock r
HQ(config-if)#clock rate 128000
HQ(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
HQ(config-if)#int s0/0/1
HQ(config-if)#ip addr 172.16.254.5 255.255.255.252
HQ(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
HQ(config-if)#
HQ#
%SYS-5-CONFIG_I: Configured from console by console

HQ#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]

```


### BR1:

```
BR1(config)#int g0/0
BR1(config-if)#ip addr 172.16.240.1 255.255.248.0
BR1(config-if)#no sh

BR1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

BR1(config-if)#int g0/1
BR1(config-if)#ip addr 172.16.224.1 255.255.240.0
BR1(config-if)#no sh

BR1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

BR1(config-if)#int s0/0/0
BR1(config-if)#ip addr 172.16.254.2 255.255.255.252
BR1(config-if)#no sh

BR1(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

BR1(config-if)#int s0/0/1
BR1(config-if)#ip addr 172.16.254.9 255.255.255.252
BR1(config-if)#no sh

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
BR1(config-if)#
BR1#
%SYS-5-CONFIG_I: Configured from console by console

BR1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]

```


### BR2:

```
BR2(config)#int g0/0
BR2(config-if)#ip addr 172.16.252.1 255.255.254.0
BR2(config-if)#no sh

BR2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

BR2(config-if)#int g0/1
BR2(config-if)#ip addr 172.16.248.1 255.255.252.0
BR2(config-if)#no sh

BR2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to up

BR2(config-if)#int s0/0/0
BR2(config-if)#ip addr 172.16.254.10 255.255.255.252
BR2(config-if)#cl
BR2(config-if)#clock r
BR2(config-if)#clock rate 128000
BR2(config-if)#no sh

BR2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

BR2(config-if)#int s0/0/1
BR2(config-if)#ip addr
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

BR2(config-if)#ip addr 172.16.254.6 255.255.255.252
BR2(config-if)#cl
BR2(config-if)#clock r
BR2(config-if)#clock rate 128000
BR2(config-if)#no sh

BR2(config-if)#
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up
BR2#
%SYS-5-CONFIG_I: Configured from console by console

BR2#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
```

### **Шаг 5: 	Проверьте подключения.**


### a.	С маршрутизатора HQ отправьте эхо-запрос с помощью команды ping на адрес интерфейса S0/0/0 маршрутизатора BR1. 

### HQ:

```
HQ#ping 172.16.254.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.254.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 7/9/10 ms
```

### b.	С маршрутизатора HQ отправьте эхо-запрос с помощью команды ping на адрес интерфейса S0/0/1 маршрутизатора BR2. 


### HQ:

```
HQ#ping 172.16.254.6

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.254.6, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/16/38 ms

```

### c.	С маршрутизатора BR1 отправьте эхо-запрос с помощью команды ping на адрес интерфейса S0/0/0 маршрутизатора BR2. 


### BR1:

```
BR1#ping  172.16.254.10

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.254.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/8/11 ms

```

### d.	Если эхо-запросы с помощью команды ping не прошли, найдите и устраните неполадки подключений. 


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB18/18.docx%20-%20Word%2016.11.2023%2016_59_20.png?raw=true)














