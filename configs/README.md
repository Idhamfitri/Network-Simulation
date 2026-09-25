# Configuration notes and device exports

## Device map

| Device | Intended role | Export filename |
| --- | --- | --- |
| MY-DIST-MLS1-B1/B2/B3 | Building SVIs, DHCP, ACLs, OSPF | `MY-DIST-MLS1-B1.txt` and corresponding B2/B3 files |
| MY-CORE-MLS1/MLS2 | Routed Malaysia backbone uplinks | `MY-CORE-MLS1.txt`, `MY-CORE-MLS2.txt` |
| MY-CORE-ROUTER | Malaysia WAN default route and PAT | `MY-CORE-ROUTER.txt` |
| MY-ISP1/2 and SG-ISP1/2 | Simulated ISP transit using static routes | One file per device |
| SG-CORE-R1-HQ/R2-HQ | Singapore edge and WAN links | One file per device |
| SG-DIST-MLS1-HQ | Singapore SVIs and OSPF | `SG-DIST-MLS1-HQ.txt` |
| INTERNET-RTR | Optional simulated public server reachability | `INTERNET-RTR.txt` |

## VLAN and access/distribution examples

| VLAN | Function | Malaysia example (Building 1) | HQ subnet |
| --- | --- | --- | --- |
| 10 | Administration | `10.10.10.0/24` | `10.1.10.0/24` |
| 20 | Production | `10.10.20.0/24` | `10.1.20.0/24` |
| 30 | IT Support | `10.10.30.0/24` | `10.1.30.0/24` |
| 40 | Guest Wi-Fi | `10.10.40.0/24` | `10.1.40.0/24` |
| 50 | HQ servers | — | `10.1.50.0/24` |
| 99 | Native VLAN in design | No subnet specified | No subnet specified |

Buildings 2 and 3 use `10.11.X.0/24` and `10.12.X.0/24`. For the full table see [`../docs/ip_addressing_scheme.csv`](../docs/ip_addressing_scheme.csv). The source assigns `Fa0/6–20` to a department per access switch, guest ports `Fa0/23–24` in Malaysia (`Fa0/21–22` at HQ), and uses VLAN 99 as native. Confirm the actual ports and native VLAN in `show interfaces trunk`.

Example for **one Administration access switch**, assuming VLAN 10 already exists:

```ios
interface range FastEthernet0/6 - 20
 switchport mode access
 switchport access vlan 10
```

Example of a distribution uplink to an access switch, with interface name and allowed VLAN list verified for that link:

```ios
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40
```

Example Building 1 SVI gateways, pending comparison with actual export:

```ios
ip routing
interface Vlan10
 ip address 10.10.10.1 255.255.255.0
 no shutdown
interface Vlan20
 ip address 10.10.20.1 255.255.255.0
 no shutdown
interface Vlan30
 ip address 10.10.30.1 255.255.255.0
 no shutdown
interface Vlan40
 ip address 10.10.40.1 255.255.255.0
 no shutdown
```

## Routed link plan from the source notes

The following is a *link plan*, not verified interface output. The source has transcription errors on some rows; use the `.pkt` configuration as authority for port numbering.

