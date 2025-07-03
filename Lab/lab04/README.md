Задачи

Разработаете и задокументируете адресное пространство для лабораторного стенда.

Настроите ip адреса на каждом активном порту

Настроите каждый VPC в каждом офисе в своем VLAN.

Настроите VLAN/Loopbackup interface управления для сетевых устройств

Настроите сети офисов так, чтобы не возникало broadcast штормов, а использование линков было максимально оптимизировано

##  Меняем название ПК и вланов для удобства и назначаем адреса в таблице 

Также оставляем добавленный в прошлой лабе SW30 в Чокурдах

![alt text](1.png)



### Вланы
![alt text](vlan.png)

Дальше для удобства выдаем адреса 10.10.номер коммутатора.номер порта из сети

Loopback - 10.0.0.номер коммутатора

Vlan - 192.168.номер влан.номер коммутатора

## Чокурдах

![alt text](чук.png)

## Триада + Лабытнанги

![Триада + Лабытнанги](https://github.com/user-attachments/assets/f770e2bc-6b58-43c2-9241-429b0daa8de2)


##  СПБ

![spb1](https://github.com/user-attachments/assets/3b90816a-9590-4e04-bd1c-3a939af94b3c)
![spb2](https://github.com/user-attachments/assets/2e0b7f91-b037-4c71-a8a4-432b05f37320)

## МСК

![image](https://github.com/user-attachments/assets/5e29212a-bcb6-4ee8-8b25-aed42f3eeef4)

## Пример настройки VPC

![pc](https://github.com/user-attachments/assets/3604269d-8e31-471a-8976-681999ef81c5)

## Пример настройки коммутаторов и маршрутизаторов

![SW29](https://github.com/user-attachments/assets/6221c068-594b-4d57-8aed-64a8c9de3c95)

![SW30](https://github.com/user-attachments/assets/9422dc92-0dd4-4e5e-8a6e-36f12c3e4503)

Для маршрутизации Vlan используется технология "роутер на палочке" через сабинтерфейсы на маршрутизаторе R28

![R28](https://github.com/user-attachments/assets/d894c8f9-8c45-41cf-8607-e411762339cc)

В офисе Санкт-Петербург маршрутизация Vlan осуществляется сразу на коммутаторах SW9 и SW10 посредством SVI интерфейсов который будут шлюзом по умолчанию для пользовательских хостов сети:

SW9 - int Vlan 30 - 192.168.30.1/24

SW10 - int Vlan 40 - 192.168.40.1/24

В офисе Москва на L2 сегменте сети организованна отказаустойчивость за счет избыточности линков с использованием технологии HSRP соответственно на L3 коммутаторах SW4 SW5 помимо SVI интерфейсов VLAN подняты виртуальные шлюзы по умолчанию для соответсвующих VLAN.

SW4

interface Vlan10

 ip address 192.168.10.4 255.255.255.0
 
 standby 1 ip 192.168.10.1
 
 standby 1 priority 110
 
 standby 1 preempt

 
 
interface Vlan20

 ip address 192.168.20.4 255.255.255.0
 
 standby 2 ip 192.168.20.1
 
 standby 2 preempt

 
SW5

interface Vlan10

ip address 192.168.10.5 255.255.255.0

standby 1 ip 192.168.10.1

standby 1 preempt



interface Vlan20

ip address 192.168.20.5 255.255.255.0

standby 2 ip 192.168.20.1

standby 2 priority 110

standby 2 preempt





