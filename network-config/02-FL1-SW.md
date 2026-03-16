# FL1-SW Configuration (Floor 1 Switch)

> Open the CLI tab of **FL1-SW** in Packet Tracer and paste the sections
> below **one block at a time**.

---

## 1. Enter Privileged EXEC and Global Configuration

```
enable
configure terminal
hostname FL1-SW
```

---

## 2. Create VLANs

```
vlan 10
 name Admin
exit
vlan 20
 name Developer
exit
vlan 40
 name IoT
exit
vlan 50
 name Server
exit
```

---

## 3. Trunk Port to CORE-SW

Pick the port that connects FL1-SW **up** to CORE-SW (for example
**GigabitEthernet0/1** or **FastEthernet0/24**).

```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,40,50
 no shutdown
exit
```

> **Adjust the interface name** to match the actual cable in your topology.

---

## 4. Access Ports — Admin PCs (VLAN 10)

Assign the ports connected to ADM-PC1 through ADM-PC5 and PRINTER-FL1 to
VLAN 10. Example using FastEthernet0/1–0/6:

```
interface range FastEthernet0/1 - 6
 switchport mode access
 switchport access vlan 10
 no shutdown
exit
```

| Port | Device      |
|------|-------------|
| Fa0/1 | ADM-PC1   |
| Fa0/2 | ADM-PC2   |
| Fa0/3 | ADM-PC3   |
| Fa0/4 | ADM-PC4   |
| Fa0/5 | ADM-PC5   |
| Fa0/6 | PRINTER-FL1 |

---

## 5. Access Ports — Developer PCs & Laptops (VLAN 20)

Assign the ports connected to DEV-PC1 through DEV-PC10 and DEV-LAP1 through
DEV-LAP5 to VLAN 20. Example using FastEthernet0/7–0/21:

```
interface range FastEthernet0/7 - 21
 switchport mode access
 switchport access vlan 20
 no shutdown
exit
```

---

## 6. Access Ports — IoT Cameras and Access Point (VLAN 40)

Assign the ports connected to CAM-F1-1, CAM-F1-2, and AP-F1.

```
interface range FastEthernet0/22 - 24
 switchport mode access
 switchport access vlan 40
 no shutdown
exit
```

> **AP-F1** also goes into VLAN 40 so the wireless IoT device (Door Smart
> Door) can reach the IoT subnet.

| Port   | Device   |
|--------|----------|
| Fa0/22 | CAM-F1-1 |
| Fa0/23 | CAM-F1-2 |
| Fa0/24 | AP-F1    |

> Adjust port numbers to match your physical cabling.

---

## 7. Save Configuration

```
end
write memory
```