| Endpoints | Planned network | Planned host addresses |
| --- | --- | --- |
| B1 DIST ↔ MY CORE MLS1 / MLS2 | `10.255.10.0/30` / `10.255.10.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| B2 DIST ↔ MY CORE MLS1 / MLS2 | `10.255.20.0/30` / `10.255.20.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| B3 DIST ↔ MY CORE MLS1 / MLS2 | `10.255.30.0/30` / `10.255.30.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| MY CORE MLS1 / MLS2 ↔ MY CORE ROUTER | `172.16.0.0/30` / `172.16.0.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| MY CORE ROUTER ↔ MY-ISP1 / MY-ISP2 | `192.168.100.0/30` / `192.168.100.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| MY-ISP1 ↔ SG-ISP1 / SG-ISP2 | `192.168.200.0/30` / `192.168.200.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| MY-ISP2 ↔ SG-ISP1 / SG-ISP2 | `192.168.201.0/30` / `192.168.201.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| SG-ISP1 ↔ HQ R1 / HQ R2 | `192.168.210.0/30` / `192.168.210.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| SG-ISP2 ↔ HQ R1 / HQ R2 | `192.168.211.0/30` / `192.168.211.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |
| HQ R1 / R2 ↔ SG DIST | `10.255.40.0/30` / `10.255.40.4/30` | `.1 ↔ .2` / `.5 ↔ .6` |

### OSPF: Malaysia distribution switches

The source assigns VLAN interfaces to building areas and routed uplinks to Area 0. Router IDs below are those in the pasted notes, not validated exports.

**Building 1:**

```ios
router ospf 10
 router-id 1.1.1.1
 network 10.10.10.0 0.0.0.255 area 10
 network 10.10.20.0 0.0.0.255 area 10
 network 10.10.30.0 0.0.0.255 area 10
 network 10.10.40.0 0.0.0.255 area 10
 network 10.255.10.0 0.0.0.3 area 0
 network 10.255.10.4 0.0.0.3 area 0
```

**Building 2:**

```ios
router ospf 10
 router-id 2.2.2.2
 network 10.11.10.0 0.0.0.255 area 20
 network 10.11.20.0 0.0.0.255 area 20
 network 10.11.30.0 0.0.0.255 area 20
 network 10.11.40.0 0.0.0.255 area 20
 network 10.255.20.0 0.0.0.3 area 0
 network 10.255.20.4 0.0.0.3 area 0
```

**Building 3:**

```ios
router ospf 10
 router-id 3.3.3.3
 network 10.12.10.0 0.0.0.255 area 30
 network 10.12.20.0 0.0.0.255 area 30
 network 10.12.30.0 0.0.0.255 area 30
 network 10.12.40.0 0.0.0.255 area 30
 network 10.255.30.0 0.0.0.3 area 0
 network 10.255.30.4 0.0.0.3 area 0
```

### OSPF: Malaysia core and edge

The source combines the two core switches into a single sample. Their routed uplink prefixes differ: confirm each interface and include *only connected networks* in each device's export.

```ios
! MY-CORE-MLS1 example; router ID from source: 10.10.10.10
router ospf 10
 router-id 10.10.10.10
 network 10.255.10.0 0.0.0.3 area 0
 network 10.255.20.0 0.0.0.3 area 0
 network 10.255.30.0 0.0.0.3 area 0
 network 172.16.0.0 0.0.0.3 area 0
```

```ios
! MY-CORE-MLS2 example; router ID from source: 10.10.10.11
router ospf 10
 router-id 10.10.10.11
 network 10.255.10.4 0.0.0.3 area 0
 network 10.255.20.4 0.0.0.3 area 0
 network 10.255.30.4 0.0.0.3 area 0
 network 172.16.0.4 0.0.0.3 area 0
```

```ios
! MY-CORE-ROUTER example
router ospf 10
 router-id 20.20.20.20
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.4 0.0.0.3 area 0
 default-information originate
ip route 0.0.0.0 0.0.0.0 192.168.100.2
ip route 0.0.0.0 0.0.0.0 192.168.100.6 10
```

OSPF is intended only within the enterprise devices; the ISP routers use static routes. The source text listing ISP routers under Area 0 contradicts that intent.

### OSPF: Singapore HQ

The source assigns HQ user/server VLANs to Area 40 on the distribution MLS and both HQ uplinks to Area 0. It also suggests Area 40 network statements on the HQ core routers without matching connected VLAN interfaces. Confirm real interfaces before enabling those statements.

```ios
! SG-DIST-MLS1-HQ example
router ospf 10
 router-id 120.120.120.120
 network 10.1.10.0 0.0.0.255 area 40
 network 10.1.20.0 0.0.0.255 area 40
 network 10.1.30.0 0.0.0.255 area 40
 network 10.1.40.0 0.0.0.255 area 40
 network 10.1.50.0 0.0.0.255 area 40
 network 10.255.40.0 0.0.0.3 area 0
 network 10.255.40.4 0.0.0.3 area 0
```

```ios
! SG-CORE-R1-HQ example
router ospf 10
 router-id 110.110.110.110
 network 10.255.40.0 0.0.0.3 area 0
