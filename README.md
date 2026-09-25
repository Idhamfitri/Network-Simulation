# Enterprise Multi-Site Network Simulation

Cisco Packet Tracer coursework project designing a Malaysia manufacturing campus across three buildings and a Singapore headquarters. The proposed network uses a three-layer campus design, VLAN segmentation, distribution-switch inter-VLAN routing, OSPF, local DHCP, ACLs and simulated Internet access through NAT/PAT.

**Context:** CST337 Network Configurations and Protocols, Universiti Sains Malaysia, academic session 2025/2026. The scenario specifies approximately 2,000 employees at the Malaysia facility; this is a design requirement, not a tested capacity or number of simulated endpoints.

## Topology

Place your existing screenshot at `docs/network_topology.png` to display it here:

<!-- Remove this line and the surrounding comment markers when the image exists.
![Malaysia campus and Singapore HQ topology](docs/network_topology.png)
-->

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

The report's **narrative** assigns Area 0 to the backbone, Areas 10/20/30 to Malaysia Buildings 1/2/3 and Area 40 to Singapore HQ. Its area table contradicts this mapping. The table below is the *intended design from the narrative*, pending comparison with actual device configurations.

| Area | Intended location |
| --- | --- |
| 0 | Backbone and core transit |
| 10 | Malaysia Building 1 |
| 20 | Malaysia Building 2 |
| 30 | Malaysia Building 3 |
| 40 | Singapore HQ |

See [addressing and routing notes](docs/addressing.md) for the discrepancy and WAN boundary.

### Traffic policy

- Production (VLAN 20) to Administration (VLAN 10): deny.
- Guest (VLAN 40) to internal enterprise networks: deny; permit simulated external access where routed/NATed.
- IT Support (VLAN 30) to other departments: permit under the documented lab policy.
- Other inter-VLAN traffic: permit unless another rule applies.

The report documents ACL policy but does not establish a zero-trust design or stateful firewall controls. Confirm rule placement, destination prefixes, and observed results in the original simulation.

## Run and verify

1. Open the original `.pkt` file from `src/` using Cisco Packet Tracer.
2. Compare device names, gateway addresses, link IPs and OSPF areas with [the addressing map](docs/addressing.md).
3. Test VLAN membership, DHCP, routing, denied and permitted ACL flows, PAT and any failover using [the verification checklist](docs/verification.md).
4. Export sanitized running configurations into `configs/` and record real test output in `docs/evidence/`. Record the Packet Tracer version used.

No `.pkt` file, original configs or test output were available when this README was prepared. The report describes tests, but this repository should show direct evidence before claiming independently reproducible results.

## Repository contents

```text
configs/  Exported, sanitized IOS device configurations
  README.md
docs/
  addressing.md
  ip_addressing_scheme.csv
  security-policy.md
  verification.md
  network_topology.png      Add the existing topology screenshot
  evidence/                 Add redacted command output and test screenshots
src/
  README.md
  *.pkt                     Add the original Packet Tracer simulation
README.md
```

## Scope and limitations

This is a simulated academic design, not a production deployment. The report does not provide measured throughput, recovery time, demonstrated support for 2,000 simultaneous endpoints, or proof of every proposed redundancy feature. The usable address ranges in the CSV describe subnet host addresses; DHCP exclusions and reservations must be checked against real configurations. Before publishing the `.pkt` file and configuration exports, remove the lab WLAN passphrase and any other credentials.

## Contributors

Group assignment by **Wan Shan Jie, Idham Fitri bin Mohd Rodzi, and Muhammad Aqif bin Ali Yasak**. Add each member's verified contribution if you wish to describe individual ownership in applications. Confirm permission to publish shared project files.

## Resume-ready description

> Collaborated on a Cisco Packet Tracer enterprise network design connecting a three-building Malaysia campus and Singapore HQ, with VLAN segmentation, SVI routing, OSPF, DHCP, ACLs and simulated NAT/PAT.

Adapt this sentence to the work you personally performed and add tested outcomes only after documenting evidence.
