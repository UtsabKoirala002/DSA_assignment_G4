# Wireless IoT — Connect Door Smart Door to AP-F1

This section explains how to connect the **Door Smart Door** (wireless IoT
device) to the **AP-F1** access point in Cisco Packet Tracer by matching the
SSID.

---

## Step 1 — Configure AP-F1 with an SSID

1. Click **AP-F1** in the Packet Tracer workspace.
2. Go to the **Config** tab → **Port 1** (the wireless interface).
3. Set the following:

| Setting       | Value            |
|---------------|------------------|
| SSID          | SmartOffice-IoT  |
| Authentication| WPA2-PSK         |
| PSK Pass Phrase | iotpassword123 |
| Channel       | 1 (or Auto)      |

4. Make sure the **Port Status** is **On**.

> You can choose any SSID name — just make sure it **matches exactly** on
> the Door Smart Door.

---

## Step 2 — Configure the Door Smart Door

1. Click the **Door Smart Door** device in Packet Tracer.
2. Go to the **Config** tab.
3. Under **Wireless0** (or the wireless interface listed):
   - Set **SSID** to `SmartOffice-IoT` (must match AP-F1 exactly).
   - Set **Authentication** to **WPA2-PSK**.
   - Set **PSK Pass Phrase** to `iotpassword123` (must match AP-F1).
4. Under **Settings** or **Interface**, verify:

| Setting       | Value              |
|---------------|--------------------|
| IP Address    | 172.16.40.6        |
| Subnet Mask   | 255.255.255.240    |
| Default Gateway | 172.16.40.1      |

5. Make sure **IoT Server** is set to **Home Gateway** or left at default,
   depending on your Packet Tracer version.

---

## Step 3 — Verify Connectivity

1. Wait a few seconds for the wireless association to complete. You should
   see a **green wireless signal** indicator between the Door Smart Door
   and AP-F1.
2. From any PC on the network, open **Command Prompt** and try:

```
ping 172.16.40.6
```

3. On **CORE-SW**, verify the IoT VLAN is working:

```
show ip arp
show vlan brief
```

---

## Troubleshooting

| Problem                             | Fix                                        |
|-------------------------------------|--------------------------------------------|
| No wireless link shown              | Check that the SSID and PSK match exactly  |
| Door gets IP but cannot ping gateway| Verify AP-F1's switch port is in VLAN 40   |
| "Request timed out"                 | Check that VLAN 40 SVI on CORE-SW is up    |
| Door has wrong IP                   | Manually set the static IP on the device   |
