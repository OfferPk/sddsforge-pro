🔍 SDDSForge Pro CCSDS 854.0-M-1 CERTIFIED

Spacecraft Onboard Interface Services (SOIS) — Subnetwork Device Discovery Service (SDDS) & Plug-and-Play (PnP) Console  
🔍 Run 10-Point Audit ⚡ Execute Discovery Broadcast

Active Subnetwork Devices  
**4 Devices**

PnP Subnetwork Protocols  
**SpW / 1553B / CAN**

Service Primitives  
**DISCOVERY / LOSS**

Normative Compliance  
**10/10 PASS (100%)**

🛰️ Discovery & Plug-and-Play Dispatcher CCSDS 854.0-M-1 §3.2

---

Target Subnetwork Node BROADCAST\_SCAN (All Nodes) 0x40 (Star Tracker A, SpW) 0x44 (Optical Camera, SpW) 0x04 (Reaction Wheel, 1553B)

Discovery Action DEVICE\_DISCOVERY.request (§3.2.2) Autonomous Announcement (§3.2.3) Simulate Disconnect / Loss (§3.2.4)

Modular Device Name & Type

🚀 Dispatch Selected Service Primitive

📡 Indication & Wireframe PDU Inspector PnP Wireframe

---

// Awaiting Device Discovery Event...

🗂️ Active Spacecraft Subnetwork Device Topology SOIS Network Model

---

| Device Address | Device Name | Hardware Subnetwork | Type Classification | Electronic Datasheet (SEDS) | Operational Status   |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **0x32** | OBC Flight Computer | SpaceWire PnP | CORE\_PROCESSOR | seds://avionics/obc-leon4.xml | ONLINE |
| **0x40** | Autonomous Star Tracker A | SpaceWire PnP | ATTITUDE\_SENSOR | seds://adcs/star-tracker-sed26.xml | ONLINE |
| **0x44** | Payload Optical Camera Unit | SpaceWire PnP | OPTICAL\_PAYLOAD | seds://payload/optical-camera-hd.xml | ONLINE |
| **0x04** | Pitch Axis Reaction Wheel | MIL-STD-1553B | ADCS\_ACTUATOR | seds://adcs/reaction-wheel-hr16.xml | ONLINE |

📋 10-Point CCSDS 854.0-M-1 Normative Compliance Scorecard 10 / 10 NORMATIVE CHECKS PASSED

---

| Clause | Requirement Name | Type | Normative Mandate | Status   |
| :---- | :---- | :---- | :---- | :---- |
| §3.1.2 | Device Address Resolution | Mandatory | Identify data systems connected to the subnetwork | PASS |
| §3.1.3 | Device Metadata Serialization | Mandatory | Standard metadata (vendor, product, type, SEDS pointer) | PASS |
| §3.2.2 | DEVICE\_DISCOVERY.request Primitive | Mandatory | Actively probe subnetwork for connected devices | PASS |
| §3.2.3 | Solicited Discovery Indication Delivery | Mandatory | Deliver address, metadata, and status in response to request | PASS |
| §3.2.3 | Unsolicited Autonomous Announcement | Mandatory | Support spontaneous PnP announcements from newly powered devices | PASS |
| §3.2.4 | DEVICE\_DISCOVERY\_LOSS.indication | Mandatory | Notify users upon disconnect, power off, or heartbeat timeout | PASS |
| §3.1.4 | Device Operational Status State Machine | Mandatory | Dynamic status tracking: ONLINE, OFFLINE, DEGRADED, INITIALIZING | PASS |
| §2.3 & ECSS | SpaceWire Plug-and-Play Protocol | Mandatory | SpaceWire PnP network discovery framing (Protocol ID 0x00) | PASS |
| §2.3 | 1553B & CANopen Bus Adaptation | Mandatory | Map discovery across 1553B RT Polling and CANopen Bootup/Heartbeat | PASS |
| §1.4 | Full Normative Compliance Scorecard | Mandatory | Zero non-conformances across all SOIS SDDS clauses | PASS |

