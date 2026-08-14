# Lab 2 Connectivity Issue

## Topology

![Topology - packet sent from PC0 to PC1](packet%20send%20from%20pc%200%20to%20pc%201.png)

## What was wrong

In this lab the initial issue was the both the pc had static IPs and also the gateway of pc-a was wrong.
also the pc 0 and 1 had subnet /30 so we had to change that initially i tried to ipconfig and to see if both of the devices had a ipaddress then i tried to like ipconfig /renew (mistake on my end) because it was static ip not dhcp and the /30 subnet was not enough to have 3 devices so i had to change the subnet to /24

## Testing after the fix

after that i tried to ping from pc 1 to pc 0 it work tried arp -a it did show pc 0 internet address, physical address (mac) and also type

**Ping from PC1 to PC0 + arp -a**

![PC1 ping to PC0](pc%201%20ping%20to%20pc%200.png)

**Ping from PC0 to PC1**

![PC0 ping to PC1](pc%200%20ping%20to%20pc%201.png)

## Files

- `Connectivity issue.pkt` Packet Tracer file for this lab
