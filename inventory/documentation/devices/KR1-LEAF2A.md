# KR1-LEAF2A
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.183.0.15/24 | 10.183.0.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.183.0.15/24
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS |
| ---- | ----- |
| False | True |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| admin | 15 | network-admin |
| ansible | 15 | network-admin |
| cvpadmin | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 $6$Df86J4/SFMDE3/1K$Hef4KstdoxNDaami37cBquTWOTplC.miMPjXVgQxMe92.e5wxlnXOLlebgPj8Fz1KO0za/RCO7ZIs4Q6Eiq1g1
username ansible privilege 15 role network-admin secret sha512 $6$Dzu11L7yp9j3nCM9$FSptxMPyIL555OMO.ldnjDXgwZmrfMYwHSr0uznE5Qoqvd9a6UdjiFcJUhGLtvXVZR1r.A/iF5aAt50hf/EK4/
username cvpadmin privilege 15 role network-admin secret sha512 $6$rZKcbIZ7iWGAWTUM$TCgDn1KcavS0s.OV8lacMTUkxTByfzcGlFlYUWroxYuU7M/9bIodhRO7nXGzMweUxvbk8mJmQl8Bh44cRktUj.
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 10.183.0.1:9910 | MGMT | key, | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=10.183.0.1:9910 -cvauth=key, -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

# MLAG

## MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| KR1_LEAF2 | Vlan4094 | 10.192.0.5 | Port-Channel2000 |

Dual primary detection is disabled.

## MLAG Device Configuration

```eos
!
mlag configuration
   domain-id KR1_LEAF2
   local-interface Vlan4094
   peer-address 10.192.0.5
   peer-link Port-Channel2000
   reload-delay mlag 300
   reload-delay non-mlag 330
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **rapid-pvst**

### Rapid-PVST Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 1-4094 | 16384 |

### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode rapid-pvst
no spanning-tree vlan-id 4093-4094
spanning-tree vlan-id 1-4094 priority 16384
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 3900 | 4000 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 3900 4000
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 110 | OP_Zone_1 | - |
| 120 | 10LAN_Zone_1 | - |
| 2103 | Teletron | - |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

## VLANs Device Configuration

```eos
!
vlan 110
   name OP_Zone_1
!
vlan 120
   name 10LAN_Zone_1
!
vlan 2103
   name Teletron
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet3 | MLAG_PEER_KR1-LEAF2B_Ethernet3 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 2000 |
| Ethernet4 | MLAG_PEER_KR1-LEAF2B_Ethernet4 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 2000 |

*Inherited from Port-Channel Interface

#### IPv6

| Interface | Description | Type | Channel Group | IPv6 Address | VRF | MTU | Shutdown | ND RA Disabled | Managed Config Flag | IPv6 ACL In | IPv6 ACL Out |
| --------- | ----------- | ---- | --------------| ------------ | --- | --- | -------- | -------------- | -------------------| ----------- | ------------ |
| Ethernet1 | P2P_LINK_TO_KR1-SPINE1_Ethernet3 | routed | - | - | default | 1500 | false | - | - | - | - |
| Ethernet2 | P2P_LINK_TO_KR1-SPINE2_Ethernet3 | routed | - | - | default | 1500 | false | - | - | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_KR1-SPINE1_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ipv6 enable
!
interface Ethernet2
   description P2P_LINK_TO_KR1-SPINE2_Ethernet3
   no shutdown
   mtu 1500
   no switchport
   ipv6 enable
!
interface Ethernet3
   description MLAG_PEER_KR1-LEAF2B_Ethernet3
   no shutdown
   channel-group 2000 mode active
!
interface Ethernet4
   description MLAG_PEER_KR1-LEAF2B_Ethernet4
   no shutdown
   channel-group 2000 mode active
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel2000 | MLAG_PEER_KR1-LEAF2B_Po3 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel2000
   description MLAG_PEER_KR1-LEAF2B_Po3
   no shutdown
   switchport
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.100.0.5/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.100.1.5/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.100.0.5/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.100.1.5/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan110 | OP_Zone_1 | OP_Zone | - | false |
| Vlan120 | 10LAN_Zone_1 | 10LAN_Zone | - | false |
| Vlan2103 | Teletron | Teletron | - | false |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | false |
| Vlan4094 | MLAG_PEER | default | 1500 | false |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan110 |  OP_Zone  |  10.1.10.2/24  |  -  |  10.1.10.1  |  -  |  -  |  -  |
| Vlan120 |  10LAN_Zone  |  10.1.20.2/24  |  -  |  10.1.20.1  |  -  |  -  |  -  |
| Vlan2103 |  Teletron  |  -  |  10.2.110.1/24  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  -  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.192.0.4/31  |  -  |  -  |  -  |  -  |  -  |


### VLAN Interfaces Device Configuration

```eos
!
interface Vlan110
   description OP_Zone_1
   no shutdown
   vrf OP_Zone
   ip address 10.1.10.2/24
   ip virtual-router address 10.1.10.1
!
interface Vlan120
   description 10LAN_Zone_1
   no shutdown
   vrf 10LAN_Zone
   ip address 10.1.20.2/24
   ip virtual-router address 10.1.20.1
!
interface Vlan2103
   description Teletron
   no shutdown
   vrf Teletron
   ip address virtual 10.2.110.1/24
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ipv6 enable
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 10.192.0.4/31
```

