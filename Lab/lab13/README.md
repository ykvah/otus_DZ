Цель: 

Настроите GRE между офисами Москва и С.-Петербург.

Настроите DMVMN между Москва и Чокурдах, Лабытнанги.


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

     !
	 	 
	 R14(config)#interface Tunnel11
	 R14(config-if)#description DMVPN
	 R14(config-if)#ip address 10.10.11.14 255.255.255.0
	 R14(config-if)#ip mtu 1400
	 R14(config-if)#ip nhrp map multicast dynamic
	 R14(config-if)#ip nhrp network-id 11
	 R14(config-if)#ip tcp adjust-mss 1360
	 R14(config-if)#tunnel source 10.10.214.2
	 R14(config-if)#tunnel mode gre multipoint
	 R14(config-if)#tunnel key 11
	 !




 
	 R15(config)#interface Tunnel10
	 R15(config-if)#description DMVPN
	 R15(config-if)#ip address 10.10.10.15 255.255.255.0
	 R15(config-if)#ip mtu 1400
	 R15(config-if)#ip nhrp map multicast dynamic
	 R15(config-if)#ip nhrp network-id 1
	 R15(config-if)#ip tcp adjust-mss 1360
	 R15(config-if)#tunnel source 10.10.215.2
	 R15(config-if)#tunnel mode gre multipoint
	 R15(config-if)#tunnel key 1
	 !


---

     !
	 R27(config)##interface Tunne10
	 R27(config-if)#ip address 10.10.10.27 255.255.255.0
	 R27(config-if)#no ip redirects
	 R27(config-if)#ip mtu 1400
	 R27(config-if)#ip tcp adjust-mss 1360
	 R27(config-if)#ip nhrp network-id 1
	 R27(config-if)#ip nhrp map multicast 10.10.215.2
	 R27(config-if)#ip nhrp map 10.10.10.15 10.10.215.2
	 R27(config-if)#ip nhrp nhs 10.10.10.15
	 R27(config-if)#tunnel source 10.10.57.2
	 R27(config-if)#tunnel mode gre multipoint
	 R27(config-if)#tunnel key 10
	 R27(config-if)exit	 
	 R27(config)#interface Tunnel11
	 R27(config-if)#ip address 10.10.11.27 255.255.255.0
	 R27(config-if)#no ip redirects
	 R27(config-if)#ip mtu 1400
	 R27(config-if)#ip nhrp map multicast 10.10.214.2
	 R27(config-if)#ip nhrp map 10.10.11.14 10.10.214.2
	 R27(config-if)#ip nhrp network-id 11
	 R27(config-if)#ip nhrp nhs 10.10.11.14
	 R27(config-if)#ip tcp adjust-mss 1360
	 R27(config-if)#tunnel source 10.10.57.2
	 R27(config-if)#tunnel mode gre multipoint
	 R27(config-if)#tunnel key 11
	 !






---
####



---
####

