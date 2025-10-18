# Project 0 - Secure Company Network - Layer 2 Switch Configuration

## Switch Configuration Commands

**DMZ-SW**
```
!! Task 2 - Basic Configuration

! Basic Config
en
conf t
hostname DMZ-SW
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
```

**MKT-SW** (VTP Server)
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname MKT-SW
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

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard
! VLAN Definition + Assignment
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
vlan 999
name blackhole
exit

int range f0/1, f0/3
switchport mode access
switchport access vlan 20
exit
int f0/2
switchport mode access
switchport voice vlan 70
exit
int f0/24
switchport mode access
switchport access vlan 50
exit
int range f0/4-23
switchport mode access
switchport access vlan 999
shutdown
exit

! Trunk Configuration
int range g0/1-2
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk

do wr

! Config STP Portfast + BPDU Guard
int range f0/1-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr

! Config PortSecurity
int range f0/1, f0/3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown

do wr
exit
do sh port-security
```
**HR-SW**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname HR-SW
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

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard
! VLAN Definition + Assignment
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
vlan 999
name blackhole
exit

int range f0/1, f0/3
switchport mode access
switchport access vlan 20
exit
int f0/2
switchport mode access
switchport voice vlan 70
exit
int f0/24
switchport mode access
switchport access vlan 50
exit
int range f0/4-23
switchport mode access
switchport access vlan 999
shutdown
exit

! Trunk Configuration
int range g0/1-2
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk

do wr


! Config STP Portfast + BPDU Guard
int range f0/1-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr


! Config PortSecurity
int range f0/1, f0/3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown

do wr
exit
do sh port-security
```
**FIN-SW**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname FIN-SW
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

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard
! VLAN Definition + Assignment
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
vlan 999
name blackhole
exit

int range f0/1, f0/3
switchport mode access
switchport access vlan 20
exit
int f0/2
switchport mode access
switchport voice vlan 70
exit
int f0/24
switchport mode access
switchport access vlan 50
exit
int range f0/4-23
switchport mode access
switchport access vlan 999
shutdown
exit

! Trunk Configuration
int range g0/1-2
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk

do wr

! Config STP Portfast + BPDU Guard
int range f0/1-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr

! Config PortSecurity
int range f0/1, f0/3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown

do wr
exit
do sh port-security
```
**PR-SW**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname PR-SW
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

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard
! VLAN Definition + Assignment
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
vlan 999
name blackhole
exit

int range f0/1, f0/3
switchport mode access
switchport access vlan 20
exit
int f0/2
switchport mode access
switchport voice vlan 70
exit
int f0/24
switchport mode access
switchport access vlan 50
exit
int range f0/4-23
switchport mode access
switchport access vlan 999
shutdown
exit

! Trunk Configuration
int range g0/1-2
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk

do wr

! Config STP Portfast + BPDU Guard
int range f0/1-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr

! Config PortSecurity
int range f0/1, f0/3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown

do wr
exit
do sh port-security
```
**IT-SW**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname IT-SW
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

do wr

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard
! VLAN Definition + Assignment
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
vlan 999
name blackhole
exit

int range f0/1, f0/3
switchport mode access
switchport access vlan 20
exit
int f0/2
switchport mode access
switchport voice vlan 70
exit
int f0/24
switchport mode access
switchport access vlan 50
exit
int range f0/4-23
switchport mode access
switchport access vlan 999
shutdown
exit

! Trunk Configuration
int range g0/1-2
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk

do wr

! Config STP Portfast + BPDU Guard
int range f0/1-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr

! Config PortSecurity
int range f0/1, f0/3
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation shutdown

do wr
exit
do sh port-security
```
**SVR-SW**
```
!! Task 2 - Basic Configuration
! Basic Config
en
conf t
hostname SVR-SW
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

!! Task 3 - VLANS, Trunks, Portfast, + BPDUGuard, DHCP Snooping, Port Security
! VLAN Definition + Assignment
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
vlan 999
name blacklist
exit
vlan 90
name SERVER
exit

do wr

int range f0/1-3
switchport mode access
switchport access vlan 90
exit
int range f0/5
switchport mode access
switchport access vlan 50
exit

! Trunk Configuration
int range g0/1-2, f0/4
switchport mode trunk
switchport trunk allowed vlan except 999
exit

do wr

!! View Configs
do sh vlan

do sh int trunk


! Config STP Portfast + BPDU Guard
int range f0/1-3, f0/5-24
spanning-tree portfast
spanning-tree bpduguard enable
exit

do wr


! Config DHCP Snooping
ip dhcp snooping
int f0/1
ip dhcp snooping trust
exit
! Assign IP DHCP Snooping to VLAN 90
ip dhcp snooping vlan 90

do wr

! View configured trusted DHCP VLANS, interfaces/ports
do sh ip dhcp snooping


```