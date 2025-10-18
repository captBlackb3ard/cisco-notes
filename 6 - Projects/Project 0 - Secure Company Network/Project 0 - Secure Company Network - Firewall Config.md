# Project 0 - Secure Company Network - Firewall Configuration

## FIREWALL Configuration Commands

**PER-FW1**
```
!! Task 9 - Config Firewall Interface, Security Zones, and Levels

conf t
hostname PER-FW1

int g1/3
description Link to CORE-SW1
ip add 10.2.2.2 255.255.255.252
no sh
! Security Zones + Level
nameif INSIDE1
security-level 100
exit

int g1/4
description Link to CORE-SW2
ip add 10.2.2.10 255.255.255.252
no sh
! Security Zones + Level
nameif INSIDE2
security-level 100
exit

int g1/5
description Link to DMZ
ip add 10.11.11.1 255.255.255.224
no sh
! Security Zones + Level
nameif DMZ
security-level 60
exit

int g1/1
description Link to SEACOM-ISP
ip add 105.100.50.2 255.255.255.252
no sh
! Security Zones + Level
nameif OUTSIDE1
security-level 0
exit

int g1/2
description Link to SAFCOM-ISP
ip add 197.200.100.2 255.255.255.252
no sh
! Security Zones + Level
nameif OUTSIDE2
security-level 0
exit

!! Task 10 - Config Firewall Routing-OSPF and Static Routes
! Default Static Route
route OUTSIDE1 0.0.0.0 0.0.0.0 105.100.50.1

! Backup Static Route (Administrative Distance 70)
route OUTSIDE2 0.0.0.0 0.0.0.0 197.200.100.1 70

! OSPF Config
router ospf 10
router-id 6.6.6.6
network 105.100.50.0 255.255.255.252 area 0
network 197.200.100.0 255.255.255.252 area 0
network 10.2.2.0 255.255.255.252 area 0
network 10.2.2.8 255.255.255.252 area 0
network 10.11.11.0 255.255.255.224 area 0


!! Task 11 - Config Firewall Inspection Policy
! Config Object Network & NAT (LAN + WLAN Allowed Internet Access)

object network INSIDE1-OUTSIDE1
subnet 172.16.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE1) dynamic interface

object network INSIDE2-OUTSIDE1
subnet 172.16.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE1) dynamic interface

object network INSIDEw1-OUTSIDEw1
subnet 10.20.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE1) dynamic interface

object network INSIDEw2-OUTSIDEw1
subnet 10.20.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE1) dynamic interface

object network INSIDE1-OUTSIDE2
subnet 172.16.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE2) dynamic interface

object network INSIDE2-OUTSIDE2
subnet 172.16.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE2) dynamic interface

object network INSIDEw1-OUTSIDEw2
subnet 10.20.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE2) dynamic interface

object network INSIDEw2-OUTSIDEw2
subnet 10.20.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE2) dynamic interface

object network DMZ-OUTSIDE1
subnet 10.11.11.0 255.255.255.224
nat (DMZ, OUTSIDE1) dynamic interface

object network DMZ-OUTSIDE2
subnet 10.11.11.0 255.255.255.224
nat (DMZ, OUTSIDE2) dynamic interface
ex

conf t
! Firewall Inspection Policy (ICMP, WEB, DNS)
access-list RES extended permit icmp any any
access-list RES extended permit tcp any any eq 80
access-list RES extended permit tcp any any eq 53
access-list RES extended permit udp any any eq 53
!Bind to Interface (DMZ, ISP1, ISP2)
access-group RES in interface DMZ
access-group RES in interface OUTSIDE1
access-group RES in interface OUTSIDE2

wr mem
```

**PER-FW2**
```
!! Task 9 - Config Firewall Interface, Security Zones, and Levels

conf t
hostname PER-FW2


int g1/3
description Link to CORE-SW1
ip add 10.2.2.14 255.255.255.252
no sh
! Security Zones + Level
nameif INSIDE1
security-level 100
exit

int g1/4
description Link to CORE-SW2
ip add 10.2.2.6 255.255.255.252
no sh
! Security Zones + Level
nameif INSIDE2
security-level 100
exit

int g1/1
description Link to SAFCOM-ISP
ip add 197.200.100.6 255.255.255.252
no sh
! Security Zones + Level
nameif OUTSIDE1
security-level 0
exit

int g1/2
description Link to SEACOM-ISP
ip add 105.100.50.6 255.255.255.252
no sh
! Security Zones + Level
nameif OUTSIDE2
security-level 0
exit


!! Task 10 - Config Firewall Routing-OSPF and Static Routes
! Default Static Route
route OUTSIDE2 0.0.0.0 0.0.0.0 197.200.100.5

! Backup Static Route (Administrative Distance 70)
route OUTSIDE1 0.0.0.0 0.0.0.0 105.100.50.5 70

! OSPF Config
router ospf 10
router-id 7.7.7.7
network 105.100.50.4 255.255.255.252 area 0
network 197.200.100.4 255.255.255.252 area 0
network 10.2.2.12 255.255.255.252 area 0
network 10.2.2.4 255.255.255.252 area 0


!! Task 11 - Config Firewall Inspection Policy

! Config Object Network & NAT (LAN + WLAN Allowed Internet Access)
object network INSIDE1-OUTSIDE1
subnet 172.16.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE1) dynamic interface

object network INSIDE2-OUTSIDE1
subnet 172.16.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE1) dynamic interface

object network INSIDEw1-OUTSIDEw1
subnet 10.20.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE1) dynamic interface

object network INSIDEw2-OUTSIDEw1
subnet 10.20.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE1) dynamic interface

object network INSIDE1-OUTSIDE2
subnet 172.16.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE2) dynamic interface

object network INSIDE2-OUTSIDE2
subnet 172.16.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE2) dynamic interface

object network INSIDEw1-OUTSIDEw2
subnet 10.20.0.0 255.255.0.0
nat (INSIDE1, OUTSIDE2) dynamic interface

object network INSIDEw2-OUTSIDEw2
subnet 10.20.0.0 255.255.0.0
nat (INSIDE2, OUTSIDE2) dynamic interface
ex

conf t
! Firewall Inspection Policy (ICMP, WEB, DNS)
access-list RES extended permit icmp any any
access-list RES extended permit tcp any any eq 80
access-list RES extended permit tcp any any eq 53
access-list RES extended permit udp any any eq 53
!Bind to Interface (DMZ, ISP1, ISP2)
access-group RES in interface OUTSIDE1
access-group RES in interface OUTSIDE2


wr mem
```