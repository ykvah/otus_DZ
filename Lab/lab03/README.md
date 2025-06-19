# ДЗ к занятию DHCPv4/v6 и SLAAC

# v4
## Топология и таблица адресации
Порты изменены в соответствии с нумерацией доступной в EVE

![alt text](image.png)

Таблица заполнена в соответствии с указаниями ниже

![image](https://github.com/user-attachments/assets/d4ba6063-bdf6-4b88-99c9-5015755c3785)

## 1

a.	One subnet, “Subnet A”, supporting 58 hosts (the client VLAN at R1).
#### Subnet A:192.168.1.0/26 (адреса по 192.168.1.62 + бродкаст .63)
answers here.
Record the first IP address in the Addressing Table for R1 G0/0/1.100. 
#### 192.168.1.1

b.	One subnet, “Subnet B”, supporting 28 hosts (the management VLAN at R1). 
Subnet B: 
#### 192.168.1.64/27 
Type your answers here.
Record the first IP address in the Addressing Table for R1 G0/0/1.200. Record the second IP address in the Address Table for S1 VLAN 200 and enter the associated default gateway.
#### 192.168.1.65

c.	One subnet, “Subnet C”, supporting 12 hosts (the client network at R2).
Subnet C: 
#### 192.168.1.96/28
Type your answers here.
Record the first IP address in the Addressing Table for R2 G0/0/1.
#### 192.168.1.97




Далее базовые настройки

### 3
![1-3](https://github.com/user-attachments/assets/ffe084d5-55c1-4f01-b823-6200c2529c54)

### 4
![1-4](https://github.com/user-attachments/assets/73f37ac7-1e2a-457d-a47e-5c322ce70d84)

### 5
![1-5](https://github.com/user-attachments/assets/bf5fa14d-a8ea-48ad-9147-667f6416aaf3)

### 6
![1-6](https://github.com/user-attachments/assets/c09b99df-936e-4ec0-a697-78914a9c2fb9)

### 7
![1-7](https://github.com/user-attachments/assets/97313e1e-e9f2-42ca-ac69-81a17433d97c)

### 8-9
![1-8,9](https://github.com/user-attachments/assets/a1d78cbb-fc66-48a0-876b-7bb1fd4319e6)

## 2 Конфиг R1
### 1-2
![2-1,2](https://github.com/user-attachments/assets/a2021472-98bb-49f4-856c-80e8dc9cfead)

### 3
![2-3](https://github.com/user-attachments/assets/a38f8ed7-e8de-4e8c-b234-1545a7baee76)

### 4 проверка с ПК А
![2-4](https://github.com/user-attachments/assets/3cedcde7-61b5-4d9f-a25d-dcb21a119087)


### 3 Конфиг R2

ТК на R1 G0/0/1 нет ип пропингуем ПК-А и G0/0/0 на R1
![3](https://github.com/user-attachments/assets/70894b02-7e61-4ccd-88c6-69929ba6267d)

