
Задачи
Настроите политику маршрутизации для сетей офиса.

Распределите трафик между двумя линками с провайдером.

Настроите отслеживание линка через технологию IP SLA.(только для IPv4)

Настройте для офиса Лабытнанги маршрут по-умолчанию.

План работы и изменения зафиксированы в документации .


### Схема

<img width="879" height="428" alt="сх+ip" src="https://github.com/user-attachments/assets/fd07ff7e-f416-4cb1-8360-c33133c25a5e" />


---
В прошлой работе была выбрана неудачная адресация. Переделал

### Адресация

Назначаем адресацию на линки 

R25 - R26 10.10.56.0/30

R25 - R27 10.10.57.0/30

R25 - R28 10.10.58.0/30

R26 - R28 10.10.68.0/30

<img width="429" height="552" alt="ip" src="https://github.com/user-attachments/assets/4ccc8efe-116b-4cc1-93fa-47e43bcc5181" />

#### Настройка

### Начнем с R27 в том числе настроим маршрут по-умолчанию для офиса Лабытнанги (через R25)

<img width="960" height="689" alt="27" src="https://github.com/user-attachments/assets/7cb6ccf8-a263-4e76-84a9-119c7b041f3c" />

---
### R28 

Настраиваем ip  sla

Проверяет доступность R25 и R26 каждые 5 секунд

настройка PBR
Часть трафика через R25, часть через R26. 

Создаем access-list и привязываем его к route-map

Применяем PBR к исходящему интерфейсу, где идёт трафик

<img width="771" height="878" alt="28" src="https://github.com/user-attachments/assets/e25193aa-d699-4062-8a04-e9137637f04e" />

---
### R26

<img width="955" height="776" alt="26" src="https://github.com/user-attachments/assets/1c71d242-714f-4750-bbc3-7aa954f7faea" />

---
### R25

<img width="954" height="872" alt="25" src="https://github.com/user-attachments/assets/e9a729ed-b984-4473-a605-6d4061887865" />

исправил адреса на правильные

<img width="547" height="102" alt="25исп" src="https://github.com/user-attachments/assets/6cff2cae-4eda-4774-9513-f414b3f71764" />


---
## Проверяем 

show ip sla summary
show track

<img width="828" height="695" alt="pr28" src="https://github.com/user-attachments/assets/77c08a04-573e-48cc-90b5-c2f2915c6ea4" />


