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

### 1. Mise en place du serveur DHCP

### 2. Explorer un peu DHCP
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5NTcyODM0NiwxMDcyNTY1ODgxLDQ2MD
QxOTU3MCwtNjM1NzA0ODIsODc0NDQ3NDhdfQ==
-->