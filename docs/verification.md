# Verification checklist

Fill in **Observed result**, simulator version, date, and evidence path after running the actual project. The expected column describes the design; it is not proof that these tests passed.

| Test | Expected | Useful commands / checks | Observed result | Evidence |
| --- | --- | --- | --- | --- |
| VLAN membership and trunks | Correct access ports and permitted VLANs | `show vlan brief`; `show interfaces trunk` | Pending | — |
| SVI gateways and DHCP | Correct gateway and client lease per subnet | `show ip interface brief`; `show ip dhcp pool`; `show ip dhcp binding`; client IP config | Pending | — |
| Local/inter-building connectivity | Allowed flows reach destination | Host ping/traceroute; `show ip route` | Pending | — |
| OSPF areas and neighbors | Correct area per routed interface, expected neighbors and routes | `show ip ospf neighbor`; `show ip ospf interface brief`; `show ip route ospf` | Pending | — |
| WAN return path | Site-to-site reachability in both directions | `show ip route`; traceroute from each site | Pending | — |
| Production isolation | Production to Admin denied; other allowed path succeeds | Ping to test hosts; `show ip access-lists`; `show running-config interface vlan 20` | Pending | — |
| Guest isolation | Guest to internal denied; external permitted | Ping to internal and simulated external host; ACL counters | Pending | — |
| PAT | Inside clients translated at Malaysia edge | Generate external traffic; `show ip nat translations`; `show ip nat statistics` | Pending | — |
| Failover (if configured) | Alternate path maintains reachability after one link is disabled | Record baseline route/ping, disable one lab link, repeat, restore | Pending | — |

For each completed test, record the source and destination IPs, command, outcome, and one screenshot or text output under `docs/evidence/`. Remove credentials and sensitive information. Explain failed tests and any simulator limitations; do not replace the observation with the expected result.
