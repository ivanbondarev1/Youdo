# **Лабораторная работа. "Поиск и устранение неполадок в работе расширенной версии EIGRP"**
## **Топология** 
![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/Cisco%20Packet%20Tracer%2028.10.2023%2011_22_42.png?raw=true)

## **Задачи**
+ ### Часть 1. Построение сети и загрузка конфигураций устройств   
+ ### Часть 2. Поиск и устранение неполадок в работе EIGRP 
 


## **Решение**
## **Часть 1. Построение сети и загрузка конфигураций устройств**

### **Шаг 1. Подключите кабели в сети в соответствии с топологией.**
### **Шаг 2. Настройте узлы ПК.**
### **Шаг 3. Загрузите конфигурации маршрутизаторов**

### R1:
```
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#enable secret class
R1(config)#no ip domain lookup
R1(config)#key chain EIGRP-KEYS
R1(config-keychain)#key 1
R1(config-keychain-key)# key-string cisco123
R1(config-keychain-key)#line con 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#logging synchronous
R1(config-line)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#banner motd @Unauthorized Access is Prohibited!@


R1(config)#interface lo1

R1(config-if)#description Connection to Branch 11
R1(config-if)#ip add 172.16.11.1 255.255.255.0
R1(config-if)#interface lo2

R1(config-if)#description Connection to Branch 12
R1(config-if)#ip add 172.16.12.1 255.255.255.0
R1(config-if)#interface lo3

R1(config-if)#description Connection to Branch 13
R1(config-if)#ip add 172.16.13.1 255.255.255.0
R1(config-if)#interface lo4

R1(config-if)#description Connection to Branch 14
R1(config-if)#ip add 172.16.14.1 255.255.255.0
R1(config-if)#interface g0/0
R1(config-if)#description R1 LAN Connection
R1(config-if)#ip add 192.168.1.1 255.255.255.0
R1(config-if)#no shutdown

R1(config-if)#interface s0/0/0
R1(config-if)#description Serial Link to R2
R1(config-if)#clock rate 128000
This command applies only to DCE interfaces
R1(config-if)#ip add 192.168.12.1 255.255.255.252
R1(config-if)#ip authentication mode eigrp 1 md5
R1(config-if)#ip authentication key-chain eigrp 1 EIGRP-KEYS
R1(config-if)#ip hello-interval eigrp 1 30
R1(config-if)#ip hold-time eigrp 1 90
                  ^
% Invalid input detected at '^' marker.
	
R1(config-if)#ip bandwidth-percent eigrp 1 40
                 ^
% Invalid input detected at '^' marker.
	
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to down
R1(config-if)#interface s0/0/1
R1(config-if)#description Serial Link to R3
R1(config-if)#bandwidth 128
R1(config-if)#ip add 192.168.13.1 255.255.255.252
R1(config-if)#ip authentication mode eigrp 1 md5
R1(config-if)#ip authentication key-chain eigrp 1 EIGRP-KEYS
R1(config-if)#ip bandwidth-percent eigrp 1 40
                 ^
% Invalid input detected at '^' marker.
	
R1(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R1(config-if)#router eigrp 1
R1(config-router)#eigrp router-id 1.1.1.1
R1(config-router)#network 192.168.1.0 0.0.0.255
R1(config-router)#network 192.168.12.0 0.0.0.3
R1(config-router)#network 192.168.13.0 0.0.0.3
R1(config-router)#network 172.16.0.0 0.0.255.255
R1(config-router)#passive-interface g0/0
R1(config-router)#auto-summary
R1(config-router)#end
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

%LINK-5-CHANGED: Interface Loopback2, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback2, changed state to up

%LINK-5-CHANGED: Interface Loopback3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback3, changed state to up

%LINK-5-CHANGED: Interface Loopback4, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback4, changed state to up

%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

R1(config-router)#end
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
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R2
R2(config)#enable secret class
R2(config)#no ip domain lookup
R2(config)#key chain EIGRP-KEYS
R2(config-keychain)#key 1
R2(config-keychain-key)# key-string Cisco123
R2(config-keychain-key)#line con 0
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#logging synchronous
R2(config-line)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#banner motd @Unauthorized Access is Prohibited!@
R2(config)#interface g0/0
R2(config-if)#description R2 LAN Connection
R2(config-if)#ip add 192.168.2.1 255.255.255.0
R2(config-if)#no shutdown

R2(config-if)#interface s0/0/0
R2(config-if)#description Serial Link to R1
R2(config-if)#bandwidth 128
R2(config-if)#ip add 192.168.12.2 255.255.255.252
R2(config-if)#ip authentication mode eigrp 1 md5
R2(config-if)#ip authentication key-chain eigrp 1 EIGRP-KEYS
R2(config-if)#ip bandwidth-percent eigrp 1 40
                 ^
% Invalid input detected at '^' marker.
	
R2(config-if)#ip hello-interval eigrp 1 30
R2(config-if)#ip hold-time eigrp 1 90
                  ^
% Invalid input detected at '^' marker.
	
R2(config-if)#no shutdown

R2(config-if)#interface s0/0/1
R2(config-if)#description Serial Link to R3
R2(config-if)#bandwidth 128
R2(config-if)#ip add 192.168.23.1 255.255.255.252
R2(config-if)#ip authentication mode eigrp 1 md5
R2(config-if)#ip bandwidth-percent eigrp 1 40
                 ^
% Invalid input detected at '^' marker.
	
R2(config-if)#ip hello-interval eigrp 1 30
R2(config-if)#ip hold-time eigrp 1 90
                  ^
% Invalid input detected at '^' marker.
	
R2(config-if)#no shutdown

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to down
R2(config-if)#interface lo0

R2(config-if)#ip add 209.165.200.225 255.255.255.252
R2(config-if)#description Connection to ISP
R2(config-if)#router eigrp 1
R2(config-router)#eigrp router-id 2.2.2.2
R2(config-router)#network 192.168.2.0 0.0.0.255
R2(config-router)#network 192.168.12.0 0.0.0.3
R2(config-router)#network 192.168.23.0 0.0.0.3 
R2(config-router)#passive-interface g0/0
R2(config-router)#ip route 0.0.0.0 0.0.0.0 lo0
%Default route without gateway, if not a point-to-point interface, may impact performance
R2(config)#end
%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

R2(config)#end
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R2#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R2#
R2#
%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R2#


```

