
# Расширение сети небольшого оператора связи

### Содержание
###### [Задачи](#задачи_)
###### [Схема](#Схема_)
###### [Протоколы](#Протоколы_)
###### [Адресация](#Адресация_)
###### [Конфиг коммутаторов](#Конфиг_коммутаторов)
###### [Конфиг маршрутизаторов агрегации](#Конфиг_маршрутизаторов_агрегации)
###### [Конфиг маршрутизаторов ядра](#Конфиг_маршрутизаторов_ядра)
###### [Конфиг BRAS](#Конфиг_BRAS)
###### [Подключение клиента](#Подключение_клиента)
###### [Развитие](#Развитие_)





---
---
---
<a name="задачи_"></a>
### Цели и задачи
Мы расширяем сеть небольшого оператора и продлеваем ее в новый район, выстраивая новую инфраструктуру, соединив ее с имеющейся.
Необходимо проработать подключение к BRAS и спроектировать разветвленную сеть оборудования, которое будет резервироваться с минимальными затратами и предоставлять возможность для подключения "последний мили" в сторону новых клиентов.





---
---
---
### Схема
<a name="Схема_"></a>

<img width="970" height="650" alt="1" src="https://github.com/user-attachments/assets/a1aae947-aa29-45c6-a292-bec08febcb2d" />





---
---
---
#### Пояснения

Брас новый или уже имеющийся у оператора
("победить" EVE-ng не удалось и вместо образа ASR у меня выступает что-то вроде Циски 7200 L3-ADVENTERPRISE9-15.5.2T).

core_1 и core_2 выполняют функции ядра. Могут быть соединены с другими маршрутизаторами ядра. Имеют резервный линк между собой на случай падения связи с брасом.

Agr1, Agr2, Agr3 - маршрутизаторы агрегации, соединенные друг с другом. Agr1 и Agr3 имеют резервные линки к ядрам.

ACC - коммутаторы доступа. В их порты можно подключать клиента напрямую или достраивать сеть в сторону клиента, например, радиомостом или связкой shdsl модемов, если уже имеется телефонная линия.

Коммутаторы доступа соединены в кольца с Agr







---
---
---
### Что настраиваем
<a name="Протоколы_"></a>
На L2 кольцах настраиваем STP или аналоги, делая линки между ACC не приоритетными.

Все маршрутизаторы будут в Area0 OSPF и подключаются к RR по ibgp +MPLS +LDP.

Все данные от клиентов "кладем" в L2PVN (xconnect) и передаем их по MPLS до выходного сабинтерфейса на брасе.





---
---
---
### Адресация
<a name="Адресация_"></a>

Loopback - 10.42.0.xx/32

Сеть для управления и мониторинга - 10.42.42.xx/24

Сеть между линками - 10.42.xx.xx/30

Сеть управления сейчас завернута в сторону браса, но это ограничение обрезанной схемы. В идеале, если это часть сети оператора, все управление будет уходить на общий сервер доступов и мониторинга.


<details>
<img width="558" height="862" alt="ip11" src="https://github.com/user-attachments/assets/e55b64a1-211a-49e4-a365-437b821afb9f" />

Адресацию между ACC убрал, но на картинке осталась.

<img width="1108" height="635" alt="ip3" src="https://github.com/user-attachments/assets/17ab43a4-83a1-4c5b-899c-0df424af8257" />

</details>




---
---
## Настройка

### Конфиг коммутаторов 
<a name="Конфиг_коммутаторов"></a>

Настройка STP на кольце.

Позже еще увеличил веса линков между ACC_1_1 и ACC_2_2 // ACC_3_1 и ACC_2_1.

Например, на ACC_1_1 (порт к ACC_2_2)

<details>
<img width="937" height="1028" alt="image" src="https://github.com/user-attachments/assets/1d408725-9d5f-467e-aefe-de3075b67d05" />

Уже исправленный ACC_3_1

<img width="668" height="984" alt="image" src="https://github.com/user-attachments/assets/9362267d-b436-402a-9bec-9185b989ee58" />

</details>

---
---
---
#### Полный набор команд для конфигурации коммутаторов


---
##### ACC_1_1

<details>
en
   
conf t

hostname ACC_1_1

no ip domain-lookup

banner motd #Unauthorized access prohibited! Uhodi#

vlan 42

name YPR

interface vlan 42

ip address 10.42.42.21 255.255.255.0

ip default-gateway 10.42.42.1

interface Loopback0

ip address 10.42.0.21 255.255.255.255

no shutdown

exit

spanning-tree mode rapid-pvst

