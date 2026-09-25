# Verification checklist

| Test | Expected | Useful commands / checks | Observed result | Evidence |
| --- | --- | --- | --- | --- |
| VLAN membership and trunks | Correct access ports and permitted VLANs | `show vlan brief`; `show interfaces trunk` | good | — |
| SVI gateways and DHCP | Correct gateway and client lease per subnet | `show ip interface brief`; `show ip dhcp pool`; `show ip dhcp binding`; client IP config | good | — |
| Local/inter-building connectivity | Allowed flows reach destination | Host ping/traceroute; `show ip route` | good | — |
| OSPF areas and neighbors | Correct area per routed interface, expected neighbors and routes | `show ip ospf neighbor`; `show ip ospf interface brief`; `show ip route ospf` | good | — |
| WAN return path | Site-to-site reachability in both directions | `show ip route`; traceroute from each site | good | — |
| Production isolation | Production to Admin denied; other allowed path succeeds | Ping to test hosts; `show ip access-lists`; `show running-config interface vlan 20` | good | — |
| Guest isolation | Guest to internal denied; external permitted | Ping to internal and simulated external host; ACL counters | good | — |
| PAT | Inside clients translated at Malaysia edge | Generate external traffic; `show ip nat translations`; `show ip nat statistics` | good | — |
| Failover (if configured) | Alternate path maintains reachability after one link is disabled | Record baseline route/ping, disable one lab link, repeat, restore | good | — |


