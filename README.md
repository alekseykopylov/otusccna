# Лабораторная работа 1

## Configure Router-on-a-Stick Inter-VLAN Routing

### Топология

![image](https://user-images.githubusercontent.com/99170470/152750516-5a3fd07b-a1ab-46e2-9d8f-686837fbbc63.png)

### Адресация

|Устройство|Интерфейс|IP-адрес|Маска|Шлюз|
|-|-|-|-|-|
|R1|G0/0/1.3|192.168.3.1|255.255.255.0|N/A|
||G0/0/1.4|192.168.4.1|255.255.255.0|N/A|
||G0/0/1.8|N/A|N/A|N/A|
|S1|VLAN 3|192.168.3.11|255.255.255.0|192.168.3.1|
|S2|VLAN 3|192.168.3.12|255.255.255.0|192.168.3.1|
|PC-A|NIC|192.168.3.3|255.255.255.0|192.168.3.1|
|PC-B|NIC|192.168.4.3|255.255.255.0|192.168.4.1|

### Таблица VLAN

|VLAN|Имя|Интерфейс|
|-|-|-|
|3|Management|S1: Vlan 3<br />S2: VLAN 3<br />S1: F0/6|
|4|Operations|S2: F0/18|
|7|ParkingLot|S1: F0/2-4, F0/7-24, G0/1-2<br />S2: F0/2-17, F0/19-24, G0/1-2|
|8|Native|N/A|

### Базовые настройки роутера и коммутаторов

  hostname R1 (S1, S2)<br />
  enable password 7 0822404F1A0A<br />
  no ip domain-lookup<br />
  banner motd ^CSuda nelzya!^C<br />
  line con 0<br />
  password 7 0822455D0A16<br />
  login<br />
  line vty 0 4<br />
  password 7 0822455D0A16<br />
  login<br />
  line vty 5 15<br />
  password 7 0822455D0A16<br />
  login<br />
  clock set 11:00:00 7 Feb 2022
  
### Создание вланов на коммутаторах

  conf t  
  vlan 3  
  name Management  
  vlan 4  
  name Operations  
  vlan 7  
  name ParkingLot  
  vlan 8  
  name Native  
  
  S1:  
  interface Vlan3  
  ip address 192.168.3.11 255.255.255.0  
  interface FastEthernet0/6  
  switchport access vlan 3  
  switchport mode access  
  
  S2:  
  interface Vlan3  
  ip address 192.168.3.12 255.255.255.0  
  interface FastEthernet0/18  
  switchport access vlan 4  
  switchport mode access  
  
  int range \<interfaces\>  
  sw m a  
  sw a v 7  
  shut  
  
### Создание транков
  interface FastEthernet0/1  
  switchport trunk native vlan 8  
  switchport trunk allowed vlan 3-4,8  
  switchport mode trunk  
  
  S1:  
  interface FastEthernet0/5  
  switchport mode trunk  
  
