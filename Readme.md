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

- Nom de domaine
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
### Checklist routes



# III. DHCP

### 1. Mise en place du serveur DHCP

### 2. Explorer un peu DHCP
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYwNDE5NTcwLC02MzU3MDQ4Miw4NzQ0ND
c0OF19
-->