spanning-tree vlan 1-4094 priority 4096

interface e 0/0

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

no shutdown

exit

interface e 0/1

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

spanning-tree vlan 1-4094 cost 200

no shutdown

exit

interface e 0/2

spanning-tree portfast

exit

interface e 0/3

spanning-tree portfast

exit

exit

copy running-config startup-config

</details>




---
##### ACC_2_1

<details>
en
   
conf t

hostname ACC_2_1

no ip domain-lookup

banner motd #Unauthorized access prohibited! Uhodi#

vlan 42

name YPR

interface vlan 42

ip address 10.42.42.22 255.255.255.0

ip default-gateway 10.42.42.1

interface Loopback0

ip address 10.42.0.22 255.255.255.255

no shutdown

exit

spanning-tree mode rapid-pvst

spanning-tree vlan 1-4094 priority 32768

interface e 0/0

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

no shutdown

exit

interface e 0/1

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

spanning-tree vlan 1-4094 cost 200

no shutdown

exit

interface e 0/3

spanning-tree portfast

exit

interface e 0/2

spanning-tree portfast

exit

exit

copy running-config startup-config

</details>




---
   ##### ACC_2_2

<details>
en
   
conf t

hostname ACC_2_2

no ip domain-lookup

banner motd #Unauthorized access prohibited! Uhodi#

vlan 42

name YPR

interface vlan 42

ip address 10.42.42.23 255.255.255.0

ip default-gateway 10.42.42.1

interface Loopback0

ip address 10.42.0.23 255.255.255.255

no shutdown

exit

spanning-tree mode rapid-pvst

spanning-tree vlan 1-4094 priority 32768

interface e 0/0

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

no shutdown

exit

interface e 0/2

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

spanning-tree vlan 1-4094 cost 200

no shutdown

exit

interface e 0/1

spanning-tree portfast

exit

interface e 0/3

spanning-tree portfast

exit

exit

copy running-config startup-config

</details>




---
   ##### ACC_3_1

<details>
en
   
conf t

hostname ACC_3_1

no ip domain-lookup

banner motd #Unauthorized access prohibited! Uhodi#

vlan 42

name YPR

interface vlan 42

ip address 10.42.42.24 255.255.255.0

ip default-gateway 10.42.42.1

no shutdown

interface Loopback0

ip address 10.42.0.24 255.255.255.255

exit

spanning-tree mode rapid-pvst

spanning-tree vlan 1-4094 priority 4096

interface e 0/0

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

no shutdown

exit

interface e 0/3

switchport

switchport trunk encapsulation dot1q

switchport mode trunk

switchport trunk allowed vlan all

spanning-tree portfast disable

spanning-tree bpduguard disable

spanning-tree vlan 1-4094 cost 200

no shutdown

exit

interface e 0/1

spanning-tree portfast

exit

interface e 0/2

spanning-tree portfast

exit

exit

copy running-config startup-config

</details>




---
---
---
### Конфиг маршрутизаторов агрегации
<a name="Конфиг_маршрутизаторов_агрегации"></a>


#### Полный набор команд для конфигурации

Добавляем на каждый Agr настройку backup peer на случай перестроения канала. 

##### Agr1

<details>
en
   
conf t
   
hostname Agr1
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
hostname Agr1
   
mpls ip
   
mpls label protocol ldp
   
interface e1/1.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.11 255.255.255.0
   
ip default-gateway 10.42.42.1
   
interface e0/0.111
   
description CLIENT VLAN111
   
encapsulation dot1Q 111
   
xconnect 10.42.0.30 111 encapsulation mpls
   
backup peer 10.42.0.12 111
   
exit
   
exit
   
interface e0/0.212
   
description BACKUP CLIENT VLAN212
   
encapsulation dot1Q 212
   
xconnect 10.42.0.30 212 encapsulation mpls
   
backup peer 10.42.0.12 212 
   
exit
   
exit
   
interface Loopback0
   
ip address 10.42.0.11 255.255.255.255
   
no shutdown
   
exit
   
interface e0/0
   
ip address 10.0.1.2 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/2
   
ip address 10.0.1.10 255.255.255.252
   
mpls ip
   
ip ospf 1 area 0
   
no shutdown
   
exit
   
interface e0/3
   
ip address 10.0.2.5 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/1
   
ip address 10.0.2.21 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e1/1
   
mpls ip
   
no shutdown
   
exit
   
router ospf 1
   
router-id 10.42.0.11
   
network 10.42.0.11 0.0.0.0 area 0
   
no network 10.0.0.0 0.255.255.255 area 0

