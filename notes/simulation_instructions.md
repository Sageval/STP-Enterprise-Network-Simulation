
# Simulation Instructions – STP Enterprise Network

This guide outlines how to test and validate the functionality of the Cisco Packet Tracer simulation, including STP, VLANs, HSRP, and Layer 3 routing.

---

## 1. Open the Simulation

- File: `stp.pkt`
- Software: Cisco Packet Tracer v8.x+

---

## 2. VLANs and PCs

- VLAN 10: PC0 (192.168.10.11)
- VLAN 20: PC1 (192.168.20.12)
- VLAN 30: PC2 (192.168.30.13)
- VLAN 40: PC3 (192.168.40.100)

Each PC should use the respective virtual gateway (e.g., 192.168.10.1 for VLAN 10).

---

## 3. STP Testing

### Normal Flow:
- Root Bridge is S3.
- Path: PC0–S1–S3–PC3.
- Link between S1–S2 should be **blocked**.

### Failover Test:
- On S1, run:
  ```
  interface fa0/24
  shutdown
  ```
- Wait for ~2 seconds (Rapid-PVST).
- Traffic should reroute via S1–S2–S3.

---

## 4. Inter-VLAN Routing Test

- Ping from PC0 (VLAN 10) to PC1 (VLAN 20)
- Ping PC2 or PC3
- Verify:
  - IPs set correctly
  - Default gateways match virtual IPs (192.168.X.1)

---

## 5. HSRP Redundancy Test

### Step 1: Check current state
On Dist1:
```
show standby brief
```

You should see Dist1 as ACTIVE and Dist2 as STANDBY.

### Step 2: Failover Test
On Dist1:
```
interface vlan 10
shutdown
```
Or power off the switch entirely.

### Step 3: Verify Traffic Flow
- Ping between VLANs should continue.
- Dist2 should take over.

---

## 6. Restore Configuration

Re-enable Dist1:
```bash
interface vlan 10
no shutdown
```

Restore S1–S3 link:
```bash
interface fa0/24
no shutdown
```

---

## 7. Monitor and Verify

- `show spanning-tree vlan X`
- `show ip interface brief`
- `show standby brief`

---

## 8. Optional

- Try adding an ACL on Dist1/Dist2 to restrict inter-VLAN access.
- Add a syslog server and configure logging on switches.
- Simulate DHCP via router or switch.

---

Happy simulating!
