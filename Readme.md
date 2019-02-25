---


---

<h1 id="tp5---premier-pas-dans-le-monde-cisco">TP5 - Premier pas dans le monde Cisco</h1>
<h1 id="i.-préparation-du-lab">I. Préparation du lab</h1>
<h2 id="préparation-vms">Préparation VMs</h2>
<pre><code>[root@vm1 ~]# ip route
10.5.1.0/24 dev enp0s3 proto kernel scope link src 10.5.1.0 metric 100
</code></pre>
<pre><code>[root@vm2 ~]# ip route
10.5.2.0/24 dev enp0s3 proto kernel scope link src 10.5.2.10 metric 100
</code></pre>
<pre><code>[root@vm3 ~]# ip route
10.5.2.0/24 dev enp0s3 proto kernel scope link src 10.5.2.11 metric 100

</code></pre>
<h1 id="ii.-lancement-et-configuration-du-lab">II. Lancement et configuration du lab</h1>
<h3 id="checklist-ip-vms">Checklist IP VMs</h3>
<ul>
<li>Client1:</li>
</ul>
<pre><code>[root@client ~]# hostname
client.1
</code></pre>
<ul>
<li>Client2:</li>
</ul>
<pre><code>[root@client ~]# hostname
client.2
</code></pre>
<ul>
<li>Serveur1:</li>
</ul>
<pre><code>[root@serveur ~]# hostname
serveur.1
</code></pre>
<h3 id="checklist-ip-routeurs">Checklist IP Routeurs</h3>
<p><strong>Définition des IP Statiques</strong></p>
<ul>
<li>R1:</li>
</ul>
<pre><code>r1#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.1.254      YES manual up                    up
Ethernet0/1                10.5.12.1       YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down

</code></pre>
<ul>
<li>R2:</li>
</ul>
<pre><code>r2#show ip int br
Interface                  IP-Address      OK? Method Status                Protocol
Ethernet0/0                10.5.2.254      YES manual up                    up
Ethernet0/1                10.5.12.2       YES manual up                    up
Ethernet0/2                unassigned      YES unset  administratively down down
Ethernet0/3                unassigned      YES unset  administratively down down
</code></pre>
<h1 id="iii.-dhcp">III. DHCP</h1>
<h2 id="mise-en-place-du-serveur-dhcp">1. Mise en place du serveur DHCP</h2>
<ol>
<li>Renommer la machine</li>
</ol>
<pre><code>[root@client ~]# nano /etc/hostname
hostname dhcp-net2.tp5.b1

</code></pre>
<ol start="2">
<li>Installer le serveur DHCP</li>
</ol>
<ul>
<li>éteindre la VM dans GNS3</li>
<li>ouvrir VirtualBox</li>
<li>ajouter une troisième carte en NAT à la VM</li>
<li>démarrer la VM dans VirtualBox</li>
</ul>
<pre><code>[root@dhcp-net2 ~]# yum install -y dhcp
</code></pre>
<ol start="3">
<li>Configuration du serveur DHCP</li>
</ol>
<p>Dans le fichier  <code>sudo nano /etc/dhcp/dhcpd.conf</code>  de dhcp-net2 :</p>
<pre><code># dhcpd.conf
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
</code></pre>
<ol start="5">
<li>Démarrer le serveur DHCP</li>
</ol>
<pre><code>[root@dhcp-net2 ~]# systemctl start dhcpd
</code></pre>
<p>Verification:</p>
<pre><code>[root@dhcp-net2 ~]# systemctl status dhcpd -l
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
</code></pre>
<ol start="6">
<li>Faire un test</li>
</ol>
<p>Sur client1 :</p>
<ul>
<li>IP dynamique  <code>sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3</code></li>
</ul>
<pre><code>NAME=enp0s3
DEVICE=enp0s3

BOOTPROTO=dhcp
ONBOOT=yes

</code></pre>
<ul>
<li><code>ip a</code>:</li>
</ul>
<pre><code>enp0s3: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:35:66:94 brd ff:ff:ff:ff:ff:ff
    inet 10.5.2.50/24 brd 10.5.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::a00:27ff:fe35:6694/64 scope link
       valid_lft forever preferred_lft forever
</code></pre>

