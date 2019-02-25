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
routeur1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.1.254      YES manual up                    up
Ethernet0/1                10.5.12.1       YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down

```
-   R2:
```
routeur2#show ip int br
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

4. Configuration du serveur DHCP

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
6. 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc3NzU5ODY3LC02OTU3MjgzNDYsMTA3Mj
U2NTg4MSw0NjA0MTk1NzAsLTYzNTcwNDgyLDg3NDQ0NzQ4XX0=

-->