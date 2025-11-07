Цель: 

Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.

Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.


---
### Схема

<img width="2758" height="1707" alt="Снимок экрана 2025-11-01 165512" src="https://github.com/user-attachments/assets/5c80fb52-9766-4f73-a2b1-e7b01625773f" />

---
### Настройка 

#### Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.

#### R14

	 crypto isakmp policy 10
	 encr aes
	 authentication pre-share
	 group 14
	 crypto isakmp key AAAAAAA)) address 10.10.124.2    
    
	crypto ipsec transform-set GRE-IPSEC esp-3des esp-sha-hmac 
	mode transport
	
	crypto ipsec profile PROTECT-GRE
	set transform-set GRE-IPSEC 
	
	interface Tunnel14
	tunnel protection ipsec profile PROTECT-GRE
	 !
	 
---
#### R15

	 crypto isakmp policy 10
	 encr aes
	 authentication pre-share
	 group 15
	 crypto isakmp key AAAAAAA)) address 10.10.124.2    
    
	crypto ipsec transform-set GRE-IPSEC esp-3des esp-sha-hmac 
	mode transport
	
	crypto ipsec profile PROTECT-GRE
	set transform-set GRE-IPSEC 
	
	interface Tunnel15
	tunnel protection ipsec profile PROTECT-GRE
	 !



---
#### R18

	 crypto isakmp policy 10
	 encr aes
	 authentication pre-share
	 group 15
	 crypto isakmp key AAAAAAA)) address 10.10.215.2    
    
	crypto ipsec transform-set GRE-IPSEC esp-3des esp-sha-hmac 
	mode transport
	
	crypto ipsec profile PROTECT-GRE
	set transform-set GRE-IPSEC 
	
	interface Tunnel15
	tunnel protection ipsec profile PROTECT-GRE
	 !



---
#### 




---
#### R18




---
#### R18




---
#### R18

