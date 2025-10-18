# Project 0 - Secure Company Network - Layer 3 Switch Configuration

## Switch Configuration Commands

**Distribution Switch-SW1**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname CORE-SW1
ban motd ^Cloud Compute Limited. Unauthorised Access is Prohibited. All Activity is Logged.^
enable password cisco123
service password-encryption
no ip domain-lookup
ip domain-name cloudcompute.com
username admin password ccompute

do wr

! Config SSH Access
! Enable SSH with 1024-bit keys, set domain (for key generation), and force SSHv2
! Timeout authentication process (default 120)
crypto key generate rsa general-keys modulus 1024 
ip ssh version 2 
ip ssh time-out 60

! Create Standard ACL for MGT VLAN
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 deny any

! Access Security Config
! Console/Privileged Exec
line con 0
password cisco123
exec-timeout 3 0
logg syn
exit
! Secure remote access: disable Telnet, enforce SSH only, and apply timeouts
line vty 0 15
login local 
transport input ssh
exec-timeout 5 0
access-class 1 in
logg syn
exit


do wr

! VLAN Definition
vlan 10
name MGT
exit
vlan 20
name LAN
exit
vlan 50
name WLAN
exit
vlan 70
name VoIP
exit
vlan 90
name SERVER
exit

! Trunk Configuration
int range g1/0/5-10
switchport mode trunk
exit

do wr

!! Task 4 - EtherChannel Configuration
int range g1/0/22-24
channel-group 1 mode active
exit
int port-channel 1
switchport mode trunk
no shut
exit

do wr

! Confirm etherchannel configurations
do sh etherchannel port 

!! Task 5 - IP Address Assignment
! Enable Routing
ip routing
int g1/0/3
no switchport
ip add 10.2.2.1 255.255.255.252
no sh
exit
int g1/0/4
no switchport
ip add 10.2.2.5 255.255.255.252
no sh
exit

do wr

!! Task 6 - HSRP and inter-VLAN routing
!VLAN 10
int vlan 10
no sh
ip add 192.168.10.3 255.255.255.0
!Specify HSRP params for VLAN 10 SVI
standby 10 ip 192.168.10.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 20
int vlan 20
no sh
ip add 172.16.0.3 255.255.0.0
!Specify HSRP params for VLAN 10 SVI
standby 20 ip 172.16.0.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 50
int vlan 50
no sh
ip add 10.20.0.2 255.255.0.0
!Specify HSRP params for VLAN 10 SVI
standby 50 ip 10.20.0.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 90
int vlan 90
no sh
ip add 10.11.11.34 255.255.255.224
!Specify HSRP params for VLAN 10 SVI
standby 90 ip 10.11.11.33
! Does not require ip helper address (static servers)
exit

do wr

! View HSRP
do sh standby brief


!! Task 8 - COnfigure OSPF

! Configure Process ID 10
router ospf 10

! Assign router ID 
router-id 1.1.1.1
!advertise the direct connected networks
network 10.2.2.0 0.0.0.3 area 0
network 10.2.2.4 0.0.0.3 area 0
network 192.168.10.0 0.0.0.255 area 0
network 172.16.0.0 0.0.255.255 area 0
network 10.20.0.0 0.0.255.255 area 0
network 10.11.11.32 0.0.0.31 area 0
exit

do wr
```

**Distribution Switch-SW2**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname CORE-SW2
ban motd ^Cloud Compute Limited. Unauthorised Access is Prohibited. All Activity is Logged.^
enable password cisco123
service password-encryption
no ip domain-lookup
ip domain-name cloudcompute.com
username admin password ccompute

do wr

! Config SSH Access
! Enable SSH with 1024-bit keys, set domain (for key generation), and force SSHv2
! Timeout authentication process (default 120)
crypto key generate rsa general-keys modulus 1024 
ip ssh version 2 
ip ssh time-out 60

! Create Standard ACL for MGT VLAN
access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 deny any

! Access Security Config
! Console/Privileged Exec
line con 0
password cisco123
exec-timeout 3 0
logg syn
exit
! Secure remote access: disable Telnet, enforce SSH only, and apply timeouts
line vty 0 15
login local 
transport input ssh
exec-timeout 5 0
access-class 1 in
logg syn
exit

do wr

! VLAN Definition
vlan 10
name MGT
exit
vlan 20
name LAN
exit
vlan 50
name WLAN
exit
vlan 70
name VoIP
exit
vlan 90
name SERVER
exit

! Trunk Configuration
int range g1/0/5-10
switchport mode trunk
exit

do wr

!! Task 4 - EtherChannel Configuration
int range g1/0/22-24
channel-group 1 mode passive
exit
int port-channel 1
switchport mode trunk
no shut
exit

do wr

! Confirm etherchannel configurations
do sh etherchannel port 


!! Task 5 - IP Address Assignment
! Enable Routing
ip routing
int g1/0/3
no switchport
ip add 10.2.2.13 255.255.255.252
no sh
exit
int g1/0/4
no switchport
ip add 10.2.2.9 255.255.255.252
no sh
exit

do wr

!! Task 6 - HSRP and inter-VLAN routing
!VLAN 10
int vlan 10
no sh
ip add 192.168.10.2 255.255.255.0
!Specify HSRP params for VLAN 10 SVI
standby 10 ip 192.168.10.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 20
int vlan 20
no sh
ip add 172.16.0.2 255.255.0.0
!Specify HSRP params for VLAN 10 SVI
standby 20 ip 172.16.0.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 50
int vlan 50
no sh
ip add 10.20.0.3 255.255.0.0
!Specify HSRP params for VLAN 10 SVI
standby 50 ip 10.20.0.1
!Config IP DHCP Relay Helper
ip helper-address 10.11.11.38
exit

!VLAN 90
int vlan 90
no sh
ip add 10.11.11.35 255.255.255.224
!Specify HSRP params for VLAN 10 SVI
standby 90 ip 10.11.11.33
! Does not require ip helper address (static servers)
exit

do wr

! View HSRP
do sh standby brief



!! Task 8 - COnfigure OSPF

! Configure Process ID 10
router ospf 10

! Assign router ID 
router-id 2.2.2.2
!advertise the direct connected networks
network 10.2.2.8 0.0.0.3 area 0
network 10.2.2.12 0.0.0.3 area 0
network 192.168.10.0 0.0.0.255 area 0
network 172.16.0.0 0.0.255.255 area 0
network 10.20.0.0 0.0.255.255 area 0
network 10.11.11.32 0.0.0.31 area 0
exit

do wr
```
