# Tshiamo-Network-Design
CMPG 325 individual network design project — Cisco Packet Tracer network for Tshiamo Dental &amp; Surgery Practice, featuring VLAN segmentation, dual-ISP default routing, and secure guest wireless access.

# CMPG 325 - Tshiamo Dental & Surgery Practice Network Design

**Student**: Batmani, Jojo
**Student Number**: 46699104
**Project ID**: CMPG325-2026-003
**Client ID**: CLI-003
**Organisation**: Tshiamo Dental & Surgery Practice (Potchefstroom)
**Industry**: Healthcare
**Assigned Challenge**: Default Routing (edge/ISP path design)
**Difficulty**: Foundational

---

## Project Overview

This repository contains the complete network design, implementation, and documentation for the Tshiamo Dental & Surgery Practice computer network. The network is designed and simulated in Cisco Packet Tracer, addressing the client's requirements for secure data connectivity, VoIP telephony, guest wireless access, and dual-ISP Internet connectivity.

## Client Requirements

- Design and simulate the network in Cisco Packet Tracer
- Use assigned addressing block: 192.168.11.0/24
- Separate VoIP traffic from data traffic
- Accommodate after-hours contractor wireless access (CR14)
- Implement Default Routing (edge/ISP path design)
- Document all design decisions and evidence in GitHub

## Network Design Summary

| Component | Description |
|-----------|-------------|
| **Topology** | Router-on-a-stick with a single core switch |
| **VLANs** | 4 VLANs (DATA, VOIP, GUEST, MGMT) |
| **Subnetting** | VLSM from 192.168.11.0/24 |
| **Routing** | Default routes to dual ISPs at the network edge |
| **Security** | VLAN isolation + ACL for the guest network |
| **Wireless** | Dedicated contractor/guest SSID |

## Repository Structure

- `docs/` — design documentation and milestone submissions
- `configs/` — device configuration files
- `packet-tracer/` — Packet Tracer simulation files
- `screenshots/` — evidence and verification screenshots

## Milestones

| Milestone | Description | Status | Due Date |
|-----------|-------------|--------|----------|
| Milestone 1 | Client Design Review | In Progress | 28 August 2026 |
| Milestone 2 | Implementation & Testing | Pending | 2 October 2026 |
| Final Submission | Complete portfolio & video | Pending | 16 October 2026 |

## Key Design Decisions

1. **Router-on-a-Stick** — selected for cost-effectiveness and clarity in demonstrating inter-VLAN routing at foundation level.
2. **VLSM Subnetting** — efficiently allocates the /24 block into appropriately sized subnets per VLAN, sized to each VLAN's actual device count and growth risk.
3. **Default Routing (edge/ISP path design)** — two default routes at the edge router, one per ISP, giving redundancy for cloud-dependent practice systems.
4. **Dedicated Guest VLAN** — isolates contractor traffic from internal systems via ACL while still providing Internet access.

## Academic Integrity

This is an individual project. All work submitted is my own. Any AI assistance used has been documented, and I remain responsible for the correctness, understanding, verification, and academic integrity of everything submitted, per the applicable NWU AI Policy.

---
*CMPG 325 - Computer Networks | North-West University*
