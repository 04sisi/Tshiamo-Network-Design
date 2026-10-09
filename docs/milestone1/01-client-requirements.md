# 1. Client Requirements

**Project:** CMPG325-2026-003 | **Client ID:** CLI-003
**Organisation:** Tshiamo Dental & Surgery Practice (Potchefstroom)

## 1.1 Business Context

Tshiamo Dental & Surgery Practice is a healthcare facility in Potchefstroom requiring a reliable, secure computer network to support daily operations including patient administration, clinical consultations, surgical procedures, and practice management.

Because the practice handles patient records and relies on cloud-based scheduling and billing, the design prioritises three things directly: confidentiality of patient data (driving the VLAN isolation and guest ACL design in Section 3.4 of the Logical Topology document), continuity of clinical communication (driving the dedicated VoIP VLAN so phone calls are never degraded by data traffic), and uptime for cloud-dependent administrative systems (driving the dual-ISP default routing design in Section 3.3). Each major design decision traces back to one of these three operational priorities.

## 1.2 Functional Requirements

| ID | Requirement | Priority | Source |
|----|-------------|----------|--------|
| R1 | Network must be designed and simulated in Cisco Packet Tracer | Critical | Project Brief |
| R2 | Use assigned addressing block 192.168.11.0/24 | Critical | Project Brief |
| R3 | Provide appropriate connectivity and network services for the assigned scenario | Critical | Project Brief |
| R4 | VoIP traffic must be separated from data traffic | Critical | Project Brief (Sec 8) |
| R5 | Network must accommodate after-hours cleaning/security contractor wireless access (CR14) | High | Project Brief (Sec 10) |
| R6 | Implement Default Routing (edge/ISP path design) | Critical | Project Brief (Sec 9) |
| R7 | Demonstrate end-to-end connectivity and successful testing | High | Project Brief (Sec 11) |
| R8 | All significant design decisions documented in GitHub | High | Project Brief (Sec 12) |

## 1.3 Technical Requirements

- Topology: appropriate device arrangement for a small healthcare practice
- Addressing: VLSM subnetting of 192.168.11.0/24 to support VLAN segmentation
- Routing: edge router with default routes toward two ISPs (Default Routing — edge/ISP path design)
- Security: VLAN isolation, ACL-based restriction for the guest/contractor network
- Wireless: dedicated SSID for contractor access with limited privileges
- VoIP: dedicated VLAN for voice traffic, separated from data traffic

## 1.4 Design Constraints

| Constraint | Description | Mitigation |
|------------|--------------|------------|
| Address Space | Single /24 block (192.168.11.0/24) | VLSM subnetting into appropriately sized VLANs |
| VoIP Separation | VoIP traffic must be separated from data traffic | Dedicated VLAN 20 with a separate subnet and access ports |
| Contractor Access | Limited wireless access for non-staff, after hours | Dedicated VLAN 30 with ACLs restricting internal access |
| Edge/ISP Routing | Default Routing (edge/ISP path design) required | Two default routes at the edge router, one toward each ISP |

## 1.5 Client Change Request — CR14

**Request:** After-hours cleaning/security contractor requires limited wireless access.

**Accommodation Strategy:**
- Deploy a dedicated wireless network (SSID: `Tshiamo-Contractor`) on VLAN 30
- Assign contractor devices to the Guest subnet (192.168.11.192/27)
- Implement ACLs that deny all traffic from VLAN 30 to VLAN 10 (DATA) and VLAN 20 (VOIP)
- Permit traffic from VLAN 30 to the Internet only
- DHCP scope on VLAN 30 provides automatic addressing (192.168.11.200–220)
- No access to patient records, practice management systems, or internal servers
