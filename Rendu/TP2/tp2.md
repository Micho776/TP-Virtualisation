# TP2 : + de réseau + de hax

## **Part1 : Network Setup**

```bash
PC1> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
PC1    10.2.1.11/24         10.2.1.254        00:50:79:66:68:00  20028  127.0.0.1:20029
       fe80::250:79ff:fe66:6800/64

PC1> ping 10.2.1.12

84 bytes from 10.2.1.12 icmp_seq=1 ttl=64 time=1.275 ms
PC1> ping 10.2.2.11

84 bytes from 10.2.2.11 icmp_seq=1 ttl=63 time=18.412 ms

VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.2.2.11/24         10.2.2.254        00:50:79:66:68:02  20034  127.0.0.1:20035
       fe80::250:79ff:fe66:6802/64

VPCS> ping 10.2.2.12

84 bytes from 10.2.2.12 icmp_seq=1 ttl=64 time=0.755 ms
84 bytes from 10.2.2.12 icmp_seq=2 ttl=64 time=1.216 ms

VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.2.2.12/24         10.2.2.254        00:50:79:66:68:03  20030  127.0.0.1:20031
       fe80::250:79ff:fe66:6803/64

VPCS> ping 10.2.1.12

84 bytes from 10.2.1.12 icmp_seq=1 ttl=63 time=30.633 ms
84 bytes from 10.2.1.12 icmp_seq=2 ttl=63 time=20.047 ms
```

## **Part2 : C'est mieux avec internet**

### **1. Accès internet routeur**

🌞 **Prouver que...**

```bash
r1.tp2.efrei#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/19/28 ms
```

### **3. Vrai accès internet clients**

```bash
PC1> ip dns 1.1.1.1 PC1> ping efrei.fr efrei.fr resolved to 51.210.229.203 84
bytes from 51.210.229.203 icmp_seq=1 ttl=253 time=40.057 ms 84 bytes from
51.210.229.203 icmp_seq=2 ttl=253 time=39.501 ms 84 bytes from 51.210.229.203
icmp_seq=3 ttl=253 time=34.633 ms ^C PC1> show NAME IP/MASK GATEWAY MAC LPORT
RHOST:PORT PC1 10.2.1.11/24 10.2.1.254 00:50:79:66:68:00 20018 127.0.0.1:20019
fe80::250:79ff:fe66:6800/64
```

### **4. DHCP again**

🌞 **Test test test** : ajouter un nouveau VPCS au LAN1, le bro `node5.tp2.efrei`

```bash
VPCS> dhcp DDORA IP 10.2.1.164/24 GW 10.2.1.254 VPCS> show NAME IP/MASK GATEWAY
MAC LPORT RHOST:PORT VPCS1 10.2.1.164/24 10.2.1.254 00:50:79:66:68:04 20031
127.0.0.1:20032 fe80::250:79ff:fe66:6804/64 VPCS> ping efrei.fr efrei.fr
resolved to 51.210.229.203 84 bytes from 51.210.229.203 icmp_seq=1 ttl=253
time=50.887 ms 84 bytes from 51.210.229.203 icmp_seq=2 ttl=253 time=36.424 ms
```

## **Part3 : Time to attack all this**

### **4. DHCP spoofing**

```jsx
VPCS> show

NAME   IP/MASK              GATEWAY           MAC                LPORT  RHOST:PORT
VPCS1  10.2.1.236/24        10.2.1.178        00:50:79:66:68:06  20031  127.0.0.1:20032
       fe80::250:79ff:fe66:6806/64

VPCS> dhcp -r
DORA IP 10.2.1.236/24 GW 10.2.1.178

VPCS> ping efrei.fr
efrei.fr resolved to 51.210.229.203

84 bytes from 51.210.229.203 icmp_seq=1 ttl=253 time=30.161 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=253 time=39.882 ms
84 bytes from 51.210.229.203 icmp_seq=3 ttl=253 time=27.344 ms
84 bytes from 51.210.229.203 icmp_seq=4 ttl=253 time=40.374 ms
84 bytes from 51.210.229.203 icmp_seq=5 ttl=253 time=26.589 ms
```

## **Part4 : Alors koa c tou ? On refé just la mem choz ke o tp1 enfet enfet ? Bah non**

### **A. Setup**

🌞 **Préparer le DNS spoof**

```bash
[miche@dhcpspoof ~]$ cat /tmp/spoofed_hosts
10.2.1.67 efrei.fr
```

```bash
[miche@dhcpspoof ~]$ cat /tmp/dnsmasq.conf
interface=enp0s3
listen-address=10.2.1.67
port=53
no-hosts
addn-hosts=/tmp/spoofed_hosts
no-resolv
server=1.1.1.1
server=8.8.8.8
```

### B. Vérification

🌞 **S'assurer que c'est up & running**, on en profite pour réviser un peu de shell

```html
[miche@dhcpspoof tmp]$ jobs [1]+ Done sudo dnsmasq -C /tmp/dnsmasq.conf -q
[miche@dhcpspoof tmp]$ ps -ef | grep dnsmasq dnsmasq 1752 1 0 14:59 ? 00:00:00
dnsmasq -C /tmp/dnsmasq.conf -q miche 1754 1448 0 14:59 pts/0 00:00:00 grep
--color=auto dnsmasq [miche@dhcpspoof tmp]$ sudo ss -lnpu | grep dnsmasq UNCONN
0 0 0.0.0.0:53 0.0.0.0:* users:(("dnsmasq",pid=1752,fd=4)) UNCONN 0 0 [::]:53
[::]:* users:(("dnsmasq",pid=1752,fd=6))
```

```bash
[miche@dhcpspoof tmp]$ dig @1.1.1.1 efrei.fr

; <<>> DiG 9.18.33 <<>> @1.1.1.1 efrei.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31120
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;efrei.fr.			IN	A

;; ANSWER SECTION:
efrei.fr.		1220	IN	A	51.210.229.203

;; Query time: 45 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Thu Jan 08 15:18:35 CET 2026
;; MSG SIZE  rcvd: 53
```

```bash
[miche@dhcpspoof tmp]$ dig @127.0.0.1 efrei.fr

; <<>> DiG 9.18.33 <<>> @127.0.0.1 efrei.fr
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 24543
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;efrei.fr.			IN	A

;; ANSWER SECTION:
efrei.fr.		0	IN	A	10.2.1.67

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1) (UDP)
;; WHEN: Thu Jan 08 15:19:01 CET 2026
;; MSG SIZE  rcvd: 53
```

### **C. Hax ?**

🌞 **Relance ton attaque DHCP spoof** depuis la machine attaquante

```bash
dhcp-option=6,10.2.1.67
```

🌞 **Test test test** : ajouter un nouveau VPCS au LAN1, le bro `node7.tp2.efrei`

```bash
VPCS> dhcp
DORA IP 10.2.1.235/24 GW 10.2.1.178

VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.2.1.235/24
GATEWAY     : 10.2.1.178
DNS         : 10.2.1.67
DHCP SERVER : 10.2.1.178
DHCP LEASE  : 43199, 43200/21600/37800
MAC         : 00:50:79:66:68:06
LPORT       : 20043
RHOST:PORT  : 127.0.0.1:20044
MTU         : 1500

VPCS> ping efrei.fr
efrei.fr resolved to 10.2.1.67

84 bytes from 10.2.1.67 icmp_seq=1 ttl=64 time=0.996 ms
84 bytes from 10.2.1.67 icmp_seq=2 ttl=64 time=1.567 ms
```
