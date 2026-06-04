```
nano /etc/apt/sources.list.d/alt.list
rpm [p11] http://192.168.1.150 /p11/branch/x86_64 classic
rpm [p11] http://192.168.1.150 /p11/branch/noarch classic
```
```bash
apt-get update && apt-get dist-upgrade -y
```
```bash
update-kernel -y
```
```bash
reboot
```
```bash
apt-get install lamp-server -y
```
```bash
systemctl enable --now httpd2
systemctl enable --now mariadb
systemctl enable --now mysqld
```
```bash
mysql_secure_installation
1.enter
2.n
3.2
4.n
5.n
6.n
7.y
```
```bash
mysql -u root -p
```
```bash
CREATE DATABASE `shark`;
```
```bash
CREATE USER 'fish'@'%' IDENTIFIED BY '123';
```
```bash
GRANT ALL PRIVILEGES ON `shark` .* TO 'fish'@'%';
```
```bash
FLUSH PRIVILEGES;
```
```bash
show variables like 'port';
```
```bash
systemctl restart httpd2
```
```bash
mc /var/www/html
shift+f4
<?php phpinfo(); ?>
index.php
```
```bash
mkdir /var/www/html/shadow
```
```bash

```
```bash

