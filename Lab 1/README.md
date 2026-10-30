# Lab 1 Basic Connectivity Between Two Switches

A Cisco Packet Tracer lab building a small switched network and verifying end-to-end
connectivity with `ping`. The lab also shows what happens when the two hosts are moved
onto **different** subnets with no router in between.

## Topology

```
      PC0                                                PC1
 192.168.1.1/24                                   192.168.1.2/24
       |                                                 |
       |                                                 |
   [ Switch1 ] Fa0/1 -------------------- Fa0/1 [ Switch2 ]
   2960-24TT                                        2960-24TT
```

| Device  | Model        | Interface | Address        | Subnet mask     |
|---------|--------------|-----------|----------------|-----------------|
| PC0     | PC-PT        | FastEthernet0 | 192.168.1.1  | 255.255.255.0 |
| PC1     | PC-PT        | FastEthernet0 | 192.168.1.2  | 255.255.255.0 |
| Switch1 | Cisco 2960-24TT | Fa0/1 → Switch2, plus one access port to PC0 | — | — |
| Switch2 | Cisco 2960-24TT | Fa0/1 → Switch1, plus one access port to PC1 | — | — |

Switches are unconfigured (default VLAN 1) this lab is about Layer 1/2 cabling and
Layer 3 host addressing, so no switch CLI configuration is required.

## Objectives

1. Cable two 2960 switches together with a copper cross-over link.
2. Connect one PC to each switch with straight-through cable.
3. Statically address both PCs in the same `192.168.1.0/24` network.
4. Verify connectivity in both directions with `ping`.
5. Observe the failure mode when the hosts sit in different subnets.

## Steps

### 1. Build the topology

- Drop two **2960-24TT** switches and two **PC-PT** end devices onto the logical workspace.
- Connect `Switch1 Fa0/1` ⟷ `Switch2 Fa0/1` (switch-to-switch = **cross-over**).
- Connect each PC to its local switch (PC-to-switch = **straight-through**).
- Wait for the link lights to turn from amber to green (spanning-tree convergence).

### 2. Address the hosts

On each PC: **Desktop → IP Configuration → Static**

| | PC0 | PC1 |
|---|---|---|
| IPv4 Address | `192.168.1.1` | `192.168.1.2` |
| Subnet Mask  | `255.255.255.0` | `255.255.255.0` |
| Default Gateway | *(none)* | *(none)* |

No default gateway is needed — both hosts are in the same broadcast domain and subnet,
so traffic never has to leave the local network.

### 3. Verify connectivity

From **PC0 → Desktop → Command Prompt**:

```
C:\>ping 192.168.1.2
```

Then the reverse, from **PC1**:

```
C:\>ping 192.168.1.1
```

## Results

**PC0 → PC1**  4/4 replies, 0% loss:

![Ping from PC0 to PC1 succeeding with 0% packet loss](./ping%20from%20pc%200%20to%20pc%201.png)

```
Pinging 192.168.1.2 with 32 bytes of data:

Reply from 192.168.1.2: bytes=32 time=12ms TTL=128
Reply from 192.168.1.2: bytes=32 time=6ms  TTL=128
Reply from 192.168.1.2: bytes=32 time=6ms  TTL=128
Reply from 192.168.1.2: bytes=32 time=6ms  TTL=128

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 6ms, Maximum = 12ms, Average = 7ms
```

**PC1 → PC0** — 4/4 replies, 0% loss:

![Ping from PC1 back to PC0 succeeding with 0% packet loss](./Screenshot%202026-08-12%20113238.png)

```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time=14ms TTL=128
Reply from 192.168.1.1: bytes=32 time=6ms  TTL=128
Reply from 192.168.1.1: bytes=32 time=6ms  TTL=128
Reply from 192.168.1.1: bytes=32 time=6ms  TTL=128

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 6ms, Maximum = 14ms, Average = 8ms
```

The first reply in each run is slower (12–14 ms vs 6 ms) because that packet waits on the
**ARP** request/reply that resolves the destination IP to a MAC address. Once the entry is
cached, the remaining three pings go straight out.

## Extension what breaks across subnets

The topology screenshot below shows PC1 re-addressed to `192.168.2.2/24` while PC0 stays on
`192.168.1.1/24`, traced in **Simulation mode**:

![Packet Tracer topology in simulation mode with PC1 moved to 192.168.2.2/24](./Screenshot%202026-08-12%20112021.png)

The event list shows the PDU leaving PC0 and reaching Switch1 and Switch2 — the Layer 2 path
is fine — but the ping still fails. PC0 ANDs the destination `192.168.2.2` against its own
`/24` mask, concludes the destination is off-net, and looks for a default gateway. There
isn't one, so the packet never gets a next hop.

**Takeaway:** switches only move frames inside a broadcast domain. Getting between
`192.168.1.0/24` and `192.168.2.0/24` needs a **router** (or a Layer 3 switch with SVIs) and
a default gateway configured on each host.

## Files

| File | Description |
|------|-------------|
| `Connectivity.pkt` | Packet Tracer save file for this lab |
| `ping from pc 0 to pc 1.png` | PC0 → PC1 ping output |
| `Screenshot 2026-08-12 113238.png` | PC1 → PC0 ping output |
| `Screenshot 2026-08-12 112021.png` | Topology + simulation panel |

Open `Connectivity.pkt` with **Cisco Packet Tracer 8.x**.

## Commands used

| Command | Purpose |
|---------|---------|
| `ping <ip>` | Test reachability to a host |
| `ipconfig` | Show the PC's current IP, mask and gateway |
| `arp -a` | View the cached IP-to-MAC mappings |
