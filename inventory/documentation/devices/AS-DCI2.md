# AS-DCI2
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [Name Servers](#name-servers)
  - [Domain Lookup](#domain-lookup)
  - [NTP](#ntp)
  - [Management SSH](#management-ssh)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [TACACS Servers](#tacacs-servers)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
- [Aliases](#aliases)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Logging](#logging)
  - [SNMP](#snmp)
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
  - [Standard Access-lists](#standard-access-lists)
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
| Management1 | oob_management | oob | MGMT | 198.18.98.108/24 | 198.18.98.1 |

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
   ip address 198.18.98.108/24
```

## DNS Domain

### DNS domain: netdevenv.net

### DNS Domain Device Configuration

```eos
dns domain netdevenv.net
!
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 198.18.100.160 | MGMT |

### Name Servers Device Configuration

```eos
ip name-server vrf MGMT 198.18.100.160
```

## Domain Lookup

### DNS Domain Lookup Summary

| Source interface | vrf |
| ---------------- | --- |
| Management1 | MGMT |

### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf MGMT source-interface Management1
```

## NTP

### NTP Summary

#### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 129.6.15.28 | MGMT | True | - | - | - | - | - | - | - |
| 129.6.15.29 | MGMT | - | - | - | - | - | - | - | - |
| 132.163.97.4 | MGMT | - | - | - | - | - | - | - | - |

#### NTP Authentication

### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT 129.6.15.28 prefer
ntp server vrf MGMT 129.6.15.29
ntp server vrf MGMT 132.163.97.4
```

## Management SSH


### SSH timeout and management

| Idle Timeout | SSH Management |
| ------------ | -------------- |
| default | Enabled |

### Max number of SSH sessions limit and per-host limit

| Connection Limit | Max from a single Host |
| ---------------- | ---------------------- |
| - | - |

### Ciphers and algorithms

| Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
|---------|----------------------|----------------|---------------------------|
| default | default | default | default |

### VRFs

| VRF | Status |
| --- | ------ |
| MGMT | Enabled |

### Management SSH Configuration

```eos
!
management ssh
   !
   vrf MGMT
      no shutdown
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

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
username admin privilege 15 role network-admin secret sha512 $6$pwmzPZqxql2yZ.ge$Cx7b5gTa9C7go/skTK0W88zdkBxFBuAmsol.TkWcnGlpRQohUMxzzHQmIzyZ/5M8T33.qSZMFPsgMgmTzM6r01
username ansible privilege 15 role network-admin secret sha512 $6$K0SFGHUNW.al69Yo$///VGqkYDYDpUjXKFkSWa5.WVnANiBPD8d/An/HP2I/IOd6AYSXJ9VL6FrjYrwp/SLsqwZLB7KV4hB9HGAyKv.
username cvpadmin privilege 15 role network-admin secret sha512 $6$lhcX5i6a7cVCvwux$n4KM19WOV6cFG0QumteDf9A3SA.GmBJidlSuHfWd/x9KB2yEVjynw8Kr10KJFQ0eeBgBGONpdT1HzP1uGjg3C.
```

## TACACS Servers

### TACACS Servers

| VRF | TACACS Servers | Single-Connection |
| --- | -------------- | ----------------- |
| MGMT | 198.18.100.150 | False |

### TACACS Servers Device Configuration

```eos
!
tacacs-server host 198.18.100.150 vrf MGMT timeout 10 key 7 Arista321!
```

## AAA Server Groups

### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| TACACS_GROUP | tacacs+ | MGMT | 198.18.100.150 |

### AAA Server Groups Device Configuration

```eos
!
aaa group server tacacs+ TACACS_GROUP
   server 198.18.100.150 vrf MGMT
```

## AAA Authentication

### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | local |
| Login | console | local |

### AAA Authentication Device Configuration

```eos
aaa authentication login default local
aaa authentication login console local
!
```

# Aliases

```eos
alias CVP ping vrf MGMT 198.18.99.90
alias M99 ping vrf MGMT 198.18.99.1
alias M98 ping vrf MGMT 198.18.98.1
alias MAC sh bgp evpn route-type mac-ip
alias PRE sh bgp evpn route-type ip-prefix ipv4

!
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 198.18.99.90:9910 | MGMT | key, | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=198.18.99.90:9910 -cvauth=key, -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## Logging

### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |
| Console | critical |
| Monitor | notifications |
| Buffer | debugging |

| VRF | Source Interface |
| --- | ---------------- |
| - | Management1 |
| MGMT | Management1 |

| VRF | Hosts | Ports | Protocol |
| --- | ----- | ----- | -------- |
| MGMT | 198.18.100.150 | Default | UDP |

### Logging Servers and Features Device Configuration

```eos
!
logging buffered 1000 debugging
logging console critical
logging monitor notifications
logging vrf MGMT host 198.18.100.150
logging source-interface Management1
logging vrf MGMT source-interface Management1
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| Al Catraz | SJ | All | Disabled |
| Al Catraz | SJ | bgp, entity arista-ent-sensor-alarm, external-alarm arista-external-alarm-deasserted-notif, snmp link-down, snmp link-up | Enabled |
| Al Catraz | SJ |  | Disabled |

### SNMP ACLs
| IP | ACL | VRF |
| -- | --- | --- |
| IPv4 | SNMP_ACL | MGMT |

### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |
| Management1 | MGMT |

### SNMP VRF Status

| VRF | Status |
| --- | ------ |
| MGMT | Enabled |

### SNMP Hosts Configuration

| Host | VRF | Community | Username | Authentication level | SNMP Version |
| ---- |---- | --------- | -------- | -------------------- | ------------ |
| 198.18.100.150 | MGMT | public | - | - | 2c |

### SNMP Views Configuration

| View | MIB Family Name | Status |
| ---- | --------------- | ------ |
| VIEW_ALL | ISO | Included |

### SNMP Groups Configuration

| Group | SNMP Version | Authentication | Read | Write | Notify |
| ----- | ------------ | -------------- | ---- | ----- | ------ |
| SNMPV3_RO | v3 | priv | VIEW_ALL | - | - |

### SNMP Users Configuration

| User | Group | Version | Authentication | Privacy | Remote Address | Remote Port | Engine ID |
| ---- | ----- | ------- | -------------- | ------- | -------------- | ----------- | --------- |
| V3USER | SNMPV3_RO | v3 | MD5 | aes | - | - | - |

### SNMP Device Configuration

```eos
!
snmp-server contact Al Catraz
snmp-server location SJ
snmp-server ipv4 access-list SNMP_ACL vrf MGMT
snmp-server vrf MGMT local-interface Management1
snmp-server view VIEW_ALL ISO included
snmp-server group SNMPV3_RO v3 priv read VIEW_ALL
snmp-server user V3USER SNMPV3_RO v3 auth MD5 Arista654! priv aes Arista987!
snmp-server host 198.18.100.150 vrf MGMT version 2c public
snmp-server enable traps bgp
snmp-server enable traps entity arista-ent-sensor-alarm
snmp-server enable traps external-alarm arista-external-alarm-deasserted-notif
snmp-server enable traps snmp link-down
snmp-server enable traps snmp link-up
snmp-server vrf MGMT
```

# MLAG

## MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| AS_DCI1_2 | Vlan4094 | 10.255.252.12 | Port-Channel3 |

Dual primary detection is disabled.

## MLAG Device Configuration

```eos
!
mlag configuration
   domain-id AS_DCI1_2
   local-interface Vlan4094
   peer-address 10.255.252.12
   peer-link Port-Channel3
   reload-delay mlag 300
   reload-delay non-mlag 330
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 16384 |

### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 16384
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 10 | VLAN_A | - |
| 20 | VLAN_B | - |
| 110 | VLAN_C | - |
| 120 | VLAN_D | - |
| 210 | VLAN_E | - |
| 220 | VLAN_F | - |
| 3009 | MLAG_iBGP_PROD | LEAF_PEER_L3 |
| 3019 | MLAG_iBGP_DEV | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

## VLANs Device Configuration

```eos
!
vlan 10
   name VLAN_A
!
vlan 20
   name VLAN_B
!
vlan 110
   name VLAN_C
!
vlan 120
   name VLAN_D
!
vlan 210
   name VLAN_E
!
vlan 220
   name VLAN_F
!
vlan 3009
   name MLAG_iBGP_PROD
   trunk group LEAF_PEER_L3
!
vlan 3019
   name MLAG_iBGP_DEV
   trunk group LEAF_PEER_L3
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
| Ethernet3 | MLAG_PEER_AS-DCI1_Ethernet3 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |
| Ethernet4 | MLAG_PEER_AS-DCI1_Ethernet4 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 3 |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_AS-SPINE1_Ethernet6 | routed | - | 198.18.201.25/31 | default | 1500 | false | - | - |
| Ethernet2 | P2P_LINK_TO_AS-SPINE2_Ethernet6 | routed | - | 198.18.201.27/31 | default | 1500 | false | - | - |
| Ethernet5 | P2P_LINK_TO_SJ-DCI2_Ethernet5 | routed | - | 10.255.250.5/31 | default | 1500 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_AS-SPINE1_Ethernet6
   no shutdown
   mtu 1500
   no switchport
   ip address 198.18.201.25/31
!
interface Ethernet2
   description P2P_LINK_TO_AS-SPINE2_Ethernet6
   no shutdown
   mtu 1500
   no switchport
   ip address 198.18.201.27/31
!
interface Ethernet3
   description MLAG_PEER_AS-DCI1_Ethernet3
   no shutdown
   channel-group 3 mode active
!
interface Ethernet4
   description MLAG_PEER_AS-DCI1_Ethernet4
   no shutdown
   channel-group 3 mode active
!
interface Ethernet5
   description P2P_LINK_TO_SJ-DCI2_Ethernet5
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.250.5/31
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel3 | MLAG_PEER_AS-DCI1_Po3 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel3
   description MLAG_PEER_AS-DCI1_Po3
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
| Loopback0 | EVPN_Overlay_Peering | default | 198.18.10.9/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 198.18.11.9/32 |

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
   ip address 198.18.10.9/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 198.18.11.9/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | Used for Testing | PROD | - | false |
| Vlan20 | Not in use | PROD | - | false |
| Vlan110 | Used for Testing | DEV | - | false |
| Vlan120 | Not in use | DEV | - | false |
| Vlan3009 | MLAG_PEER_L3_iBGP: vrf PROD | PROD | 1500 | false |
| Vlan3019 | MLAG_PEER_L3_iBGP: vrf DEV | DEV | 1500 | false |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | false |
| Vlan4094 | MLAG_PEER | default | 1500 | false |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan10 |  PROD  |  -  |  10.1.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan20 |  PROD  |  -  |  10.1.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan110 |  DEV  |  -  |  10.10.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan120 |  DEV  |  -  |  10.10.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan3009 |  PROD  |  10.255.251.13/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3019 |  DEV  |  10.255.251.13/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.251.13/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.252.13/31  |  -  |  -  |  -  |  -  |  -  |

### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description Used for Testing
   no shutdown
   vrf PROD
   ip address virtual 10.1.10.1/24
!
interface Vlan20
   description Not in use
   no shutdown
   vrf PROD
   ip address virtual 10.1.20.1/24
!
interface Vlan110
   description Used for Testing
   no shutdown
   vrf DEV
   ip address virtual 10.10.10.1/24
!
interface Vlan120
   description Not in use
   no shutdown
   vrf DEV
   ip address virtual 10.10.20.1/24
!
interface Vlan3009
   description MLAG_PEER_L3_iBGP: vrf PROD
   no shutdown
   mtu 1500
   vrf PROD
   ip address 10.255.251.13/31
!
interface Vlan3019
   description MLAG_PEER_L3_iBGP: vrf DEV
   no shutdown
   mtu 1500
   vrf DEV
   ip address 10.255.251.13/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ip address 10.255.251.13/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 10.255.252.13/31
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
| 10 | 10010 | - | - |
| 20 | 10020 | - | - |
| 110 | 10110 | - | - |
| 120 | 10120 | - | - |
| 210 | 10210 | - | - |
| 220 | 10220 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| DEV | 20 | - |
| PROD | 10 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description AS-DCI2_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 20 vni 10020
   vxlan vlan 110 vni 10110
   vxlan vlan 120 vni 10120
   vxlan vlan 210 vni 10210
   vxlan vlan 220 vni 10220
   vxlan vrf DEV vni 20
   vxlan vrf PROD vni 10
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

#### Virtual Router MAC Address: 00:1c:73:00:dc:01

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:01
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| DEV | true |
| MGMT | false |
| PROD | true |

### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf DEV
no ip routing vrf MGMT
ip routing vrf PROD
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| DEV | false |
| MGMT | false |
| PROD | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 198.18.98.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 198.18.98.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65103|  198.18.10.9 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### DCI-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65003 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- |
| 10.255.250.4 | 65002 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 10.255.251.12 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - |
| 198.18.0.8 | 65003 | default | - | Inherited from peer group DCI-OVERLAY-PEERS | Inherited from peer group DCI-OVERLAY-PEERS | - | Inherited from peer group DCI-OVERLAY-PEERS | - |
| 198.18.10.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 198.18.10.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 198.18.201.24 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 198.18.201.26 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 10.255.251.12 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | DEV | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - |
| 10.255.251.12 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | PROD | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| DCI-OVERLAY-PEERS | True |
| EVPN-OVERLAY-PEERS | True |

### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 198.18.10.9:10010 | 10010:10010 | - | - | learned |
| 20 | 198.18.10.9:10020 | 10020:10020 | - | - | learned |
| 110 | 198.18.10.9:10110 | 10110:10110 | - | - | learned |
| 120 | 198.18.10.9:10120 | 10120:10120 | - | - | learned |
| 210 | 198.18.10.9:10210 | 10210:10210 | - | - | learned |
| 220 | 198.18.10.9:10220 | 10220:10220 | - | - | learned |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| DEV | 198.18.10.9:20 | connected |
| PROD | 198.18.10.9:10 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65103
   router-id 198.18.10.9
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor DCI-OVERLAY-PEERS peer group
   neighbor DCI-OVERLAY-PEERS update-source Loopback0
   neighbor DCI-OVERLAY-PEERS bfd
   neighbor DCI-OVERLAY-PEERS ebgp-multihop 3
   neighbor DCI-OVERLAY-PEERS send-community
   neighbor DCI-OVERLAY-PEERS maximum-routes 0
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 Arista456!
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 Arista123!
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65003
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description AS-DCI1
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 Arista789!
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.255.250.4 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.250.4 remote-as 65002
   neighbor 10.255.250.4 local-as 65102 no-prepend replace-as
   neighbor 10.255.250.4 description SJ-DCI2
   neighbor 10.255.251.12 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.251.12 description AS-DCI1
   neighbor 198.18.0.8 peer group DCI-OVERLAY-PEERS
   neighbor 198.18.0.8 remote-as 65003
   neighbor 198.18.0.8 description SJ-DCI2
   neighbor 198.18.10.1 peer group EVPN-OVERLAY-PEERS
   neighbor 198.18.10.1 remote-as 65100
   neighbor 198.18.10.1 description AS-SPINE1
   neighbor 198.18.10.2 peer group EVPN-OVERLAY-PEERS
   neighbor 198.18.10.2 remote-as 65100
   neighbor 198.18.10.2 description AS-SPINE2
   neighbor 198.18.201.24 peer group IPv4-UNDERLAY-PEERS
   neighbor 198.18.201.24 remote-as 65100
   neighbor 198.18.201.24 description AS-SPINE1_Ethernet6
   neighbor 198.18.201.26 peer group IPv4-UNDERLAY-PEERS
   neighbor 198.18.201.26 remote-as 65100
   neighbor 198.18.201.26 description AS-SPINE2_Ethernet6
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 198.18.10.9:10010
      route-target both 10010:10010
      redistribute learned
   !
   vlan 110
      rd 198.18.10.9:10110
      route-target both 10110:10110
      redistribute learned
   !
   vlan 120
      rd 198.18.10.9:10120
      route-target both 10120:10120
      redistribute learned
   !
   vlan 20
      rd 198.18.10.9:10020
      route-target both 10020:10020
      redistribute learned
   !
   vlan 210
      rd 198.18.10.9:10210
      route-target both 10210:10210
      redistribute learned
   !
   vlan 220
      rd 198.18.10.9:10220
      route-target both 10220:10220
      redistribute learned
   !
   address-family evpn
      neighbor DCI-OVERLAY-PEERS activate
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf DEV
      rd 198.18.10.9:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 198.18.10.9
      neighbor 10.255.251.12 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf PROD
      rd 198.18.10.9:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 198.18.10.9
      neighbor 10.255.251.12 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 3600 | 3600 | 5 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 3600 min-rx 3600 multiplier 5
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
| 10 | permit 198.18.10.0/24 eq 32 |
| 20 | permit 198.18.11.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 198.18.10.0/24 eq 32
   seq 20 permit 198.18.11.0/24 eq 32
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

## Standard Access-lists

### Standard Access-lists Summary

#### MGMT_ACL

| Sequence | Action |
| -------- | ------ |
| 10 | permit 198.18.0.0/16 |

#### SNMP_ACL

| Sequence | Action |
| -------- | ------ |
| 10 | permit host 198.18.100.150 |

### Standard Access-lists Device Configuration

```eos
!
ip access-list standard MGMT_ACL
   10 permit 198.18.0.0/16
!
ip access-list standard SNMP_ACL
   10 permit host 198.18.100.150
```

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| DEV | enabled |
| MGMT | disabled |
| PROD | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance DEV
!
vrf instance MGMT
!
vrf instance PROD
```

# Quality Of Service
