# KR1-LEAF1B Commands Output

## Table of Contents

- [show lldp neighbors](#show-lldp-neighbors)
- [show ip interface brief](#show-ip-interface-brief)
- [show interfaces description](#show-interfaces-description)
- [show version](#show-version)
- [show running-config](#show-running-config)
## show interfaces description

```
Interface                      Status         Protocol           Description
Et1                            up             up                 P2P_LINK_TO_KR1-SPINE1_Ethernet2
Et2                            up             up                 P2P_LINK_TO_KR1-SPINE2_Ethernet2
Et3                            up             up                 MLAG_PEER_KR1-LEAF1A_Ethernet3
Et4                            up             up                 MLAG_PEER_KR1-LEAF1A_Ethernet4
Et5                            up             up                 
Et6                            up             up                 
Et7                            up             up                 
Et8                            up             up                 
Lo0                            up             up                 EVPN_Overlay_Peering
Lo1                            up             up                 VTEP_VXLAN_Tunnel_Source
Ma1                            up             up                 oob_management
Po3                            up             up                 MLAG_PEER_KR1-LEAF1A_Po3
Vl110                          up             up                 OP_Zone_1
Vl130                          up             up                 10LAN_Zone_1
Vl3998                         up             up                 
Vl3999                         up             up                 
Vl4000                         up             up                 
Vl4093                         up             up                 MLAG_PEER_L3_PEERING
Vl4094                         up             up                 MLAG_PEER
Vx1                            up             up                 KR1-LEAF1B_VTEP
```
## show ip interface brief

```
Address
Interface       IP Address         Status      Protocol          MTU    Owner  
--------------- ------------------ ----------- ------------- ---------- -------
Ethernet1       unassigned         up          up               1500           
Ethernet2       unassigned         up          up               1500           
Loopback0       10.100.0.4/32      up          up              65535           
Loopback1       10.100.1.3/32      up          up              65535           
Management1     10.183.0.14/24     up          up               1500           
Vlan110         10.1.10.3/24       up          up               1500           
Vlan130         10.1.30.3/24       up          up               1500           
Vlan3998        unassigned         up          up               9164           
Vlan3999        unassigned         up          up               9164           
Vlan4000        unassigned         up          up               9164           
Vlan4093        unassigned         up          up               1500           
Vlan4094        10.192.0.1/31      up          up               1500
```
## show lldp neighbors

```
Last table change time   : 3:07:56 ago
Number of table inserts  : 20
Number of table deletes  : 8
Number of table drops    : 0
Number of table age-outs : 1

Port          Neighbor Device ID       Neighbor Port ID    TTL
---------- ------------------------ ---------------------- ---
Et1           KR1-SPINE1               Ethernet2           120
Et2           KR1-SPINE2               Ethernet2           120
Et3           KR1-LEAF1A               Ethernet3           120
Et4           KR1-LEAF1A               Ethernet4           120
Et5           DC1-L2LEAF1A             Ethernet2           120
Ma1           KR1-LEAF2B               Management1         120
Ma1           DC1-L2LEAF2A             Management1         120
Ma1           KR1-LEAF1A               Management1         120
Ma1           KR1-SPINE1               Management1         120
Ma1           KR1-SPINE2               Management1         120
Ma1           DC1-L2LEAF1A             Management1         120
Ma1           KR1-LEAF2A               Management1         120
```
## show running-config

```
! Command: show running-config
! device: KR1-LEAF1B (vEOS-lab, EOS-4.27.2F)
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
vlan internal order ascending range 3900 4000
!
transceiver qsfp default-mode 4x10G
!
service routing protocols model multi-agent
!
hostname KR1-LEAF1B
!
spanning-tree mode rapid-pvst
no spanning-tree vlan-id 4093-4094
spanning-tree vlan-id 1-4094 priority 16384
!
vlan 110
   name OP_Zone_1
!
vlan 130
   name 10LAN_Zone_1
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
!
vrf instance 10LANTEST_Zone
!
vrf instance MGMT
!
vrf instance OP_Zone
!
management api http-commands
   no shutdown
   !
   vrf MGMT
      no shutdown
!
interface Port-Channel3
   description MLAG_PEER_KR1-LEAF1A_Po3
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
!
interface Ethernet1
   description P2P_LINK_TO_KR1-SPINE1_Ethernet2
   no switchport
   ipv6 enable
!
interface Ethernet2
   description P2P_LINK_TO_KR1-SPINE2_Ethernet2
   no switchport
   ipv6 enable
!
interface Ethernet3
   description MLAG_PEER_KR1-LEAF1A_Ethernet3
   channel-group 3 mode active
!
interface Ethernet4
   description MLAG_PEER_KR1-LEAF1A_Ethernet4
   channel-group 3 mode active
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
   ip address 10.100.0.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   ip address 10.100.1.3/32
!
interface Management1
   description oob_management
   vrf MGMT
   ip address 10.183.0.14/24
!
interface Vlan110
   description OP_Zone_1
   vrf OP_Zone
   ip address 10.1.10.3/24
   ip virtual-router address 10.1.10.1
!
interface Vlan130
   description 10LAN_Zone_1
   vrf 10LANTEST_Zone
   ip address 10.1.30.3/24
   ip virtual-router address 10.1.30.1
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   ipv6 enable
!
interface Vlan4094
   description MLAG_PEER
   no autostate
   ip address 10.192.0.1/31
!
interface Vxlan1
   description KR1-LEAF1B_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 130 vni 10130
   vxlan vrf 10LANTEST_Zone vni 30
   vxlan vrf OP_Zone vni 10
!
ip virtual-router mac-address 00:1c:73:00:dc:11
!
ip routing ipv6 interfaces 
ip routing vrf 10LANTEST_Zone
no ip routing vrf MGMT
ip routing vrf OP_Zone
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.100.0.0/24 eq 32
   seq 20 permit 10.100.1.0/24 eq 32
!
ipv6 unicast-routing
!
mlag configuration
   domain-id KR1_LEAF1
   local-interface Vlan4094
   peer-address 10.192.0.0
   peer-link Port-Channel3
   reload-delay mlag 300
   reload-delay non-mlag 330
!
ip route vrf MGMT 0.0.0.0/0 10.183.0.1
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
!
router bgp 65101
   router-id 10.100.0.4
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor MLAG peer group
   neighbor MLAG remote-as 65101
   neighbor MLAG next-hop-self
   neighbor MLAG description KR1-LEAF1A
   neighbor MLAG route-map RM-MLAG-PEER-IN in
   neighbor MLAG send-community
   neighbor MLAG maximum-routes 12000
   neighbor Overlay peer group
   neighbor Overlay update-source Loopback0
   neighbor Overlay bfd
   neighbor Overlay ebgp-multihop 3
   neighbor Overlay send-community
   neighbor Overlay maximum-routes 0
   neighbor Underlay peer group
   neighbor Underlay send-community
   neighbor Underlay maximum-routes 12000
   neighbor 10.100.0.1 peer group Overlay
   neighbor 10.100.0.1 remote-as 65001
   neighbor 10.100.0.1 description KR1-SPINE1
   neighbor 10.100.0.2 peer group Overlay
   neighbor 10.100.0.2 remote-as 65001
   neighbor 10.100.0.2 description KR1-SPINE2
   redistribute connected route-map RM-CONN-2-BGP
   neighbor interface Et1-2 peer-group Underlay remote-as 65001
   neighbor interface Vl4093 peer-group MLAG remote-as 65101
   !
   vlan-aware-bundle 10LANTEST_Zone
      rd 10.100.0.4:30
      route-target both 30:30
      redistribute learned
      vlan 130
   !
   vlan-aware-bundle OP_Zone
      rd 10.100.0.4:10
      route-target both 10:10
      redistribute learned
      vlan 110
   !
   address-family evpn
      neighbor Overlay activate
   !
   address-family ipv4
      neighbor MLAG activate
      neighbor MLAG next-hop address-family ipv6 originate
      no neighbor Overlay activate
      neighbor Underlay activate
      neighbor Underlay next-hop address-family ipv6 originate
   !
   vrf 10LANTEST_Zone
      rd 10.100.0.4:30
      route-target import evpn 30:30
      route-target export evpn 30:30
      router-id 10.100.0.4
      redistribute connected
   !
   vrf OP_Zone
      rd 10.100.0.4:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.100.0.4
      redistribute connected
!
end
```
## show version

```
Arista vEOS-lab
Hardware version: 
Serial number: CF83838BDE59540DB0073D4126C92C2F
Hardware MAC address: 5002.001f.92ea
System MAC address: 5002.001f.92ea

Software image version: 4.27.2F
Architecture: i686
Internal build version: 4.27.2F-26069621.4272F
Internal build ID: b3360a82-d532-4043-b6b0-50707eede2a9
Image format version: 1.0
Image optimization: None

Uptime: 3 hours and 18 minutes
Total memory: 2004396 kB
Free memory: 944260 kB
```