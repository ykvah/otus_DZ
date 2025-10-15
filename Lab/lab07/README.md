Цель: 

Настроите IS-IS в ISP Триада

R23 и R25 находятся в зоне 2222

R24 находится в зоне 24

R26 находится в зоне 26

Настройка осуществляется одновременно для IPv4 и IPv6


---
### Схема

<img width="510" height="366" alt="сх" src="https://github.com/user-attachments/assets/03c3c660-0956-46da-83b3-b19deb813717" />

---
### Адресация

Добавляем еще не настроенные порты

<img width="439" height="453" alt="ip" src="https://github.com/user-attachments/assets/649c59e6-4c58-457a-96c4-e8a53253b62d" />



### Назначение NET идентификаторов
NET идентификатор состоит из: Authority and Format Id (AFI), Area Id, System Id и N-Selector (NSEL).

* AFI равный 49 относились к классу локальных.
* Area ID - номер области, к которой принадлежит маршрутизатор.
* System ID - это идентификатор маршрутизатора. У каждого маршрутизатора в топологии он должен быть уникальным.
* NSEL (аналог порта в протоколах транспортного уровня) всегда 00.

System Id (6 байт) получим путем добавленя названия маршрутизатора

* R23 NET 49.2222.0023.0023.0023.00
* R25 NET 49.2222.0025.0025.0025.00
* R24 NET 49.0024.0024.0024.0024.00
* R26 NET 49.0026.0026.0026.0026.00

---
### Настройка

#### 23 

<img width="957" height="792" alt="23" src="https://github.com/user-attachments/assets/c55f35f7-cc71-4b4e-95f3-23aaeef50858" />

---
#### 24

<img width="959" height="614" alt="24" src="https://github.com/user-attachments/assets/0f85a39c-3e21-49af-b0da-8645e7567540" />

---

На R25 и R26 дописываем недостающие настройки 

``` R25(config)#router isis

R25(config-router)#net 49.2222.0025.0025.0025.00

R25(config-router)#passive-interface default

R25(config-router)#exit

R25(config)#interface e0/0

R25(config-if)#ip address 10.10.53.1 255.255.255.252

R25(config-if)#ip router isis

R25(config-if)#no shutdown

R25(config-if)#exit

R25(config)#interface e0/2

R25(config-if)#ip router isis

R25(config-if)#no shutdown

R25(config-if)#exit ```



``` R26(config)#router isis

R26(config-router)#net 49.0026.0026.0026.0026.00

R26(config-router)#exit

R26(config)#interface e0/0

R26(config-if)#ip address 10.10.46.2 255.255.255.252

R26(config-if)#ip router isis

R26(config-if)#no shutdown

R26(config-if)#exit

R26(config)#interface e0/2

R26(config-if)#ip router isis ```


---




