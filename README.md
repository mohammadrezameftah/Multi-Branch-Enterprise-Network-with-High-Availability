<img width="1414" height="839" alt="Screenshot 2026-08-11 140419" src="https://github.com/user-attachments/assets/76dd3223-4cff-4147-9b9c-08ebeb33b9c9" />

# Multi-Branch-Enterprise-Network-with-High-Availability
Multi-branch enterprise network topology in GNS3 with 5 branches, VRRP/HSRP high availability, active/standby core routers, ISP redundancy, and VLAN segmentation (VLAN 10: Users, VLAN 20: Servers, VLAN 99: Management). Ideal for practicing multi-site networking, redundancy protocols, and enterprise routing.

Project Overview
Enterprise-grade multi-branch network infrastructure designed in GNS3, featuring 5 interconnected branches with full redundancy and high availability. This project simulates a production-like environment for medium-to-large organizations, demonstrating real-world enterprise networking concepts.

Key Highlights
5 Distinct Branches – User access, Server farm, HQ core, and redundant sites

High Availability (HA) – VRRP/HSRP with Active/Standby core routers (HQ-R1 Active, HQ-R2 Standby)

ISP Redundancy – Dual ISP connections (ISP2 & ISP3) for uninterrupted internet access

VLAN Segmentation – Comprehensive VLAN design across User, Server, and Management networks

Cloud Integration – DNS and web services via cloud devices

> **Note:** CONFIGURATION.md documents the hub and BR1 as the reference spoke pattern; BR2–BR5 reuse the same spoke template with their own addressing.

