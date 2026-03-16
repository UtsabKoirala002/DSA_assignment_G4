# Verification Commands

Use these commands to confirm that DHCP, routing, VLANs, and IPv6 are
working correctly.

---

## 1. Verify DHCP on PCs

### On a PC (Desktop → Command Prompt)

```
ipconfig
```

Expected output (example for an Admin PC in VLAN 10):

```
IPv4 Address. . . . . . . . . : 172.16.10.6
Subnet Mask . . . . . . . . . : 255.255.255.224
Default Gateway . . . . . . . : 172.16.10.1
DNS Server  . . . . . . . . . : 172.16.50.2
```

> If the PC shows `0.0.0.0` or `169.254.x.x`, DHCP is not working. Check
> the troubleshooting section below.

### Renew DHCP Lease

If a PC does not have an IP yet, force a renewal:

```
ipconfig /release
ipconfig /renew
```

---

## 2. Verify DHCP on CORE-SW

```
enable
show ip dhcp binding
```

This shows all IP addresses the switch has handed out:

```
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
172.16.10.6      00E0.A3B1.1234          --                      Automatic
172.16.20.6      00E0.A3B1.5678          --                      Automatic
...
```

```
show ip dhcp pool
```

This shows pool statistics — how many addresses are allocated vs. available.

---

## 3. Verify VLANs

### On CORE-SW / FL1-SW / FL2-SW

```
show vlan brief
```

Expected output:

```
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------
1    default                          active
10   Admin                            active    Fa0/1, Fa0/2 ...
20   Developer                        active    Fa0/7, Fa0/8 ...
30   Management                       active    Fa0/1, Fa0/2 ...
40   IoT                              active    Fa0/22, Fa0/23 ...
50   Server                           active    Fa0/24
```

### Check Trunk Status

```
show interfaces trunk
```

Verify the trunk port lists the correct allowed VLANs.

---

## 4. Verify Routing (CORE-SW and OFFICE-RTR)

### On CORE-SW

```
show ip route
```

Look for:
- `O` routes (OSPF) or `S*` (static default) pointing to 172.16.1.1
- `C` (connected) routes for each VLAN subnet

### On OFFICE-RTR

```
show ip route
```

Look for:
- `S*` static default route via 172.16.99.1
- `O` OSPF routes for 172.16.10.0, 172.16.20.0, etc. (learned from CORE-SW)
- `C` connected routes for 172.16.99.0/30 and 172.16.1.0/30

---

## 5. Verify OSPF Adjacency

### On OFFICE-RTR or CORE-SW

```
show ip ospf neighbor
```

You should see one neighbor in **FULL** state:

```
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2         1     FULL/DR         00:00:38    172.16.1.2      Gi0/1
```

If the neighbor list is empty, OSPF adjacency has not formed. Check:
- Both devices are in `area 0`
- The network statements cover the transit link (172.16.1.0/30)
- Interfaces are `no shutdown`

---

## 6. Verify IPv6

### On OFFICE-RTR

```
show ipv6 interface brief
show ipv6 route
```

### On ADM-PC1 (Command Prompt)

```
ipconfig /all
ping 2001:DB8:10::1
```

---

## 7. End-to-End Connectivity Tests

### From any Admin PC

```
ping 172.16.10.1      ! VLAN 10 gateway
ping 172.16.20.1      ! VLAN 20 gateway (inter-VLAN)
ping 172.16.50.2      ! APP-SERVER
ping 172.16.99.1      ! ISP-RTR (through OFFICE-RTR)
```

### From a Developer PC

```
ping 172.16.20.1      ! VLAN 20 gateway
ping 172.16.10.1      ! VLAN 10 gateway (inter-VLAN)
ping 172.16.50.2      ! APP-SERVER
```

---

## 8. Troubleshooting Checklist

| Symptom                        | Check                                           |
|--------------------------------|-------------------------------------------------|
| PC gets 0.0.0.0 IP            | Verify DHCP pool exists on CORE-SW              |
| PC gets 169.254.x.x           | PC cannot reach DHCP server — check VLAN & trunk|
| Cannot ping default gateway    | Verify SVI is `no shutdown` on CORE-SW          |
| Cannot ping other VLANs        | Verify `ip routing` is enabled on CORE-SW       |
| Cannot ping ISP                | Verify OSPF or static default route on CORE-SW  |
| OSPF neighbor not forming      | Check area numbers and network statements       |
| `router ospf 1` gives error    | Run `sdm prefer lanbase-routing` and reload     |
