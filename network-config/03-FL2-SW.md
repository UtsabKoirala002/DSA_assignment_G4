# FL2-SW Configuration (Floor 2 Switch)

> Open the CLI tab of **FL2-SW** in Packet Tracer and paste the sections
> below **one block at a time**.

---

## 1. Enter Privileged EXEC and Global Configuration

```
enable
configure terminal
hostname FL2-SW
```

---

## 2. Create VLANs

```
vlan 30
 name Management
exit
vlan 40
 name IoT
exit
```

---

## 3. Trunk Port to CORE-SW

Pick the port that connects FL2-SW **up** to CORE-SW (for example
**GigabitEthernet0/1** or **FastEthernet0/24**).

```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 30,40
 no shutdown
exit
```

> **Adjust the interface name** to match the actual cable in your topology.

---

## 4. Access Ports — Management PCs & Laptops (VLAN 30)

Assign the ports connected to MGT-PC1–PC5, MGT-LAP1–LAP10, and PRINTER-FL2.

Example using FastEthernet0/1–0/16:

```
interface range FastEthernet0/1 - 16
 switchport mode access
 switchport access vlan 30
 no shutdown
exit
```

| Port    | Device      |
|---------|-------------|
| Fa0/1   | MGT-PC1     |
| Fa0/2   | MGT-PC2     |
| Fa0/3   | MGT-PC3     |
| Fa0/4   | MGT-PC4     |
| Fa0/5   | MGT-PC5     |
| Fa0/6   | MGT-LAP1    |
| Fa0/7   | MGT-LAP2    |
| Fa0/8   | MGT-LAP3    |
| Fa0/9   | MGT-LAP4    |
| Fa0/10  | MGT-LAP5    |
| Fa0/11  | MGT-LAP6    |
| Fa0/12  | MGT-LAP7    |
| Fa0/13  | MGT-LAP8    |
| Fa0/14  | MGT-LAP9    |
| Fa0/15  | MGT-LAP10   |
| Fa0/16  | PRINTER-FL2 |

---

## 5. Access Ports — IoT Cameras and Access Point (VLAN 40)

```
interface range FastEthernet0/17 - 19
 switchport mode access
 switchport access vlan 40
 no shutdown
exit
```

| Port   | Device   |
|--------|----------|
| Fa0/17 | CAM-F2-1 |
| Fa0/18 | CAM-F2-2 |
| Fa0/19 | AP-F2    |

> Adjust port numbers to match your physical cabling.

---

## 6. Save Configuration

```
end
write memory
```
