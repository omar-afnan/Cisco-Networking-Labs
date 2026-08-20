# Cisco Networking Labs

My hands-on practice labs from studying Cisco networking / CCNA, built and tested in
**Cisco Packet Tracer**.

Each lab lives in its own folder with the `.pkt` save file, screenshots of the working
result, and a `README.md` explaining the topology, the addressing plan, the steps, and
what actually happened when it was verified.

## Labs

| # | Lab | Topic | Key skills |
|---|-----|-------|-----------|
| 1 | [Basic Connectivity Between Two Switches](./Lab%201) | Switching, IPv4 addressing | Cabling, static IPs, `ping` verification, ARP, why cross-subnet traffic needs a router |
| 2 | [Connectivity Issue Troubleshooting](./Lab%202) | Troubleshooting, sub- netting | Wrong default gateway, `/30` vs `/24` masks, static vs DHCP, `ipconfig`, `ping`, `arp -a` |
| 3 | [Multi-Router Connectivity](./lab%203) | Routing, inter-VLAN | Multi-subnet topology, static routes, ARP across routers, ICMP echo request/reply, simulation mode |
| 4 | [Same-Subnet PC Communication](./Lab%204) | Subnetting, /30 networks | `/30` subnet calculations, network vs broadcast addresses, Layer 2 vs Layer 3 connectivity |
| 5 | [Switch Configuration and Management](./lab%205) | Switch CLI, management VLAN | Switch replacement, VLAN 1 IP configuration, PortFast, switch management access |

*More labs are added as I work through them.*

## How to open a lab

1. Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) (8.x), free with a Networking Academy account.
2. Clone this repo:
   ```bash
   git clone https://github.com/omar-afnan/Cisco-Networking-Labs.git
   ```
3. Open the `.pkt` file inside the lab folder you want.
4. Read that lab's `README.md` alongside it.

## Repo layout

```
Cisco-Networking-Labs/
├── Lab 1/
│   ├── Connectivity.pkt      <- Packet Tracer save file
│   ├── README.md             <- lab write-up
│   └── *.png                 <- screenshots of the results
├── Lab 2/
│   ├── Connectivity issue.pkt
│   ├── README.md
│   └── *.png
├── lab 3/
│   ├── Connectivity.pkt
│   ├── question.txt
│   └── *.png
├── Lab 4/
│   ├── [lab files]
│   ├── readme.md
│   └── *.png
├── lab 5/
│   ├── connectivity issue.pkt
│   ├── readme.md
│   └── *.png
└── README.md                 <- you are here
```

## Conventions

- Every lab folder is self-contained, with topology, config steps and evidence all in one place.
- Screenshots are included as proof the lab actually converged, not just that it was built.
- Write-ups record the **failure cases** too, since that's usually where the learning is.

## Topics covered so far

- **Layer 1:** cable selection (straight-through vs cross-over) and link status
- **Layer 2:** switch-to-switch links, broadcast domains, MAC learning, switch management VLAN
- **Layer 3:** static IPv4 addressing, subnet masks, default gateways, static routing, multi-subnet topologies
- **Subnetting:** /30 networks, network addresses, broadcast addresses, same-subnet requirements
- **Switch Configuration:** CLI access, VLAN 1 management IP, PortFast configuration
- **Troubleshooting:** `ping`, `ipconfig`, `arp -a`, Packet Tracer Simulation mode, physical connectivity issues
