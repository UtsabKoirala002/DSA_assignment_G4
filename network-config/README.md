# Smart Office Network Configuration Guide

## Cisco Packet Tracer — DHCP, Static IPs, and IPv6

This guide provides **exact CLI commands** for configuring the Smart Office
network in Cisco Packet Tracer. Copy and paste each block into the correct
device's CLI.

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
