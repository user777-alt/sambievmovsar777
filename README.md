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
apt-get update && apt-get install -y iptables tzdata sudo frr 

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
sed -i 's/ospfd=no/ospfd=yes' /etc/frr/daemons
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
```
# HQ-SRV
```bash
hostnamectl set-hostname hq-srv.au-team.irpo; exec bash
```
```bash
cat > /etc/net/ifaces/enp7s1/options <<EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
echo "192.168.100.2/25" > /etc/net/ifaces/enp7s1/ipv4address 
```
```bash
echo "default via 192.168.100.1" > /etc/net/ifaces/enp7s1/ipv4route
```
```bash
echo "nameserver 9.9.9.9" > /etc/net/ifaces/enp7s1/resolv.conf
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
enp7s1           UP             192.168.100.2/25 fe80::be24:11ff:fef0:121/64 
```
```bash
timedatectl set-timezone Asia/Yekaterinburg
```
```bash
useradd sshuser -u 2026 -U
passwd sshuser
```
```bash
usermod -a -G wheel sshuser
```
```bash
vim /etc/sudoers
## Same thing without a password
# WHEEL_USERS ALL=(ALL:ALL) NOPASSWD: ALL
sshuser ALL=(ALL) NOPASSWD: ALL
```
```bash
apt-get update && apt-get install -y openssh-server nano
```
```bash
cat > /etc/openssh/sshd_config << EOF
Port 2026
MaxAuthTries 2
Banner /etc/openssh/sshd_banner
AllowUsers sshuser
EOF
```
```bash
echo "«Authorized access only»" > /etc/openssh/sshd_banner
```
```bash
systemctl enable sshd --now
systemctl restart sshd

systemctl status sshd
```
```bash
ssh sshuser@localhost -p 2026
```
```bash
nano /etc/bind/options.conf
listen-on { any; };
forward first;
forwarders {9.9.9.9; };
allow-query { any; };
```
```bash
nano /etc/bind/local.conf
// Add other zones here
// Зона прямого просмотра (A-записи)
zone "au-team.irpo" {
    type master;
    file "/etc/bind/db.au-team.irpo";
};

// Зона обратного просмотра для сети 192.168.100.0
zone "100.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.100";
};

// Зона обратного просмотра для сети 192.168.200.64
zone "64.200.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.200.64";
};
```
```bash
nano /etc/bind/db.au-team.irpo
$TTL 86400
@   IN  SOA hq-srv.au-team.irpo. root.au-team.irpo. (
        2025121001 ; serial
        3600       ; refresh
        1800       ; retry
        604800     ; expire
        86400 )    ; minimum

    IN  NS  hq-srv.au-team.irpo.

hq-rtr   IN  A   192.168.100.1
br-rtr   IN  A   192.168.3.1
hq-srv   IN  A   192.168.100.2
hq-cli   IN  A   192.168.200.66
br-srv   IN  A   192.168.3.2

docker  IN   A   172.16.1.1
web     IN   A   172.16.2.1
```
```bash
nano /etc/bind/db.192.168.100
$TTL 86400
@   IN  SOA hq-srv.au-team.irpo. root.au-team.irpo. (
        2025121001
        3600
        1800
        604800
        86400 )

    IN  NS  hq-srv.au-team.irpo.

1   IN  PTR  hq-rtr.au-team.irpo.
2   IN  PTR  hq-srv.au-team.irpo.
```
```bash
nano /etc/bind/db.192.168.200.64
$TTL 86400
@   IN  SOA hq-srv.au-team.irpo. root.au-team.irpo. (
        2025121001
        3600
        1800
        604800
        86400 )

    IN  NS  hq-srv.au-team.irpo.

