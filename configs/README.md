# Device configuration exports

Export each device's `show running-config` output into a clearly named text file, for example `MY-DIST-MLS1-B1.txt`. Keep the device hostname and interface mapping consistent with `src/` and the topology. Before committing, replace WLAN pre-shared keys, passwords, SNMP communities, tokens, or other secrets with `<REDACTED>`; document any redaction in the file header. Check that redaction does not remove the VLAN, routing, ACL, DHCP, or NAT lines needed to reproduce the network. Do not add untested sample configs and label them as your originals.
