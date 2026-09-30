# Enterprise Multi-Site Network Simulation

Cisco Packet Tracer coursework project designing a Malaysia manufacturing campus across three buildings and a Singapore headquarters. The proposed network uses a three-layer campus design, VLAN segmentation, distribution-switch inter-VLAN routing, OSPF, local DHCP, ACLs and simulated Internet access through NAT/PAT.


## Topology

Malaysia has three building networks with access switches, distribution multilayer switches and dual core multilayer switches. Singapore HQ includes a server VLAN. The report proposes redundant campus uplinks and dual ISP paths; actual failover depends on the configuration in the Packet Tracer project.

## Network design

| Component | Design in the assignment report |
| --- | --- |
| VLANs | 10 Administration, 20 Production, 30 IT Support, 40 Guest; 50 Servers at HQ; 99 native |
| Subnets | Separate `/24` per site/building and VLAN; see [addressing CSV](docs/ip_addressing_scheme.csv) |
| Gateways | SVIs on distribution multilayer switches with `ip routing` |
| Routing | Multi-area OSPF on internal links; static routes discussed for ISP/WAN transit |
| Address assignment | Local DHCP pools on distribution switches |
| Security | Inbound ACLs on Production and Guest SVIs; see [policy](docs/security-policy.md) |
| Internet | PAT on the Malaysia core router in the simulated network |
| WLAN | Guest access point shown per Malaysia building; report discusses a larger proposed WLAN |

### OSPF areas

Assigns Area 0 to the backbone, Areas 10/20/30 to Malaysia Buildings 1/2/3 and Area 40 to Singapore HQ.

| Area | Intended location |
| --- | --- |
| 0 | Backbone and core transit |
| 10 | Malaysia Building 1 |
| 20 | Malaysia Building 2 |
| 30 | Malaysia Building 3 |
| 40 | Singapore HQ |

### Traffic policy

- Production (VLAN 20) to Administration (VLAN 10): deny.
- Guest (VLAN 40) to internal enterprise networks: deny; permit simulated external access where routed/NATed.
- IT Support (VLAN 30) to other departments: permit under the documented lab policy.
- Other inter-VLAN traffic: permit unless another rule applies.


## Scope and limitations

This is a simulated academic design, not a production deployment. The report does not provide measured throughput, recovery time, demonstrated support for 2,000 simultaneous endpoints, or proof of every proposed redundancy feature. The usable address ranges in the CSV describe subnet host addresses; DHCP exclusions and reservations must be checked against real configurations. Before publishing the `.pkt` file and configuration exports, remove the lab WLAN passphrase and any other credentials.
