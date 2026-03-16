# IPv6 Configuration

This section adds IPv6 addressing to **OFFICE-RTR** and shows how to
configure a sample PC.

## IPv6 Addressing Plan

| Interface / Link         | IPv6 Address               |
|--------------------------|----------------------------|
| OFFICE-RTR Gi0/0 (WAN)  | 2001:DB8:99::2/64          |
| OFFICE-RTR Gi0/1 (LAN)  | 2001:DB8:1::1/64           |
| ISP-RTR (WAN side)       | 2001:DB8:99::1/64          |
| CORE-SW VLAN 10 SVI      | 2001:DB8:10::1/64          |
| CORE-SW VLAN 20 SVI      | 2001:DB8:20::1/64          |
| CORE-SW VLAN 30 SVI      | 2001:DB8:30::1/64          |
| Sample PC (ADM-PC1)     | Auto via SLAAC or manual   |

> The `2001:DB8::/32` prefix is used for documentation purposes. Replace it
> with the prefix assigned by your instructor if different.

---

## 1. OFFICE-RTR — IPv6

Open the CLI tab of **OFFICE-RTR** and paste:

```
enable
configure terminal

ipv6 unicast-routing

interface GigabitEthernet0/0
 ipv6 address 2001:DB8:99::2/64
 ipv6 enable
 no shutdown
exit

interface GigabitEthernet0/1
 ipv6 address 2001:DB8:1::1/64
 ipv6 enable
 no shutdown
exit

ipv6 route ::/0 2001:DB8:99::1

end
write memory
```

### What Each Command Does

| Command                          | Purpose                                    |
|----------------------------------|--------------------------------------------|
| `ipv6 unicast-routing`           | Enables the router to forward IPv6 packets |
| `ipv6 address …/64`             | Assigns a global unicast IPv6 address      |
| `ipv6 enable`                    | Activates IPv6 on the interface            |
| `ipv6 route ::/0 2001:DB8:99::1`| Default IPv6 route toward ISP-RTR          |

---

## 2. CORE-SW — IPv6 on SVIs (Optional)

If your assignment requires IPv6 on the internal VLANs, add these on
**CORE-SW** after the IPv4 configuration:

```
enable
configure terminal

sdm prefer dual-ipv4-and-ipv6 default
end
write memory
reload
```

> After reload, continue:

```
enable
configure terminal

ipv6 unicast-routing

interface vlan 10
 ipv6 address 2001:DB8:10::1/64
 ipv6 enable
exit

interface vlan 20
 ipv6 address 2001:DB8:20::1/64
 ipv6 enable
exit

interface vlan 30
 ipv6 address 2001:DB8:30::1/64
 ipv6 enable
exit

interface GigabitEthernet0/1
 ipv6 address 2001:DB8:1::2/64
 ipv6 enable
exit

end
write memory
```

---

## 3. Sample PC — ADM-PC1 IPv6

### Option A — Automatic (SLAAC)

1. Click **ADM-PC1** → **Desktop** tab → **IP Configuration**.
2. Under the **IPv6 Configuration** section, select **Automatic**.
3. The PC will obtain an IPv6 address via SLAAC from the router
   advertisement sent by CORE-SW's VLAN 10 SVI.

### Option B — Manual / Static

1. Click **ADM-PC1** → **Desktop** tab → **IP Configuration**.
2. Select **Static** under IPv6.
3. Fill in:

| Field               | Value                  |
|---------------------|------------------------|
| IPv6 Address        | 2001:DB8:10::10        |
| Prefix Length       | 64                     |
| IPv6 Gateway        | 2001:DB8:10::1         |
| IPv6 DNS Server     | 2001:DB8:50::2 (or ::) |

---

## 4. Verify IPv6

On **OFFICE-RTR**:

```
show ipv6 interface brief
show ipv6 route
ping 2001:DB8:99::1
```

On **ADM-PC1** (Command Prompt):

```
ipconfig /all
ping 2001:DB8:10::1
```
