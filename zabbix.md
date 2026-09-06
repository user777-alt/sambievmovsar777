```
apt-get install postgresql17-server zabbix-server-pgsql fping postgresql17-server zabbix-common-database-pgsql postgresql17 apache2 apache2-mod_php8.4 php8.4 php8.4-mbstring php8.4-sockets php8.4-gd php8.4-xmlreader php8.4-pgsql php8.4-ldap php8.4-openssl zabbix-phpfrontend-apache2 zabbix-phpfrontend-php8.4 -y
```
```bash
/etc/init.d/postgresql initdb
```
```bash
systemctl enable --now postgresql
```
```bash
su - postgres -s /bin/sh -c 'createuser --no-superuser --no-createdb --no-createrole --encrypted --pwprompt zabbix'
```
```bash
su - postgres -s /bin/sh -c 'createdb -O zabbix zabbix'
```
```bash
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/schema.sql zabbix'
```
```bash
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/images.sql zabbix'
```
```bash
su - postgres -s /bin/sh -c 'psql -U zabbix -f /usr/share/doc/zabbix-common-database-pgsql-*/data.sql zabbix'
```
```bash
nano /var/lib/pgsql/data/postgresql.conf
listen_addresses = '*'         # what IP address(es) to listen on;
```
```bash
nano /var/lib/pgsql/data/pg_hba.conf
host    all             all             127.0.0.1/32            trust
host    all             all             192.168.XXX.YYY/32        md5
```
```bash
systemctl restart postgresql
```
```bash
$ psql -h 192.168.1.1 -U zabbix -d zabbix
```
```bash
systemctl enable --now httpd2
```
```bash
 nano /etc/php/8.4/apache2-mod_php/php.ini
memory_limit = 256M
post_max_size = 32M
max_execution_time = 600
max_input_time = 600
date.timezone = Asia/Yekaterinburg
always_populate_raw_post_data = -1
```
```bash
systemctl restart httpd2
```
```bash
nano /etc/zabbix/zabbix_server.conf
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=22
```
```bash
systemctl enable --now zabbix_pgsql
```
```bash
ln -s /etc/httpd2/conf/addon.d/A.zabbix.conf /etc/httpd2/conf/extra-enabled/
```
```bash
service httpd2 restart
```
```bash
chown apache2:apache2 /var/www/webapps/zabbix/ui/conf
```
```bash
192.168.1.1/zabbix/setup.php
```
```bash
mkdir ens
```
```bash
cd ens
```
```bash
cp options ipv4address ipv4route resolv.conf /etc/net/ifaces/ens
```
```bash
nano options
```
```bash
nano ipv4route
```
```bash
nano ipv4address
```
```bash
systemctl restart network
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



