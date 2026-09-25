# Addressing and routing plan

This map is transcribed from the group report. Compare it with the original Packet Tracer file before presenting it as implemented device state.

| Site | VLAN 10 Admin | VLAN 20 Production | VLAN 30 IT | VLAN 40 Guest | VLAN 50 Servers |
| --- | --- | --- | --- | --- | --- |
| Malaysia Building 1 | 10.10.10.0/24 | 10.10.20.0/24 | 10.10.30.0/24 | 10.10.40.0/24 | — |
| Malaysia Building 2 | 10.11.10.0/24 | 10.11.20.0/24 | 10.11.30.0/24 | 10.11.40.0/24 | — |
| Malaysia Building 3 | 10.12.10.0/24 | 10.12.20.0/24 | 10.12.30.0/24 | 10.12.40.0/24 | — |
| Singapore HQ | 10.1.10.0/24 | 10.1.20.0/24 | 10.1.30.0/24 | 10.1.40.0/24 | 10.1.50.0/24 |

VLAN 99 is listed as native in the report; no management subnet is specified. Example SVI gateways in the report are `10.10.10.1` and `10.1.10.1`. Do not assume all other gateways follow this pattern until checked. Routed transit /30 assignments are not tabulated in the report; document them from actual configs.

## OSPF correction needed

The narrative says **Area 0 = core/backbone, Area 10 = Building 1, Area 20 = Building 2, Area 30 = Building 3, Area 40 = Singapore HQ**. The report's later Area table instead maps Area 0 to Building A, Area 10 to Building B, and both Areas 20 and 30 to Building C. These conflict. Inspect `show ip ospf interface brief`, neighbors, and device configurations; publish the actual area map here after verifying it. Ensure non-backbone areas have valid connectivity to Area 0.

## WAN boundary

The report discusses internal OSPF and static routes on WAN/ISP transit. Record the configured route origins and return paths for each site. Do not describe OSPF as spanning the ISP unless the configs prove that behavior.
