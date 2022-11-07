# US_DC

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
| AS_FABRIC | l3leaf | AS-BL1 | 198.18.252.107/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | l3leaf | AS-BL2 | 198.18.252.108/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | l3leaf | AS-LEAF1 | 198.18.252.103/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | l3leaf | AS-LEAF2 | 198.18.252.104/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | l3leaf | AS-LEAF3 | 198.18.252.105/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | l3leaf | AS-LEAF4 | 198.18.252.106/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | spine | AS-SPINE1 | 198.18.252.101/24 | vEOS-LAB | Provisioned |
| AS_FABRIC | spine | AS-SPINE2 | 198.18.252.102/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-BL1 | 198.18.251.107/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-BL2 | 198.18.251.108/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-LEAF1 | 198.18.251.103/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-LEAF2 | 198.18.251.104/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-LEAF3 | 198.18.251.105/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | l3leaf | SJ-LEAF4 | 198.18.251.106/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | spine | SJ-SPINE1 | 198.18.251.101/24 | vEOS-LAB | Provisioned |
| SJ_FABRIC | spine | SJ-SPINE2 | 198.18.251.102/24 | vEOS-LAB | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | AS-BL1 | Ethernet1 | spine | AS-SPINE1 | Ethernet5 |
| l3leaf | AS-BL1 | Ethernet2 | spine | AS-SPINE2 | Ethernet5 |
| l3leaf | AS-BL1 | Ethernet3 | mlag_peer | AS-BL2 | Ethernet3 |
| l3leaf | AS-BL1 | Ethernet4 | mlag_peer | AS-BL2 | Ethernet4 |
| l3leaf | AS-BL2 | Ethernet1 | spine | AS-SPINE1 | Ethernet6 |
| l3leaf | AS-BL2 | Ethernet2 | spine | AS-SPINE2 | Ethernet6 |
| l3leaf | AS-LEAF1 | Ethernet1 | spine | AS-SPINE1 | Ethernet1 |
| l3leaf | AS-LEAF1 | Ethernet2 | spine | AS-SPINE2 | Ethernet1 |
| l3leaf | AS-LEAF1 | Ethernet3 | mlag_peer | AS-LEAF2 | Ethernet3 |
| l3leaf | AS-LEAF1 | Ethernet4 | mlag_peer | AS-LEAF2 | Ethernet4 |
| l3leaf | AS-LEAF2 | Ethernet1 | spine | AS-SPINE1 | Ethernet2 |
| l3leaf | AS-LEAF2 | Ethernet2 | spine | AS-SPINE2 | Ethernet2 |
| l3leaf | AS-LEAF3 | Ethernet1 | spine | AS-SPINE1 | Ethernet3 |
| l3leaf | AS-LEAF3 | Ethernet2 | spine | AS-SPINE2 | Ethernet3 |
| l3leaf | AS-LEAF3 | Ethernet3 | mlag_peer | AS-LEAF4 | Ethernet3 |
| l3leaf | AS-LEAF3 | Ethernet4 | mlag_peer | AS-LEAF4 | Ethernet4 |
| l3leaf | AS-LEAF4 | Ethernet1 | spine | AS-SPINE1 | Ethernet4 |
| l3leaf | AS-LEAF4 | Ethernet2 | spine | AS-SPINE2 | Ethernet4 |
| l3leaf | SJ-BL1 | Ethernet1 | spine | SJ-SPINE1 | Ethernet5 |
| l3leaf | SJ-BL1 | Ethernet2 | spine | SJ-SPINE2 | Ethernet5 |
| l3leaf | SJ-BL1 | Ethernet3 | mlag_peer | SJ-BL2 | Ethernet3 |
| l3leaf | SJ-BL1 | Ethernet4 | mlag_peer | SJ-BL2 | Ethernet4 |
| l3leaf | SJ-BL2 | Ethernet1 | spine | SJ-SPINE1 | Ethernet6 |
| l3leaf | SJ-BL2 | Ethernet2 | spine | SJ-SPINE2 | Ethernet6 |
| l3leaf | SJ-LEAF1 | Ethernet1 | spine | SJ-SPINE1 | Ethernet1 |
| l3leaf | SJ-LEAF1 | Ethernet2 | spine | SJ-SPINE2 | Ethernet1 |
| l3leaf | SJ-LEAF1 | Ethernet3 | mlag_peer | SJ-LEAF2 | Ethernet3 |
| l3leaf | SJ-LEAF1 | Ethernet4 | mlag_peer | SJ-LEAF2 | Ethernet4 |
| l3leaf | SJ-LEAF2 | Ethernet1 | spine | SJ-SPINE1 | Ethernet2 |
| l3leaf | SJ-LEAF2 | Ethernet2 | spine | SJ-SPINE2 | Ethernet2 |
| l3leaf | SJ-LEAF3 | Ethernet1 | spine | SJ-SPINE1 | Ethernet3 |
| l3leaf | SJ-LEAF3 | Ethernet2 | spine | SJ-SPINE2 | Ethernet3 |
| l3leaf | SJ-LEAF3 | Ethernet3 | mlag_peer | SJ-LEAF4 | Ethernet3 |
| l3leaf | SJ-LEAF3 | Ethernet4 | mlag_peer | SJ-LEAF4 | Ethernet4 |
| l3leaf | SJ-LEAF4 | Ethernet1 | spine | SJ-SPINE1 | Ethernet4 |
| l3leaf | SJ-LEAF4 | Ethernet2 | spine | SJ-SPINE2 | Ethernet4 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 198.18.200.0/24 | 256 | 24 | 9.38 % |
| 198.18.201.0/24 | 256 | 24 | 9.38 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| AS-BL1 | Ethernet1 | 198.18.201.25/31 | AS-SPINE1 | Ethernet5 | 198.18.201.24/31 |
| AS-BL1 | Ethernet2 | 198.18.201.27/31 | AS-SPINE2 | Ethernet5 | 198.18.201.26/31 |
| AS-BL2 | Ethernet1 | 198.18.201.29/31 | AS-SPINE1 | Ethernet6 | 198.18.201.28/31 |
| AS-BL2 | Ethernet2 | 198.18.201.31/31 | AS-SPINE2 | Ethernet6 | 198.18.201.30/31 |
| AS-LEAF1 | Ethernet1 | 198.18.201.9/31 | AS-SPINE1 | Ethernet1 | 198.18.201.8/31 |
| AS-LEAF1 | Ethernet2 | 198.18.201.11/31 | AS-SPINE2 | Ethernet1 | 198.18.201.10/31 |
| AS-LEAF2 | Ethernet1 | 198.18.201.13/31 | AS-SPINE1 | Ethernet2 | 198.18.201.12/31 |
| AS-LEAF2 | Ethernet2 | 198.18.201.15/31 | AS-SPINE2 | Ethernet2 | 198.18.201.14/31 |
| AS-LEAF3 | Ethernet1 | 198.18.201.17/31 | AS-SPINE1 | Ethernet3 | 198.18.201.16/31 |
| AS-LEAF3 | Ethernet2 | 198.18.201.19/31 | AS-SPINE2 | Ethernet3 | 198.18.201.18/31 |
| AS-LEAF4 | Ethernet1 | 198.18.201.21/31 | AS-SPINE1 | Ethernet4 | 198.18.201.20/31 |
| AS-LEAF4 | Ethernet2 | 198.18.201.23/31 | AS-SPINE2 | Ethernet4 | 198.18.201.22/31 |
| SJ-BL1 | Ethernet1 | 198.18.200.25/31 | SJ-SPINE1 | Ethernet5 | 198.18.200.24/31 |
| SJ-BL1 | Ethernet2 | 198.18.200.27/31 | SJ-SPINE2 | Ethernet5 | 198.18.200.26/31 |
| SJ-BL2 | Ethernet1 | 198.18.200.29/31 | SJ-SPINE1 | Ethernet6 | 198.18.200.28/31 |
| SJ-BL2 | Ethernet2 | 198.18.200.31/31 | SJ-SPINE2 | Ethernet6 | 198.18.200.30/31 |
| SJ-LEAF1 | Ethernet1 | 198.18.200.9/31 | SJ-SPINE1 | Ethernet1 | 198.18.200.8/31 |
| SJ-LEAF1 | Ethernet2 | 198.18.200.11/31 | SJ-SPINE2 | Ethernet1 | 198.18.200.10/31 |
| SJ-LEAF2 | Ethernet1 | 198.18.200.13/31 | SJ-SPINE1 | Ethernet2 | 198.18.200.12/31 |
| SJ-LEAF2 | Ethernet2 | 198.18.200.15/31 | SJ-SPINE2 | Ethernet2 | 198.18.200.14/31 |
| SJ-LEAF3 | Ethernet1 | 198.18.200.17/31 | SJ-SPINE1 | Ethernet3 | 198.18.200.16/31 |
| SJ-LEAF3 | Ethernet2 | 198.18.200.19/31 | SJ-SPINE2 | Ethernet3 | 198.18.200.18/31 |
| SJ-LEAF4 | Ethernet1 | 198.18.200.21/31 | SJ-SPINE1 | Ethernet4 | 198.18.200.20/31 |
| SJ-LEAF4 | Ethernet2 | 198.18.200.23/31 | SJ-SPINE2 | Ethernet4 | 198.18.200.22/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 198.18.0.0/24 | 256 | 8 | 3.13 % |
| 198.18.10.0/24 | 256 | 8 | 3.13 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| AS_FABRIC | AS-BL1 | 198.18.10.9/32 |
| AS_FABRIC | AS-BL2 | 198.18.10.10/32 |
| AS_FABRIC | AS-LEAF1 | 198.18.10.5/32 |
| AS_FABRIC | AS-LEAF2 | 198.18.10.6/32 |
| AS_FABRIC | AS-LEAF3 | 198.18.10.7/32 |
| AS_FABRIC | AS-LEAF4 | 198.18.10.8/32 |
| AS_FABRIC | AS-SPINE1 | 198.18.10.1/32 |
| AS_FABRIC | AS-SPINE2 | 198.18.10.2/32 |
| SJ_FABRIC | SJ-BL1 | 198.18.0.9/32 |
| SJ_FABRIC | SJ-BL2 | 198.18.0.10/32 |
| SJ_FABRIC | SJ-LEAF1 | 198.18.0.5/32 |
| SJ_FABRIC | SJ-LEAF2 | 198.18.0.6/32 |
| SJ_FABRIC | SJ-LEAF3 | 198.18.0.7/32 |
| SJ_FABRIC | SJ-LEAF4 | 198.18.0.8/32 |
| SJ_FABRIC | SJ-SPINE1 | 198.18.0.1/32 |
| SJ_FABRIC | SJ-SPINE2 | 198.18.0.2/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 198.18.1.0/24 | 256 | 6 | 2.35 % |
| 198.18.11.0/24 | 256 | 6 | 2.35 % |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| AS_FABRIC | AS-BL1 | 198.18.11.9/32 |
| AS_FABRIC | AS-BL2 | 198.18.11.9/32 |
| AS_FABRIC | AS-LEAF1 | 198.18.11.5/32 |
| AS_FABRIC | AS-LEAF2 | 198.18.11.5/32 |
| AS_FABRIC | AS-LEAF3 | 198.18.11.7/32 |
| AS_FABRIC | AS-LEAF4 | 198.18.11.7/32 |
| SJ_FABRIC | SJ-BL1 | 198.18.1.9/32 |
| SJ_FABRIC | SJ-BL2 | 198.18.1.9/32 |
| SJ_FABRIC | SJ-LEAF1 | 198.18.1.5/32 |
| SJ_FABRIC | SJ-LEAF2 | 198.18.1.5/32 |
| SJ_FABRIC | SJ-LEAF3 | 198.18.1.7/32 |
| SJ_FABRIC | SJ-LEAF4 | 198.18.1.7/32 |
