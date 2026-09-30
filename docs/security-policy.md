# Segmentation policy

| Source | Destination | Expected | Placement |
| --- | --- | --- | --- |
| Production (VLAN 20) | Administration (VLAN 10) | Deny | Inbound on Production SVI |
| Guest (VLAN 40) | Internal company subnets | Deny | Inbound on Guest SVI |
| Guest (VLAN 40) | Simulated Internet | Permit | Guest ACL and edge route/NAT |
| IT Support (VLAN 30) | Other internal VLANs | Permit | Subject to destination and device policy |
| Other departments | Inter-VLAN destinations | Permit unless specifically restricted | SVI routing and ACLs |

An ACL statement needs explicit internal prefixes or a carefully reviewed summary. To check the implicit deny at the end of each ACL and make sure the intended external traffic remains permitted and A source-SVI ACL controls traffic entering that SVI. Inspect reverse-direction behavior and stateful expectations separately.
