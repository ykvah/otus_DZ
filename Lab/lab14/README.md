Цель: 

Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.

Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.


---
### Схема

<img width="2758" height="1707" alt="Снимок экрана 2025-11-01 165512" src="https://github.com/user-attachments/assets/5c80fb52-9766-4f73-a2b1-e7b01625773f" />

---
### Настройка 

#### Настроите GRE поверх IPSec между офисами Москва и С.-Петербург.


#### R15

crypto isakmp policy 10
	 encr aes
	 authentication pre-share
	 group 2
	 crypto isakmp key AAAAAAA)) address 10.10.214.2
	 crypto isakmp key AAAAAAA)) address 10.10.215.2
	 
	 crypto ipsec transform-set GRE-IPSEC esp-3des esp-sha-hmac
	 mode transport
	 
	 crypto ipsec profile PROTECT-GRE
	 set transform-set GRE-IPSEC


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
#### Настроите DMVPN поверх IPSec между Москва и Чокурдах, Лабытнанги.

На R14 и R15 нужно добавить строку с адресом R28 и R27 в политику.

#### R27


Text).

  !
  crypto isakmp policy 10
	 encr aes
	 authentication pre-share
	 group 2
	 crypto isakmp key AAAAAAA)) address 10.10.214.2
	 crypto isakmp key AAAAAAA)) address 10.10.215.2
	 
	 crypto ipsec transform-set GRE-IPSEC esp-3des esp-sha-hmac
	 mode transport
	 
	 crypto ipsec profile PROTECT-GRE
	 set transform-set GRE-IPSEC
	 !

---
#### R28




---
#### R18

