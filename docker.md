```
nano /etc/apt/sources.list.d/alt.list
rpm [p11] http://192.168.1.150 /p11/branch/x86_64 classic
rpm [p11] http://192.168.1.150 /p11/branch/noarch classic
```
```bash
apt-get update && apt-get dist-upgrade -y
```
```bash
apt-get install docker-engine docker-compose-v2 httpd2 -y
```
```bash
lsblk
```
```bash
mount /dev/sr0 /mnt
```
```bash
ls la /mnt
```
```bash
cp -r /mnt/docker /root/docker
```
```bash
ls -la /root/docker
```
```bash
groupadd docker
```
```bash
usermod -aG docker u
```
```bash
systemctl enable --now docker
systemctl enable --now httpd2
```
```bash
docker load -i /root/docker/mariadb_latest.tar
```
```bash
docker load -i /root/docker/site_latest.tar
```
```bash
docker image ls
```
```bash
scp user@192.168.1.14:/home/user/site.yml /root/docker
```
```bash
cd /root/docker
```
```bash
docker compose -f site.yml up -d
```
```bash
docker compose -f site.yml stop
```
