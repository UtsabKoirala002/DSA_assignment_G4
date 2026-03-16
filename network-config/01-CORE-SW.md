# CORE-SW Configuration (Cisco 3560-24PS)

> Open the CLI tab of **CORE-SW** in Packet Tracer and paste the sections
> below **one block at a time**.

---

## 1. Enter Privileged EXEC and Global Configuration

```
enable
configure terminal
hostname CORE-SW
```

---

## 2. Enable IP Routing

```
ip routing
```

---

## 3. Create VLANs

```
vlan 10
 name Admin
exit
vlan 20
 name Developer
exit
vlan 30
 name Management
exit
vlan 40
 name IoT
exit
vlan 50
 name Server
exit
```

---

## 4. Configure SVI (Switch Virtual Interfaces) — VLAN Gateways

These interfaces act as the **default gateway** for every VLAN.

```
interface vlan 10
 ip address 172.16.10.1 255.255.255.224
 no shutdown
exit

interface vlan 20
 ip address 172.16.20.1 255.255.255.224
 no shutdown
exit

interface vlan 30
 ip address 172.16.30.1 255.255.255.224
 no shutdown
exit

interface vlan 40
 ip address 172.16.40.1 255.255.255.240
 no shutdown
exit

interface vlan 50
 ip address 172.16.50.1 255.255.255.248
 no shutdown
exit
```

---

## 5. Trunk Port to OFFICE-RTR (Routed Layer-3 Link)

Pick the physical port that connects CORE-SW to OFFICE-RTR (for example
**GigabitEthernet0/1**). Configure it as a **routed port** (no switchport).

```
interface GigabitEthernet0/1
 no switchport
 ip address 172.16.1.2 255.255.255.252
 no shutdown
exit
```

> **Adjust the interface name** if your cable goes into a different port.

---

## 6. Trunk Port to FL1-SW

Pick the port connected to FL1-SW (for example **FastEthernet0/1**).

```
interface FastEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,40,50
 no shutdown
exit
```

---

## 7. Trunk Port to FL2-SW

Pick the port connected to FL2-SW (for example **FastEthernet0/2**).

```
interface FastEthernet0/2
 switchport mode trunk
 switchport trunk allowed vlan 30,40,50
 no shutdown
exit
```

---

## 8. Access Port for APP-SERVER

Pick the port connected to APP-SERVER (for example **FastEthernet0/24**).

```
interface FastEthernet0/24
 switchport mode access
 switchport access vlan 50
 no shutdown
exit
```

---

## 9. DHCP Pools

DHCP is configured on CORE-SW because it owns the SVI gateways.

### Exclude Static Addresses First

```
ip dhcp excluded-address 172.16.10.1 172.16.10.5
ip dhcp excluded-address 172.16.20.1 172.16.20.5
ip dhcp excluded-address 172.16.30.1 172.16.30.5
```

> This reserves .1–.5 in each subnet for gateways, printers, and other
> static devices.

### Admin Pool (VLAN 10)

```
ip dhcp pool ADMIN-POOL
 network 172.16.10.0 255.255.255.224
 default-router 172.16.10.1
 dns-server 172.16.50.2
exit
```

### Developer Pool (VLAN 20)

```
ip dhcp pool DEV-POOL
 network 172.16.20.0 255.255.255.224
 default-router 172.16.20.1
 dns-server 172.16.50.2
exit
```

### Management Pool (VLAN 30)

```
ip dhcp pool MGT-POOL
 network 172.16.30.0 255.255.255.224
 default-router 172.16.30.1
 dns-server 172.16.50.2
exit
```

---

## 10. Fix "router ospf 1 — Invalid input detected" on the 3560

The Cisco 3560 in Packet Tracer uses a limited SDM (Switch Database Manager)
template by default that does **not** include OSPF support. You must change
the template first.

### Step A — Change the SDM Template

```
end
configure terminal
sdm prefer lanbase-routing
end
write memory
reload
```

Packet Tracer will ask you to confirm the reload — type **yes** and wait for
the switch to restart.

### Step B — After Reload, Configure OSPF

```
enable
configure terminal

router ospf 1
 router-id 2.2.2.2
 network 172.16.10.0 0.0.0.31 area 0
 network 172.16.20.0 0.0.0.31 area 0
 network 172.16.30.0 0.0.0.31 area 0
 network 172.16.40.0 0.0.0.15 area 0
 network 172.16.50.0 0.0.0.7 area 0
 network 172.16.1.0 0.0.0.3 area 0
exit
```

### Fallback — If OSPF Still Does Not Work

Use a **static default route** pointing to OFFICE-RTR instead:

```
ip route 0.0.0.0 0.0.0.0 172.16.1.1
```

---

## 11. Save Configuration

```
end
write memory
```
