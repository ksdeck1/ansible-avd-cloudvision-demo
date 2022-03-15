# DC_FABRIC

# Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

# Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision |
| --- | ---- | ---- | ------------- | -------- | -------------------------- |
| DC_FABRIC | l3leaf | KR1-LEAF1A | 10.183.0.13/24 | vEOS-LAB | Provisioned |
| DC_FABRIC | l3leaf | KR1-LEAF1B | 10.183.0.14/24 | vEOS-LAB | Provisioned |
| DC_FABRIC | l3leaf | KR1-LEAF2A | 10.183.0.15/24 | vEOS-LAB | Provisioned |
| DC_FABRIC | l3leaf | KR1-LEAF2B | 10.183.0.16/24 | vEOS-LAB | Provisioned |
| DC_FABRIC | spine | KR1-SPINE1 | 10.183.0.11/24 | vEOS-LAB | Provisioned |
| DC_FABRIC | spine | KR1-SPINE2 | 10.183.0.12/24 | vEOS-LAB | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | KR1-LEAF1A | Ethernet1 | spine | KR1-SPINE1 | Ethernet1 |
| l3leaf | KR1-LEAF1A | Ethernet2 | spine | KR1-SPINE2 | Ethernet1 |
| l3leaf | KR1-LEAF1A | Ethernet3 | mlag_peer | KR1-LEAF1B | Ethernet3 |
| l3leaf | KR1-LEAF1A | Ethernet4 | mlag_peer | KR1-LEAF1B | Ethernet4 |
| l3leaf | KR1-LEAF1B | Ethernet1 | spine | KR1-SPINE1 | Ethernet2 |
| l3leaf | KR1-LEAF1B | Ethernet2 | spine | KR1-SPINE2 | Ethernet2 |
| l3leaf | KR1-LEAF2A | Ethernet1 | spine | KR1-SPINE1 | Ethernet3 |
| l3leaf | KR1-LEAF2A | Ethernet2 | spine | KR1-SPINE2 | Ethernet3 |
| l3leaf | KR1-LEAF2A | Ethernet3 | mlag_peer | KR1-LEAF2B | Ethernet3 |
| l3leaf | KR1-LEAF2A | Ethernet4 | mlag_peer | KR1-LEAF2B | Ethernet4 |
| l3leaf | KR1-LEAF2B | Ethernet1 | spine | KR1-SPINE1 | Ethernet4 |
| l3leaf | KR1-LEAF2B | Ethernet2 | spine | KR1-SPINE2 | Ethernet4 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.172.0.0/24 | 256 | 16 | 6.25 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| KR1-LEAF1A | Ethernet1 | 10.172.0.1/31 | KR1-SPINE1 | Ethernet1 | 10.172.0.0/31 |
| KR1-LEAF1A | Ethernet2 | 10.172.0.3/31 | KR1-SPINE2 | Ethernet1 | 10.172.0.2/31 |
| KR1-LEAF1B | Ethernet1 | 10.172.0.5/31 | KR1-SPINE1 | Ethernet2 | 10.172.0.4/31 |
| KR1-LEAF1B | Ethernet2 | 10.172.0.7/31 | KR1-SPINE2 | Ethernet2 | 10.172.0.6/31 |
| KR1-LEAF2A | Ethernet1 | 10.172.0.9/31 | KR1-SPINE1 | Ethernet3 | 10.172.0.8/31 |
| KR1-LEAF2A | Ethernet2 | 10.172.0.11/31 | KR1-SPINE2 | Ethernet3 | 10.172.0.10/31 |
| KR1-LEAF2B | Ethernet1 | 10.172.0.13/31 | KR1-SPINE1 | Ethernet4 | 10.172.0.12/31 |
| KR1-LEAF2B | Ethernet2 | 10.172.0.15/31 | KR1-SPINE2 | Ethernet4 | 10.172.0.14/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.100.0.0/24 | 256 | 6 | 2.35 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC_FABRIC | KR1-LEAF1A | 10.100.0.3/32 |
| DC_FABRIC | KR1-LEAF1B | 10.100.0.4/32 |
| DC_FABRIC | KR1-LEAF2A | 10.100.0.5/32 |
| DC_FABRIC | KR1-LEAF2B | 10.100.0.6/32 |
| DC_FABRIC | KR1-SPINE1 | 10.100.0.1/32 |
| DC_FABRIC | KR1-SPINE2 | 10.100.0.2/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.100.1.0/24 | 256 | 4 | 1.57 % |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC_FABRIC | KR1-LEAF1A | 10.100.1.3/32 |
| DC_FABRIC | KR1-LEAF1B | 10.100.1.3/32 |
| DC_FABRIC | KR1-LEAF2A | 10.100.1.5/32 |
| DC_FABRIC | KR1-LEAF2B | 10.100.1.5/32 |
