=======================Lab 9  Troubleshooting Static Routing===================
Objective
The objective of this lab was to troubleshoot a static routing problem that prevented Router2 from reaching the network 192.168.4.0/24. The network contained three routers and two PCs:


PC0 → Router2 → Router3 → Router4 → PC2
The goal was to configure the required routes so that PC0 could successfully ping PC2 at 192.168.4.2.

Network Addressing
Device	Interface	IP Address	Subnet Mask	Default Gateway
PC0	FastEthernet0	192.168.1.2	255.255.255.0	192.168.1.1
Router2	FastEthernet0/0	192.168.1.1	255.255.255.0	N/A
Router2	FastEthernet0/1	192.168.2.1	255.255.255.0	N/A
Router3	FastEthernet0/1	192.168.2.2	255.255.255.0	N/A
Router3	FastEthernet0/0	192.168.3.1	255.255.255.0	N/A
Router4	FastEthernet0/0	192.168.3.2	255.255.255.0	N/A
Router4	FastEthernet0/1	192.168.4.1	255.255.255.0	N/A
PC2	FastEthernet0	192.168.4.2	255.255.255.0	192.168.4.1
Initial Problem
A static route was configured on Router2 to reach the destination network 192.168.4.0/24, but Router2 and PC0 could not successfully ping PC2. The ping tests produced unreachable responses or request timeouts.

Troubleshooting Process
1. Checked the end-device configuration
PC0 was checked first. Its IPv4 configuration was:


IP address:       192.168.1.2
Subnet mask:      255.255.255.0
Default gateway:  192.168.1.1
PC2 was also checked:


IP address:       192.168.4.2
Subnet mask:      255.255.255.0
Default gateway:  192.168.4.1
The default gateway for each PC was the router interface directly connected to that PC’s local network.

2. Checked Router3 interfaces
On Router3, the following command was used:


show ip interface brief
The output confirmed that both interfaces were configured and operational:


FastEthernet0/0   192.168.3.1   up   up
FastEthernet0/1   192.168.2.2   up   up

![Router3 Interface Configuration](Screenshot%202026-08-24%20184655.png)
3. Checked Router3’s routing table
The following command was used:


show ip route
Initially, Router3 only had directly connected routes for:


192.168.2.0/24
192.168.3.0/24
Router3 did not have routes to the end networks 192.168.1.0/24 and 192.168.4.0/24. Therefore, it could not correctly forward traffic in both directions.

![Router3 Routing Table](Screenshot%202026-08-24%20185027.png)

4. Added static routes to Router3
Router3 was configured with a route to PC2’s network through Router4:


enable
configure terminal
ip route 192.168.4.0 255.255.255.0 192.168.3.2
A return route to PC0’s network through Router2 was also configured:


ip route 192.168.1.0 255.255.255.0 192.168.2.1
end
copy running-config startup-config
The routing table then displayed:


S    192.168.1.0/24 [1/0] via 192.168.2.1
S    192.168.4.0/24 [1/0] via 192.168.3.2
5. Tested Router3-to-Router4 connectivity
Router3 was tested with:


ping 192.168.3.2
The first ping may show one failed packet while ARP resolves the next-hop MAC address. Repeated tests showed successful connectivity.

![Router3 Ping to Router4](router%203%20ping%20to%20router%204.png)

6. Tested Router2-to-PC2 connectivity
Router2 was tested with:


ping 192.168.4.1
The result was:


!!!!!
Success rate is 100 percent (5/5)
This confirmed that Router2 could reach Router4’s interface through Router3.

7. Configured Router4’s return route
Although Router4 could receive traffic from Router2, it needed a route back to PC0’s network. The following route was added on Router4:


enable
configure terminal
ip route 192.168.1.0 255.255.255.0 192.168.3.1
end
copy running-config startup-config
This instructed Router4 to send traffic for 192.168.1.0/24 to Router3 at 192.168.3.1.

![Router4 CLI Configuration](Router4%20cli.png)
![Add Router4 Return Route](Add%20Router4's%20return%20route.png)

8. Verified Router4-to-PC0 connectivity
Router4 was tested with:


ping 192.168.1.2
The result was:


!!!!!
Success rate is 100 percent (5/5)
This confirmed that the return path from Router4 to PC0 was working.

Final Verification
The final test was performed from PC0:


ping 192.168.4.2
The expected successful path was:


PC0 → Router2 → Router3 → Router4 → PC2
PC2 → Router4 → Router3 → Router2 → PC0
Important Commands
The following commands were useful during troubleshooting:


show ip interface brief
show ip route
show running-config | include ip route
ping 192.168.3.2
ping 192.168.4.1
ping 192.168.4.2
Explanation of the Main Issue
The main issue was missing routing information on Router3 and the missing return route on Router4. A router only forwards packets when it has a route to the destination network. In addition, the destination network must have a route back to the original source network so that reply packets can return successfully.

The static routes used the next-hop router addresses:

Router3 forwarded 192.168.4.0/24 traffic to Router4 at 192.168.3.2.

Router3 forwarded 192.168.1.0/24 traffic to Router2 at 192.168.2.1.

Router4 forwarded 192.168.1.0/24 traffic to Router3 at 192.168.3.1.

Conclusion
The static routing problem was successfully resolved. PC0 and PC2 had correct IP addresses, subnet masks, and default gateways. Router3 was configured with routes to both end networks, while Router4 was given a return route to PC0’s network. The successful !!!!! ping results confirmed that traffic could travel from Router4 back to PC0 and that the network was correctly configured for end-to-end communication.
