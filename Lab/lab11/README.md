Цель: 

Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).

Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).

Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.

Настроить провайдера Ламас так, чтобы в офис Москва отдавался только маршрут по умолчанию и префикс офиса С.-Петербург.

---
### Схема

<img width="1413" height="519" alt="сх" src="https://github.com/user-attachments/assets/0c0c08e1-95b2-4f25-82a0-b1b48048179e" />

---
### Настройка 


#### Настроить фильтрацию в офисе Москва так, чтобы не появилось транзитного трафика(As-path).


     !
	R14(config)#ip as-path access-list 1 permit ^$
	R14(config)#route-map 1 permit 10
	R14(config-route-map)#match as-path 1
	 !

---

     !
	R14(config-route-map)#router bgp 1001
	R14(config-router)#address-family ipv4 unicast
	R14(config-router-af)#neighbor 10.10.214.1 route-map 1 out
	 !

---

     !
	R15(config)#ip as-path access-list 1 permit ^$
	R15(config)#route-map 1 permit 10
	R15(config-route-map)#match as-path 1
	 !

---
     !
	R15(config-route-map)#router bgp 1001
	R15(config-router)#address-family ipv4 unicast
	R15(config-router-af)#nei
	R15(config-router-af)#neighbor 10.10.215.1 route-map 1 out
	 !

---
#### Настроить фильтрацию в офисе С.-Петербург так, чтобы не появилось транзитного трафика(Prefix-list).

     !
	R18(config)#ip prefix-list SPB permit 10.0.0.0/32 le 32
	R18(config)#ip prefix-list SPB permit 10.10.86.0/30 le 32
	R18(config)#ip prefix-list SPB permit 10.10.87.0/30 le 32
	R18(config)#ip prefix-list SPB permit 10.10.163.0/30 le 32
	R18(config)#route-map 2 permit 10
	R18(config-route-map)#match ip address prefix-list SPB
	 !

---

     !
	R18(config-router)#address-family ipv4
	R18(config-router-af)#neighbor 10.10.124.1 route-map 2 out
	R18(config-router-af)#neighbor 10.10.126.1 route-map 2 out
	 !
---

#### Настроить провайдера Киторн так, чтобы в офис Москва отдавался только маршрут по умолчанию.

   !
	R22(config)#router bgp 101
	R22(config-router)#address-family ipv4 unicast
	R22(config-router-af)#neighbor 10.10.215.2 default-originate
	 !

---



---




---

	 
