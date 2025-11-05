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



---




---




---




---




---
   
