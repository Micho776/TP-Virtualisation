# TP2 Part1 : Network Setup

## Ya pas grand choses ici

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

### 📁 p1_routed_ping.pcap

## **TP2 Part2 : C'est mieux avec internet**

### **1. Accès internet routeur**

🌞 **Prouver que...**

```bash
r1.tp2.efrei#ping 1.1.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/19/28 ms
```

+ptite capture wireshark en plus

**3. Vrai accès internet clients**

```bash
PC1> ip dns 1.1.1.1 PC1> ping efrei.fr efrei.fr resolved to 51.210.229.203 84
bytes from 51.210.229.203 icmp_seq=1 ttl=253 time=40.057 ms 84 bytes from
51.210.229.203 icmp_seq=2 ttl=253 time=39.501 ms 84 bytes from 51.210.229.203
icmp_seq=3 ttl=253 time=34.633 ms ^C PC1> show NAME IP/MASK GATEWAY MAC LPORT
RHOST:PORT PC1 10.2.1.11/24 10.2.1.254 00:50:79:66:68:00 20018 127.0.0.1:20019
fe80::250:79ff:fe66:6800/64
```
