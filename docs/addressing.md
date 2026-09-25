# Addressing and routing plan

| Site | VLAN 10 Admin | VLAN 20 Production | VLAN 30 IT | VLAN 40 Guest | VLAN 50 Servers |
| --- | --- | --- | --- | --- | --- |
| Malaysia Building 1 | 10.10.10.0/24 | 10.10.20.0/24 | 10.10.30.0/24 | 10.10.40.0/24 | — |
| Malaysia Building 2 | 10.11.10.0/24 | 10.11.20.0/24 | 10.11.30.0/24 | 10.11.40.0/24 | — |
| Malaysia Building 3 | 10.12.10.0/24 | 10.12.20.0/24 | 10.12.30.0/24 | 10.12.40.0/24 | — |
| Singapore HQ | 10.1.10.0/24 | 10.1.20.0/24 | 10.1.30.0/24 | 10.1.40.0/24 | 10.1.50.0/24 |

VLAN 99 is listed as native in the report; no management subnet is specified. Example SVI gateways in the report are `10.10.10.1` and `10.1.10.1`. 