! Next hops from source link plan: SG-ISP1 .1 and SG-ISP2 .1
ip route 0.0.0.0 0.0.0.0 192.168.210.1
ip route 0.0.0.0 0.0.0.0 192.168.211.1 10
```

```ios
! SG-CORE-R2-HQ example
router ospf 10
 router-id 110.110.110.111
 network 10.255.40.4 0.0.0.3 area 0
! Next hops inferred from source link plan; confirm in running config
ip route 0.0.0.0 0.0.0.0 192.168.210.5
ip route 0.0.0.0 0.0.0.0 192.168.211.5 10
```

The HQ edge may need additional routes for the Malaysia prefixes and/or controlled default origination. Test both directions before claiming full site-to-site reachability. The source contains a conflicting alternative that places HQ VLAN network statements on edge routers; do not merge the alternatives blindly.

## DHCP: Building 1 example

The source only includes one complete DHCP pool. Address exclusions reserve `.1`–`.20`, so the ordinary DHCP leasing range begins at `.21`, not `.2`. Build the other VLAN pools and buildings from their *actual* exports.

```ios
ip dhcp excluded-address 10.10.10.1 10.10.10.20
ip dhcp excluded-address 10.10.20.1 10.10.20.20
ip dhcp excluded-address 10.10.30.1 10.10.30.20
ip dhcp excluded-address 10.10.40.1 10.10.40.20
ip dhcp pool ADMIN_B1
 network 10.10.10.0 255.255.255.0
 default-router 10.10.10.1
 dns-server 8.8.8.8
 domain-name mycompany.local
```

Check DHCP bindings and client gateways with `show ip dhcp binding` and a client IP configuration view. The DNS server above is from the lab notes; a simulated resolver may require a different address.

## ACLs: policy and source examples

The source uses one global named ACL for all Production subnets and another for all Guest subnets. When attached inbound to each building's SVI, only local-source entries normally match there. Keep the actual `show running-config` export separate from this simplified **Building 1 example**.

```ios
ip access-list extended PROD_TO_ADMIN_B1
 deny ip 10.10.20.0 0.0.0.255 10.10.10.0 0.0.0.255
 deny ip 10.10.20.0 0.0.0.255 10.11.10.0 0.0.0.255
 deny ip 10.10.20.0 0.0.0.255 10.12.10.0 0.0.0.255
 deny ip 10.10.20.0 0.0.0.255 10.1.10.0 0.0.0.255
 permit ip 10.10.20.0 0.0.0.255 any
interface Vlan20
 ip access-group PROD_TO_ADMIN_B1 in
```

Guest-to-company subnet filtering, using the broader Malaysia summary so all three buildings are covered (the pasted draft used `10.10.0.0/16` and thereby omitted Buildings 2 and 3):

```ios
ip access-list extended GUEST_POLICY_B1
 deny ip 10.10.40.0 0.0.0.255 10.8.0.0 0.7.255.255
 deny ip 10.10.40.0 0.0.0.255 10.1.0.0 0.0.255.255
 permit ip 10.10.40.0 0.0.0.255 any
interface Vlan40
 ip access-group GUEST_POLICY_B1 in
