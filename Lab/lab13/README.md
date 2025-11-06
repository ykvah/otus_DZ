Цель: 

Настроите GRE между офисами Москва и С.-Петербург.

Настроите DMVMN между Москва и Чокурдах, Лабытнанги.

Все узлы в офисах в лабораторной работе должны иметь IP связность.

План работы и изменения зафиксированы в документации.

---
### Схема

<img width="2758" height="1707" alt="Снимок экрана 2025-11-01 165512" src="https://github.com/user-attachments/assets/5c80fb52-9766-4f73-a2b1-e7b01625773f" />

---
### Настройка 


#### Настроите GRE между офисами Москва и С.-Петербург.

Mежду роутером R15 Москва и R18 С.-Петербург

возьмём для GRE туннеля адрес 10.10.10.10 255.255.255.0.


     !
	 R15(config)##interface Tunnel10
	 R15(config-if)#ip address 10.10.10.10 255.255.255.0
	 R15(config-if)#ip mtu 1400
	 R15(config-if)#ip tcp adjust-mss 1360
	 R15(config-if)# tunnel source 10.10.215.2
	 R15(config-if)#tunnel destination 10.10.124.2
	 !


     !
	 R18(config)##interface Tunnel10
	 R18(config-if)#ip address 10.10.10.10 255.255.255.0
	 R18(config-if)#ip mtu 1400
	 R18(config-if)#ip tcp adjust-mss 1360
	 R18(config-if)# tunnel source 10.10.124.2
	 R18(config-if)#tunnel destination 10.10.215.2
	 !


---
#### Настроите DMVMN между Москва и Чокурдах, Лабытнанги.

редистрибьюция ISIS в BGP на R24 Триада


     !
	 R24#conf t
	 R24(config)#router bgp 520
	 R24(config-router)#address-family ipv4 unicast
	 R24(config-router-af)#redistribute isis
	 R24(config-router-af)#exit
	 !

 редистрибьюция присоединённых сетей на R25 в Триада


     !
	 R25(config)#router isis
	 R25(config-router)#redistribute connected
	 !

---



     !
	 R14(config)##interface Tunnel10
	 R14(config-if)#ip address 10.10.10.10 255.255.255.0
	 R14(config-if)#ip mtu 1400
	 R14(config-if)#ip tcp adjust-mss 1360
	 R14(config-if)# tunnel source 10.10.214.2
	 R14(config-if)#tunnel destination 10.10.57.2
	 R14(config-if)#exit
	 R14(config)#interface t10
	 R14(config-if)#tunnel mode gre multipoint
	 Tunnel set mode failed. p2mp tunnels cannot have a tunnel destination.
	 R14(config-if)#no tunnel destination 10.10.57.2
	 R14(config-if)#tunnel mode gre multipoint
	 R14(config-if)#ip nhrp network-id 100
	 R14(config-if)#ip nhrp map multicast dynamic
	 !

---


     !
	 R27(config)##interface Tunnel10
	 R27(config-if)#ip address 10.10.10.10 255.255.255.0
	 R27(config-if)#ip mtu 1400
	 R27(config-if)#ip tcp adjust-mss 1360
	 R27(config-if)# tunnel source 10.10.57.2
	 R27(config-if)#tunnel destination 10.10.214.2
	 R14(config-if)exit
	 R14(config)#interface t10
	 R14(config-if)#tunnel mode gre multipoint
	 Tunnel set mode failed. p2mp tunnels cannot have a tunnel destination.
	 R14(config-if)#no tunnel destination 10.10.57.2
	 R14(config-if)#tunnel mode gre multipoint
	 R14(config-if)#ip nhrp network-id 100
	 R14(config-if)#ip nhrp map multicast dynamic
	 
	 !


---
####



---
####

