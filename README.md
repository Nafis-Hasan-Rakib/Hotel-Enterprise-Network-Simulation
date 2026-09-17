# Enterprise Hotel Network Infrastructure & Multi-Floor VoIP Simulation

[![Cisco Packet Tracer](https://img.shields.io/badge/Cisco%20Packet%20Tracer-v8.0%2B-049cdb?style=for-the-badge&logo=cisco&logoColor=white)](https://www.netacad.com/courses/packet-tracer)
[![Routing Protocol](https://img.shields.io/badge/Routing-OSPF%20Area%200-orange?style=for-the-badge&logo=cisco)](https://www.cisco.com)
[![VoIP Service](https://img.shields.io/badge/VoIP-Cisco%20CME%20%2F%20SCCP-blue?style=for-the-badge)](https://www.cisco.com)
[![Network Security](https://img.shields.io/badge/Security-Extended%20ACLs-red?style=for-the-badge)](https://www.cisco.com)
[![Network Scale](https://img.shields.io/badge/Scale-100%2B%20Endpoints-success?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-100%25%20Operational-brightgreen?style=for-the-badge)]()

---

## 📌 Project Overview

An enterprise-grade, multi-tier converged network infrastructure designed and simulated for a modern luxury hotel enterprise. Built on **Cisco Packet Tracer**, this architecture supports **100+ endpoints** seamlessly integrating **Data, Voice over IP (VoIP), Smart IoT Automation, and Centralized Data Center Services** across 4 physical floors, a 24/7 Security Operations Center, and a dedicated Server Farm.

The design utilizes a **Hierarchical Hub-and-Spoke / Partial Mesh WAN Topology** driven by a central **COREBACKBONE** Cisco 2811 router running dynamic **OSPF Area 0**, ensuring sub-second convergence, zero single point of failure, and strict access isolation via **Extended Access Control Lists (ACLs)**.

---

## 🗺️ Network Topology Architecture

![Enterprise Hotel Network Topology](docs/topology_overview.png)

*Figure 1: Fully converged hotel enterprise topology featuring 7 Cisco 2811 Routers, 17 Catalyst 2950 Switches, 16 Cisco 7960 IP Phones, Server Farm, IoT Surveillance, and Wireless Access Points.*

---

## 🌟 Key Technical Highlights

- **Dynamic OSPF Routing Backbone**: Area 0 single-area routing interconnecting 7 enterprise routers across high-speed point-to-point `/30` WAN serial links.
- **Enterprise VoIP Telephony**: Multi-router Cisco Unified Communications Manager Express (CME) running SCCP protocol with inter-router VoIP dial-peers (`1xx` through `6xx`) enabling seamless inter-floor voice calling.
- **VLAN Segmentation & 802.1Q Trunking**: Structured Layer-2 switched infrastructure with Voice VLAN prioritization (CoS/QoS tagged frames) and NM-ESW-16 SVI integration.
- **Smart IoT Surveillance & Automation**: Webcams, motion detectors, smart doors, and environmental sensors authenticated to a central registration server (`192.168.40.2`).
- **Extended ACL Security Hardening**: Traffic filtering blocking guest wireless subnets from sensitive Data Center resources while isolating IoT video feeds strictly to authorized Security Monitoring workstations.
- **High-Capacity DHCP Pools**: Scalable `/24` subnet pools engineered for fluctuating guest device densities, preventing IP exhaustion during peak hotel occupancy.

---

## 🏢 Floor-by-Floor Functional Distribution

| Floor / Zone | Router Hostname | Switch Infrastructure | Services & Connected Endpoints | Subnet Scope |
| :--- | :--- | :--- | :--- | :--- |
| **Core WAN** | `COREBACKBONE` | Router2(4) Mesh | Central WAN aggregator connecting all 6 branch routers | `10.0.0.0/30` - `10.0.0.36/30` |
| **Floor 1** | `Router2(3)` | Switch0, Switch2, Switch4, Switch16 | Front Desk, Guest Reception, IP Phones (Ext: 101-103), Wireless AP | `192.168.10.0/24` - `192.168.13.0/24` |
| **Security** | `Router2(3)(3)` | Switch1 | 24/7 Monitoring Station, CCTV Surveillance Console, IP Phone (Ext: 201) | `192.168.20.0/24` |
| **Floor 2** | `2ndFloor` | Switch12, Switch13, Switch14, Switch15 | Guest Suites, Lounge PCs, IP Phones (Ext: 301-304), Guest Wireless | `192.168.30.0/24` - `192.168.33.0/24` |
| **Floor 3** | `Router2(1)` | Switch3, Switch5, Switch7 | Conference Rooms, IoT Sensors, IP Phones (Ext: 401-403), Guest AP | `192.168.40.0/24` - `192.168.42.0/24` |
| **Floor 4** | `Router2` | Switch8, Switch9, Switch10 | Hotel Executive Offices, Smart IoT Surveillance, IP Phones (Ext: 501-503) | `192.168.50.0/24` - `192.168.52.0/24` |
| **Server Farm**| `Router2(3)(2)` | Switch11, Switch17 | DNS, HTTP/Web, Syslog, Central Storage, IP Phones (Ext: 601-602) | `192.168.60.0/24` |

---

## 📊 IP Addressing & WAN Allocation Matrix

### 1. WAN Serial Interconnects (Point-to-Point /30)
| WAN Serial Link | Router A Interface | Router B Interface | Subnet Network | Subnet Mask | Usable Range |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Core ↔ Floor 2** | `COREBACKBONE` (Se0/2/0) | `2ndFloor` (Se0/2/0) | `10.0.0.0` | `255.255.255.252` | `10.0.0.1 - 10.0.0.2` |
| **Core ↔ Floor 1** | `COREBACKBONE` (Se0/2/1) | `Floor 1` (Se0/2/0) | `10.0.0.4` | `255.255.255.252` | `10.0.0.5 - 10.0.0.6` |
| **Core ↔ Floor 4** | `COREBACKBONE` (Se0/3/0) | `Floor 4` (Se0/2/0) | `10.0.0.8` | `255.255.255.252` | `10.0.0.9 - 10.0.0.10` |
| **Core ↔ Floor 3** | `COREBACKBONE` (Se0/3/1) | `Floor 3` (Se0/2/0) | `10.0.0.12` | `255.255.255.252` | `10.0.0.13 - 10.0.0.14` |
| **Floor 1 ↔ Security** | `Floor 1` (Se0/2/1) | `Security` (Se0/2/0) | `10.0.0.16` | `255.255.255.252` | `10.0.0.17 - 10.0.0.18` |
| **Floor 2 ↔ Server Farm** | `2ndFloor` (Se0/2/1) | `Server Farm` (Se0/2/0) | `10.0.0.20` | `255.255.255.252` | `10.0.0.21 - 10.0.0.22` |
| **Floor 1 ↔ Floor 3** | `Floor 1` (Se0/3/0) | `Floor 3` (Se0/2/1) | `10.0.0.24` | `255.255.255.252` | `10.0.0.25 - 10.0.0.26` |
| **Floor 3 ↔ Floor 4** | `Floor 3` (Se0/3/0) | `Floor 4` (Se0/2/1) | `10.0.0.28` | `255.255.255.252` | `10.0.0.29 - 10.0.0.30` |
| **Floor 4 ↔ Server Farm** | `Floor 4` (Se0/3/0) | `Server Farm` (Se0/2/1) | `10.0.0.32` | `255.255.255.252` | `10.0.0.33 - 10.0.0.34` |
| **Core ↔ Server Farm** | `COREBACKBONE` (Fa0/0) | `Server Farm` (Fa0/1) | `10.0.0.36` | `255.255.255.252` | `10.0.0.37 - 10.0.0.38` |

### 2. Departmental & Floor Local Area Networks (/24)
| Zone / Function | Subnet Network | Subnet Mask | Default Gateway | Primary Service |
| :--- | :--- | :--- | :--- | :--- |
| **Floor 1 Front Desk LAN** | `192.168.10.0` | `255.255.255.0` | `192.168.10.1` | Reception PCs, Check-in Desks |
| **Floor 1 Staff Offices** | `192.168.11.0` | `255.255.255.0` | `192.168.11.1` | Concierge & Concierge Telephony |
| **Floor 1 Hospitality Hub** | `192.168.12.0` | `255.255.255.0` | `192.168.12.1` | Lobby Terminals & Billing Printers |
| **Floor 1 Guest Wi-Fi** | `192.168.13.0` | `255.255.255.0` | `192.168.13.1` | Public Guest Laptops & Smartphones |
| **Security Control Center** | `192.168.20.0` | `255.255.255.0` | `192.168.20.1` | Surveillance Workstations & Video Walls |
| **Floor 2 Guest Suite LAN** | `192.168.30.0` | `255.255.255.0` | `192.168.30.1` | In-Room Interactive Consoles |
| **Floor 2 Guest Wi-Fi** | `192.168.31.0` | `255.255.255.0` | `192.168.31.1` | High-density Wireless Access Point |
| **Floor 2 Business Center** | `192.168.32.0` | `255.255.255.0` | `192.168.32.1` | Executive Guest Workstations |
| **Floor 2 Staff Operations** | `192.168.33.0` | `255.255.255.0` | `192.168.33.1` | Housekeeping & Floor Management |
| **Floor 3 Conference Hub** | `192.168.40.0` | `255.255.255.0` | `192.168.40.1` | Banquet Halls & Event Systems |
| **Floor 3 Guest Wi-Fi** | `192.168.41.0` | `255.255.255.0` | `192.168.41.1` | Meeting Attendees Wireless |
| **Floor 3 Smart IoT Systems**| `192.168.42.0` | `255.255.255.0` | `192.168.42.1` | Environmental & Climate Automation |
| **Floor 4 Executive Admin** | `192.168.50.0` | `255.255.255.0` | `192.168.50.1` | General Manager & Finance Desks |
| **Floor 4 HR / Accounts** | `192.168.51.0` | `255.255.255.0` | `192.168.51.1` | Sensitive Payroll & Staff Database |
| **Floor 4 Smart Surveillance**| `192.168.52.0` | `255.255.255.0` | `192.168.52.1` | IP Webcams, Motion & Door Sensors |
| **Server Farm Data Center** | `192.168.60.0` | `255.255.255.0` | `192.168.60.1` | DNS, HTTP Web, ERP, Syslog Servers |

---

## ☎️ VoIP Dial-Peer Matrix & CME Extension Plan

Inter-router VoIP communications utilize **Cisco CallManager Express (CME)** with configured **Voice Dial-Peers** routing voice packets across OSPF WAN serial links:

```text
[Phone 101] ---> (Floor 1 CME) ---> [OSPF WAN Mesh] ---> (Floor 4 CME) ---> [Phone 501]
```

| Floor / Department | Extension Range | Dial-Peer Target IP | Routing Pattern | Protocol |
| :--- | :--- | :--- | :--- | :--- |
| **Floor 1 (Front Desk & Concierge)** | `101, 102, 103` | Local CME (`192.168.10.1`) | `1..` | SCCP / G.711 |
| **Security Operations Substation** | `201` | Router `10.0.0.18` | `2..` | VoIP Dial-Peer |
| **Floor 2 (Guest Suites & Lounge)** | `301, 302, 303, 304`| Router `10.0.0.2` | `3..` | VoIP Dial-Peer |
| **Floor 3 (Conference & Banquet)** | `401, 402, 403` | Router `10.0.0.14` | `4..` | VoIP Dial-Peer |
| **Floor 4 (Executive Management)** | `501, 502, 503` | Router `10.0.0.10` | `5..` | VoIP Dial-Peer |
| **Server Farm (IT Administration)** | `601, 602` | Router `10.0.0.22` | `6..` | VoIP Dial-Peer |

---

## 🛡️ Enterprise Network Security & Extended ACLs

To enforce strict defense-in-depth security, **Extended Access Control Lists** are deployed at distribution layer interfaces:

### 1. Server Farm Protection (`FILTER_SERVERFARM`)
- **Location**: Router2(3)(2) [Server Farm Router] applied on `FastEthernet0/0` outbound.
- **Policy**: Explicitly drops guest wireless subnets (`192.168.13.0/24`, `192.168.31.0/24`, `192.168.41.0/24`) from reaching Server Farm (`192.168.60.0/24`).
- **Permit**: Authorizes Management, Staff PCs, and VoIP phones with stateful return access.

### 2. IoT Surveillance Stream Isolation (`IOT_SURVEILLANCE_SECURITY`)
- **Location**: Router2 [Floor 4 Router] applied on `Vlan1` outbound.
- **Policy**: Restricts access to CCTV Webcams and Sensors (`192.168.52.0/24`) exclusively to the Security Operations Center (`192.168.20.0/24`) and Floor 4 Admin (`192.168.50.0/24`), dropping all external and guest requests.

---


## 🚀 How to Run & Verify the Simulation

### Prerequisites
- **Cisco Packet Tracer** version **8.0** or higher (tested on v8.2+).

### Step-by-Step Instructions
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/<YOUR-USERNAME>/hotel-enterprise-network-simulation.git
   cd hotel-enterprise-network-simulation
   ```
2. **Open Topology**:
   - Double-click `hotel_network_topology.pkt` to launch Cisco Packet Tracer.
   - Wait 30-45 seconds for Spanning Tree Protocol (STP) and OSPF neighbor adjacencies to converge (all link lights turn green).
3. **Verify Routing Convergence**:
   - Access CLI on `COREBACKBONE` or any Floor Router:
     ```cisco
     enable
     show ip route ospf
     show ip interface brief
     ```
4. **Test VoIP Inter-Floor Calling**:
   - Open GUI for Phone `101` (Floor 1) and Phone `501` (Floor 4).
   - Dial `501` on Phone `101` keypad. Verify ringing audio and active call connection.
5. **Verify Security ACLs**:
   - Ping `192.168.60.2` (Server Farm) from a Guest PC (`192.168.13.5`) -> **Destination Host Unreachable** (Blocked by ACL).
   - Ping `192.168.60.2` from Admin PC (`192.168.50.2`) -> **Reply Received** (Permitted).

---


## 📜 License

This project is open-source under the MIT License - free to use for educational, research, and portfolio reference.
