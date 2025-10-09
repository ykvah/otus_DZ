#### Задачи

Настроить OSPF офисе Москва

Разделить сеть на зоны

Настроить фильтрацию между зонами

Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.

Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.

Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.

Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.

---
### Схема

<img width="1008" height="722" alt="Снимок+ip" src="https://github.com/user-attachments/assets/90754958-c1b7-44c2-9736-de7af05c7040" />

Так как поставленная задача противоречит изначальной схеме 

не будем мудрить с виртуальным линком и добавим физический линк между R14 e1/1 и R15 e1/1

---
### Адресация

Продолжаем корректировать адресацию

R14 - R19 10.10.149.0/30

R14 - R12 10.10.142.0/30

R14 - R13 10.10.143.0/30

R14 - R15 10.10.145.0/30

R15 - R20 10.10.150.0/30

R15 - R12 10.10.152.0/30

R15 - R13 10.10.153.0/30

<img width="347" height="679" alt="ip" src="https://github.com/user-attachments/assets/1dbea01b-63df-404c-9dc8-201cd0da1420" />




---
#### Настройка

---
### R20 

Фильтрация должна настраиваться на R15

+ включаем passive на лупбэке чтобы он не менял статус и не слал Helo парекеты

<img width="857" height="724" alt="R20" src="https://github.com/user-attachments/assets/13cca1d9-e73f-4aad-a6c6-87c71a7183f1" />


---
### R19

Чтобы выполнить поставленную задачу назначаем area 101 stub area а на R14 укажем no-summary

<img width="1119" height="763" alt="R19" src="https://github.com/user-attachments/assets/624a4570-a173-4dd7-b604-dd1ef793fe77" />


---
### R14

<img width="850" height="1030" alt="R14" src="https://github.com/user-attachments/assets/b617e623-736e-4b93-9e87-6ebcd57f2b85" />


---
### R15

Добовляем фильтрацию и включаем ее для area 102

<img width="948" height="1034" alt="R15" src="https://github.com/user-attachments/assets/58addd1c-2615-42c5-b7e5-9af2876e53d7" />


---
### R12

<img width="951" height="1034" alt="R12" src="https://github.com/user-attachments/assets/50c9261d-b356-4c67-81af-06fed0d7a07b" />


---
### R13

<img width="958" height="853" alt="R13" src="https://github.com/user-attachments/assets/5899df78-2579-4be6-bd70-287407c17b16" />


---
### Проверка










