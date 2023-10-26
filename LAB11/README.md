# **Лабораторная работа. "Настройка EtherChannel"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB11/Cisco%20Packet%20Tracer%2026.10.2023%2020_01_47.png?raw=true)

## **Задачи**
+ ### Часть 1. Настройка базовых параметров коммутатора 
+ ### Часть 2. Настройка PAgP 
+ ### Часть 3. Настройка LACP 



## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Выполните инициализацию и перезагрузку коммутаторов.**

### **Шаг 3. Настройте базовые параметры каждого коммутатора.**

### S1:
```
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#h S1
S1(config)#no ip domain-loo
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#logging syn
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#line vty 0 15
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#banner motd *STAY_OUT*
S1(config)#vlan 99 
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 10
S1(config-vlan)#name Staff
S1(config-vlan)#int vlan 99
S1(config-if)#ip addr 192.168.99.11 255.255.255.0

%LINK-5-CHANGED: Interface Vlan99, changed state to up

S1(config-if)#no sh
S1(config-if)#
S1#
%SYS-5-CONFIG_I: Configured from console by console

S1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S1#

```


### S2:

```
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#h S2
S2(config)#no ip domain-loo
S2(config)#enable sec class
S2(config)#line con 0
S2(config-line)#logging syn
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#line vty 0 15
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#banner motd *STAY_OUT*
S2(config)#vlan 99 
S2(config-vlan)#name Management
S2(config-vlan)#exit
S2(config)#vlan 10
S2(config-vlan)#name Staff
S2(config-vlan)#int vlan 99
S2(config-if)#ip addr 192.168.99.12 255.255.255.0
S2(config-if)#no sh
%LINK-5-CHANGED: Interface Vlan99, changed state to up

S2(config-if)#no sh
S2(config-if)#
S2#
%SYS-5-CONFIG_I: Configured from console by console

S2#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S2#

```

### S3:

```
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#h S3
S3(config)#no ip domain-loo
S3(config)#enable sec class
S3(config)#line con 0
S3(config-line)#logging syn
S3(config-line)#pass cisco
S3(config-line)#login
S3(config-line)#exit
S3(config)#line vty 0 15
S3(config-line)#pass cisco
S3(config-line)#login
S3(config-line)#exit
S3(config)#banner motd *STAY_OUT*
S3(config)#vlan 99 
S3(config-vlan)#name Management
S3(config-vlan)#exit
S3(config)#vlan 10
S3(config-vlan)#name Staff
S3(config-vlan)#int vlan 99
S3(config-if)#ip addr 192.168.99.13 255.255.255.0
S3(config-if)#no sh
%LINK-5-CHANGED: Interface Vlan99, changed state to up

S3(config-if)#no sh
S3(config-if)#
S3#
%SYS-5-CONFIG_I: Configured from console by console

S3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S3#


```

### **Шаг 4: Настройте компьютеры.**


## **Часть 2. Настройка протокола PAgP**

### **Шаг 1. Настройте PAgP на S1 и S3.**

### S1:

```
S1(config)#int ra fa0/3-4
S1(config-if-range)#sh


S1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down

S1(config-if-range)#channel-group 1 mode desirable 
S1(config-if-range)#
Creating a port-channel interface Port-channel 1

S1(config-if-range)#no sh


S1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up

S1(config-if-range)#
```


### S3:

```
S3(config)#int ra fa0/3-4
S3(config-if-range)#sh


S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down

S3(config-if-range)#channel-gr
S3(config-if-range)#channel-group 1 mode de
S3(config-if-range)#channel-group 1 mode auto
S3(config-if-range)#
Creating a port-channel interface Port-channel 1

S3(config-if-range)#no sh


S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up

%LINK-5-CHANGED: Interface Port-channel1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel1, changed state to up

S3(config-if-range)#

```

### **Шаг 2. Проверьте конфигурации на портах**

### S1:

```
S1#sh run | section interface FastEthernet0/3  
interface FastEthernet0/3
 channel-group 1 mode desirable
S1#
```



```
S1#show interfaces f0/3 switchport 
Name: Fa0/3
Switchport: Enabled
Administrative Mode: dynamic auto
Operational Mode: static access
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: native
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: All
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
 
```



### **Шаг 3. Убедитесь, что порты объединены**

### S1:

```
S1#sh etherchannel summary 
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/3(P) Fa0/4(P) 
S1#

```


