# Fixing PC-to-PC Connectivity in Packet Tracer

## Lab Title

**Configuring IPv4 Addresses and Troubleshooting Connectivity Between Two PCs**

---

## Objective

The objective of this lab was to establish communication between two PCs connected through two Layer 2 switches in Cisco Packet Tracer.

The topology consisted of:

```text
PC0 ── Switch1 ── Switch2 ── PC1
```

Because the topology contained **only Layer 2 switches and no router**, both PCs needed to be configured within the **same IP subnet** to communicate directly.

---

## Topology

```text
+------+       +----------+       +----------+       +------+
| PC0  |-------| Switch1  |-------| Switch2  |-------| PC1  |
+------+       +----------+       +----------+       +------+
```

All physical connections were operational, indicated by the **green link indicators** in Packet Tracer. This confirmed that the physical connections and switch interfaces were functioning correctly.

---

## Initial Configuration

The PCs were initially configured with the following IPv4 addresses:

| Device | IPv4 Address | Subnet Mask           | Default Gateway |
| ------ | ------------ | --------------------- | --------------- |
| PC0    | 192.168.1.1  | 255.255.255.252 (/30) | 0.0.0.0         |
| PC1    | 192.168.1.5  | 255.255.255.252 (/30) | 0.0.0.0         |

At first, the configuration appeared reasonable because both PCs used the same subnet mask.

However, the two IP addresses actually belonged to **different /30 networks**.

---

## Problem Identified

The subnet mask `255.255.255.252` is equivalent to **/30**.

A /30 subnet contains **4 total IP addresses**:

* 1 network address
* 2 usable host addresses
* 1 broadcast address

### PC0 Network

PC0 was configured with:

```text
192.168.1.1/30
```

This address belongs to the `192.168.1.0/30` network:

| Address     | Purpose               |
| ----------- | --------------------- |
| 192.168.1.0 | Network address       |
| 192.168.1.1 | Usable host → **PC0** |
| 192.168.1.2 | Usable host           |
| 192.168.1.3 | Broadcast address     |

### PC1 Network

PC1 was configured with:

```text
192.168.1.5/30
```

This address belongs to a **different** network, `192.168.1.4/30`:

| Address     | Purpose               |
| ----------- | --------------------- |
| 192.168.1.4 | Network address       |
| 192.168.1.5 | Usable host → **PC1** |
| 192.168.1.6 | Usable host           |
| 192.168.1.7 | Broadcast address     |

Therefore:

```text
PC0 → 192.168.1.1/30 → Network 192.168.1.0/30

PC1 → 192.168.1.5/30 → Network 192.168.1.4/30
```

The PCs were on **different IP networks**.

Since the topology contained only Layer 2 switches, there was no router available to route traffic between these two networks.

---

## Solution

PC1's IPv4 address was changed from:

```text
192.168.1.5
```

to:

```text
192.168.1.2
```

The final configuration was:

| Device | IPv4 Address | Subnet Mask           | Default Gateway |
| ------ | ------------ | --------------------- | --------------- |
| PC0    | 192.168.1.1  | 255.255.255.252 (/30) | 0.0.0.0         |
| PC1    | 192.168.1.2  | 255.255.255.252 (/30) | 0.0.0.0         |

Both PCs were now members of the same network:

```text
192.168.1.0/30
```

The available host addresses were:

```text
192.168.1.1 → PC0
192.168.1.2 → PC1
```

A default gateway was **not required** because the PCs were communicating directly within the same local subnet. No router was needed.

---

## Verification

Connectivity was first tested from **PC1 to PC0** using:

```text
ping 192.168.1.1
```

The ping was successful:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed that PC1 could successfully send ICMP Echo Requests to PC0 and receive ICMP Echo Replies.

### Reverse Connectivity Test

A second test was performed from **PC0 to PC1**:

```text
ping 192.168.1.2
```

The result was also successful:

```text
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirmed that communication worked in **both directions**.

---

## Result

The connectivity issue was caused by assigning the two PCs IP addresses from **different /30 subnets**.

The problem was resolved by assigning PC1 the address `192.168.1.2`, placing both PCs in the same `192.168.1.0/30` network.

### Final Network

```text
192.168.1.0/30

Network:    192.168.1.0
PC0:        192.168.1.1
PC1:        192.168.1.2
Broadcast:  192.168.1.3
```

Both ping tests completed with **0% packet loss**, confirming successful end-to-end connectivity between PC0 and PC1.
