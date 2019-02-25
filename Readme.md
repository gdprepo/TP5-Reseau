# TP5 - Premier pas dans le monde Cisco

# I. Préparation du lab

## Préparation VMs
```
[root@vm1 ~]# ip route
10.5.1.0/24 dev enp0s3 proto kernel scope link src 10.5.1.0 metric 100
```
```
[root@vm2 ~]# ip route
10.5.2.0/24 dev enp0s3 proto kernel scope link src 10.5.2.10 metric 100
```
```
[root@vm3 ~]# ip route
10.5.2.0/24 dev enp0s3 proto kernel scope link src 10.5.2.11 metric 100

```
# II. Lancement et configuration du lab

### Checklist IP VMs

* Client1:
```
[root@client ~]# hostname
client.1
```
* Client2:
```
[root@client ~]# hostname
client.2
```
* Serveur1:
```
[root@serveur ~]# hostname
serveur.1
```

### Checklist IP Routeurs

**Définition des IP Statiques**

-   R1:
```
r1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.1.254      YES manual up                    up
Ethernet0/1                10.5.12.1       YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down

```
-   R2:
```
r2#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.2.254      YES manual up                    up
Ethernet0/1                10.5.12.2       YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
```

# III. DHCP

## 1. Mise en place du serveur DHCP

1. Renommer la machine

```
[root@client ~]# nano /etc/hostname
hostname dhcp-net2.tp5.b1

```
2. Installer le serveur DHCP
-   éteindre la VM dans GNS3
-   ouvrir VirtualBox
-   ajouter une troisième carte en NAT à la VM
-   démarrer la VM dans VirtualBox
```
[root@dhcp-net2 ~]# yum install -y dhcp
```

3. Configuration du serveur DHCP

Dans le fichier  `sudo nano /etc/dhcp/dhcpd.conf`  de dhcp-net2 :

```
# dhcpd.conf
# option definitions common to all supported networks

option domain-name "tp5.b1";
default-lease-time 600;
max-lease-time 7200;

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.

authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).

log-facility local7;
subnet 10.5.2.0 netmask 255.255.255.0 {
range 10.5.2.50 10.5.2.70;
option domain-name "tp5.b1";
option routers 10.5.2.254;
option broadcast-address 10.5.2.255;
}
```
5. Démarrer le serveur DHCP

```
[root@dhcp-net2 ~]# systemctl start dhcpd
```

Verification:
```
[root@dhcp-net2 ~]# systemctl status dhcpd -l
● dhcpd.service - DHCPv4 Server Daemon
   Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; vendor preset: disabled)
   Active: active (running) since mer. 2019-02-20 17:27:20 CET; 2min 48s ago
     Docs: man:dhcpd(8)
           man:dhcpd.conf(5)
 Main PID: 3881 (dhcpd)
   Status: "Dispatching packets..."
   CGroup: /system.slice/dhcpd.service
           └─3881 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid

févr. 20 17:27:20 dhcp-net2.tp5.b1 dhcpd[3881]: Listening on LPF/eth0/00:50:00:00:05:00/10.5.2.0/24
févr. 20 17:27:20 dhcp-net2.tp5.b1 dhcpd[3881]: Sending on   LPF/eth0/00:50:00:00:05:00/10.5.2.0/24
févr. 20 17:27:20 dhcp-net2.tp5.b1 dhcpd[3881]: Sending on   Socket/fallback/fallback-net
févr. 20 17:27:20 dhcp-net2.tp5.b1 systemd[1]: Started DHCPv4 Server Daemon.
```

6. Faire un test

Sur client1 :

-   IP dynamique  `sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3`

```
NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=dhcp
ONBOOT=yes

```
-   `ip a`:
```
enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:35:66:94 brd ff:ff:ff:ff:ff:ff
    inet 10.5.2.50/24 brd 10.5.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::a00:27ff:fe35:6694/64 scope link
       valid_lft forever preferred_lft forever
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODI5MzA5MTMxLC02OTU3MjgzNDYsMTA3Mj
U2NTg4MSw0NjA0MTk1NzAsLTYzNTcwNDgyLDg3NDQ0NzQ4XX0=

-->
