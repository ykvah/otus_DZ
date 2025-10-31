Цель: 

Настроите iBGP в офисом Москва между маршрутизаторами R14 и R15.
Настроите iBGP в провайдере Триада, с использованием RR.
Настройте офиса Москва так, чтобы приоритетным провайдером стал Ламас.
Настройте офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
Все сети в лабораторной работе должны иметь IP связность.


---
### Схема

<img width="1413" height="519" alt="сх" src="https://github.com/user-attachments/assets/0c0c08e1-95b2-4f25-82a0-b1b48048179e" />

---
### Настройка 


#### Мосва


     !
	 R14(config)#router ospf 1
	 R14(config-router)#network 10.0.0.14 0.0.0.0 area 0
	 R14(config-router)#end
	 R14(config)#router bgp 1001 
	 R14(config-router)#neighbor 10.0.0.15 remote-as 1001
	 R14(config-router)#neighbor 10.0.0.15 update-source l0
	 R14(config-router)#address-family ipv4 unicast
	 R14(config-router-af)#neighbor 10.0.0.15 activate
	 !

	 
---

     !
	R15(config)#router ospf 1
	R15(config-router)#network 10.0.0.15 0.0.0.0 area 0
	R15(config-router)#end
	R15(config)#router bgp 1001
	R15(config-router)#neighbor 10.0.0.14 remote-as 1001
	R15(config-router)#neighbor 10.0.0.14 update-source l
	R15(config-router)#address-family ipv4 unicast
	R15(config-router-af)#neighbor 10.0.0.14 activate
	 !


#### Триада



     !
	R23#show run | sec bgp
	router bgp 520
	bgp log-neighbor-changes
	neighbor 10.0.0.24 remote-as 520
	neighbor 10.0.0.24 update-source Loopback0
	neighbor 10.0.0.25 remote-as 520
	neighbor 10.0.0.25 update-source Loopback0
	!
	address-family ipv4
	neighbor 10.0.0.24 activate
	neighbor 10.0.0.25 activate
	neighbor 10.0.0.25 route-reflector-client
	exit-address-family

	 !

---

     !
	R24#show run | s bgp
	redistribute bgp 520
	router bgp 520
	bgp log-neighbor-changes
	neighbor 10.0.0.23 remote-as 520
	neighbor 10.0.0.23 update-source Loopback0
	neighbor 10.0.0.26 remote-as 520
	neighbor 10.0.0.26 update-source Loopback0
	neighbor 10.10.224.1 remote-as 301
	neighbor 10.10.124.2 remote-as 2042
	!
	address-family ipv4
	network 10.10.224.0 mask 255.255.255.252
	network 10.10.124.0 mask 255.255.255.252
	neighbor 10.0.0.23 activate
	neighbor 10.0.0.26 activate
	neighbor 10.0.0.26 route-reflector-client
	neighbor 10.10.224.1 activate
	neighbor 10.10.124.2 activate
	exit-address-family

	 !

---




---





---

	 
