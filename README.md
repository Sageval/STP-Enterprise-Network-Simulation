# STP-Enterprise-Network-Simulation
This project simulates a Layer 2 and Layer 3 enterprise network in Cisco Packet Tracer, demonstrating switch and router redundancies.
# Spanning Tree Protocol (STP) & Enterprise Network Simulation - Cisco Packet Tracer

## 📁 Repository Structure

```
STP-Simulation/
├── README.md
├── topology/
│   ├── network_diagram.png
│   └── STP-Simulation.pkt
├── configs/
│   ├── Dist1.txt
│   ├── Dist2.txt
│   ├── S1.txt
│   ├── S2.txt
│   └── S3.txt
└── notes/
    └── simulation_instructions.md
```

---

## 📘 README.md (Summary)

### 🧠 Overview

This project simulates a Layer 2 and Layer 3 enterprise network in **Cisco Packet Tracer**, demonstrating:

* Spanning Tree Protocol (Rapid-PVST)
* VLAN segmentation
* Inter-VLAN routing (via Layer 3 switches)
* Gateway redundancy using HSRP
* Optional EtherChannel, SNMP, Syslog integrations

### 🔧 Technologies Used

* Cisco Packet Tracer (v8.x)
* Cisco IOS CLI (3560 Multilayer Switches)
* STP (802.1w - Rapid PVST)
* HSRP for Layer 3 redundancy
* VLANs, trunking, access ports

---

### 🏗️ Network Design

* 3 Access Switches (S1, S2, S3)
* 2 Distribution Layer 3 Switches (Dist1, Dist2)
* 4 End Devices:

  * PC0 - VLAN 10 (Management)
  * PC1 - VLAN 20 (Sales)
  * PC2 - VLAN 30 (Engineering)
  * PC3 - VLAN 40 (Resources)

### 🔌 VLAN and IP Scheme

| VLAN | Name        | Subnet          | Virtual Gateway |
| ---- | ----------- | --------------- | --------------- |
| 10   | Management  | 192.168.10.0/24 | 192.168.10.1    |
| 20   | Sales       | 192.168.20.0/24 | 192.168.20.1    |
| 30   | Engineering | 192.168.30.0/24 | 192.168.30.1    |
| 40   | Resources   | 192.168.40.0/24 | 192.168.40.1    |

### 🛠️ STP Configuration

* **Rapid-PVST** mode enabled on all switches
* **S3** is Root Bridge for all VLANs
* Redundant links blocked by STP dynamically

### 🔄 HSRP Redundancy

* **Dist1**: Active router for all VLANs (priority 110)
* **Dist2**: Standby router (priority 100)
* Clients use virtual IP as gateway (e.g., 192.168.10.1)

---

### 🦪 Testing Steps

1. Verify PCs in different VLANs can ping each other
2. Shut down a trunk link between S1 and S3 — STP reroutes via S2
3. Power off Dist1 — Dist2 takes over as gateway using HSRP

### 📦 Files

* `.pkt` file: Open in Cisco Packet Tracer
* `configs/`: Full CLI output from each switch
* `topology/`: Diagram image (PNG)
* `simulation_instructions.md`: Setup guide and testing notes

---

## ✅ Next Steps

* Add ACLs to segment VLAN access
* Integrate SNMP monitoring & Syslog logging
* Create NAT and DHCP simulations

---

## 📃 Simulation Instructions

### 1. Initial Setup

* Open `STP-Simulation.pkt` in Cisco Packet Tracer
* Ensure all switches and PCs are powered on

### 2. VLAN Configuration

* VLANs 10 to 40 are pre-configured on all switches
* PC0 = VLAN 10 (Management)
* PC1 = VLAN 20 (Sales)
* PC2 = VLAN 30 (Engineering)
* PC3 = VLAN 40 (Resources)

### 3. Trunk Links

* Trunk links connect all access switches (S1–S3) to both distribution switches (Dist1, Dist2)
* STP automatically blocks redundant paths (initially S1–S2)

### 4. STP Behavior Test

* On S1, shut down the interface to S3:

  ```
  interface fa0/24
  shutdown
  ```
* Wait for STP convergence (Rapid-PVST: \~2 seconds)
* Ping from PC0 to PC3 → traffic reroutes via S2 and S3

### 5. Inter-VLAN Routing

* Confirm that PCs in different VLANs (e.g., PC0 ➔ PC1) can ping each other
* Routing is handled by Dist1 and Dist2 (Layer 3 switches)

### 6. HSRP Redundancy Test

* Default gateways on PCs are the HSRP virtual IPs (e.g., 192.168.10.1)
* On Dist1, shut down VLAN interface:

  ```
  interface vlan 10
  shutdown
  ```
* Or power off Dist1
* Dist2 should take over as active HSRP router
* All devices remain reachable

### 7. Restore Connectivity

* Re-enable interfaces:

  ```
  interface vlan 10
  no shutdown
  interface fa0/24
  no shutdown
  ```

### 8. Monitor STP & HSRP

* Use these commands for verification:

  * `show spanning-tree vlan 10`
  * `show standby brief`
  * `show ip interface brief`

---