## VXLAN Interface

### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

#### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 110 | 10110 | - | - |
| 120 | 10120 | - | - |
| 2103 | 22103 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| 10LAN_Zone | 20 | - |
| OP_Zone | 10 | - |
| Teletron | 230 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description KR1-LEAF2A_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 120 vni 10120
   vxlan vlan 2103 vni 22103
   vxlan vrf 10LAN_Zone vni 20
   vxlan vrf OP_Zone vni 10
   vxlan vrf Teletron vni 230
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:1c:73:00:dc:11

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:11
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| 10LAN_Zone | true |
| MGMT | false |
| OP_Zone | true |
| Teletron | true |

### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf 10LAN_Zone
no ip routing vrf MGMT
ip routing vrf OP_Zone
ip routing vrf Teletron
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| 10LAN_Zone | false |
| MGMT | false |
| OP_Zone | false |
| Teletron | false |

### IPv6 Routing Device Configuration

```eos
!
ipv6 unicast-routing
ip routing ipv6 interfaces
```

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 10.183.0.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 10.183.0.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102|  10.100.0.5 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### MLAG

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65102 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### Overlay

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### Underlay

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | -------------- |
| 10.100.0.1 | 65001 | default | - | Inherited from peer group Overlay | Inherited from peer group Overlay | - | Inherited from peer group Overlay | - |
| 10.100.0.2 | 65001 | default | - | Inherited from peer group Overlay | Inherited from peer group Overlay | - | Inherited from peer group Overlay | - |

### BGP Neighbor Interfaces

| Neighbor Interface | Peer Group | Remote AS | Peer Filter |
| ------------------ | ---------- | --------- | ----------- |
| Ethernet1 | Underlay | 65001 | - |
| Ethernet2 | Underlay | 65001 | - |
| Vlan4093 | MLAG | 65102 | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| Overlay | True |

### Router BGP VLAN Aware Bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| 10LAN_Zone | 10.100.0.5:20 | 20:20 | - | - | learned | 120 |
| OP_Zone | 10.100.0.5:10 | 10:10 | - | - | learned | 110 |
| Teletron | 10.100.0.5:230 | 230:230 | - | - | learned | 2103 |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| 10LAN_Zone | 10.100.0.5:20 | connected |
| OP_Zone | 10.100.0.5:10 | connected |
| Teletron | 10.100.0.5:230 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 10.100.0.5
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor MLAG peer group
   neighbor MLAG remote-as 65102
   neighbor MLAG next-hop-self
   neighbor MLAG description KR1-LEAF2B
   neighbor MLAG send-community
   neighbor MLAG maximum-routes 12000
   neighbor MLAG route-map RM-MLAG-PEER-IN in
   neighbor Overlay peer group
   neighbor Overlay update-source Loopback0
   neighbor Overlay bfd
   neighbor Overlay ebgp-multihop 3
   neighbor Overlay send-community
   neighbor Overlay maximum-routes 0
   neighbor Underlay peer group
   neighbor Underlay send-community
   neighbor Underlay maximum-routes 12000
   neighbor interface Ethernet1 peer-group Underlay remote-as 65001
   neighbor interface Ethernet2 peer-group Underlay remote-as 65001
   neighbor interface Vlan4093 peer-group MLAG remote-as 65102
   neighbor 10.100.0.1 peer group Overlay
   neighbor 10.100.0.1 remote-as 65001
   neighbor 10.100.0.1 description KR1-SPINE1
   neighbor 10.100.0.2 peer group Overlay
   neighbor 10.100.0.2 remote-as 65001
   neighbor 10.100.0.2 description KR1-SPINE2
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle 10LAN_Zone
      rd 10.100.0.5:20
      route-target both 20:20
      redistribute learned
      vlan 120
   !
   vlan-aware-bundle OP_Zone
      rd 10.100.0.5:10
      route-target both 10:10
      redistribute learned
      vlan 110
   !
   vlan-aware-bundle Teletron
      rd 10.100.0.5:230
      route-target both 230:230
      redistribute learned
      vlan 2103
   !
   address-family evpn
      neighbor Overlay activate
   !
   address-family ipv4
      neighbor MLAG next-hop address-family ipv6 originate
      neighbor MLAG activate
      no neighbor Overlay activate
      neighbor Underlay next-hop address-family ipv6 originate
      neighbor Underlay activate
   !
   vrf 10LAN_Zone
      rd 10.100.0.5:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 10.100.0.5
      redistribute connected
   !
   vrf OP_Zone
      rd 10.100.0.5:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.100.0.5
      redistribute connected
   !
   vrf Teletron
      rd 10.100.0.5:230
      route-target import evpn 230:230
      route-target export evpn 230:230
      router-id 10.100.0.5
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.100.0.0/24 eq 32 |
| 20 | permit 10.100.1.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.100.0.0/24 eq 32
   seq 20 permit 10.100.1.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

#### RM-MLAG-PEER-IN

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | set origin incomplete |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| 10LAN_Zone | enabled |
| MGMT | disabled |
| OP_Zone | enabled |
| Teletron | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance 10LAN_Zone
!
vrf instance MGMT
!
vrf instance OP_Zone
!
vrf instance Teletron
```

# Quality Of Service
