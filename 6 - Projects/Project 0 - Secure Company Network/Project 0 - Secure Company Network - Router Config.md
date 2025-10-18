# Project 0 - Secure Company Network - Router Configuration

## Router Configuration Commands

**CLOUD-R0**
```
!! Task 2 - Basic Configuration

! Router Config
en
conf t
hostname CLOUD-R0
enable password cisco123
service password-encryption
ban motd ^Cloud Router Limited. Unauthorised Access is Prohibited. All Activity is Logged.^
ip domain-name cloudcompute.com

! Access Security Config
! Console/Privileged Exec
line con 0
password cisco123
exec-timeout 3 0
logg syn
exit

do wr

!! Task 5 - IP Address Assignment
!Interface Configuration
int g0/0
description Link to SEACOM-ISP
ip add 20.20.20.2 255.255.255.252
no sh
exit
int g0/1
description Link to SAFCOM-ISP
ip add 30.30.30.2 255.255.255.252
no sh
exit
int g0/2
description Link to Internet-Cloud
ip add 8.0.0.1 255.0.0.0
no sh
exit

do wr

!! Task 8 - COnfigure OSPF
! Configure Process ID 10
router ospf 10

! Assign router ID 
router-id 3.3.3.3
!advertise the direct connected networks
network 20.20.20.0 0.0.0.3 area 0
network 10.10.10.0 0.0.0.3 area 0
network 8.0.0.0 0.255.255.255 area 0
exit

do wr

! View OSPF Neighbours
do sh ip ospf neighbor
```

**SEACOM-ISP**
```
!! Task 2 - Basic Configuration


! Router Config
en
conf t
hostname SEACOM-ISP
enable password cisco123
service password-encryption
ban motd ^SEACOM ISP. Unauthorised Access is Prohibited. All Activity is Logged.^
ip domain-name cloudcompute.com
username admin password password123

! Config SSH Access
! Enable SSH with 1024-bit keys, set domain (for key generation), and force SSHv2
! Timeout authentication process (default 120)
crypto key generate rsa general-keys modulus 1024 
ip ssh version 2 
ip ssh time-out 60

! Access Security Config
! Console/Privileged Exec
line con 0
password cisco123
exec-timeout 3 0
logg syn
exit
! Config VTY SSH Access
line vty 0 15
! Specify authentication based on local db
login local
! Only permit SSH (no Telnet)
transport input ssh
exit

do wr

!! Task 5 - IP Address Assignment
!Interface Configuration
int g0/0
description Link to CLOUD-R0
ip add 20.20.20.1 255.255.255.252
no sh
exit
int g0/1
description Link to PER-FW1
ip add 105.100.50.1 255.255.255.252
no sh
exit
int g0/2
description Link to PER-FW2
ip add 105.100.50.5 255.255.255.252
no sh
exit

do wr

!! Task 8 - COnfigure OSPF
! Configure Process ID 10
router ospf 10

! Assign router ID 
router-id 4.4.4.4
!advertise the direct connected networks
network 20.20.20.0 0.0.0.3 area 0
network 105.100.50.0 0.0.0.3 area 0
network 105.100.50.4 0.0.0.3 area 0
exit

do wr

! View OSPF Neighbours
do sh ip ospf neighbor

```

**SAFCOM-ISP**
```
!! Task 2 - Basic Configuration


! Router Config
en
conf t
hostname SAFCOM-ISP
enable password cisco123
service password-encryption
ban motd ^SAFCOM ISP. Unauthorised Access is Prohibited. All Activity is Logged.^
ip domain-name cloudcompute.com
username admin password password123

! Config SSH Access
! Enable SSH with 1024-bit keys, set domain (for key generation), and force SSHv2
! Timeout authentication process (default 120)
crypto key generate rsa general-keys modulus 1024 
ip ssh version 2 
ip ssh time-out 60

! Access Security Config
! Console/Privileged Exec
line con 0
password cisco123
exec-timeout 3 0
logg syn
exit
! Config VTY SSH Access
line vty 0 15
! Specify authentication based on local db
login local
! Only permit SSH (no Telnet)
transport input ssh
exit

do wr

!! Task 5 - IP Address Assignment
!Interface Configuration
int g0/0
description Link to CLOUD-R0
ip add 30.30.30.1 255.255.255.252
no sh
exit
int g0/1
description Link to PER-FW1
ip add 197.200.100.1 255.255.255.252
no sh
exit
int g0/2
description Link to PER-FW2
ip add 197.200.100.5 255.255.255.252
no sh
exit

do wr


!! Task 8 - COnfigure OSPF
! Configure Process ID 10
router ospf 10

! Assign router ID 
router-id 5.5.5.5
!advertise the direct connected networks
network 30.30.30.0 0.0.0.3 area 0
network 197.200.100.0 0.0.0.3 area 0
network 197.200.100.4 0.0.0.3 area 0
exit

do wr

! View OSPF Neighbours
do sh ip ospf neighbor
```