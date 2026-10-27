# Lab 01: VLANs and Inter-VLAN Routing

**Status:** In Progress

## Objective

Design and configure a small enterprise network using Cisco Packet Tracer, implementing VLAN segmentation, 802.1Q trunking, and inter-VLAN routing.

## Network Topology

- 1 Cisco multilayer switch
- 2 Cisco access switches
- 2 PCs

## VLAN and IP Addressing Plan

| VLAN | Department | Network | Default Gateway |
|---|---|---|---|
| 10 | Accounting | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |

## Configuration Progress

### Accounting Access Switch
- Created VLAN 10 and named it ACCOUNTING.
- Assigned Fa0/1 to VLAN 10.
- Configured Fa0/2 as an 802.1Q trunk.
- Verified VLAN membership using `show vlan brief`.
- Verified trunk operation using `show interfaces trunk`.

### IT Access Switch
- Created VLAN 20 and named it IT.
- Assigned Fa0/1 to VLAN 20.
- Initially configured Fa0/3 as the trunk port.
- Identified that Fa0/2 was the actual connected uplink.
- Correcting the trunk configuration on Fa0/2.

## Troubleshooting Log

### Issue 01: Trunk Not Operational on IT Switch

**Problem:** The `show interfaces trunk` command returned no active trunks.

**Investigation:** Ran `show interfaces status` and discovered that Fa0/2 was connected while Fa0/3 was not connected.

**Root Cause:** The trunk was configured on the wrong physical interface.

**Resolution:** Reconfigure Fa0/2 as the trunk port and verify using `show interfaces trunk`.

**Lesson Learned:** Always verify physical interface connectivity before configuring switch ports. A correct configuration on the wrong interface will not establish the intended connection.

## Next Steps

- Verify the IT switch trunk is operational.
- Configure VLAN 10 and VLAN 20 on the multilayer switch.
- Configure SVIs for both VLANs.
- Enable inter-VLAN routing.
- Configure PC IP addresses and default gateways.
- Test connectivity between Accounting and IT.
- Capture final verification screenshots.