network 10.0.1.2 0.0.0.0 area 0
 
network 10.0.1.10 0.0.0.0 area 0
 
network 10.0.2.5 0.0.0.0 area 0
 
network 10.0.2.21 0.0.0.0 area 0

mpls ldp router-id Loopback0 force
   
router bgp 420000
   
bgp log-neighbor-changes
   
bgp router-id 10.42.0.11
neighbor 10.42.0.1   remote-as 420000
   
neighbor 10.42.0.2   remote-as 420000
   
neighbor 10.42.0.12  remote-as 420000
   
neighbor 10.42.0.13  remote-as 420000
   
neighbor 10.42.0.30 remote-as 420000
   
neighbor 10.42.0.1 update-source Loopback0
   
neighbor 10.42.0.2 update-source Loopback0
   
neighbor 10.42.0.12 update-source Loopback0
   
neighbor 10.42.0.13 update-source Loopback0
   
neighbor 10.42.0.30 update-source Loopback0
   
neighbor 10.42.0.1   route-reflector-client
   
neighbor 10.42.0.2   route-reflector-client
   
neighbor 10.42.0.12  route-reflector-client
   
neighbor 10.42.0.13  route-reflector-client
   
neighbor 10.42.0.30 route-reflector-client
   
address-family ipv4
   
neighbor 10.42.0.1   activate
   
neighbor 10.42.0.2   activate
   
neighbor 10.42.0.12  activate
   
neighbor 10.42.0.13  activate
   
neighbor 10.42.0.30  activate
   
exit-address-family
   
address-family vpnv4

address-family vpnv4
  
neighbor 10.42.0.1 activate
 
neighbor 10.42.0.1 send-community both
  
neighbor 10.42.0.2 activate
  
neighbor 10.42.0.2 send-community both
  
neighbor 10.42.0.12 activate
  
neighbor 10.42.0.12 send-community both
  
neighbor 10.42.0.13 activate
  
neighbor 10.42.0.13 send-community both

neighbor 10.42.0.30 activate

neighbor 10.42.0.30 send-community both
   
exit-address-family
   
exit
   
exit
   
copy running-config startup-config

</details>


---
##### Agr2

<details>
en
   
conf t
   
hostname Agr2
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
mpls ip
   
mpls label protocol ldp
   
interface e0/0.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.12 255.255.255.0
   
ip default-gateway 10.42.42.1

   
interface e0/0.211
   
description Client VLAN211
   
encapsulation dot1q 211
   
no ip address
   
xconnect 10.42.0.30 211 encapsulation mpls
   
backup peer 10.42.0.13 211
   
exit
   
exit
   
interface e0/0.212
   
description Client VLAN212
   
encapsulation dot1q 212
   
no ip address
   
xconnect 10.42.0.30 212 encapsulation mpls
   
backup peer 10.42.0.11 212
   
exit
   
exit
   
interface e0/0.111
   
description BACKUP CLIENT VLAN111
   
encapsulation dot1Q 111
   
xconnect 10.42.0.30 111 encapsulation mpls
   
backup peer 10.42.0.11 111
   
exit
   
exit
   
interface e0/0.311
   
description BACKUP CLIENT VLAN311
   
encapsulation dot1Q 311
   
xconnect 10.42.0.30 311 encapsulation mpls
   
backup peer 10.42.0.13 311 
   
exit
   
exit
   
interface Loopback0
   
ip address 10.42.0.12 255.255.255.255
   
no shutdown
   
exit
   
interface e0/0
   
ip address 10.0.2.6 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/1
   
ip address 10.0.2.9 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/2
   
ip address 10.0.2.13 255.255.255.252
   
mpls ip
   
ip ospf 1 area 0
   
no shutdown
   
exit

interface e0/3
   
ip address 10.0.2.25 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
router ospf 1
   
router-id 10.42.0.12
   
network 10.42.0.12 0.0.0.0 area 0
   
network 10.0.2.6 0.0.0.0 area 0
   
network 10.0.2.25 0.0.0.0 area 0

mpls ldp router-id Loopback0 force

router bgp 420000

bgp log-neighbor-changes

bgp router-id 10.42.0.12

neighbor 10.42.0.11   remote-as 420000

neighbor 10.42.0.11 update-source Loopback0

address-family ipv4

neighbor 10.42.0.11   activate

exit-address-family

address-family vpnv4

address-family vpnv4

neighbor 10.42.0.11 activate

neighbor 10.42.0.11 send-community both

exit-address-family

exit

exit

copy running-config startup-config

