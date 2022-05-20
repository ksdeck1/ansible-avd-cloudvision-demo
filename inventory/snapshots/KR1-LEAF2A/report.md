# KR1-LEAF2A Commands Output

## Table of Contents

- [show lldp neighbors](#show-lldp-neighbors)
- [show ip interface brief](#show-ip-interface-brief)
- [show interfaces description](#show-interfaces-description)
- [show version](#show-version)
- [show running-config](#show-running-config)
## show interfaces description

```
Interface                      Status         Protocol           Description
Et1                            up             up                 P2P_LINK_TO_KR1-LEAF1A_Ethernet1
Et2                            up             up                 P2P_LINK_TO_KR1-LEAF1B_Ethernet1
Et3                            up             up                 P2P_LINK_TO_KR1-LEAF2A_Ethernet1
Et4                            up             up                 P2P_LINK_TO_KR1-LEAF2B_Ethernet1
Et5                            up             up                 
Et6                            up             up                 
Et7                            up             up                 
Et8                            up             up                 
Lo0                            up             up                 EVPN_Overlay_Peering
Ma1                            up             up                 oob_management
```
## show ip interface brief

```
Address
Interface       IP Address         Status      Protocol          MTU    Owner  
--------------- ------------------ ----------- ------------- ---------- -------
Ethernet1       unassigned         up          up               1500           
Ethernet2       unassigned         up          up               1500           
Ethernet3       unassigned         up          up               1500           
Ethernet4       unassigned         up          up               1500           
Loopback0       10.100.0.1/32      up          up              65535           
Management1     10.183.0.11/24     up          up               1500
```
## show lldp neighbors

```
Last table change time   : 0:01:59 ago
Number of table inserts  : 4
Number of table deletes  : 0
Number of table drops    : 0
Number of table age-outs : 0

Port          Neighbor Device ID       Neighbor Port ID    TTL
---------- ------------------------ ---------------------- ---
Et1           KR1-LEAF1A               Ethernet1           120
Et2           KR1-LEAF1B               Ethernet1           120
Et3           KR1-LEAF2A               Ethernet1           120
Et4           KR1-LEAF2B               Ethernet1           120
```
## show running-config

```
! Command: show running-config
! device: KR1-SPINE1 (vEOS-lab, EOS-4.27.2F)
!
! boot system flash:/vEOS-lab.swi
!
no aaa root
!
username admin privilege 15 role network-admin secret sha512 $6$Df86J4/SFMDE3/1K$Hef4KstdoxNDaami37cBquTWOTplC.miMPjXVgQxMe92.e5wxlnXOLlebgPj8Fz1KO0za/RCO7ZIs4Q6Eiq1g1
username ansible privilege 15 role network-admin secret sha512 $6$Dzu11L7yp9j3nCM9$FSptxMPyIL555OMO.ldnjDXgwZmrfMYwHSr0uznE5Qoqvd9a6UdjiFcJUhGLtvXVZR1r.A/iF5aAt50hf/EK4/
username cvpadmin privilege 15 role network-admin secret sha512 $6$rZKcbIZ7iWGAWTUM$TCgDn1KcavS0s.OV8lacMTUkxTByfzcGlFlYUWroxYuU7M/9bIodhRO7nXGzMweUxvbk8mJmQl8Bh44cRktUj.
!
alias cc clear counters
alias log bash sudo cat /var/log/messages
alias ls bash ls -lrt /var/log/agents
alias nzdrop show inter counters queue det | egrep \ 0$-v
alias senz show interface counter error | nz
alias shimet show bgp evpn route-type imet detail | awk '/for imet/ { print "VNI: " $7 ", VTEP: " $8, "RD: " $11 }'
alias ship show int | awk '/^[A-Z]/ { intf = $1 } /Internet address is/ { print intf, $4 }'
alias shmacip show bgp evpn route-type mac-ip detail | awk '/for mac-ip/ { if (NF == 11) { print "RD: " $11, "VNI: " $7, "MAC: " $8 } else { print "RD: " $12, "VNI: " $7, "MAC: " $8, "IP: " $9 } }' | sed -e s/,//g
alias shmc show int | awk '/^[A-Z]/ { intf = $1 } /Hardware .* address is/ { print intf, $6 }'
alias shprefix show bgp evpn route-type ip-prefix ipv4 detail | awk '/for ip-prefix/ { print "ip-prefix: " $7, "RD: " $10 }'
alias sis show interfaces ethernet %1 status
alias snz show interface counter | nz
alias spd show port-channel %1 detail all
alias sqnz show interface counter queue | nz
alias srnz show interface counter rate | nz
alias wsrnz watch 1 show interface counter rate | nz
!
alias support
   110 bash sudo mkdir /mnt/flash/support
   200 bash echo ======= show tech-support =======
   210 show tech-support | no-more | redirect flash:/support/show-tech
   300 bash echo ======= show agent log =======
   310 show agent log | no-more | redirect flash:/support/show-agent
   400 bash echo ======= show agent qtrace =======
   410 show agent qtrace | no-more | redirect flash:/support/show-agent-qtrace
   500 bash echo ======= show logging system =======
   510 show logging system | no-more | redirect flash:/support/show-logg-system
   900 bash echo ======= tar zcvf /mnt/flash/support_$HOSTNAME.tar.gz =======
   910 bash sudo tar zcvf /mnt/flash/support_$HOSTNAME.tar.gz /mnt/flash/support
   920 bash sudo rm -rf /mnt/flash/support
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.183.0.1:9910 -cvauth=key, -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
!
vlan internal order ascending range 1006 1199
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model multi-agent
!
hostname KR1-SPINE1
!
spanning-tree mode none
!
vrf instance MGMT
!
management api http-commands
   no shutdown
   !
   vrf MGMT
      no shutdown
!
interface Ethernet1
   description P2P_LINK_TO_KR1-LEAF1A_Ethernet1
   no switchport
   ipv6 enable
!
interface Ethernet2
   description P2P_LINK_TO_KR1-LEAF1B_Ethernet1
   no switchport
   ipv6 enable
!
interface Ethernet3
   description P2P_LINK_TO_KR1-LEAF2A_Ethernet1
   no switchport
   ipv6 enable
!
interface Ethernet4
   description P2P_LINK_TO_KR1-LEAF2B_Ethernet1
   no switchport
   ipv6 enable
!
interface Ethernet5
!
interface Ethernet6
!
interface Ethernet7
!
interface Ethernet8
!
interface Loopback0
   description EVPN_Overlay_Peering
   ip address 10.100.0.1/32
!
interface Management1
   description oob_management
   vrf MGMT
   ip address 10.183.0.11/24
   no lldp transmit
   no lldp receive
!
ip routing ipv6 interfaces 
no ip routing vrf MGMT
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.100.0.0/24 eq 32
!
ipv6 unicast-routing
!
ip route vrf MGMT 0.0.0.0/0 10.183.0.1
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
!
router bgp 65001
   router-id 10.100.0.1
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor Overlay peer group
   neighbor Overlay next-hop-unchanged
   neighbor Overlay update-source Loopback0
   neighbor Overlay bfd
   neighbor Overlay ebgp-multihop 3
   neighbor Overlay send-community
   neighbor Overlay maximum-routes 0
   neighbor Underlay peer group
   neighbor Underlay send-community
   neighbor Underlay maximum-routes 12000
   neighbor 10.100.0.3 peer group Overlay
   neighbor 10.100.0.3 remote-as 65101
   neighbor 10.100.0.3 description KR1-LEAF1A
   neighbor 10.100.0.4 peer group Overlay
   neighbor 10.100.0.4 remote-as 65101
   neighbor 10.100.0.4 description KR1-LEAF1B
   neighbor 10.100.0.5 peer group Overlay
   neighbor 10.100.0.5 remote-as 65102
   neighbor 10.100.0.5 description KR1-LEAF2A
   neighbor 10.100.0.6 peer group Overlay
   neighbor 10.100.0.6 remote-as 65102
   neighbor 10.100.0.6 description KR1-LEAF2B
   redistribute connected route-map RM-CONN-2-BGP
   neighbor interface Et1-2 peer-group Underlay remote-as 65101
   neighbor interface Et3-4 peer-group Underlay remote-as 65102
   !
   address-family evpn
      neighbor Overlay activate
   !
   address-family ipv4
      no neighbor Overlay activate
      neighbor Underlay activate
      neighbor Underlay next-hop address-family ipv6 originate
!
end
```
## show version

```
Arista vEOS-lab
Hardware version: 
Serial number: 67A1F35F1C44312E4C95324C0BFBB3BA
Hardware MAC address: 5002.00fe.b05d
System MAC address: 5002.00fe.b05d

Software image version: 4.27.2F
Architecture: i686
Internal build version: 4.27.2F-26069621.4272F
Internal build ID: b3360a82-d532-4043-b6b0-50707eede2a9
Image format version: 1.0
Image optimization: None

Uptime: 5 minutes
Total memory: 2004396 kB
Free memory: 1073612 kB
```
