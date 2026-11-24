# ZARQA UNIVERSITY
## Faculty of Information Technology – Department of Computer Science
### Computer Networks Course – Packet Tracer Project (Phase 1: Network Analysis and Design)

---

**Project Title:** Hospital Network Design with VLAN Segmentation and Redundancy  
**Course Learning Outcome (CLO):** S1  
**Section:** 5  
**Phase:** Phase 1 – Network Analysis and Design  


---

## Executive Summary & Project Motivation

Modern healthcare facilities rely heavily on continuous, secure, and high-speed network connectivity. Any downtime in a hospital network can disrupt critical life-saving operations across Emergency, Laboratory, and Radiology departments. Furthermore, sensitive patient data—such as Electronic Medical Records (EMR) and high-resolution Picture Archiving and Communication System (PACS) radiology images—must be strictly isolated from unauthorized access.

This document outlines the **Phase 1 Network Analysis and Design** for a comprehensive enterprise hospital network simulated in **Cisco Packet Tracer**. The architecture implements strict VLAN segmentation across 16 sectors, zero-single-point-of-failure core redundancy, multi-ISP WAN connectivity, and specialized support for VoIP, CCTV, IoT environmental monitoring, and wireless guest access.

---

## Part 1: Network Requirements Analysis

Based on the hospital operational scenario, the following core networking requirements were identified:

1. **Logical Segmentation (16 Isolated VLANs):**  
   Each functional sector in the hospital must operate within its own dedicated Virtual Local Area Network (VLAN) and IP subnet to contain broadcast traffic and enforce logical boundaries. This includes 12 clinical/administrative departments plus 4 infrastructure/service sectors (CCTV, Guest, Servers, and Management).
2. **Strict Access Control Policy:**  
   Inter-VLAN communication must be tightly restricted. Only the **IT Department (VLAN 10)** is permitted to reach and ping all other subnets for administration and troubleshooting. All other departments and guest networks remain completely isolated from one another.
3. **High Availability & Core Redundancy:**  
   To ensure uninterrupted patient care, the hospital core must eliminate any single point of failure (SPOF) through redundant edge routers (`2x Cisco 2911`), active/standby firewalls (`2x Cisco ASA 5506-X`), cross-linked Layer-3 core switches (`2x Cisco 3560-24PS`), and dual-homed critical servers.
4. **Multi-Service & Multi-Site WAN Connectivity:**  
   The infrastructure must simultaneously support central enterprise servers, VoIP telephony, coaxial-based CCTV monitoring, IoT data center sensors (UPS/motion), and segregated guest Wi-Fi. Additionally, it must connect a remote branch clinic and a radiologist's home office over an MPLS backbone via three distinct Internet Service Providers (**Orange**, **Zain**, and **Umniah**) alongside 4G cellular coverage.

---

## Part 2: Proposed Network Topology & Hierarchical Architecture

The network follows a scalable **Hierarchical Enterprise Design** divided into distinct operational zones:

### 2.1 VLAN Segmentation Plan

| VLAN ID | Sector / Department Name | Role & Connected Devices | Access Policy |
| :---: | :--- | :--- | :--- |
| **10** | **IT Department** | IT Management & Support PCs, Printers | **Full Access** (Can reach & ping all VLANs) |
| **20** | **Reception** | Front-desk PCs, Reception Printers | Isolated (Access to Servers/WAN as permitted) |
| **30** | **Accounting & HR** | General/Clinic/Tax Accounting, HR PCs, Fingerprint | Isolated from other departments |
| **40** | **Laboratory** | Lab Workstations, Lab Printers | Isolated from other departments |
| **50** | **Radiology** | Radiology Doctors & Staff PCs, PACS Clients | Isolated (High-bandwidth access to PACS) |
| **60** | **Store & Services** | Medical/Stationery/Pharmacy Store PCs & Printers | Isolated from other departments |
| **70** | **Emergency (EMR)** | Emergency Room Workstations & Printers | Isolated from other departments |
| **80** | **Pharmacy** | Main & Emergency Pharmacy PCs & Printers | Isolated from other departments |
| **90** | **Clinical** | Outpatient/Inpatient Clinical Workstations | Isolated from other departments |
| **100** | **Medical Records** | Patient Records & Archive PCs & Printers | Isolated from other departments |
| **110** | **Admin & Maintenance** | Hospital Management, Secretary, Maintenance PCs | Isolated from other departments |
| **120** | **Dialysis** | Dialysis Unit Workstations & Monitoring | Isolated from other departments |
| **130** | **CCTV Surveillance** | IP Cameras, NVR Server, Coaxial Monitor TVs | Isolated Surveillance Network |
| **140** | **Guest Wi-Fi** | Visitor Laptops & Mobile Devices via WLC/APs | Internet-only (Strictly isolated behind FW) |
| **150** | **Central Servers** | DHCP, DNS, Web, File, Database, PACS, NVR | Accessible per security rules |
| **999** | **Management** | Network Infrastructure & IoT Gateway Management | Restricted to IT Admins |