### R3:

```

Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R3
R3(config)#enable secret class
R3(config)#no ip domain lookup
R3(config)#key chain EIGRP-KEYS
R3(config-keychain)#key 1
R3(config-keychain-key)# key-string Cisco123
R3(config-keychain-key)#line con 0
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#logging synchronous
R3(config-line)#line vty 0 4
R3(config-line)#password cisco
R3(config-line)#login
R3(config-line)#banner motd @Unauthorized Access is Prohibited!@

R3(config)#interface lo3

R3(config-if)#description Connection to Branch 33
R3(config-if)#ip add 172.16.33.1 255.255.255.0
R3(config-if)#interface lo4

R3(config-if)#description Connection to Branch 34
R3(config-if)#ip add 172.16.34.1 255.255.255.0
R3(config-if)#interface lo5

R3(config-if)#description Connection to Branch 35
R3(config-if)#ip add 172.16.35.1 255.255.255.0
R3(config-if)#interface lo6

R3(config-if)#description Connection to Branch 36
R3(config-if)#ip add 172.16.36.1 255.255.255.0
R3(config-if)#interface g0/0
R3(config-if)#description R3 LAN Connection
R3(config-if)#ip add 192.168.3.1 255.255.255.0
R3(config-if)#no shutdown

R3(config-if)#interface s0/0/0
R3(config-if)#description Serial Link to R1
R3(config-if)#ip add 192.168.13.2 255.255.255.252
R3(config-if)#ip authentication mode eigrp 1 md5
R3(config-if)#ip authentication key-chain eigrp 1 EIGRP-KEYS
R3(config-if)#ip hello-interval eigrp 1 30
R3(config-if)#ip hold-time eigrp 1 90
                  ^
% Invalid input detected at '^' marker.
	
R3(config-if)#clock rate 128000
R3(config-if)#bandwidth 128
R3(config-if)#no shutdown

R3(config-if)#interface s0/0/1
R3(config-if)#description Serial Link to R2
R3(config-if)#bandwidth 128
R3(config-if)#ip add 192.168.23.2 255.255.255.252
R3(config-if)#ip authentication mode eigrp 1 md5
R3(config-if)#ip authentication key-chain eigrp 1 eigrp-keys
R3(config-if)#ip bandwidth-percent eigrp 1 40
                 ^
% Invalid input detected at '^' marker.
	
R3(config-if)#ip hello-interval eigrp 1 30
R3(config-if)#ip hold-time eigrp 1 90
                  ^
% Invalid input detected at '^' marker.
	
R3(config-if)#no shutdown

R3(config-if)#router eigrp 1
R3(config-router)#router-id 3.3.3.3
                   ^
% Invalid input detected at '^' marker.
	
R3(config-router)#network 192.168.3.0 0.0.0.255
R3(config-router)#network 192.168.13.0 0.0.0.3
R3(config-router)#network 192.168.23.0 0.0.0.3
R3(config-router)#network 172.16.0.0 0.0.255.255
R3(config-router)#passive-interface g0/0
R3(config-router)#auto-summary
R3(config-router)#end
R3#copy run st
%LINK-5-CHANGED: Interface Loopback3, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback3, changed state to up

%LINK-5-CHANGED: Interface Loopback4, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback4, changed state to up

%LINK-5-CHANGED: Interface Loopback5, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback5, changed state to up

%LINK-5-CHANGED: Interface Loopback6, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback6, changed state to up

%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

%SYS-5-CONFIG_I: Configured from console by console

R3#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R3#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R3#
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

R3#
```



## **Поиск и устранение неполадок в работе EIGRP**


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/8.2.3.7%20Lab%20-%20Troubleshooting%20Advanced%20EIGRP.pdf%20-%20Word%2013.11.2023%2012_24_29.png?raw=true)


![](https://github.com/ivanbondarev1/Youdo/blob/main/LAB15/8.2.3.7%20Lab%20-%20Troubleshooting%20Advanced%20EIGRP.pdf%20-%20Word%2013.11.2023%2012_26_39.png?raw=true)


