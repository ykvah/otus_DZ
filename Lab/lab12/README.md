Цель: 

Настроите NAT(PAT) на R14 и R15. Трансляция должна осуществляться в адрес автономной системы AS1001.

Настроите NAT(PAT) на R18. Трансляция должна осуществляться в пул из 5 адресов автономной системы AS2042.

Настроите статический NAT для R20.

Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.

Настроите статический NAT(PAT) для офиса Чокурдах.

Настроите для IPv4 DHCP сервер в офисе Москва на маршрутизаторах R12 и R13. VPC1 и VPC7 должны получать сетевые настройки по DHCP.

Настроите NTP сервер на R12 и R13. Все устройства в офисе Москва должны синхронизировать время с R12 и R13.

---
### Схема

<img width="2758" height="1707" alt="Снимок экрана 2025-11-01 165512" src="https://github.com/user-attachments/assets/5c80fb52-9766-4f73-a2b1-e7b01625773f" />


---
### Настройка 


#### Настроите NAT(PAT) на R14 и R15.



     !
	R14(config)#access-list 1 permit 10.10.142.0 0.0.0.3
	R14(config)#access-list 1 permit 10.10.143.0 0.0.0.3
	R14(config)#access-list 1 permit 10.10.152.0 0.0.0.3
	R14(config)#access-list 1 permit 10.10.153.0 0.0.0.3
	R14(config)#access-list 1 permit 10.10.149.0 0.0.0.3
	R14(config)#access-list 1 permit 10.10.150.0 0.0.0.3
	R14(config)#ip nat inside source list 1 interface e0/2 overload
	R14(config)#interface range e0/0-1, e0/3
	R14(config-if-range)#ip nat inside
	R14(config-if-range)#interface e0/2
	R14(config-if)#ip nat outside
	 !


---


     !
	R15(config)#access-list 1 permit 10.10.142.0 0.0.0.3
	R15(config)#access-list 1 permit 10.10.143.0 0.0.0.3
	R15(config)#access-list 1 permit 10.10.152.0 0.0.0.3
	R15(config)#access-list 1 permit 10.10.153.0 0.0.0.3
	R15(config)#access-list 1 permit 10.10.149.0 0.0.0.3
	R15(config)#access-list 1 permit 10.10.150.0 0.0.0.3
	R15(config)#ip nat inside source list 1 interface e0/2 overload
	R15(config)#interface range e0/0-1, e0/3
	R15(config-if-range)#ip nat inside
	R15(config-if-range)#interface e0/2
	R15(config-if)#ip nat outside
	 !


---
#### Настроите NAT(PAT) на R18

Пул с адресами VPC и VPC8

     !
	R18(config)#ip nat pool NAT_R18 192.168.0.8 192.168.0.13 netmask 255.255.255.0
	R18(config)#access-list 1 permit 192.168.0.0 0.0.255.255
	R18(config)#ip nat inside source list 1 pool NAT_R18 overload
	R18(config)#interface range e0/0-1
	R18(config-if-range)#ip nat inside
	R18(config-if-range)#exit
	R18(config)#interface range e0/2-3
	R18(config-if-range)#ip nat outside
	 !

---

#### Настроите статический NAT для R20

Для этого настраиваем NAT на R15


     !
	R15(config)#ip nat inside source static 10.10.150.1 10.10.215.2
	R15(config)#interface e0/3
	 !

---
#### Настроите NAT так, чтобы R19 был доступен с любого узла для удаленного управления.

Для этого настраиваем NAT на R14 c пробросом портов для подключения 


     !
	R14(config)#ip nat inside source static tcp 10.10.149.1 22 10.10.214.2 22 extendable
	R14(config)#ip nat inside source static tcp 10.10.149.1 23 10.10.214.2 23 extendable
	R14(config)#interface Ethernet0/3
	R14(config)#ip nat inside
	 !



---
#### Настроите статический NAT(PAT) для офиса Чокурдах.


     !
	 R28(config)#ip nat pool NAT-POOL1 10.10.68.1 10.10.68.1 prefix-length 30
	R28(config)#access-list 100 permit ip 10.10.58.0 0.0.0.3 any
	R28(config)#access-list 100 permit ip 10.10.68.0 0.0.0.3 any
	R28(config)#ip nat inside source list 100 pool NAT-POOL1 overload
	R28(config)#interface Ethernet0/0
	R28(config-if)#ip nat outside
	R28(config-if)#interface Ethernet0/0
	R28(config-if)# ip nat outside
	R28(config-if)#interface Ethernet0/0
	R28(config-if)#interface Ethernet0/2
	R28(config-if)# ip nat inside
	 !




---




---
   
