Lab 8 Troubleshooting Telnet Access
Objective
The objective of this lab was to configure and verify Telnet access from PC0 to Router2. The PC and router were connected on the same network, 192.168.1.0/24.

Network Addressing
Device	Interface	IP Address	Subnet Mask	Default Gateway
PC0	FastEthernet0	192.168.1.2	255.255.255.0	192.168.1.1
Router2	FastEthernet0/0	192.168.1.1	255.255.255.0	N/A
Steps Performed
We first checked whether PC0 had an IP address configured.

We configured PC0 with the following network settings:

IP address: 192.168.1.2

Subnet mask: 255.255.255.0

Default gateway: 192.168.1.1

The default gateway was configured as the IP address of Router2’s FastEthernet0/0 interface. Although both devices were in the same subnet, adding the default gateway is good network configuration practice and is needed if PC0 later communicates with a different network.

We opened the Router2 CLI and entered privileged EXEC mode:


enable
We then entered global configuration mode:


configure terminal
We selected the interface connected to PC0, which was FastEthernet0/0:


interface fast ethernet 0/0
We configured the router interface with its IP address and subnet mask:


ip address 192.168.1.1 255.255.255.0
We enabled the interface using:

![Configure Router2 Interface](Configure%20Router2%20interface.png)

no shutdown
We exited interface configuration mode and configured Telnet access through the router’s VTY lines:


exit
line vty 0 4
password cisco
login
transport input telnet
We also configured an enable secret password for privileged access:

![Configure Telnet on Router2](Configure%20Telnet%20on%20Router2.png)

enable secret class
We exited configuration mode and saved the router configuration:


end
copy running-config startup-config
Verification
Before testing Telnet, we verified basic network connectivity from PC0 using the ping command:


ping 192.168.1.1
PC0 successfully received replies from Router2, which confirmed that the IP address configuration, subnet mask, cable connection, and router interface were working correctly.

After successful ping testing, we used Telnet from PC0:


telnet 192.168.1.1
When prompted, we entered the Telnet password:
cisco
After connecting, the router displayed the user EXEC prompt:

Router2>
To access privileged EXEC mode remotely, we entered:

enable
Then we entered the enable secret password:

class
The successful final prompt was:


Router2#
Conclusion
The Telnet issue was resolved by correctly assigning IP addresses to PC0 and Router2, configuring PC0’s default gateway, enabling Router2’s FastEthernet0/0 interface, and configuring VTY lines with a password and login command. Successful ping replies confirmed connectivity, and successful Telnet access confirmed that remote router management was configured correctly. 
