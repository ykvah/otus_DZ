Цель: 

Настроите eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.

Настроите eBGP между провайдерами Киторн и Ламас.

Настроите eBGP между Ламас и Триада.

Настроите eBGP между офисом С.-Петербург и провайдером Триада.

Организуете IP доступность между пограничным роутерами офисами Москва и С.-Петербург.


---
### Схема

<img width="1413" height="519" alt="сх" src="https://github.com/user-attachments/assets/0c0c08e1-95b2-4f25-82a0-b1b48048179e" />


---
### Адресация

Добавляем недостающую адресацию 

R21 - R22 - 10.10.222.0/30

R21 - R15 - 10.10.215.0/30

R21 - R24 - 10.10.224.0/30

R22 - R14 - 10.10.214.0/30

R22 - R23 - 10.10.223.0/30

R18 - R24 - 10.10.124.0/30

R18 - R26 - 10.10.126.0/30

<img width="354" height="750" alt="ip" src="https://github.com/user-attachments/assets/8e6a558a-8e8c-4cbb-b496-d79abb3c867f" />


---
### Настройка 


#### R22 

     !
	router bgp 101
	bgp router-id 10.0.0.22
	bgp log-neighbor-changes
	neighbor 10.10.214.2 remote-as 1001
	neighbor 10.10.222.1 remote-as 301
	neighbor 10.10.223.2 remote-as 520
	network 10.0.0.22 mask 255.255.255.255
	address-family ipv4
	neighbor 10.10.214.2 activate
	neighbor 10.10.222.1 activate
	neighbor 10.10.223.2 activate
	exit-address-family

	 !



---
#### R21

     !
	router bgp 301
	bgp router-id 10.0.0.21
	bgp log-neighbor-changes
	neighbor 10.10.222.2 remote-as 101
	neighbor 10.10.224.2 remote-as 520
	neighbor 10.10.215.2 remote-as 1001
	network 10.0.0.21 mask 255.255.255.255
	address-family ipv4
	neighbor 10.10.215.2 activate
	neighbor 10.10.222.2 activate
	neighbor 10.10.224.2 activate
	exit-address-family

	 !


---
#### R14

     !
	 router bgp 1001
	 bgp router-id 10.0.0.14
	 neighbor 10.10.214.1 remote-as 101
	 neighbor 10.10.145.2 remote-as 1001
	 neighbor 10.10.145.2 next-hop-self
	 network 10.0.0.14 mask 255.255.255.255
	 address-family ipv4
	 neighbor 10.10.214.1 activate
	 neighbor 10.10.145.2 activate
	 exit-address-family

	 !


---
#### R15


     !
	 router bgp 1001
	 bgp router-id 10.0.0.15
	 bgp log-neighbor-changes
	 network 10.0.0.15 mask 255.255.255.255
	 neighbor 10.10.145.1 remote-as 1001
	 neighbor 10.10.145.1 next-hop-self
	 neighbor 10.10.215.1 remote-as 301
	 address-family ipv4
	 neighbor 10.10.215.1 activate
	 neighbor 10.10.145.1 activate
	 exit-address-family

	 !







---
#### R18


     !
	 router bgp 2042
	 bgp router-id 10.0.0.18
	 bgp log-neighbor-changes
	 neighbor 10.10.124.1 remote-as 520
	 neighbor 10.10.126.1 remote-as 520
	 network 10.0.0.18 mask 255.255.255.255
	 address-family ipv4
	 neighbor 10.10.124.1 activate
	 neighbor 10.10.126.1 activate
	 exit-address-family

	 !

---
#### R23


         !
	 router bgp 520
	 bgp router-id 10.0.0.23
	 bgp log-neighbor-changes
	 neighbor 10.10.223.1 remote-as 101
	 neighbor 10.10.34.1 remote-as 520
	 neighbor 10.10.34.1 next-hop-self
	 network 10.0.0.23 mask 255.255.255.255
	 address-family ipv4
	 neighbor 10.10.223.1 activate
	 neighbor 10.10.34.1 activate
	 exit-address-family

	 !


---
#### R24

     !
	 router bgp 520
	 bgp router-id 10.0.0.24
	 bgp log-neighbor-changes
	 neighbor 10.10.224.1 remote-as 301
	 neighbor 10.10.124.2 remote-as 2042
	 neighbor 10.10.34.2 remote-as 520
	 neighbor 10.10.34.2 next-hop-self
	 network 10.0.0.24 mask 255.255.255.255
	 address-family ipv4
	 neighbor 10.10.224.1 activate
	 neighbor 10.10.34.2 activate
	 neighbor 10.10.124.2 activate
	 exit-address-family

	 !
---
---
---

### Проверка

<img width="602" height="352" alt="pr" src="https://github.com/user-attachments/assets/d9b9dc41-7d40-4961-9f9d-74ec628cc17b" />


