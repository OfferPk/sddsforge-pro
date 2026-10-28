# CCSDS 854.0-M-1 SOIS Subnetwork Device Discovery Service (SDDS) Engineering Specification

## 1\. Executive Summary & Normative Baseline

- **Document Identifier**: CCSDS 854.0-M-1 (Magenta Book, Issue 1, December 2009\)  
- **Publication Date**: December 2009  
- **Title**: Spacecraft Onboard Interface Services (SOIS) — Subnetwork Device Discovery Service (SDDS)  
- **Plug-and-Play Baseline**: ECSS-E-ST-50-52C (SpaceWire PnP), MIL-STD-1553B, CiA 301 (CANopen)  
- **Subnetwork Working Group**: Consultative Committee for Space Data Systems (CCSDS)  
- **Implementation**: SDDSForge Pro (v1.0.0)

This document formalizes the architectural principles, service primitives, device metadata attributes, Plug-and-Play (PnP) announcement mechanics, fault/loss detection rules, and subnetwork mapping protocols defined by CCSDS 854.0-M-1 for spacecraft onboard networks.

---

## 2\. Architectural Role in SOIS Reference Model (§2)

Modern modular spacecraft (NASA Artemis Gateway, ESA Earth observation satellites, commercial responsive space missions, and modular CubeSat constellations) increasingly adopt Plug-and-Play (PnP) avionics. Sensors, actuators, instruments, and payload processors are hot-swapped, powered on, or power-cycled dynamically during flight.

Without a standardized device discovery layer, spacecraft flight software requires hardcoded address lookup tables and fails to dynamically adapt when hardware is reconfigured. The SOIS Subnetwork Device Discovery Service (SDDS) resides in the SOIS Subnetwork Layer, providing a hardware-independent discovery interface:

  ┌─────────────────────────────────────────────────────────────┐

  │       Spacecraft Flight Software Application Entities       │

  │     (Onboard Topology Manager, PnP Autoconfig, FDIR)       │

  └──────────────────────────────┬──────────────────────────────┘

                                 │

                   SDDS Service Primitives (§3.2)

       \[DEVICE\_DISCOVERY / DEVICE\_DISCOVERY\_LOSS Primitives\]

                                 │

  ┌──────────────────────────────▼──────────────────────────────┐

  │     SOIS Subnetwork Device Discovery Service (SDDS)         │

  │   • Abstract Subnetwork Device Addressing (§3.1.2)          │

  │   • Standard Device Metadata & SEDS Pointer (§3.1.3)        │

  │   • Solicited Scans vs Unsolicited Announcements (§3.2.3)   │

  │   • Loss & Timeout Notification (§3.2.4)                    │

  │   • Dynamic Status Monitoring (ONLINE / OFFLINE) (§3.1.4)   │

  └──────────────────────────────┬──────────────────────────────┘

                                 │

                   Subnetwork Framing Adapters (§2.3)

                                 │

  ┌──────────────────────────────▼──────────────────────────────┐

  │                 Spacecraft Physical Subnetworks             │

  ├──────────────────────┬──────────────────────┬───────────────┤

  │    SpaceWire PnP     │    MIL-STD-1553B     │    CANopen    │

  │ (Protocol ID 0x00)   │  (RT Status Polling) │(Heartbeat 0x700)

  └──────────────────────┴──────────────────────┴───────────────┘

---

## 3\. Service Parameters & Attributes (§3.1)

The Subnetwork Device Discovery Service defines three core parameters:

1. **Device Address (§3.1.2)**: An abstract address identifying a data system connected to the subnetwork (e.g., SpaceWire logical address, MIL-STD-1553B Remote Terminal address, CAN node identifier). Supports both specific endpoint addresses and broadcast addresses (`BROADCAST_SCAN`).  
2. **Device Metadata (§3.1.3)**: A structured record describing the device discovered:  
   - **Name**: Human-readable device designation.  
   - **Vendor Identifier**: Spacecraft hardware manufacturer ID.  
   - **Product Identifier**: Subsystem model number.  
   - **Revision**: Hardware/firmware version.  
   - **Device Type**: Functional classification (e.g., `ATTITUDE_SENSOR`, `ACTUATOR`, `PAYLOAD`).  
   - **Electronic Datasheet URI**: URI referencing the device's Spacecraft Electronic Datasheet (`seds://...`) per CCSDS 876.0-B-1.  
3. **Device Status (§3.1.4)**: Operational condition of the discovered node (`ONLINE`, `INITIALIZING`, `DEGRADED`, `OFFLINE`).

---

## 4\. Service Primitives (§3.2)

CCSDS 854.0-M-1 explicitly defines three service primitives:

### 4.1 DEVICE\_DISCOVERY.request (§3.2.2)

DEVICE\_DISCOVERY.request (

    \[Target Subnetwork Device Address\]

)

- Dispatched by the service user (e.g., PnP Topology Manager) to actively scan the subnetwork for connected devices.  
- If target address is omitted or set to broadcast, initiates a full subnetwork sweep.

### 4.2 DEVICE\_DISCOVERY.indication (§3.2.3)

DEVICE\_DISCOVERY.indication (

    Device Address,

    Device Metadata,

    Device Status,

    Discovery Mode

)

- Emitted by the SDDS provider to report the presence of an active device.  
- Supports two modes:  
  - **`SOLICITED`**: Emitted in direct response to a `DEVICE_DISCOVERY.request`.  
  - **`UNSOLICITED`**: Emitted autonomously when a newly powered-on or hot-plugged device announces itself on the subnetwork.

### 4.3 DEVICE\_DISCOVERY\_LOSS.indication (§3.2.4)

DEVICE\_DISCOVERY\_LOSS.indication (

    Device Address,

    Loss Reason

)

- Emitted when an onboard device disconnects, is powered down, or fails heartbeat polling.  
- Reasons include: `DISCONNECTED`, `HEARTBEAT_TIMEOUT`, `POWER_OFF`, and `BUS_RESET`.

---

## 5\. Subnetwork Bus Protocol Mappings (§2.3)

1. **SpaceWire Plug-and-Play (ECSS-E-ST-50-52C)**:  
   - Utilizes Protocol ID `0x00` (PnP Network Discovery).  
   - Interrogates device configuration registers via RMAP read commands upon physical link connection.  
2. **MIL-STD-1553B**:  
   - Dispatches Mode Code `0x02` (Transmit Status Word) across Remote Terminal addresses (1..30) to detect responding devices.  
3. **CANopen (CiA 301\)**:  
   - Monitors autonomous Boot-up messages (COB-ID `0x700 + NodeID`, data byte `0x00`) and periodic Heartbeat / Node Guarding frames.