</details>


---
##### Agr3


<details>
en
   
conf t
   
hostname Agr3
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
mpls ip
   
mpls label protocol ldp
   
interface e1/1.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.13 255.255.255.0
   
ip default-gateway 10.42.42.1
   
interface e0/0.311
   
description CLIENT VLAN311
   
encapsulation dot1Q 311
   
xconnect 10.42.0.30 311 encapsulation mpls
   
backup peer 10.42.0.12 311 
   
exit
   
exit
   
interface e0/0.211
   
description BACKUP Client VLAN211
   
encapsulation dot1q 211
   
no ip address
   
xconnect 10.42.0.30 211 encapsulation mpls
   
backup peer 10.42.0.12 211
   
exit
   
exit
   
interface Loopback0
   
ip address 10.42.0.13 255.255.255.255
   
no shutdown
   
exit
   
interface e0/0
   
ip address 10.0.1.6 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/1
   
ip address 10.0.2.22 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e0/2
   
ip address 10.0.1.14 255.255.255.252
   
mpls ip
   
ip ospf 1 area 0
   
no shutdown
   
exit
   
interface e0/3
   
ip address 10.0.2.26 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
interface e1/1
   
ip address 10.0.2.17 255.255.255.252
   
ip ospf 1 area 0
   
mpls ip
   
no shutdown
   
exit
   
router ospf 1
   
router-id 10.42.0.13
   
network 10.42.0.13 0.0.0.0 area 0
   
network 10.0.1.6 0.0.0.0 area 0
   
network 10.0.1.14 0.0.0.0 area 0
   
network 10.0.2.22 0.0.0.0 area 0
   
network 10.0.2.26 0.0.0.0 area 0
   
mpls ldp router-id Loopback0 force
   
router bgp 420000

bgp log-neighbor-changes
   
bgp router-id 10.42.0.13
   
neighbor 10.42.0.11   remote-as 420000
   
neighbor 10.42.0.11 update-source Loopback0
   
address-family ipv4
   
neighbor 10.42.0.11   activate
   
exit-address-family
   
address-family vpnv4
   
address-family vpnv4
   
neighbor 10.42.0.11 activate
   
neighbor 10.42.0.11 send-community both
   
exit-address-family
   
exit
   
exit
   
copy running-config startup-config

</details>


---
---
---
### Конфиг маршрутизаторов ядра
<a name="Конфиг_маршрутизаторов_ядра"></a>


#### Полный набор команд для конфигурации
 

---
##### Core_1

<details>
en
   
conf t
   
hostname core_1
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
interface e1/1.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.1 255.255.255.0
   
interface Loopback0
   
ip address 10.42.0.1 255.255.255.255
   
no shutdown
   
exit
   
interface e0/0
   
description UPLINK
   
ip address 10.0.3.1 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/2
   
ip address 10.0.1.17 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/1
   
ip address 10.0.1.1 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/3
   
ip address 10.0.1.5 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0 
   
exit
   
router ospf 1
   
router-id 10.42.0.1
   
network 10.42.0.1 0.0.0.0 area 0
   
network 10.0.3.1 0.0.0.0 area 0
   
network 10.0.1.1 0.0.0.0 area 0
   
network 10.0.1.5 0.0.0.0 area 0
   
network 10.0.1.17 0.0.0.0 area 0

mpls ldp router-id Loopback0 force

router bgp 420000

bgp router-id 10.42.0.1

bgp log-neighbor-changes

neighbor 10.42.0.11 remote-as 420000

neighbor 10.42.0.11 update-source Loopback0

address-family ipv4

neighbor 10.42.0.11 activate

exit-address-family

address-family vpnv4

neighbor 10.42.0.11 activate

neighbor 10.42.0.11 send-community both

exit-address-family


exit

ip route 0.0.0.0 0.0.0.0 10.42.0.30

exit

copy running-config startup-config

</details>


---
##### Core_2

<details>
en
   
conf t

hostname core_2
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
interface e1/1.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.2 255.255.255.0
   
ip default-gateway 10.42.42.1
   
interface Loopback0
   
ip address 10.42.0.2 255.255.255.255
   
no shutdown
   
exit
   
interface e0/0
   
description UPLINK
   
ip address 10.0.3.5 255.255.255.252
   
no shutdown
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/1
   
ip address 10.0.1.18 255.255.255.252
   
no shutdown
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/2
   
ip address 10.0.1.9 255.255.255.252
   
no shutdown
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/3
   
ip address 10.0.1.13 255.255.255.252
   
no shutdown
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
router ospf 1
   
