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

Design, implementation and testing of a secure computer network for Tshiamo Dental & Surgery Practice in Cisco Packet Tracer: VLAN segmentation (DATA, VOIP, GUEST, MGMT), VLSM addressing of 192.168.11.0/24, dual-ISP default routing, NAT, ACL-restricted contractor wireless access (CR14), Cisco CME telephony for 7 IP phones, and an internal DNS service.

## Network Design Summary

| Component | Description |
|-----------|-------------|
| **Topology** | Router-on-a-stick with a single core switch |
| **VLANs** | 10 DATA, 20 VOIP, 30 GUEST, 99 MGMT |
| **Subnetting** | VLSM from 192.168.11.0/24 |
| **Routing** | Two default routes (one per ISP) at the edge router, with live failover verified |
| **NAT** | Overload on Gig0/1 for Internet access |
| **Security** | VLAN isolation + GUEST_RESTRICT ACL; SSH-only management |
| **Voice** | Cisco CME on Router0; 7 phones registered, calls tested |
| **Wireless** | Contractor laptop via AP0 on VLAN 30 |

## Repository Structure

- `docs/milestone1/` — Client Design Review (requirements, topologies, IP addressing) + diagrams
- `docs/milestone2/` — Implementation & Testing report
- `configs/` — Router0, Switch0, ISP1, ISP2 configurations
- `packet-tracer/` — Packet Tracer `.pkt` file
- `screenshots/testing/` — numbered testing evidence (01-20)

## Milestones

| Milestone | Description | Status | Due Date |
|-----------|-------------|--------|----------|
| Milestone 1 | Client Design Review | Submitted | 28 August 2026 |
| Milestone 2 | Implementation & Testing | Complete | 2 October 2026 |
| Final Submission | Complete portfolio & video | In Progress | 16 October 2026 |

## Key Design Decisions

1. **Router-on-a-Stick** — cost-effective and clear for demonstrating inter-VLAN routing at foundation level.
2. **VLSM Subnetting** — each VLAN sized to its device count and growth risk.
3. **Default Routing (edge/ISP path design)** — two default routes, one per ISP; failover verified by shutting down Gig0/2.
4. **Dedicated Guest VLAN + ACL** — contractors reach the Internet only.
5. **NAT and CME added during implementation** — both were needed to make the design fully functional; see the Milestone 2 report for the troubleshooting behind each.

## Academic Integrity

This is an individual project. Any AI assistance used has been documented, and I remain responsible for the correctness, understanding, verification, and academic integrity of everything submitted, per the applicable NWU AI Policy.

---
*CMPG 325 - Computer Networks | North-West University*
