# Lab 01 - VLANs & Inter-VLAN Routing

## Project Overview

For my first networking project, I built a small enterprise-style network using Cisco Packet Tracer. My goal was to get hands-on experience with VLANs, trunking, and inter-VLAN routing instead of just watching videos and learning the concepts.

I created two departments, Accounting and IT, using two Cisco 2960 access switches and one Cisco 3560 multilayer switch.

Each department has its own VLAN and subnet, and the multilayer switch allows both departments to communicate.

## Network Topology

![Network Topology](01-topology.png)

### Devices Used
- 1 Cisco 3560 multilayer switch
- 2 Cisco 2960 access switches
- 2 PCs

## VLAN & IP Addressing

| Department | VLAN | IP Address | Default Gateway |
|---|---|---|---|
| Accounting | 10 | 192.168.10.10 | 192.168.10.1 |
| IT | 20 | 192.168.20.2 | 192.168.20.1 |

## Configurations

### 1. VLANs and Access Ports

I created VLAN 10 for Accounting and VLAN 20 for IT. Each PC was connected to an access port assigned to its department's VLAN.

**Accounting Switch**

```cisco
vlan 10
 name ACCOUNTING

interface fa0/1
 switchport mode access
 switchport access vlan 10
```

![Accounting VLAN](02-vlan-accounting.png)

**IT Switch**

```cisco
vlan 20
 name IT

interface fa0/1
 switchport mode access
 switchport access vlan 20
```

![IT VLAN](03-vlan-it.png)

### 2. Trunking

I configured trunk ports between both access switches and the distribution switch.

The Accounting uplink carries VLAN 10, while the IT uplink carries VLAN 20.

Example configuration from the Accounting switch:

```cisco
interface fa0/2
 switchport mode trunk
 switchport trunk allowed vlan 10
```

Both ends of each switch-to-switch connection were configured as trunks.

![Trunk Verification](04-trunk-distribution.png)

### 3. Inter-VLAN Routing

I configured Switch Virtual Interfaces (SVIs) on the multilayer switch to serve as the default gateways for both departments.

```cisco
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

ip routing
```

This allowed the distribution switch to route traffic between VLAN 10 and VLAN 20.

## Testing & Verification

After configuring the network, I used Cisco IOS commands to verify that everything was working.

- `show vlan brief` — Verified VLAN assignments.
- `show interfaces trunk` — Checked trunk connections.
- `show ip interface brief` — Verified SVI status.
- `show ip route` — Checked the routing table.
- `ping` — Tested connectivity between VLANs.

I successfully pinged the IT PC (`192.168.20.2`) from the Accounting PC (`192.168.10.10`).

![Successful Inter-VLAN Ping](05-successful-ping.png)

## Troubleshooting

I ran into two issues while building this network.

**Issue 1: Incorrect Trunk Interface**

I originally configured Fa0/3 as a trunk on the IT switch, but the actual uplink was connected to Fa0/2.

After checking my physical connections and interface configurations, I corrected the trunk port.

**Issue 2: IT PC Connectivity**

My original IT PC couldn't reach its default gateway, even after verifying the VLAN, trunk, SVI, and STP configurations.

I checked ARP, MAC address tables, and Packet Tracer Simulation Mode to investigate the issue.

I eventually replaced the original PC and assigned the new one `192.168.20.2`. Connectivity started working afterward, although I couldn't confirm the exact cause of the original problem.

## What I Learned

This lab helped me better understand:

- How VLANs separate departments into different broadcast domains.
- The difference between access ports and trunk ports.
- How 802.1Q trunking works between switches.
- How SVIs allow a multilayer switch to route between VLANs.
- How to verify configurations using Cisco IOS commands.
- How to troubleshoot connectivity issues instead of assuming the configuration is correct.

## Final Result

I successfully configured two departmental VLANs, established trunk connections, enabled inter-VLAN routing, and verified communication between both PCs.

This project gave me more confidence working with Cisco switches and troubleshooting network connectivity.

I'm continuing to build more hands-on labs as I study for the CCNA and work toward becoming a network engineer.

## Next Lab

EtherChannel & Spanning Tree Protocol (STP).
