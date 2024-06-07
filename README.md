# DHCP
____________________

Открыть порт доя распределения адресов
```
nano /etc/sysconfig/dhcpd
DHCPDARGS=ens224
```
Настройка DHCP
```
cp /etc/dhcp/dhcpd.conf{.example,}
nano /etc/dhcp/dhcpd.conf

option domain-name "HOST1";
option domain-name-servers 10.0.0.1;

default-lease-time 6000;
max-lease-time 72000;

authoritative;

subnet 10.0.0.0 netmask 255.255.255.192 {
  range 10.0.0.1 10.0.0.62;
  option routers 10.0.0.1;
}

host HOST2 {
  hardware ethernet MAC;
  fixed-address 10.0.0.2;
}

```
Проверяем файл на правильность заполнения. Обратите внимание, что файл заполнен в точности со скриншотом выше. (фигурные скобки в начале и конце секции, знаки **;** и тд.)
```
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
Запускаем:

```
systemctl enable --now dhcpd
systemctl status dhcpd
journalctl -f -u dhcpd
```
____________________


**3.	Настройте автоматическое распределение IP-адресов на роутере HQ-R.**
**a.	Учтите, что у сервера должен быть зарезервирован адрес.**
## **HQ-R**
```
nano /etc/sysconfig/dhcpd
DHCPDARGS=ens224
ctrl-x
y
enter
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/16af1efa-2fb8-44ce-8e23-128430e6d46c)  
```
cp /etc/dhcp/dhcpd.conf{.example,}
nano /etc/dhcp/dhcpd.conf
```
поправляем файл:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9263f9b8-4ac5-41e5-aa44-4d93285e774e)  


Проверяем файл на правильность заполнения. Обратите внимание, что файл заполнен в точности со скриншотом выше. (фигурные скобки в начале и конце секции, знаки **;** и тд.)
```
dhcpd -t -cf /etc/dhcp/dhcpd.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/77444d5e-534b-42d9-9b55-e976e9ce13ff)   

```
systemctl enable --now dhcpd
systemctl status dhcpd
journalctl -f -u dhcpd
```

## **HQ-SRV**
```
systemctl restart network
```
После проделанных манирпуляций HQ-SRV должен получить статический адрес.
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/84c4ba59-3f4b-442b-a86f-bb7911b963a0)  
