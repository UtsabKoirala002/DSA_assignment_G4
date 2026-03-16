# Smart Office Network Configuration Guide

## Cisco Packet Tracer — DHCP, Static IPs, and IPv6

This guide provides **exact CLI commands** for configuring the Smart Office
network in Cisco Packet Tracer. Copy and paste each block into the correct
device's CLI.

---

## ⚠️ Important: About Packet Tracer Files

**Cisco Packet Tracer `.pkt` files are proprietary binary files** that can
only be created and edited inside the Packet Tracer application itself. They
cannot be generated or modified outside of the program.

**What this means for you:**

- You must **use your own existing `.pkt` file** that already has the
  network topology built (routers, switches, PCs, cables, etc.).
- This guide gives you the **exact CLI commands** to paste into each
  device's terminal inside Packet Tracer.
- You do **not** need to upload your `.pkt` file anywhere — just open it in
  Packet Tracer and follow the steps below.

## How to Use This Guide

1. **Open your `.pkt` file** in Cisco Packet Tracer.
2. **Click on a device** (e.g., CORE-SW) in the topology.
3. Go to the **CLI** tab in the device window that opens.
4. **Copy the commands** from the matching configuration file below
   (e.g., [01-CORE-SW.md](01-CORE-SW.md)) and **paste them** into the
   CLI tab, one section at a time.
5. **Repeat** for each device in the order listed in the
   [Configuration Files](#configuration-files) table.
6. After configuring all devices, use
   [07-Verification.md](07-Verification.md) to confirm everything works.

> **Tip:** Paste commands one block at a time and wait for the prompt to
> return before pasting the next block. This avoids errors from commands
> being sent too fast.

---

## Network Topology

```
ISP-RTR ──── OFFICE-RTR ──── CORE-SW (3560-24PS)
                                 │
                          ┌──────┴──────┐
                        FL1-SW        FL2-SW
                          │              │
                 Floor 1 devices   Floor 2 devices
```

## VLAN & IP Addressing Summary

| VLAN | Name       | Subnet           | Mask              | Gateway       |
|------|------------|------------------|-------------------|---------------|
| 10   | Admin      | 172.16.10.0/27   | 255.255.255.224   | 172.16.10.1   |
| 20   | Developer  | 172.16.20.0/27   | 255.255.255.224   | 172.16.20.1   |
| 30   | Management | 172.16.30.0/27   | 255.255.255.224   | 172.16.30.1   |
| 40   | IoT        | 172.16.40.0/28   | 255.255.255.240   | 172.16.40.1   |
| 50   | Server     | 172.16.50.0/29   | 255.255.255.248   | 172.16.50.1   |
| —    | WAN        | 172.16.99.0/30   | 255.255.255.252   | —             |
| —    | Transit    | 172.16.1.0/30    | 255.255.255.252   | —             |

> **Transit link** (172.16.1.0/30) connects OFFICE-RTR to CORE-SW.

## Static IP Devices (already configured on the devices)

| Device          | VLAN | IP             | Mask              | Gateway       |
|-----------------|------|----------------|-------------------|---------------|
| APP-SERVER      | 50   | 172.16.50.2/29 | 255.255.255.248   | 172.16.50.1   |
| PRINTER-FL1     | 10   | 172.16.10.3/27 | 255.255.255.224   | 172.16.10.1   |
| PRINTER-FL2     | 30   | 172.16.30.3/27 | 255.255.255.224   | 172.16.30.1   |
| CAM-F1-1        | 40   | 172.16.40.2/28 | 255.255.255.240   | 172.16.40.1   |
| CAM-F1-2        | 40   | 172.16.40.3/28 | 255.255.255.240   | 172.16.40.1   |
| CAM-F2-1        | 40   | 172.16.40.4/28 | 255.255.255.240   | 172.16.40.1   |
| CAM-F2-2        | 40   | 172.16.40.5/28 | 255.255.255.240   | 172.16.40.1   |
| Door Smart Door | 40   | 172.16.40.6/28 | 255.255.255.240   | 172.16.40.1   |

---

## Configuration Files

Apply the configurations in this order:

| # | Device      | File                                                    |
|---|-------------|---------------------------------------------------------|
| 1 | CORE-SW     | [01-CORE-SW.md](01-CORE-SW.md)                         |
| 2 | FL1-SW      | [02-FL1-SW.md](02-FL1-SW.md)                           |
| 3 | FL2-SW      | [03-FL2-SW.md](03-FL2-SW.md)                           |
| 4 | OFFICE-RTR  | [04-OFFICE-RTR.md](04-OFFICE-RTR.md)                   |
| 5 | IPv6        | [05-IPv6-Config.md](05-IPv6-Config.md)                 |
| 6 | IoT / Wi-Fi | [06-Wireless-IoT.md](06-Wireless-IoT.md)               |
| 7 | Verification| [07-Verification.md](07-Verification.md)               |

> **Note:** Adjust interface names (e.g., `FastEthernet0/1`,
> `GigabitEthernet0/1`) if the ports in your `.pkt` topology differ from
> the defaults used in these guides. Check which port each cable is
> connected to by clicking the cable in Packet Tracer.
