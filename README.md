# 🏥 Hospital Network Design with VLAN Segmentation & High Availability

![Cisco Packet Tracer](https://img.shields.io/badge/Simulator-Cisco%20Packet%20Tracer-049FD9?style=for-the-badge&logo=cisco&logoColor=white)
![Phase](https://img.shields.io/badge/Project%20Phase-Phase%201%20(Analysis%20%26%20Design)-2EA44F?style=for-the-badge)
![VLANs](https://img.shields.io/badge/Segmentation-16%20Isolated%20VLANs-blue?style=for-the-badge)
![Redundancy](https://img.shields.io/badge/Architecture-Zero%20SPOF%20Redundancy-orange?style=for-the-badge)

> **Course:** Computer Networks – Packet Tracer Project (Phase 1)  
> **Institution:** Zarqa University – Faculty of Information Technology (Department of Computer Science)  
> **Section:** 5

---

## 📋 Overview

This repository contains the **Phase 1 Network Analysis, Hierarchical Design, and Topology Documentation** for a multi-site healthcare network infrastructure modeled in **Cisco Packet Tracer**. 

The design prioritizes **patient safety**, **data privacy**, and **zero-downtime reliability** by isolating 16 hospital sectors into dedicated VLANs, deploying fully redundant core/edge hardware, and integrating remote branch sites across three Internet Service Providers.

---

## 🎯 Key Architectural Highlights

* **16-VLAN Logical Segmentation:** Complete separation of clinical, administrative, surveillance, server, management, and guest networks.
* **Zero-Trust Inter-VLAN Access Policy:** Only **VLAN 10 (IT Department)** can reach and ping all other subnets; all other departments remain strictly isolated.
* **Zero Single Point of Failure (SPOF):**
  * Dual Edge Routers (`2x Cisco 2911`)
  * Dual Active/Standby Firewalls (`2x Cisco ASA 5506-X`) with dedicated failover link
  * Dual Cross-Linked Layer-3 Core Switches (`2x Cisco 3560-24PS`)
  * Dual-Homed Enterprise Servers connected to dual server switches (`SW-SRV-1` & `SW-SRV-2`)
* **Multi-Site & Multi-ISP WAN:** Connects the main hospital (**Orange**), a remote branch clinic (**Zain**), a radiologist's home office (**Umniah**), and mobile devices (**4G Cellular**) over an MPLS backbone.
* **Specialized Healthcare Services:** Dedicated VoIP telephony (`Cisco 2811` + `7960 IP Phones`), Coaxial CCTV monitoring, Data Center IoT environmental sensors (`UPS` & `Motion Detector`), and WLC-managed Guest Wi-Fi.

---

## 🗂️ VLAN Segmentation Table

| VLAN ID | Department / Sector | Primary Function | Access Control Rule |
| :---: | :--- | :--- | :--- |
| **10** | **IT Department** | Network Administration & Support | **Permitted** to reach/ping all VLANs |
| **20** | **Reception** | Patient Check-in & Front Desk | Isolated |
| **30** | **Accounting & HR** | Finance, Tax, HR & Biometrics | Isolated |
| **40** | **Laboratory** | Diagnostic Lab Workstations | Isolated |
| **50** | **Radiology** | Imaging & PACS Workstations | Isolated |
| **60** | **Store & Services** | Medical & General Inventory | Isolated |
| **70** | **Emergency (EMR)** | Critical Care & Emergency Rooms | Isolated |
| **80** | **Pharmacy** | Main & Emergency Dispensaries | Isolated |
| **90** | **Clinical** | Outpatient/Inpatient Clinics | Isolated |
| **100** | **Medical Records** | Patient Files & Archiving | Isolated |
| **110** | **Admin & Maintenance** | Executive Management & Facility Ops | Isolated |
| **120** | **Dialysis** | Renal Care Unit | Isolated |
| **130** | **CCTV** | IP Cameras, NVR & Coaxial Monitors | Isolated |
| **140** | **Guest Wi-Fi** | Visitor Wireless Access (Behind FW) | Internet Only |
| **150** | **Servers** | DHCP, DNS, Web, File, DB, PACS, NVR | Controlled Access |
| **999** | **Management** | Switch/Router & IoT Gateway Mgmt | IT Admin Only |

---

## 🏗️ Topology & Hardware Stack

### Core & Aggregation Infrastructure
* **Routers:** `Cisco 2911` (Hospital Edge, ISP Core/Edge, Branch Clinic), `Cisco 2811` (VoIP / PBX Gateways)
* **Firewalls:** `2x Cisco ASA 5506-X` (Hospital Perimeter), `1x Cisco ASA 5505` (Branch Clinic)
* **Multilayer Core Switches:** `2x Cisco 3560-24PS` (Layer-3 Inter-VLAN Routing)
* **Access & Server Switches:** `Cisco 2960-24TT` (6 Access Switches, 2 Server Switches, Dedicated Department Switches)
* **Wireless & IoT:** `WLC-PT`, `AccessPoint-PT-AC`, `WRT300N`, `Repeater-PT`, `DLC100` IoT Gateway, `4G Cell Towers`

### Transmission Media
* **Copper UTP (Straight-Through & Crossover):** Internal LAN, server uplinks, and inter-switch links.
* **Serial (DCE/DTE):** WAN backbone links between ISP routers.
* **Coaxial:** CCTV video distribution splitter to monitors & cellular tower backhaul.
* **RJ-11 Phone Line:** Analog telephony connection at the radiologist's home office.
* **Wireless (2.4/5 GHz & 4G):** Guest Wi-Fi, IoT sensors, and cellular mobile endpoints.

---

## 📁 Repository Structure

```text
├── docs/
│   ├── Computer_Networks_Project_Phase1_TEAM_v2_2.pdf   # Official Phase 1 Submission Form
│   ├── Hospital_Network_Phase1_Documentation.md         # Full Technical Documentation (DOCX Ready)
│   └── Hospital_Network_Phase1_3.pptx                   # Project Presentation Slides
├── topology/
│   ├── hospital_network_phase1.pkt                      # Cisco Packet Tracer Simulation File
│   └── hospital_topology_diagram.png                    # Full High-Resolution Topology Export
└── README.md
```

---

## 🚀 Upcoming Phases (Roadmap)

- [x] **Phase 1:** Requirements Analysis, Hierarchical Topology Design, Device & Media Selection, Communication Analysis
- [ ] **Phase 2:** VLSM IP Addressing Scheme & VLAN Trunking Configuration
- [ ] **Phase 3:** Layer-3 Inter-VLAN Routing & HSRP Gateway Redundancy
