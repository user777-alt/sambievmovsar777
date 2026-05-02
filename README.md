# ISP
```
hostnamectl set-hostname isp.au-team.irpo; exec bash
```
```bash
mkdir /etc/net/ifaces/enp7s2 /etc/net/ifaces/enp7s3
```
```bash
cat > /etc/net/ifaces/enp7s2/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
cat > /etc/net/ifaces/enp7s3/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
echo "172.16.1.1/25" > /etc/net/ifaces/enp7s2/ipv4address
```
```bash
echo "172.16.2.1/25" > /etc/net/ifaces/enp7s3/ipv4address
```
```bash
systemctl restart network
```
```bash
ip -c -br a
```
Должен быть такой вывод:
```bash
lo               UNKNOWN        127.0.0.1/8 ::1/128 
enp7s1           UP             192.168.120.157/24 fe80::be24:11ff:fe74:fa7/64 
enp7s2           UP             172.16.1.1/28 fe80::be24:11ff:fed1:a8dc/64 
enp7s3           UP             172.16.2.1/28 fe80::be24:11ff:fed6:e399/64
```
```bash
sed -i 's/net.ipv4.ip_forward = 0/net.ipv4.ip_forward = 1/' /etc/net/sysctl.conf
```
```bash
sysctl -p
systemctl restart network
```
```bash
apt-get update && apt-get install -y iptables tzdata

iptables -t nat -A POSTROUTING -o enp7s1 -s 172.16.1.0/28 -j MASQUERADE
iptables -t nat -A POSTROUTING -o enp7s1 -s 172.16.2.0/28 -j MASQUERADE

iptables -A FORWARD -i ens19 -o enp7s1 -s 172.16.1.0/28 -j ACCEPT
iptables -A FORWARD -i ens20 -o enp7s1 -s 172.16.2.0/28 -j ACCEPT

iptables-save > /etc/sysconfig/iptables
systemctl enable iptables --now
systemctl restart iptables
```
```bash
systemctl status iptables
iptables -t nat -L -n -v
```
```bash
timedatectl set-timezone Asia/Yekaterinburg
```
# HQ-RTR
```bash
hostnamectl set-hostname hq-rtr.au-team.irpo; exec bash
```
```bash
mkdir /etc/net/ifaces/enp7s2
mkdir /etc/net/ifaces/enp7s2.100
mkdir /etc/net/ifaces/enp7s2.200
mkdir /etc/net/ifaces/enp7s2.999
```
```bash
cat > /etc/net/ifaces/enp7s1/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
cat > /etc/net/ifaces/enp7s2/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
echo "172.16.1.2/25" > /etc/net/ifaces/enp7s1/ipv4address
```
```bash
cat > /etc/net/ifaces/enp7s1/ipv4route << EOF
default via 172.16.1.1
EOF
```
```bash
cat > /etc/net/ifaces/enp7s1/resolv.conf << EOF
nameserver 9.9.9.9
EOF
```
```bash
cat > /etc/net/ifaces/enp7s2.100/options << EOF
BOOTPROTO=static
TYPE=vlan
VID=100
HOST=enp7s2
EOF
```
```bash
echo "192.168.100.1/25" > /etc/net/ifaces/enp7s2.100/ipv4address
```
```bash
cat > /etc/net/ifaces/enp7s2.200/options << EOF
BOOTPROTO=static
TYPE=vlan
VID=200
HOST=enp7s2
EOF
```
```bash
echo "192.168.200.65/25" > /etc/net/ifaces/enp7s2.200/ipv4address 
```
```bash
cat > /etc/net/ifaces/enp7s2.999/options << EOF
BOOTPROTO=static
TYPE=vlan
VID=999
HOST=enp7s2
EOF
```
```bash
echo "192.168.99.89/25" > /etc/net/ifaces/enp7s2.999/ipv4address
```
```bash
systemctl restart network
```
```bash
ip -c -br a
```
Должен быть такой вывод:
```bash
lo               UNKNOWN        127.0.0.1/8 ::1/128 
enp7s1           UP             172.16.1.2/28 fe80::be24:11ff:feda:daba/64 
enp7s2           UP             fe80::be24:11ff:feae:ad50/64 
enp7s2.100@enp7s2 UP             192.168.100.1/27 fe80::be24:11ff:feae:ad50/64 
enp7s2.200@enp7s2 UP             192.168.200.65/28 fe80::be24:11ff:feae:ad50/64 
enp7s2.999@enp7s2 UP             192.168.99.89/29 fe80::be24:11ff:feae:ad50/64
```
```bash
sed -i 's/net.ipv4.ip_forward = 0/net.ipv4.ip_forward = 1/' /etc/net/sysctl.conf
```
```bash
sysctl -p
systemctl restart network
```
```bash
apt-get update && apt-get install -y iptables tzdata 

iptables -t nat -A POSTROUTING -o enp7s1 -s 192.168.100.0/27 -j MASQUERADE
iptables -t nat -A POSTROUTING -o enp7s1 -s 192.168.200.64/28 -j MASQUERADE
iptables -t nat -A POSTROUTING -o enp7s1 -s 192.168.99.88/29 -j MASQUERADE

iptables -A FORWARD -i ens19.10 -o enp7s1 -s 192.168.100.0/27 -j ACCEPT
iptables -A FORWARD -i ens19.20 -o enp7s1 -s 192.168.200.64/28 -j ACCEPT
iptables -A FORWARD -i ens19.99 -o enp7s1 -s 192.168.99.88/29 -j ACCEPT

iptables-save > /etc/sysconfig/iptables
systemctl enable iptables --now
systemctl restart iptables
```
```bash
timedatectl set-timezone Asia/Yekaterinburg
```
```bash
apt-get update && apt-get install sudo -y
```
```bash
useradd net_admin
passwd net_admin
```
```bash
usermod -a -G wheel net_admin
```
```bash
vim /etc/sudoers
## Same thing without a password
# WHEEL_USERS ALL=(ALL:ALL) NOPASSWD: ALL
net_admin ALL=(ALL) NOPASSWD: ALL
```
```bash
mkdir /etc/net/ifaces/gre1
```
```bash
cat > /etc/net/ifaces/gre1/options << EOF
TYPE=iptun
TUNTYPE=gre
TUNLOCAL=172.16.1.2
TUNREMOTE=172.16.2.2
TUNOPTIONS='ttl 64'
EOF
```
```bash
echo "10.10.0.1/25" > /etc/net/ifaces/gre1/ipv4address
```
```bash
systemctl restart network
ip -c -br a
```
```bash
apt-get update && apt-get install frr -y
```
```bash
vim /etc/frr/daemons
ospfd=yes
```
```bash
systemctl enable --now frr
systemctl restart frr
reboot
```
```bash
vtysh
show run
```
```bash
conf t
router ospf
ospf router-id 172.16.1.1
network 10.10.0.0/30 area 0
network 192.168.100.0/27 area 0
network 192.168.200.64/28 area 0
network 192.168.99.88/29 area 0
area 0 authentication
exit
interface gre1
ip ospf authentication-key P@ssw0rd
ip ospf authentication
no ip ospf passive
exit
exit
wr
```
```bash
show run
```
Содержание конфигурации FFR после настройки
```bash
Current configuration:
!
frr version 9.0.2
frr defaults traditional
hostname hq-rtr.au-team.irpo
log file /var/log/frr/frr.log
no ipv6 forwarding
!
interface gre1
 ip ospf authentication
 ip ospf authentication-key P@ssw0rd
 ip ospf network broadcast
 no ip ospf passive
exit
!
router ospf
 ospf router-id 172.16.1.1
 network 10.10.0.0/30 area 0
 network 192.168.99.88/29 area 0
 network 192.168.100.0/27 area 0
 network 192.168.200.64/28 area 0
 area 0 authentication
exit
!
end
```
проверка
```bash
hq-rtr.au-team.irpo# show ip ospf neighbor
```
```bash
apt-get update && apt-get install dhcp-server nano -y
```
```bash
nano /etc/dhcp/dhcpd.conf
subnet 192.168.200.64 netmask 255.255.255.240 {
        option routers                  192.168.200.65;
        option subnet-mask              255.255.255.240;

        option domain-name              "au-team.irpo";
        option domain-name-servers      192.168.100.2;

        range dynamic-bootp 192.168.200.66 192.168.200.78;
        default-lease-time 600;
        max-lease-time 7200;
}
```
```bash
systemctl enable --now dhcpd
systemctl restart dhcpd
```
```bash
systemctl status dhcpd
```
```bash
vim /etc/net/ifaces/enp7s1/resolv.conf
nameserver 192.168.100.2
```
```bash
systemctl restart network
systemctl restart dhcpd