---

### 2.2 Hierarchical Layers & Zones

* **WAN & ISP Backbone (MPLS):** Models global transit and three Jordanian ISPs—**Orange** (Hospital fiber/ONT), **Zain** (Branch Clinic), and **Umniah** (Doctor's Home)—plus 4G Cell Towers connected to a Central Office (`CO`).
* **Hospital Edge Layer:** `HOSPITAL-ONT` feeds `HOSPITAL-WAN-SW`, which distributes WAN links to dual edge routers (`R-HOSP-1` and `R-HOSP-2`).
* **Perimeter Security Layer:** Dual firewalls (`FW-HOSP-1` and `FW-HOSP-2`) inspect inbound/outbound traffic and maintain a direct failover link to prevent downtime. Guest Wi-Fi connects directly to the firewall zone to bypass internal LAN switches.
* **Core Distribution Layer (Layer 3):** Dual multilayer switches (`CORE-SW-1` and `CORE-SW-2`) cross-linked for redundancy, performing high-speed Inter-VLAN routing and enforcing ACL policies.
* **Access & Department Layer:** Six aggregation access switches (`ACCESS-SW-1` through `ACCESS-SW-6`) connect individual department switches (`2960-24TT`), ensuring physical and logical modularity.
* **Data Center & IoT Zone:** Dual server switches (`SW-SRV-1` & `SW-SRV-2`) provide dual-homed connections to critical servers, alongside a `DLC100` IoT Home Gateway monitoring `UPS-1`, `UPS-2`, and a `Motion Detector Sensor`.

---

## Part 3: Network Devices & Transmission Media Selection

### 3.1 Selected Hardware by OSI Layer

* **Layer 7 (Application):** Enterprise Servers (DHCP, DNS, Web, File, Database, PACS, NVR), Staff Workstations, Network Printers, and Cisco 7960 IP Phones.
* **Layers 3–7 (Security/Firewall):** `2x Cisco ASA 5506-X` (Hospital) and `1x Cisco ASA 5505` (Branch Clinic) for stateful packet filtering, DMZ/Guest isolation, and active/standby failover.
* **Layer 3 (Network):** `Cisco 2911` routers (Hospital Edge, ISP Core/Edge, Branch Clinic), `Cisco 2811` routers (dedicated VoIP/PBX voice gateways), and `2x Cisco 3560-24PS` Multilayer Switches for hardware-accelerated Inter-VLAN routing.
* **Layer 2 (Data Link):** `Cisco 2960-24TT` access/server/department switches and Wireless Access Points (`AccessPoint-PT-AC`) for MAC frame forwarding and 802.1Q VLAN tagging.
* **Layer 1 (Physical):** Copper UTP cabling, Serial DCE/DTE WAN links, Coaxial cables, RJ-11 analog phone lines, Hubs, Repeaters, and 4G Cellular Towers.

### 3.2 Devices and Transmission Media Matrix

| Component / Requirement | Selected Device(s) | Selected Medium | Technical Justification |
| :--- | :--- | :--- | :--- |
| **Internet / ISP Connection** | Bridge-PT ONT; Cisco 2911 ISP routers; 4G Cell Towers + Central Office (`CO`) | Serial (DCE/DTE) between ISP routers; Coaxial to cell towers; UTP to ONT | Connects Hospital (Orange), Clinic (Zain), and Doctor's Home (Umniah); Serial links accurately model synchronous WAN backbones. |
| **Edge Routing (Redundant)** | `2x Cisco 2911` (`R-HOSP-1/2`); `Cisco 2811` (`R-VOIP`) | Copper Straight-Through (UTP) | Routes hospital traffic to the ISP WAN switch; dual routers ensure continuous external connectivity if one router fails. |
| **Security & Failover** | `2x Cisco ASA 5506-X` (`FW-HOSP-1/2`); `ASA 5505` (`FW-Clinic`) | Copper Straight-Through (UTP) + direct FW-to-FW failover link | Filters external Internet and Guest Wi-Fi traffic from internal medical LANs; standby firewall takes over automatically upon failure. |
| **Core & Inter-VLAN Routing** | `2x Cisco 3560-24PS` Multilayer Switches (`CORE-SW-1/2`) | Copper Straight-Through (UTP) with redundant cross-links | Performs Layer-3 routing across all 16 VLANs; dual cross-linked cores eliminate single points of failure. |
| **Access, Wireless & End Links** | `2960-24TT` switches; `WRT300N`, `AP-AC`, `WLC-PT`, Repeaters; Coaxial Splitter | UTP Straight-Through & Crossover; Wi-Fi (2.4/5 GHz); Coaxial; RJ-11 | Dedicated switch per department; Crossover links similar devices (switches/hubs); Coaxial feeds CCTV monitors; RJ-11 connects analog home phone. |

---

## Part 4: Communication & Performance Analysis

1. **Bandwidth & Bottleneck Management:**  
   Most access ports on the `2960` and `3560` switches operate at FastEthernet ($10/100\text{ Mbps}$). High-volume data flows—specifically **PACS radiology imaging** and **Database queries**—must utilize GigabitEthernet ($1000\text{ Mbps}$) uplink ports to prevent congestion. Externally, ISP and Serial WAN links possess lower bandwidth than the internal LAN, making the WAN edge the primary bottleneck. Additionally, **VoIP traffic** is sensitive to latency and jitter and requires **Quality of Service (QoS)** prioritization.
2. **Transmission Impairments:**  
   Standard unshielded twisted-pair (UTP) copper cabling is restricted to a maximum segment length of **$100\text{ meters}$**, beyond which signal **attenuation** and **crosstalk** degrade data integrity. Extended horizontal runs across large hospital wings require intermediate IDF switches or fiber-optic uplinks.
3. **Electromagnetic & Wireless Interference:**  
   High-power diagnostic machines in the **Radiology Department** (such as MRI and X-ray units) generate strong **Electromagnetic Interference (EMI)** that can corrupt signals on unshielded copper cables; using shielded cabling (STP) or fiber optics is recommended in imaging rooms. In the **Guest Wi-Fi zone**, signals operate on the crowded $2.4\text{ GHz}$ band and suffer attenuation through concrete hospital walls, necessitating wireless repeaters and multiple access points managed by the WLC.
4. **Medium Access Control (MAC) Considerations:**  
   All modern switch-to-host and switch-to-switch Ethernet links operate in **Full-Duplex mode**, completely eliminating CSMA/CD collisions on wired segments. Conversely, wireless clients contend for the shared RF medium using **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)**. Finally, segmenting the network into **16 VLANs** divides the physical infrastructure into 16 independent broadcast domains, preventing broadcast storms from impacting critical medical systems.

---

## Next Implementation Phases (Roadmap)

* **Phase 2:** IPv4 Subnetting (VLSM) and configuration of all 16 VLANs across access and core switches.
* **Phase 3:** Inter-VLAN routing configuration (SVIs) on `CORE-SW-1` and `CORE-SW-2`, plus HSRP/VRRP gateway redundancy.
* **Phase 4:** Implementation of Extended Access Control Lists (ACLs) enforcing IT-only (`VLAN 10`) cross-network access.
* **Phase 5:** Firewall failover configuration, site-to-site VPN/routing for the Branch Clinic and Radiologist's Home, and VoIP QoS policies.