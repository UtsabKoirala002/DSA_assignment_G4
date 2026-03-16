# OFFICE-RTR Configuration (Cisco 2911)

> Open the CLI tab of **OFFICE-RTR** in Packet Tracer and paste the sections
> below **one block at a time**.

---

## 1. Enter Privileged EXEC and Global Configuration

```
enable
configure terminal
hostname OFFICE-RTR
```

---

## 2. WAN Interface — Toward ISP-RTR

This is the interface that connects OFFICE-RTR to ISP-RTR using the
172.16.99.0/30 WAN link.

```
interface GigabitEthernet0/0
 ip address 172.16.99.2 255.255.255.252
 no shutdown
exit
```

> ISP-RTR's side is 172.16.99.1/30.

---

## 3. LAN Interface — Toward CORE-SW (Transit Link)

This is a **routed point-to-point link** between OFFICE-RTR and CORE-SW
using 172.16.1.0/30.

```
interface GigabitEthernet0/1
 ip address 172.16.1.1 255.255.255.252
 no shutdown
exit
```

> CORE-SW's side is 172.16.1.2/30 (see 01-CORE-SW.md, Section 5).

---

## 4. Default Route to ISP

```
ip route 0.0.0.0 0.0.0.0 172.16.99.1
```

---

## 5. OSPF Configuration

OSPF advertises the internal subnets so CORE-SW can reach the WAN and
OFFICE-RTR knows how to reach the VLANs.

```
router ospf 1
 router-id 1.1.1.1
 network 172.16.99.0 0.0.0.3 area 0
 network 172.16.1.0 0.0.0.3 area 0
 default-information originate
exit
```

> `default-information originate` tells CORE-SW (via OSPF) to use
> OFFICE-RTR as the default gateway to the internet.

---

## 6. Save Configuration

```
end
write memory
```

---

## Full IPv6 Configuration

See [05-IPv6-Config.md](05-IPv6-Config.md) for adding IPv6 addresses to
this router.
