# Lab 01 - VLANs & Inter-VLAN Routing

## Project Overview

For my first networking lab, I built a small enterprise network using Cisco Packet Tracer. My goal was to get more hands-on experience with VLANs, trunking, and inter-VLAN routing instead of just learning the concepts.

I created two departments, Accounting and IT, and placed them in separate VLANs. I used two Layer 2 access switches and one multilayer switch to allow communication between the departments.

## Network Topology

![Network Topology](screenshots/01-topology.png)

### Devices Used
- 1 Cisco 3560 multilayer switch
- 2 Cisco 2960 access switches
- 2 PCs

## VLAN & IP Addressing

| Department | VLAN | IP Address | Default Gateway |
|---|---|---|---|
| Accounting | 10 | 192.168.10.10 | 192.168.10.1 |
| IT | 20 | 192.168.20.2 | 192.168.20.1 |

## What I Configured

### 1. VLANs and Access Ports

I created VLAN 10 for Accounting and VLAN 20 for IT. Each PC was connected to an access port assigned to its department's VLAN.

Accounting switch:

```cisco
vlan 10
 name ACCOUNTING

interface fa0/1
 switchport mode access
 switchport access vlan 10
```

![Accounting VLAN](screenshots/02-vlan-accounting.png)

IT switch:

```cisco
vlan 20
 name IT

interface fa0/1
 switchport mode access
 switchport access vlan 20
```

![IT VLAN](screenshots/03-vlan-it.png)

### 2. Trunking

I configured trunk connections between both access switches and the distribution switch.

The Accounting uplink carries VLAN 10, while the IT uplink carries VLAN 20.

Example configuration:

```cisco
interface fa0/2
 switchport mode trunk
 switchport trunk allowed vlan 10
```

I also learned that both ends of a switch-to-switch connection should be configured consistently when using trunks.

![Trunk Verification](screenshots/04-trunk-distribution.png)

### 3. Inter-VLAN Routing

I configured SVIs on the multilayer switch to act as the default gateways for both departments.

```cisco
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

ip routing
```

This allowed the multilayer switch to route traffic between VLAN 10 and VLAN 20.

## Testing & Verification

After configuring the network, I used several commands to verify everything was working.

- `show vlan brief` — Checked VLAN assignments.
- `show interfaces trunk` — Verified trunk connections.
- `show ip interface brief` — Checked SVI status.
- `show ip route` — Verified connected networks.
- `ping` — Tested connectivity between departments.

I successfully pinged the IT PC (`192.168.20.2`) from the Accounting PC (`192.168.10.10`).

![Successful Ping](screenshots/05-successful-ping.png)

## Troubleshooting

I ran into a couple of issues while building this network.

**Issue 1: Incorrect Trunk Port**

I originally configured Fa0/3 as a trunk on the IT switch, but the actual connection was on Fa0/2. After checking my interfaces, I corrected the configuration.

**Issue 2: PC Connectivity**

My original IT PC couldn't reach its default gateway even after verifying the VLAN, trunk, SVI, and STP configurations.

I checked the MAC address table, ARP, and Packet Tracer Simulation Mode to investigate the issue.

I eventually replaced the original PC, assigned it `192.168.20.2`, and connectivity started working. I couldn't confirm the exact cause of the original issue.

## What I Learned

This lab helped me better understand:

- How VLANs separate departments into different broadcast domains.
- The difference between access ports and trunk ports.
- Why trunk configurations matter on switch-to-switch connections.
- How SVIs and Layer 3 switches enable inter-VLAN routing.
- How to troubleshoot connectivity using Cisco show commands.
- Why checking physical connections and endpoint configurations is important.

## Final Result

I successfully configured two VLANs, established trunk connections, enabled inter-VLAN routing, and verified communication between both departments.

This was my first completed hands-on networking project, and I plan to continue building more labs as I prepare for the CCNA and develop my networking skills.

## Next Lab

EtherChannel and Spanning Tree Protocol (STP).