```

This example assumes the Malaysia address plan stays within `10.8.0.0/13` and the HQ address plan within `10.1.0.0/16`. Add explicit protection for **other internal routed networks**, including `172.16.0.0/16` and relevant `192.168.0.0/16` lab infrastructure, if guest access to those must be denied. Test ACL order and counters. Permitting IT Support outbound does not override restrictions applied elsewhere or establish authenticated admin access.

## WAN static-route examples from notes

All next hops depend on the planned /30 interfaces and actual topology. The source includes duplicate and incomplete route notes; the following are examples to compare with running configurations, not a complete verified route table.

```ios
! MY-ISP1
ip route 10.8.0.0 255.248.0.0 192.168.100.1
ip route 10.1.0.0 255.255.0.0 192.168.200.2
ip route 10.1.0.0 255.255.0.0 192.168.200.6 10
```

```ios
! MY-ISP2
ip route 10.8.0.0 255.248.0.0 192.168.100.5
ip route 10.1.0.0 255.255.0.0 192.168.201.2
ip route 10.1.0.0 255.255.0.0 192.168.201.6 10
```

```ios
! SG-ISP1
ip route 10.1.0.0 255.255.0.0 192.168.210.2
ip route 10.1.0.0 255.255.0.0 192.168.210.6
ip route 10.8.0.0 255.248.0.0 192.168.200.1
ip route 10.8.0.0 255.248.0.0 192.168.201.1 10
```

```ios
! SG-ISP2
ip route 10.1.0.0 255.255.0.0 192.168.211.2
ip route 10.1.0.0 255.255.0.0 192.168.211.6
ip route 10.8.0.0 255.248.0.0 192.168.200.5
ip route 10.8.0.0 255.248.0.0 192.168.201.5 10
```

On SG-ISP1/2, two equal-distance static routes to HQ may share traffic. Verify return reachability when an edge or ISP path fails; a floating route only activates when its primary route leaves the routing table, which is not necessarily the same as end-to-end failure detection.

## NAT/PAT: intended Malaysia Internet breakout

The later source draft improves on the earlier broad NAT ACL by excluding traffic to HQ. This example covers Malaysia-to-simulated-Internet traffic only. NAT behavior with **two overload interfaces using one ACL** must be verified in Packet Tracer on both paths; it may require separate route-aware rules depending on the supported IOS feature set. Do not assume the backup works without testing.

```ios
ip access-list extended NAT_MY
 deny ip 10.8.0.0 0.7.255.255 10.1.0.0 0.0.255.255
 permit ip 10.8.0.0 0.7.255.255 any
ip nat inside source list NAT_MY interface Serial0/2/0 overload
ip nat inside source list NAT_MY interface Serial0/2/1 overload
interface GigabitEthernet0/0/0
 ip nat inside
interface GigabitEthernet0/0/1
 ip nat inside
interface Serial0/2/0
 ip nat outside
interface Serial0/2/1
 ip nat outside
```

**Important source inconsistency:** The pasted NAT block assigned `192.168.100.2` and `.6` to the **MY core router**, whereas the link plan and default routes assign `.1` and `.5` to the core router and `.2` and `.6` to the ISP routers. Use `show ip interface brief` from the `.pkt` file to settle this. The source also used `G0/0/0` and `G0/0/1` as HQ NAT overload interfaces while identifying its outside ports as Serial; do not copy those conflicting HQ NAT snippets.

## Optional simulated Internet segment

The notes propose a separate Internet router and HTTP server using documentation address blocks. This is a planned extension unless the actual `.pkt` file includes it.

| Segment | Router / host | Address |
| --- | --- | --- |
| MY-ISP1 ↔ INTERNET-RTR | MY-ISP1 / INTERNET-RTR | `203.0.113.1/30` / `203.0.113.2/30` |
| Web server LAN | INTERNET-RTR / WEB-SERVER | `198.51.100.1/24` / `198.51.100.10/24` |
| Server default gateway | WEB-SERVER | `198.51.100.1` |

```ios
! INTERNET-RTR example (interface mapping must match your cabling)
interface GigabitEthernet0/0
 ip address 203.0.113.2 255.255.255.252
 no shutdown
interface GigabitEthernet0/1
 ip address 198.51.100.1 255.255.255.0
 no shutdown
```

```ios
! MY-ISP1 example: choose real interface and apply clock only on a DCE side
interface Serial0/1/0
 ip address 203.0.113.1 255.255.255.252
 no shutdown
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

Enable HTTP on the simulated server using Packet Tracer's server UI. If PAT is configured at the enterprise edge, return routes to every private enterprise subnet are **not inherently required on the Internet router** for translated outbound flows; the draft's suggested private return routes may be needed for *untranslated* site-to-site lab flows, which should be evaluated separately.

## Verify the exported configuration

```ios
show running-config
show ip interface brief
show interfaces trunk
show vlan brief
show ip ospf neighbor
show ip ospf interface brief
show ip route
show ip dhcp binding
show ip access-lists
show ip nat translations
show ip nat statistics
```