1  IN  PTR  hq-cli.au-team.irpo.
```
```bash
rm -rf /etc/net/ifaces/enp7s1/resolv.conf 
systemctl restart network
```
```bash
nano /etc/resolvconf.conf
name_servers=127.0.0.1
```
```bash
resolvconf -u
systemctl restart network
```
```bash
cat /etc/resolv.conf | grep nameserver
```
```bash
systemctl enable --now bind
systemctl restart bind
```
```bash
systemctl status bind
```
# HQ-CLI
```bash
hostnamectl set-hostname hq-cli.au-team.irpo; exec bash
```
```bash
timedatectl set-timezone Asia/Yekaterinburg
```
```bash
dhcpcd
```
```bash
ip -c -br a
```
# BR-RTR
```bash
mkdir /etc/net/ifaces/enp7s2/
```
```bash
cat > /etc/net/ifaces/enp7s2/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
cat > /etc/net/ifaces/enp7s1/options << EOF
BOOTPROTO=static
TYPE=eth
EOF
```
```bash
echo "192.168.3.1/25" > /etc/net/ifaces/enp7s2/ipv4address
```
```bash
echo "default via 172.16.2.1" > /etc/net/ifaces/enp7s1/ipv4route
```
```bash
echo "nameserver 9.9.9.9" > /etc/net/ifaces/enp7s1/resolv.conf
```
```bash
systemctl restart network
```
```bash
ip -c -br a
```
```bash
sed -i 's/net.ipv4.ip_forward = 0/net.ipv4.ip_forward = 1/' /etc/net/sysctl.conf
```
```bash
sysctl -p
systemctl restart network
```
```bash
apt-get update && apt-get install -y tzdata iptables sudo frr 
```
```bash
iptables -t nat -A POSTROUTING -o enp7s1 -s 192.168.3.0/28 -j MASQUERADE
iptables -A FORWARD -i ens19 -o enp7s1 -s 192.168.3.0/28 -j ACCEPT

iptables-save > /etc/sysconfig/iptables

systemctl enable iptables --now
systemctl restart iptables
```
```bash
systemctl status iptables
iptables -t nat -L -n -v
```
```bash
useradd net_admin
passwd net_admin 
```
```bash
usermod -a -G wheel net_admin
```
```bash 
echo > "net_admin ALL=(ALL) NOPASSWD: ALL" /etc/sudoers
```
```bash
mkdir /etc/net/ifaces/gre1
```
```bash
cat > /etc/net/ifaces/gre1/options << EOF
TYPE=iptun
TUNTYPE=gre
TUNLOCAL=172.16.2.2
TUNREMOTE=172.16.1.2
TUNOPTIONS='ttl 64'
EOF
```
```bash
echo "10.10.0.2/25" > /etc/net/ifaces/gre1/ipv4address
```
```bash
systemctl restart network
ip -c -br a
```
```bash
sed -i 's/ospfd=no/ospfd=yes' /etc/frr/daemons
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
ospf router-id 172.16.2.1
network 10.10.0.0/30 area 0
network 192.168.3.0/28 area 0
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
```bash
br-rtr.au-team.irpo# show ip ospf neighbor
```
```bash
vim /etc/net/ifaces/enp7s1/resolv.conf
nameserver 192.168.100.2
```
```bash
systemctl restart network
```
# BR-SRV
```bash
hostnamectl set-hostname br-srv.au-team.irpo; exec bash
```
```bash
vim /etc/net/ifaces/enp7s1/options
BOOTPROTO=static
TYPE=eth
```
```bash
echo "192.168.3.2/25" > /etc/net/ifaces/enp7s1/ipv4address
```
```bash
echo "default via 192.168.3.1" > /etc/net/ifaces/enp7s1/ipv4route
```
```bash
echo "nameserver 9.9.9.9" > /etc/net/ifaces/enp7s1/resolv.conf
```
```bash
systemctl restart network
```
```bash
timedatectl set-timezone Asia/Yekaterinburg
```
```bash
useradd sshuser -u 2026 -U
passwd sshuser
```
```bash
usermod -a -G wheel sshuser
```
```bash
vim /etc/sudoers
## Same thing without a password
# WHEEL_USERS ALL=(ALL:ALL) NOPASSWD: ALL
sshuser ALL=(ALL) NOPASSWD: ALL
```
```bash
apt-get update && apt-get install openssh-server -y
```
```bash
cat < /etc/openssh/sshd_config << EOF
Port 2026
MaxAuthTries 2
Banner /etc/openssh/sshd_banner
AllowUsers sshuser
EOF
```
```bash
echo "«Authorized access only»" > /etc/openssh/sshd_banner
```
```bash
systemctl enable sshd --now
systemctl restart sshd

systemctl status sshd
```
```bash
vim /etc/net/ifaces/enp7s1/resolv.conf
nameserver 192.168.100.2
```
```bash
systemctl restart network
```
### HQ-RTR,HQ-CLI,HQ-SRV,BR-RTR,BR-SRV
```bash
ping hq-rtr.au-team.irpo
ping hq-cli.au-team.irpo
ping hq-srv.au-team.irpo
ping br-rtr.au-team.irpo
ping br-srv.au-team.irpo
ping web.au-team.irpo
ping docker.au-team.irpo
```
