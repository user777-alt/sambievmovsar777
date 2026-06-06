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
parted -l
```
```bash
lsblk
```
```bash
parted
```
```bash
select /dev/sdb
```
```bash
mklabel msdos
mklabel gpt
```
```bash
unit s
```
```bash
mkpart primary ext4 0% 100%
```
```bash
parted /dev/sdb/ align-check optimal 1
```
```bash
mdadm --create --verbose --level=0 --raid-devices=2 /dev/md0 /dev/sdb1 /dev/sdc1
```
```bash
mdadm --detail --scan >> /etc/mdadm.conf
```
```bash
nano /etc/mdadm.conf
```
```bash
mdadm --assemble --scan
```
```bash
mkfs.ext4 /dev/md0
```
```bash
cat /proc/mdstat
```
```bash
blkid
```
```bash
mkdir /mnt/data
```
```bash
nano /etc/fstab
UUID=d401afb0-21c6-4cb6-aac9-686f506c6c0a       /	ext4	default       0       0	#tab
```
```bash
mount -all
```
```bash
mdadm --detail /dev/md0
```
```bash
