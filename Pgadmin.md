```
nano /etc/apt/soursec.list.d/alt.list
rpm [p11] http://192.168.1.150 /p11/branch/x86_64 classic
rpm [p11] http://192.168.1.150 /p11/branch/noarch classic
```
```bash
apt-get update && apt-get dist-upgrade -y
```
```bash
apt-get install lamp-server -y
```
```bash
systemctl enable --now httpd2
```
```bash
apt-get install php8.2-mbstring php8.2-sockets php8.2-gd2 php8.2-xmlreader php8.2-pgsql php8.2-ldap php8.2-pdo php8.2-pdo_pgsql -y
```
```bash
service httpd2 restart
```
```bash
apt-get install postgresql17-server postgresql17-contrib -y
```
```bash
/etc/init.d/postgresql initdb
```
```bash
systemctl enable --now postgresql
```
```bash
createuser -U postgres --no-superuser --no-createdb --no-createrole --encrypted --pwprompt PHP 
```
```bash
createdb -U postgres -O PHP phpdb
```
```bash
echo "http://192.168.1.78/db.php" > /var/www/html/db.php
```
```bash
systemctl restart httpd2
```
```bash
apt-get install pgadmin4 -y
```
nano /usr/lib/pgadmin4/config.py   #если что-то пойдет не так
```bash
sed -i "s/DEFAULT_SERVER = '127.0.0.1'/DEFAULT_SERVER = '0.0.0.0'/g" /usr/lib/pgadmin4/config.py
sed -i "s|CONFIG_DATABASE_URI *= *''|CONFIG_DATABASE_URI = 'postgresql://PHP:3@127.0.0.1:5432/phpdb'|g" /usr/lib/pgadmin4/config.py
```
```bash
systemctl restart pgadmin4
```
Заходим в браузер
```bash
http://192.168.1.78:8765/
sample@alt.domain
sample_password
```
Задаем имя wasd
```
localhost
postgres
PHP
3
```



