# Segmentation policy

The policy below reflects the academic report. Verify both allowed and denied paths in the Packet Tracer topology, including remote-site prefixes.

| Source | Destination | Expected | Placement |
| --- | --- | --- | --- |
| Production (VLAN 20) | Administration (VLAN 10) | Deny | Inbound on Production SVI |
| Guest (VLAN 40) | Internal company subnets | Deny | Inbound on Guest SVI |
| Guest (VLAN 40) | Simulated Internet | Permit | Guest ACL and edge route/NAT |
| IT Support (VLAN 30) | Other internal VLANs | Permit | Subject to destination and device policy |
| Other departments | Inter-VLAN destinations | Permit unless specifically restricted | SVI routing and ACLs |

An ACL statement needs explicit internal prefixes or a carefully reviewed summary. Check the implicit deny at the end of each ACL and make sure the intended external traffic remains permitted. A source-SVI ACL controls traffic entering that SVI; inspect reverse-direction behavior and stateful expectations separately. Do not publish live credentials, internal production addresses, or real security policies.
