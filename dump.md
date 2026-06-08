```
nano /etc/apt/sources.list.d/alt.list
rpm [p11] http://192.168.1.150 /p11/branch/x86_64 classic
rpm [p11] http://192.168.1.150 /p11/branch/noarch classic
```
```bash
nano /etc/sudoers
```
```bash
apt-get update && apt-get dist-upgrade -y
```
```bash
apt-get update && apt-get install apache2 mariadb php8.2 apache2-mod_php8.2 php8.2-mysqli -y
```
```bash
systemctl enable --now httpd2 mariadb
systemctl restart httpd2 mariadb
```
```bash
mkdir /mnt/add_cd
```
```bash
mount /dev/sr0 /mnt/add_cd
```
```bash
ls -la /mnt/add_cd
```
```bash
cp -r /mnt/add_cd/web /root/web
ls -la /root/web
```
```bash
CREATE DATABASE `shark`;
```
```bash
CREATE USER 'fish'@'localhost' IDENTIFIED BY '123';
```
```bash
USE shark; SHOW TABLES;
```
```bash
GRANT ALL PRIVILEGES ON `shark` .* TO 'fish'@'localhost';
```
```bash
FLUSH PRIVILEGES;
```
```bash
SELECT user, host FROM mysql.user;
```
```bash
iconv -f UTF-16 -t UTF-8 /root/web/dump.sql -o /root/web/dump-utf8.sql
```
```bash
mysql -u root -p shark < /root/web/dump-utf8.sql
```
```bash
cp /root/web/index.php /var/www/html/
cp /root/web/logo.png /var/www/html/
```
```bash
chown -R apache2:webmaster /var/www/html/
chmod 755 /var/www/html/
```
```bash
nano /var/www/html/index.php
$username = "fish";
$password = "123";
$dbname = "shark";
```
```bash
mv /var/www/html/index.html /var/www/html/index.html.default
ls -la /var/www/html/
```
```bash
systemctl restart httpd2 mariadb
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
```
```bash
