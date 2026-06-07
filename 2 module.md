# BR-SRV
```
apt-get update && apt-get install -y task-samba-dc alterator-fbi alterator-net-domain admx-* admc gpui openssh-server ansible sshpass nano docker-ce docker-compose
```bash
vim /etc/resolvconf.conf
name_servers=127.0.0.1
name_servers=192.168.1.10
resolvconf -u
systemctl restart network
```
```bash
vim /etc/sysconfig/network
HOSTNAME=br-srv.au-team.irpo
```
```bash
reboot
```
```bash
systemctl enable --now ahttpd alteratord
rm -rf /etc/samba/smb.conf /var/{lib.cache}/samba
mkdir -p /var/lib/samba/sysvol
```
```bash
samba-tool domain provision --realm=au-team.irpo --domain=au-team --adminpass='P@ssw0rd' --dns-backend=BIND9_DLZ --server-role=dc --use-rfc2307 
```
Проверяем систему:
```bash
ping ya.ru
ping br-srv.au-team.irpo
ping hq-rtr.au-team.irpo
```
```bash
samba-tool group add hq
for i in $(seq 1 5); do samba-tool user add user$i.hq 'P@ssw0rd'; done
for i in $(seq 1 5); do samba-tool group addmembers hq user$i.hq; done
```
Проверим наличие группы hq в Samba, и созданных пользователей:
```bash
samba-tool group list
samba-tool group listmembers hq
```
```bash
admx-msi-setup
```
```bash
chronyc sources
```
Пример вывода:
```bash
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* 172.16.1.1                    5   6    77    35   +859ns[  -23us] +/-  342us
```
```bash
nano /etc/ansible/hosts
[Alt]
hq-rtr.au-team.irpo ansible_ssh_user=net_admin ansible_ssh_pass=P@ssw0rd
hq-srv.au-team.irpo ansible_ssh_user=sshuser ansible_ssh_pass=P@ssw0rd
hq-cli.au-team.irpo ansible_ssh_user=sysadmin ansible_ssh_pass=P@ssw0rd
br-rtr.au-team.irpo ansible_ssh_user=net_admin ansible_ssh_pass=P@ssw0rd

[Alt:vars]
ansible_port=2026
```
```bash
nano /etc/ansible/ansible.cfg
[defaults]

interpreter_python = /usr/bin/python3

# some basic default values...
# uncomment this to disable SSH key host checking
host_key_checking = False
```
```bash
ansible -m ping all
```
```bash
ansible -m ping all
```
Вывод команды:
```bash
hq-srv.au-team.irpo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
br-rtr.au-team.irpo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
hq-rtr.au-team.irpo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
hq-cli.au-team.irpo | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
```bash
systemctl enable --now docker.socket docker.service
systemctl restart docker.socket docker.service
```
```bash
mount /dev/sr0 /mnt
```
Проверяем точку монтирования
```bash
lsblk
```
Сверяем выводы
```bash
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0    10G  0 disk 
├─sda1   8:1    0   503M  0 part /var/log
└─sda2   8:2    0   9.5G  0 part /
sr0     11:0    1 929.7M  0 rom  /mnt
```
Проверяем внутри образ
```bash
ls -la /mnt
```
```bash
total 38
dr-xr-xr-x  1 root root   256 Nov 23  2019 .
drwxr-xr-x 24 root root  4096 Dec 11 01:08 ..
dr-xr-xr-x  1 root root   332 Nov 23  2019 docker
dr-xr-xr-x  1 root root   150 Nov 23  2019 playbook
-r-xr-xr-x  1 root root 32527 Oct 13 04:22 Users.csv
dr-xr-xr-x  1 root root   220 Nov 23  2019 web
```
копируем образ в систему
```bash
cp -r /mnt/docker /root/docker
```
проверем файлы
```bash
ls -la /root/docker
```
```bash
total 951964
dr-xr-xr-x 2 root root      4096 Dec 12 22:54 .
drwx------ 8 root root      4096 Dec 12 22:54 ..
-r-xr-xr-x 1 root root 333014016 Dec 12 22:54 mariadb_latest.tar
-r-xr-xr-x 1 root root 282003968 Dec 12 22:54 postgresql_latest.tar
-r-xr-xr-x 1 root root      2716 Dec 12 22:54 readme.txt
-r-xr-xr-x 1 root root 359760896 Dec 12 22:54 site_latest.tar
```
Загружаем докер
```bash
docker load -i /root/docker/site_latest.tar
docker load -i /root/docker/mariadb_latest.tar
```
```bash

```
```bash
```
```bash
```
```bash
```
# HQ-CLI
```bash
apt-get update && apt-get install -y admx-* admc gpui sudo gpupdate nfs-{server,utils} openssh-server 
```
# HQ-SRV
```bash
apt-get update && apt-get install -y nfs-{server,utils} openssh-server apache2 mariadb php8.2 apache2-mod_php8.2 php8.2-mysqli
```
# ISP
```bash
apt-get update && apt-get install -y chrony tzdata nginx nano apache2-htpasswd 
```
# HQ-RTR
```bash
apt-get update && apt-get install openssh-server iptables 
```
# BR-RTR
```bash
apt-get update && apt-get install openssh-server iptables
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