### S3:

```
S3#sh etherchannel summary 
Flags:  D - down        P - in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------------------------------

1      Po1(SU)           PAgP   Fa0/3(P) Fa0/4(P) 

```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB11/11.docx%20-%20Word%2026.10.2023%2020_33_57.png?raw=true)


### **Шаг 4. Настройте транковые порты. **

### S1:

```
S1(config)#int po 1
S1(config-if)#sw m tr

S1(config-if)#sw tr native vlan 99
S1(config-if)#
```

### S3:

```
S3(config)#int po 1
S3(config-if)#sw m tr

S3(config-if)#sw tr native vlan 99
S3(config-if)#

```


### **Шаг 5. Убедитесь в том, что порты настроены в качестве транковых **

### a. Выполните команды show run interface идентификатор-интерфейса на S1 и S3. Какие команды включены в список для интерфейсов F0/3 и F0/4 на обоих коммутаторах? Сравните результаты с текущей конфигурацией для интерфейса Po1. Запишите наблюдения. 

```
interface Port-channel1
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
 switchport trunk native vlan 99
 switchport mode trunk
 channel-group 1 mode auto
!
interface FastEthernet0/4
 switchport trunk native vlan 99
 switchport mode trunk
 channel-group 1 mode auto
```

**Настройки на Port-channel дублируются в настройки интерфейсов fa0/1 и fa0/4**

### Выполните команды show interfaces trunk и show spanning-tree на S1 и S3. Какой транковый порт включен в список? Какая используется сеть native VLAN? Какой вывод можно сделать на основе выходных данных? 

**Port-channel1. Native vlan 99. Мы объединили несколько физических интерфейсов в один вирутальный для балансировки трафика и поместили его в нативный vlan 99**


### Какие значения стоимости и приоритета порта для агрегированного канала отображены в выходных данных команды show spanning-tree? 

**cost 12, pri 128.28**

## **Часть 3. Настройка протокола LACP**

### **Шаг 1. Настройте LACP между S1 и S2.**


### S1:

```
S1(config)#int ra fa0/1-2
S1(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down
S1(config-if-range)#sw m tr
S1(config-if-range)#sw tr native vlan 99
S1(config-if-range)#channel-group 2 mode active 
S1(config-if-range)#
Creating a port-channel interface Port-channel 2

S1(config-if-range)#no sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to down
S1(config-if-range)#
```

### S2:

```
S2(config)#int ra fa0/1-2
S2(config-if-range)#sh


S2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to down

S2(config-if-range)#sw m tr
	
S2(config-if-range)#sw tr native vlan 99
S2(config-if-range)#channel-group 2 mode passive 
S2(config-if-range)#
Creating a port-channel interface Port-channel 2

S2(config-if-range)#no sh


S2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up

%LINK-5-CHANGED: Interface Port-channel2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel2, changed state to up

S2(config-if-range)#
```


### Шаг 2. Убедитесь, что порты объединены. 

### Какой протокол использует Po2 для агрегирования каналов? Какие порты агрегируются для образования Po2? Запишите команду, используемую для проверки. 

**LACP. Fa0/1-2**



### Шаг 3. Настройте LACP между S2 и S3. 

### S2:

```
S2(config)#int ra fa0/3-4
S2(config-if-range)#sh


S2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down

S2(config-if-range)#sw m tr
S2(config-if-range)#sw tr native vlan 99
S2(config-if-range)#channel-gro
S2(config-if-range)#channel-group 3 mode ac
S2(config-if-range)#channel-group 3 mode active 
S2(config-if-range)#
Creating a port-channel interface Port-channel 3

S2(config-if-range)#no sh


S2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up



S2(config-if-range)#

```

### S3:

```
S3(config)#int ra fa0/1-2
S3(config-if-range)#sh


S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to down

S3(config-if-range)#sw m tr
S3(config-if-range)#sw tr native vlan 99
S3(config-if-range)#channel-gro
S3(config-if-range)#channel-group 3 mode pass
S3(config-if-range)#
Creating a port-channel interface Port-channel 3

S3(config-if-range)#no sh


S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/2, changed state to up

%LINK-5-CHANGED: Interface Port-channel3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel3, changed state to up

S3(config-if-range)#

```




![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB11/11.docx%20-%20Word%2026.10.2023%2021_42_41.png?raw=true)












