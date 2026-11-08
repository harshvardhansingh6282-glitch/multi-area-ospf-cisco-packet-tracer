# Multi-Area OSPF Routing Lab — Cisco Packet Tracer

## Project overview

Built and tested a multi-area OSPF network in Cisco Packet Tracer using Cisco 2911 routers. The topology contains 10 routers and four OSPF areas: Area 0 (backbone), Area 1, Area 2, and Area 3.

The lab covers IP addressing, OSPF router IDs, neighbor adjacencies, DR/BDR election, link-state advertisements (LSAs), inter-area route learning, and end-to-end connectivity testing.

> This is a simulation/lab project built in Cisco Packet Tracer, not a production network deployment.

## Objectives

- Design a multi-router topology with a shared Area 0 backbone.
- Configure OSPF process 1 and assign router IDs.
- Place routed links into the correct OSPF areas.
- Verify neighbor adjacencies and learned routes.
- Troubleshoot an inconsistent Area 0 Type 2 Network LSA.
- Validate connectivity across the OSPF areas.

## Topology and area layout

- **Area 0:** R0, R1, R4, and R7 share the backbone Ethernet segment through two interconnected switches.
- **Area 1:** R1 connects to R2 and R3.
- **Area 2:** R4 connects to R5 and R6.
- **Area 3:** R7 connects to R8 and R9.

The topology has 10 routers in total: R0 through R9.

## IP addressing plan

| Router | Interface | IP address | Connected network / peer | OSPF area |
|---|---|---:|---|---:|
| R0 | G0/0 | 10.0.0.1/24 | Shared backbone | 0 |
| R1 | G0/0 | 10.0.0.2/24 | Shared backbone | 0 |
| R4 | G0/0 | 10.0.0.3/24 | Shared backbone | 0 |
| R7 | G0/2 | 10.0.0.4/24 | Shared backbone | 0 |
| R1 | G0/1 | 10.1.12.1/30 | R2 G0/0 | 1 |
| R2 | G0/0 | 10.1.12.2/30 | R1 G0/1 | 1 |
| R1 | G0/2 | 10.1.13.1/30 | R3 G0/0 | 1 |
| R3 | G0/0 | 10.1.13.2/30 | R1 G0/2 | 1 |
| R4 | G0/1 | 10.2.45.1/30 | R5 G0/0 | 2 |
| R5 | G0/0 | 10.2.45.2/30 | R4 G0/1 | 2 |
| R4 | G0/2 | 10.2.46.1/30 | R6 G0/0 | 2 |
| R6 | G0/0 | 10.2.46.2/30 | R4 G0/2 | 2 |
| R7 | G0/0 | 10.3.78.1/30 | R8 G0/0 | 3 |
| R8 | G0/0 | 10.3.78.2/30 | R7 G0/0 | 3 |
| R7 | G0/1 | 10.3.79.1/30 | R9 G0/0 | 3 |
| R9 | G0/0 | 10.3.79.2/30 | R7 G0/1 | 3 |

## Router IDs

| Router | OSPF router ID |
|---|---|
| R0 | 1.1.1.1 |
| R1 | 11.11.11.11 |
| R2 | 22.22.22.22 |
| R3 | 3.3.3.3 |
| R4 | 4.4.4.4 |
| R5 | 5.5.5.5 |
| R6 | 6.6.6.6 |
| R7 | 7.7.7.7 |
| R8 | 8.8.8.8 |
| R9 | 9.9.9.9 |

## Example OSPF configuration

The following is the OSPF process configuration used on R1. Interface IP addresses are configured separately.

```text
router ospf 1
 router-id 11.11.11.11
 network 10.0.0.0 0.0.0.255 area 0
 network 10.1.12.0 0.0.0.3 area 1
 network 10.1.13.0 0.0.0.3 area 1
```

R1 acts as the Area 0 Designated Router (DR) on the shared broadcast backbone in the completed lab.

## Verification commands

```text
show ip interface brief
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip ospf database network
show ip route
show ip protocols
ping <destination-ip>
```

## Troubleshooting example

During verification, the Area 0 Network LSA advertised by R1 initially listed only R1 and R4 as attached routers, even though R0 and R7 showed FULL adjacency with R1.

Troubleshooting steps:
1. Checked router interface status and confirmed the backbone interfaces were up/up.
2. Checked OSPF neighbor states and confirmed R0, R4, and R7 were FULL neighbors of R1.
3. Checked CDP neighbors and MAC address tables on both interconnected switches.
4. Confirmed the two switches were connected and the backbone devices were in VLAN 1.
5. Restarted the OSPF process on R1, the DR, using `clear ip ospf process`.
6. Waited for adjacencies to reform and checked the Network LSA again.

After the OSPF process restart, the Area 0 Network LSA listed all four backbone routers:
- 1.1.1.1 (R0)
- 7.7.7.7 (R7)
- 4.4.4.4 (R4)
- 11.11.11.11 (R1)

All tested neighbors returned to FULL, and end-to-end ping tests from R0 to destinations in the other areas succeeded.

## Project outcome

- Configured a 10-router, four-area OSPF topology.
- Verified OSPF neighbor adjacencies and inter-area route learning.
- Practiced DR/BDR behavior on a broadcast network.
- Diagnosed and corrected an inconsistent Network LSA in the Packet Tracer lab.
- Verified end-to-end connectivity using ICMP ping.

## Files to add to this repository

- `README.md` — this project documentation.
- `multi-area-ospf.pkt` — your saved Cisco Packet Tracer topology file.
- `screenshots/` — screenshots of the topology, OSPF neighbor table, routing table, and successful ping tests.

## Tools

- Cisco Packet Tracer
- Cisco 2911 routers
- Cisco 2960 switches
- OSPF (multi-area)
