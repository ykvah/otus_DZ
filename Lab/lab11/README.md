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


R14(config-route-map)#router bgp 1001
R14(config-router)#address-family ipv4 unicast
R14(config-router-af)#neighbor 198.18.0.2 route-map 1 out
