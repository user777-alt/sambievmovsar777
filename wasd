```
nano /etc/apt/sources.list.d
Репозитории сервера
```bash
rpm [p11] http://192.168.1.150/p11/branch/x86_64_classic
rpm [p11] http://192.168.1.150/p11/branch/noarch classic
```
# ALT-NET
```bash
apt-get update && apt-get dist-upgrade -y
```
```bash
apt-get install ahttpd alterator alterator fbi -y
```
```bash
systemctl enable --now alteratord
systemctl enable --now ahttpd
```
```bash
apt-get install alterator-dhcp alterator-bind alterator-net-iptables -y
```
если насиройки ALT SERVER не сработали задаем ip адрес ens34
```bash
nano /etc/net/ifaces/ens34/ipv4address
192.168.20.1/24
```
```bash
systemctl restart network
reboot
```
```bash
Заходим в браузер и пишем свой ip:8080
root
password
```
Серверы
```bash
DHSP-сервер включить
Начальный ip адрес:192.168.20.10          #Важны последние цифры ip адреса 10-30 например
Конечный ip адрес:192.168.20.20
Домен поиска:8.8.8.8
Шлюз по умолчанию:192.168.1.40      #пример ip машины alt net
Применить
```
Брандмауэр
```bash
Внешние сети
Выберите внешние интерфейсы:ens34
Включить брандмауэр:поставь галочку
Применить
```
Система
```bash
Выключение компьютера
Перезагрузить компьютер сейчас
Применить
```
ALT-CLI
```bash
Настраиваем ALT-server авто-установка задаем время Екатиринбург и дальше как обычно устанавливаем ALT-server
задаем пароль:
root:2222
user:1111
```
Проверка
```bash
ip -c -br a
```
```bash
nano /etc/resolv.conf
nameserver 8.8.8.8
```
```bash
chattr +i /etc/resolv.conf
```
```bash
systemctl restart network
```
```bash
reboot
```
Проверка
```bash
ping <ip>                        #ALT-NET
ping 8.8.8.8
apt-get update
