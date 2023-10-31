# **Лабораторная работа. "Настройка Rapid PVST+, PortFast и BPDU Guard"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/Cisco%20Packet%20Tracer%2030.10.2023%2015_17_13.png?raw=true)

## **Цели:**
+ ### Часть 1. Создание сети и настройка базовых параметров устройств 
+ ### Часть 2. Настройка сетей VLAN, native VLAN и транковых каналов 
+ ### Часть 3. Настройка корневого моста и проверка сходимости PVST+ 



## **Решение**
## **Часть 1. Создание сети и настройка основных параметров устройства**

### **Шаг 1. Создайте сеть согласно топологии.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.**
### **Шаг 4. Настройте базовые параметры каждого коммутатора.**

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
S1(config)#int ra fa0/1, fa0/3, fa0/6
S1(config-if-range)#sh

S1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to down

S1(config-if-range)#
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
S2(config)#host
S2(config)#hostname Switch
Switch(config)#host S2
S2(config)#no ip domain-lookup
S2(config)#enable sec class
S2(config)#line con 0
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#logging synchronous
S2(config-line)#exit
S2(config)#line vty 0 4
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#int ra fa0/1,fa0/3
S2(config-if-range)#sh
S2(config-if-range)#
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
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
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
S3(config)#int ra fa0/1, fa0/3 , fa0/18
S3(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/18, changed state to down

S3(config-if-range)#
S3#
%SYS-5-CONFIG_I: Configured from console by console

S3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S3#

```


## **Часть 2. Определение корневого моста**
### **Шаг 1. Создайте сети VLAN.**

### S1:

```
S1(config)#vlan 10
S1(config-vlan)#name User
S1(config-vlan)#vlan 99
S1(config-vlan)#name Management
S1(config-vlan)#
```

### S2:

```
S2(config)#vlan 10
S2(config-vlan)#name User
S2(config-vlan)#vlan 99
S2(config-vlan)#name Management
S2(config-vlan)#

```


### S3:

```
S3(config)#vlan 10
S3(config-vlan)#name User
S3(config-vlan)#vlan 99
S3(config-vlan)#name Management

```






### **Шаг 2. Переведите пользовательские порты в режим доступа и назначьте сети VLAN.**

### S1:

```
S1(config)#int fa0/6
S1(config-if)#sw m ac
S1(config-if)#sw ac vlan 10

```


### S3:

```
S3(config)#int fa0/18
S3(config-if)#sw m ac
S3(config-if)#sw ac vlan 10

```


### **Шаг 3. Настройте транковые порты и назначьте их сети native VLAN 99.**


### S1:

```
S1(config)#int ra fa0/1, fa0/3
S1(config-if-range)#sw m tr
S1(config-if-range)#sw tr nat
S1(config-if-range)#sw tr native vlan 99

```



### S2:

```
S2(config)#int ra fa0/1, fa0/3
S2(config-if-range)#sw m tr
S2(config-if-range)#sw tr native vlan 99

```

### S3:

```
S3(config)#int ra fa0/1, fa0/3
S3(config-if-range)#sw m tr
S3(config-if-range)#sw tr native vlan 99

```





### **Шаг 4. Настройте административный интерфейс на всех коммутаторах.**

### S1:

```
S1(config)#int vlan 99
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan99, changed state to up

S1(config-if)#ip addr 192.168.1.11 255.255.255.0
S1(config-if)#int ra fa0/1, fa0/3, fa0/6
S1(config-if-range)#no sh



S1(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up

%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/6, changed state to up

S1(config-if-range)#
```


### S2:

```
S2(config)#int vlan 99
S2(config-if)#
%LINK-5-CHANGED: Interface Vlan99, changed state to up

S2(config-if)#ip addr 192.168.1.12 255.255.255.0
S2(config-if)#int ra fa0/1,fa0/3
S2(config-if-range)#no sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to down

S2(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan99, changed state to up

S2(config-if-range)#
S2(config-if-range)#


```

### S3:


```
S3(config)#int vlan 99
S3(config-if)#
%LINK-5-CHANGED: Interface Vlan99, changed state to up

S3(config-if)#ip addr 192.168.1.13 255.255.255.0
S3(config-if)#
S3(config-if)#int ra fa0/1, fa0/3 , fa0/18
S3(config-if-range)#no sh

%LINK-5-CHANGED: Interface FastEthernet0/1, changed state to down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to down

S3(config-if-range)#
%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/18, changed state to up
```


### **Шаг 5. Проверка конфигураций и возможности подключения.** 

### S1:

```
S1#sh vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/4, Fa0/5, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
10   User                             active    Fa0/6
99   Management                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    


```

```
S1#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      99
Fa0/3       on           802.1q         trunking      99

Port        Vlans allowed on trunk
Fa0/1       1-1005
Fa0/3       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,99
Fa0/3       1,10,99

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,99
Fa0/3       1,10,99

```

```
S1#sh run
Building configuration...

Current configuration : 1426 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/2
!
interface FastEthernet0/3
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.1.11 255.255.255.0
!
!
!
!
line con 0
 password cisco
 logging synchronous
 login
!
line vty 0 4
 password cisco
 login
line vty 5 15
 login
!
!
!
!
end

```



### S2:

```
S2#sh vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
10   User                             active    
99   Management                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

```

```
S2#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      99
Fa0/3       on           802.1q         trunking      99

Port        Vlans allowed on trunk
Fa0/1       1-1005
Fa0/3       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,99
Fa0/3       1,10,99

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,99
Fa0/3       1,10,99

```

```
S2#sh run
Building configuration...

Current configuration : 1375 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S2
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/2
!
interface FastEthernet0/3
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.1.12 255.255.255.0
!
!
!
!
line con 0
 password cisco
 logging synchronous
 login
!
line vty 0 4
 password cisco
 login
line vty 5 15
 login
!
!
!
!
end

```


### R3:

```
S3#sh vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
10   User                             active    Fa0/18
99   Management                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

```


```
S3#sh int trunk 
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      99
Fa0/3       on           802.1q         trunking      99

Port        Vlans allowed on trunk
Fa0/1       1-1005
Fa0/3       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,99
Fa0/3       1,10,99

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       none
Fa0/3       1,10,99

```

```
S3#sh run
Building configuration...

Current configuration : 1426 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname S3
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
!
!
no ip domain-lookup
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/2
!
interface FastEthernet0/3
 switchport trunk native vlan 99
 switchport mode trunk
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
 switchport access vlan 10
 switchport mode access
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.1.13 255.255.255.0
!
!
!
!
line con 0
 password cisco
 logging synchronous
 login
!
line vty 0 4
 password cisco
 login
line vty 5 15
 login
!
!
!
!
end

```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/10.docx%20-%20Word%2030.10.2023%2016_05_36.png?raw=true)


### PC-A(ping PC-C):

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/PC-A%2030.10.2023%2016_05_30.png?raw=true)



## **Часть 3. Настройка корневого моста и проверка сходимости PVST+**

### **Шаг 1. Определите текущий корневой мост.**

### С помощью какой команды пользователи определяют состояние протокола spanning-tree коммутатора Cisco Catalyst для всех сетей VLAN? Запишите команду в строке ниже. 

### ***show spanning-tree***


### S1:

```
S1#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.972E.E32C
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0001.972E.E32C
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    32778
             Address     0001.972E.E32C
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32778  (priority 32768 sys-id-ext 10)
             Address     0001.972E.E32C
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p
Fa0/6            Desg FWD 19        128.6    P2p

VLAN0099
  Spanning tree enabled protocol ieee
  Root ID    Priority    32867
             Address     0001.972E.E32C
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32867  (priority 32768 sys-id-ext 99)
             Address     0001.972E.E32C
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

```

### S2:

```
S2#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.972E.E32C
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0040.0BB6.E993
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    32778
             Address     0001.972E.E32C
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32778  (priority 32768 sys-id-ext 10)
             Address     0040.0BB6.E993
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

VLAN0099
  Spanning tree enabled protocol ieee
  Root ID    Priority    32867
             Address     0001.972E.E32C
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32867  (priority 32768 sys-id-ext 99)
             Address     0040.0BB6.E993
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p


```


### S3:


```
S3#sh spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0001.972E.E32C
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0060.47D6.8A5A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/3            Root FWD 19        128.3    P2p

VLAN0010
  Spanning tree enabled protocol ieee
  Root ID    Priority    32778
             Address     0001.972E.E32C
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32778  (priority 32768 sys-id-ext 10)
             Address     0060.47D6.8A5A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/3            Root FWD 19        128.3    P2p
Fa0/18           Desg FWD 19        128.18   P2p

VLAN0099
  Spanning tree enabled protocol ieee
  Root ID    Priority    32867
             Address     0001.972E.E32C
             Cost        19
             Port        3(FastEthernet0/3)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32867  (priority 32768 sys-id-ext 99)
             Address     0060.47D6.8A5A
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Altn BLK 19        128.1    P2p
Fa0/3            Root FWD 19        128.3    P2p


```

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/10.docx%20-%20Word%2030.10.2023%2016_31_20.png?raw=true)



### **Шаг 2. Настройте основной и вспомогательный корневые мосты для всех существующих сетей VLAN.**

### a.	Настройте коммутатор S2 в качестве основного корневого моста для всех существующих сетей VLAN. Запишите команду в строке ниже. 

### ***S2(config)#spanning-tree vlan 1,10,99 root primary***


### b.	Настройте коммутатор S1 в качестве вспомогательного корневого моста для всех существующих сетей VLAN. Запишите команду в строке ниже. 

### ***S1(config)#spanning-tree vlan 1,10,99 root secondary***

![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/1.png?raw=true)

### S1:

```
S1#sh spanning-tree vlan 1
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    24577
             Address     0040.0BB6.E993
             Cost        19
             Port        1(FastEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    28673  (priority 28672 sys-id-ext 1)
             Address     0001.972E.E32C
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Root FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

```

### S2:

```
S2#sh spanning-tree vlan 1
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    24577
             Address     0040.0BB6.E993
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    24577  (priority 24576 sys-id-ext 1)
             Address     0040.0BB6.E993
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/1            Desg FWD 19        128.1    P2p
Fa0/3            Desg FWD 19        128.3    P2p

```



### **Измените топологию 2 уровня и проверьте сходимость.**

### a.	Выполните команду debug spanning-tree events в привилегированном режиме на коммутаторе S3(такой комнды нет в pkt). 


### b.	Измените топологию, отключив интерфейс F0/1 на коммутаторе S3. 

## **Часть 4. Настройка Rapid PVST+, PortFast, BPDU Guard и проверка сходимости**

### **Настройте Rapid PVST+.** 

### a. Настройте S1 для использования Rapid PVST+. Запишите команду в строке ниже.

### ***spanning-tree mode rapid-pvst***

### S1:

```
S1(config)#spanning-tree mode rapid-pvst
```


### b. Настройте S2 и S3 для Rapid PVST+. 

### S2:

```
S2(config)#spanning-tree mode rapid-pvst

```

### S3:

```
S3(config)#spanning-tree mode rapid-pvst

```

### c. Проверьте конфигурации с помощью команды show running-config | include spanning-tree mode.


### S1:

```
S1#show running-config | include spanning-tree mode 
spanning-tree mode rapid-pvst

```



### S2:

```
S2#show running-config | include spanning-tree mode 
spanning-tree mode rapid-pvst
```


### S3:


```
S3#show running-config | include spanning-tree mode 
spanning-tree mode rapid-pvst

```


### **Шаг 2. Настройте PortFast и BPDU Guard на портах доступа.**


### a.	Настройте F0/6 на S1 с помощью функции PortFast. Запишите команду в строке ниже. 

### S1:


```
S1(config)#int fa0/6
S1(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc... to this
interface  when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION

%Portfast has been configured on FastEthernet0/6 but will only
have effect when the interface is in a non-trunking mode.

```

### b.	Настройте F0/6 на S1 с помощью функции BPDU Guard. Запишите команду в строке ниже. 

### S1:

```
S1(config)#int fa0/6
S1(config-if)#spanning-tree bpduguard enable

```

## ***Pkt очень плохо реагирует на глобальное включение bpduguard***

### c.	Глобально настройте все нетранковые порты на коммутаторе S3 с помощью функции PortFast. Запишите команду в строке ниже. 

### S3:

```
S3(config)#spanning-tree portfast default
```


### d.	Глобально настройте все нетранковые порты на коммутаторе S3 с помощью функции BPDU. Запишите команду в строке ниже. 

### S3:

```
S3(config)#spanning-tree portfast bpduguard default
```


### **Шаг 3. Проверьте сходимость Rapid PVST+.** 


### a.	Выполните команду debug spanning-tree events (такой команды нет в pkt) в привилегированном режиме на коммутаторе S3. 

### b.	Измените топологию, отключив интерфейс F0/1 на коммутаторе S3. 


### **Шаг 3. Проверьте сходимость Rapid PVST+.**


### a.	Выполните команду debug spanning-tree events в привилегированном режиме на коммутаторе S3. 

### b.	Измените топологию, отключив интерфейс F0/1 на коммутаторе S3. 


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB10/10.docx%20-%20Word%2030.10.2023%2017_19_48.png?raw=true)



























