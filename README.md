# CAS Solutions – Network Security Implementation

A network security design and Cisco Packet Tracer implementation for CAS Solutions, a small
company with a head office in Colombo and branches in Matara and Kandy. The project moves the
network from a basic setup to one with segmentation, access control and logging.

## Network design
The secure design is in `design/network_design.drawio`. It separates departments into VLANs
(Management, HR, IT, DEV, Server), using 192.168.10.0, 192.168.11.0 and 192.168.12.0 for the
main branch networks, as the brief requires.

## What is configured in the Packet Tracer file (`Topology.pkt`)

| Security measure | Where | What it does |
|---|---|---|
| Network segmentation | VLANs for Management, HR, IT, DEV and Server | Keeps departments separate |
| AAA | `R-Colombo` | Uses `aaa new-model` with local login and local exec authorisation. Only valid users can log in |
| Syslog | `R-Colombo` | Sends logs to the syslog server at 10.1.1.10 |
| Port security | `SW-DEV`, interface Fa0/3 | Port security is enabled, violation mode is restrict, and the maximum is 2 MAC addresses |
| ACL | `R-Colombo` | Extended access lists 110–113 and 120–123 control traffic between subnets |
| Firewall | Kandy branch | A Cisco ASA 5506-X firewall protects the Kandy network |

## Testing
| Test | Expected result | Result |
|---|---|---|
| Log in to `R-Colombo` with the correct username and password | Access granted | Pass |
| Log in with a user not in the local database | Access denied (`% Login invalid`) | Pass |
| Check syslog configuration (`show running-config \| include logging`) | Logging to 10.1.1.10 | Pass |
| Check port security on SW-DEV Fa0/3 (`show port-security interface`) | Enabled, secure-up, restrict | Pass |


Fill in each result from your own test, then remove any row you can't confirm.

## Commands used
- `show access-lists` and `show ip access-lists`: check the ACL rules
- `show running-config | include aaa`: check the AAA setup
- `show logging`: check that logging is working

## What I'd improve next
- Add an explicit `deny` at the end of each access list, so unmatched traffic is blocked
- Apply each access list to the correct interface and direction, and confirm this in the configuration
- Add a VPN for remote branch connections, if not already done
- Use encrypted passwords and an AAA server, instead of local accounts
- Keep the syslog server and the router clocks in sync, so log timestamps match