router-id 10.42.0.2
   
network 10.42.0.1 0.0.0.0 area 0
   
network 10.0.3.5 0.0.0.0 area 0
   
network 10.0.1.9 0.0.0.0 area 0
   
network 10.0.1.13 0.0.0.0 area 0
   
network 10.0.1.18 0.0.0.0 area 0
   
mpls ldp router-id Loopback0 force
   
router bgp 420000
   
bgp router-id 10.42.0.2
   
bgp log-neighbor-changes
   
neighbor 10.42.0.11 remote-as 420000
   
neighbor 10.42.0.11 update-source Loopback0
   
address-family ipv4
   
neighbor 10.42.0.11 activate
   
exit-address-family
   
address-family vpnv4
   
neighbor 10.42.0.11 activate
   
neighbor 10.42.0.11 send-community both
   
exit-address-family
   
exit
   
exit
   
copy running-config startup-config

</details>


---
---
---
### Конфиг BRAS
<a name="Конфиг_BRAS"></a>


#### Полный набор команд для конфигурации "заменителя" BRAS
 

---
##### BRAS

<details>
en
   
conf t
   
hostname BRAS
   
no ip domain-lookup
   
banner motd #Unauthorized access prohibited! Uhodi#
   
interface e0/0.42
   
description YPR
   
encapsulation dot1q 42
   
ip address 10.42.42.30 255.255.255.0
   
ip default-gateway 10.42.42.1
   
interface Loopback0
   
ip address 10.42.0.2 255.255.255.255
   
no shutdown
   
exit
   
interface e0/1
   
ip address 10.0.3.2 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
interface e0/2
   
ip address 10.0.3.6 255.255.255.252
   
mpls ip
   
ip ospf network point-to-point
   
ip ospf 1 area 0
   
exit
   
router ospf 1
   
router-id 10.42.0.30
   
network 10.42.0.30 0.0.0.0 area 0
   
network 10.0.3.2 0.0.0.0 area 0
   
network 10.0.3.6 0.0.0.0 area 0       
   
router bgp 420000

bgp log-neighbor-changes

bgp router-id 10.42.0.30

neighbor 10.42.0.11 remote-as 420000

neighbor 10.42.0.11 update-source Loopback0

address-family vpnv4

neighbor 10.42.0.11 activate

neighbor 10.42.0.11 send-community both

exit-address-family

interface Ethernet0/0.111

encapsulation dot1Q 111

xconnect 10.42.0.11 111 encapsulation mpls

no shutdown

interface Ethernet0/0.211

encapsulation dot1Q 211

xconnect 10.42.0.11 211 encapsulation mpls

interface Ethernet0/0.212

encapsulation dot1Q 212

xconnect 10.42.0.11 212 encapsulation mpls

interface Ethernet0/0.311

encapsulation dot1Q 311

xconnect 10.42.0.11 311 encapsulation mpls

do copy running-config startup-config
 
</details>


---
### Подключение клиента
<a name="Подключение_клиента"></a>


#### Схема

Подключаем одного клиента, настройки для которого есть на всем оборудовании.

 <img width="545" height="450" alt="111" src="https://github.com/user-attachments/assets/b3ef9839-53db-44ac-895b-0d4d2f6e51f9" />


---
##### Проверяем статусы на всем пути 

<img width="1916" height="1018" alt="итог" src="https://github.com/user-attachments/assets/4a815c09-baad-45bd-bd26-a0ce29a7c5d5" />


Заставить генерировать трафик "клиента" не удалось, поэтому MAC клиента не отображается. Либо это связано с особенностью образа, который я использовал вместо ASR.

В будущем для каждого нового клиента необходимо будет просто добавить требуемые сабинтерфейсы на оборудование и xconnect по аналогии с уже имеющимися.


---
---
---
### Развитие
<a name="Развитие_"></a>

В дальнейшем можно заменить xconnect на vpls. 

Каждый коммутатор будет в своем vlan, на каждый vlan - свой vpls.

Так как у нас имеются кольца ACC-Agr, на них можно перейти на erps.



---
##### Бонус, отсутствующий в оглавлении :)

<details>
Несколько предыдущих топологий, которые эволюционировали в нормальную для оператора.
   
<img width="949" height="704" alt="t1" src="https://github.com/user-attachments/assets/ce8dfe22-3c9c-44dc-b129-18ebe1dc0beb" />

<img width="613" height="626" alt="t2" src="https://github.com/user-attachments/assets/3c869704-a8d6-4200-a473-0adeb6f264af" />

   
</details>

---
