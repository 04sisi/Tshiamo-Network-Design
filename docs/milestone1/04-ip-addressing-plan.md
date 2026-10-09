# 4. IP Addressing Plan

## 4.1 Subnetting Scheme (VLSM) — 192.168.11.0/24

```
192.168.11.0/24  (256 total addresses)

|-- 192.168.11.0/25    (128 addrs) --> VLAN 10 DATA   [126 usable]  .0   - .127
|-- 192.168.11.128/26  (64 addrs)  --> VLAN 20 VOIP   [62 usable]   .128 - .191
|-- 192.168.11.192/27  (32 addrs)  --> VLAN 30 GUEST  [30 usable]   .192 - .223
|-- 192.168.11.224/28  (16 addrs)  --> VLAN 99 MGMT   [14 usable]   .224 - .239
|-- 192.168.11.240/30  (4 addrs)   --> Reserved                    .240 - .243
|-- 192.168.11.244/30  (4 addrs)   --> Reserved                    .244 - .247
|-- 192.168.11.248/29  (8 addrs)   --> Reserved                    .248 - .255

VLSM verification: 128 + 64 + 32 + 16 + 4 + 4 + 8 = 256 addresses (full /24 utilised)
```

**Subnet sizing rationale:**
- VLAN 10 (DATA) receives the largest block, a /25, because it carries the most device types and the most future growth risk: workstations, the practice management server, and the printer, with room to add several more PCs without re-subnetting.
- VLAN 20 (VOIP) is sized as a /26 to exactly match one phone per current staff member with modest headroom (62 usable vs 7 phones needed), since IP phones are added in step with staff, not in bulk.
- VLAN 30 (GUEST) is sized as a /27 (30 usable) rather than something tighter, because contractor and guest wireless demand is unpredictable — multiple contractor devices, plus any future visitor access — and a guest subnet is the cheapest place to keep spare capacity, since it borders the Internet-only ACL rather than sensitive internal systems.
- VLAN 99 (MGMT) is sized as a /28 (14 usable) since only network infrastructure itself (Switch0's SVI, and any future switches or APs) needs a management address, not end-user devices.
- The remaining 16 addresses are split into two /30s and one /29 and left reserved, rather than merged into an existing VLAN, so a future point-to-point link or small expansion doesn't force re-numbering the whole block.

## 4.2 VLAN 10 — DATA Network (192.168.11.0/25)

| Device | Hostname | IP Address | Default Gateway | Notes |
|--------|----------|------------|------------------|-------|
| Router0 | Router0 | 192.168.11.1 | — | Gateway for VLAN 10 |
| Server | Server0 | 192.168.11.2 | 192.168.11.1 | Practice management server |
| Printer | Printer0 | 192.168.11.3 | 192.168.11.1 | Reception network printer |
| PC | PC-Reception | 192.168.11.10 | 192.168.11.1 | Reception workstation |
| PC | PC-Admin1 | 192.168.11.11 | 192.168.11.1 | Admin workstation 1 |
| PC | PC-Admin2 | 192.168.11.12 | 192.168.11.1 | Admin workstation 2 |
| PC | PC-Consult1 | 192.168.11.20 | 192.168.11.1 | Consulting Room 1 |
| PC | PC-Consult2 | 192.168.11.21 | 192.168.11.1 | Consulting Room 2 |
| PC | PC-Surgery | 192.168.11.30 | 192.168.11.1 | Surgery theatre workstation |
| PC | PC-Manager | 192.168.11.40 | 192.168.11.1 | Practice manager office |

## 4.3 VLAN 20 — VOIP Network (192.168.11.128/26)

| Device | Hostname | IP Address | Default Gateway | Notes |
|--------|----------|------------|------------------|-------|
| Router0 | Router0 | 192.168.11.129 | — | Gateway for VLAN 20 |
| Phone | Phone-Reception | 192.168.11.130 | 192.168.11.129 | Reception phone |
| Phone | Phone-Admin1 | 192.168.11.131 | 192.168.11.129 | Admin phone 1 |
| Phone | Phone-Admin2 | 192.168.11.132 | 192.168.11.129 | Admin phone 2 |
| Phone | Phone-Consult1 | 192.168.11.140 | 192.168.11.129 | Consulting Room 1 phone |
| Phone | Phone-Consult2 | 192.168.11.141 | 192.168.11.129 | Consulting Room 2 phone |
| Phone | Phone-Surgery | 192.168.11.150 | 192.168.11.129 | Surgery phone (critical) |
| Phone | Phone-Manager | 192.168.11.160 | 192.168.11.129 | Manager office phone |

> **Note (see Milestone 2 report):** during implementation, the phones were switched to DHCP addressing and Cisco CME was added, because the Packet Tracer 7960 model requires SCCP registration to a call manager in addition to an IP address. The static plan above reflects the original Milestone 1 design as submitted.

## 4.4 VLAN 30 — GUEST Network (192.168.11.192/27)

| Device | Hostname | IP Address | Default Gateway | Notes |
|--------|----------|------------|------------------|-------|
| Router0 | Router0 | 192.168.11.193 | — | Gateway for VLAN 30 |
| AP | AP0 | 192.168.11.194 | 192.168.11.193 | Access point (static) |
| DHCP Pool | — | 192.168.11.200–220 | 192.168.11.193 | Contractor/guest devices |
| Laptop | Laptop-Contractor | DHCP | 192.168.11.193 | After-hours contractor |

DHCP configuration (on Router0 or Server0): network 192.168.11.192/27, default router 192.168.11.193, DNS 8.8.8.8 / 8.8.4.4, lease time 8 hours (suitable for contractor shifts).

> **Note (see Milestone 2 report):** during implementation, AP0's Packet Tracer model was found to expose no management IP (it operates unmanaged), and the contractor laptop was given a static address (192.168.11.205) after DHCP on VLAN 30 would not complete. The table above is the original Milestone 1 design as submitted.

## 4.5 VLAN 99 — MANAGEMENT Network (192.168.11.224/28)

| Device | Hostname | IP Address | Default Gateway | Notes |
|--------|----------|------------|------------------|-------|
| Router0 | Router0 | 192.168.11.225 | — | Gateway for VLAN 99 |
| Switch | Switch0 | 192.168.11.226 | 192.168.11.225 | Switch management (VLAN 99 SVI) |

## 4.6 WAN Links

| Link | Local Device | Local IP | Remote Device | Remote IP |
|------|---------------|----------|----------------|-----------|
| WAN-1 | Router0 Gig0/1 | 203.0.113.2/30 | ISP1 Gig0/0 | 203.0.113.1/30 |
| WAN-2 | Router0 Gig0/2 | 198.51.100.2/30 | ISP2 Gig0/0 | 198.51.100.1/30 |

Note: WAN link addressing (203.0.113.0/30, 198.51.100.0/30) is simulated point-to-point space between Router0 and each ISP, separate from the assigned LAN block 192.168.11.0/24.

## 4.7 Switch Port Assignment

| Port | VLAN | Mode | Connected Device | Notes |
|------|------|------|-------------------|-------|
| Fa0/1 | ALL | Trunk | Router0 Gig0/0 | 802.1Q trunk |
| Fa0/2 | 10 | Access | PC-Admin1 | Admin workstation |
| Fa0/3 | 10 | Access | PC-Admin2 | Admin workstation |
| Fa0/4 | 20 | Access | Phone-Admin1 | VoIP phone |
| Fa0/5 | 20 | Access | Phone-Admin2 | VoIP phone |
| Fa0/6 | 10 | Access | PC-Reception | Reception PC |
| Fa0/7 | 20 | Access | Phone-Reception | Reception phone |
| Fa0/8 | 10 | Access | Printer0 | Network printer |
| Fa0/9 | 10 | Access | PC-Consult1 | Consulting Room 1 |
| Fa0/10 | 20 | Access | Phone-Consult1 | Consulting Room 1 |
| Fa0/11 | 10 | Access | PC-Consult2 | Consulting Room 2 |
| Fa0/12 | 20 | Access | Phone-Consult2 | Consulting Room 2 |
| Fa0/13 | 10 | Access | PC-Surgery | Surgery PC |
| Fa0/14 | 20 | Access | Phone-Surgery | Surgery phone |
| Fa0/15 | 10 | Access | PC-Manager | Manager PC |
| Fa0/16 | 20 | Access | Phone-Manager | Manager phone |
| Fa0/17 | 10 | Access | Server0 | Practice server |
| Fa0/18–20 | 99 | Access | — | Reserved management |
| Fa0/21 | 30 | Access | AP0 | Guest wireless AP |
| Fa0/22–24 | 99 | Access | — | Reserved management |
