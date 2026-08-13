HSRP Configuration Snippet

! ========================================
! HQ-R1 (Active Router)
! ========================================
hostname HQ-R1
!
interface GigabitEthernet0/1
 ip address 192.168.199.1 255.255.255.0
 standby 1 ip 192.168.199.254
 standby 1 priority 110
 standby 1 preempt
 standby 1 track GigabitEthernet0/0 20
!
track 1 interface GigabitEthernet0/0 line-protocol
!
ip route 192.168.110.0 255.255.255.0 192.168.199.10
ip route 192.168.20.0 255.255.255.0 192.168.199.20
ip route 0.0.0.0 0.0.0.0 192.168.1.1
!

! ========================================
! HQ-R2 (Standby Router)
! ========================================
hostname HQ-R2
!
interface GigabitEthernet0/1
 ip address 192.168.199.2 255.255.255.0
 standby 1 ip 192.168.199.254
 standby 1 priority 100
 standby 1 preempt
 standby 1 track GigabitEthernet0/0 20
!
track 1 interface GigabitEthernet0/0 line-protocol
!
ip route 192.168.110.0 255.255.255.0 192.168.199.10
ip route 192.168.20.0 255.255.255.0 192.168.199.20
ip route 0.0.0.0 0.0.0.0 192.168.2.1
!




! ========================================
! HQ-R1 (Active)
! ========================================
HQ-R1# show standby brief

Interface   Grp  Prio P State    Active          Standby         Virtual IP
Gi0/1       1    110  P Active   local           192.168.199.2   192.168.199.254

! ========================================
! HQ-R2 (Standby)
! ========================================
HQ-R2# show standby brief

Interface   Grp  Prio P State    Active          Standby         Virtual IP
Gi0/1       1    100  P Standby  192.168.199.1   local           192.168.199.254



! multiGRE Tunnel Interface
! ========================================
interface Tunnel0
 description DMVPN-Hub-Tunnel
 ip address 10.100.0.1 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
! ========================================
! NHRP Configuration (Hub)
! ========================================
 ip nhrp network-id 100
 ip nhrp map multicast dynamic
 ip nhrp authentication DMVPN-KEY
 ip nhrp redirect
!
! ========================================
! EIGRP Configuration
! ========================================
 bandwidth 1000
 delay 1000
!
router eigrp 100
 network 10.100.0.0 0.0.0.255
 network 192.168.199.0 0.0.0.255




!
! ========================================
hostname BR1-R1
! ========================================
interface Tunnel0
 description DMVPN-Spoke-Tunnel
 ip address 10.100.0.2 255.255.255.0
 no ip redirects
 ip mtu 1400
 ip tcp adjust-mss 1360
 tunnel source GigabitEthernet0/0
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
! ========================================
! NHRP Configuration (Spoke)
! ========================================
 ip nhrp network-id 100
 ip nhrp authentication DMVPN-KEY
 ip nhrp nhs 10.100.0.1
 ip nhrp map 10.100.0.1 203.0.113.1
 ip nhrp map multicast 203.0.113.1
 ip nhrp shortcut
!
! ========================================
! EIGRP Configuration
! ========================================
 bandwidth 1000
 delay 1000
!
router eigrp 100
 network 10.100.0.0 0.0.0.255
 network 192.168.110.0 0.0.0.255
!
! ========================================
! Default Route
! ========================================
ip route 0.0.0.0 0.0.0.0 203.0.113.1
!


