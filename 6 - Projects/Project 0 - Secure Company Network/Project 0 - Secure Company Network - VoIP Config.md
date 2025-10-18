# Project 0 - Secure Company Network - VoIP Configuration


## VoIP Configuration Commands

**Voice Gateway**
```
!! Task 2 - Basic Configuration

! Router Config
en
conf t
hostname VoIP-R0
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


!! Task 13 - Config VoIP

!Configure Interface
int f0/0
description Link to SVR-Switch
no shut
exit

int fa0/0.70
encapsulation dot1Q 70
ip add 172.30.0.1 255.255.0.0
exit

! Enable DHCP Service for IP Phones
service dhcp

! Config DHCP Pool
ip dhcp pool VoIP-Pool
network 172.30.0.0 255.255.0.0
default-router 172.30.0.1
option 150 ip 172.30.0.1
exit

! Enable telephone Service
telephony-service

! Specify number of phones in the network
max-ephones 30
! Max No of Phones must match the No of Directories
max-dn 30
!Define Gateway
ip source-address 172.30.0.1 port 2000
auto assign 1 to 30
exit


! Create for each phone/directories created
ephone-dn 1
! Specify Line/Dial Number
number 401
exit

ephone-dn 2
number 402
exit

ephone-dn 3
number 403
exit

ephone-dn 4
number 404
exit

ephone-dn 5
number 405
exit

ephone-dn 6
number 406
exit

ephone-dn 7
number 407
exit


do wr
```