Telnet Troubleshooting Lab
In this exercise, we connected PC0 to Router2 using the existing Ethernet connection. The purpose of the lab was to configure the router so that it requests a password before allowing a Telnet connection.

First, we checked whether PC0 had a valid IP address. Since static addressing was used, PC0 was configured with the following network settings:


IP address:       192.168.1.2
Subnet mask:      255.255.255.0
Default gateway:  192.168.1.1
The default gateway was set to the IP address of Router2’s FastEthernet interface.

Next, Router2 was configured through its CLI. The FastEthernet0/0 interface was assigned the IP address 192.168.1.1 and enabled using the no shutdown command. The VTY lines were then configured with a password and the login command. The login command is important because it forces the router to prompt the remote user for the configured VTY password.


enable
configure terminal

interface fastethernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

line vty 0 4
password cisco
login
transport input telnet
end

copy running-config startup-config
After configuring the router, we returned to PC0 and opened the Command Prompt. First, we tested connectivity between PC0 and Router2 using the ping command:


ping 192.168.1.1
The ping test was successful, with four replies received and 0% packet loss. This confirmed that the physical connection, PC IP configuration, default gateway, and Router2 interface configuration were correct.

Finally, we attempted to establish a Telnet connection from PC0 to Router2 using:


telnet 192.168.1.1
The router successfully opened the Telnet connection and displayed the following password 


User Access Verification

Password:
This confirmed that the VTY password configuration was successful. However, Packet Tracer repeatedly displayed a password timeout error before the password could be entered successfully:


% Password: timeout expired!
[Connection to 192.168.1.1 closed by foreign host]
Despite the timeout during password entry, the router configuration was verified using:


show running-config | section line vty
The output confirmed that the VTY configuration was correct:


line vty 0 4
 password cisco
 login
 transport input telnet
Therefore, the main objective of the lab was achieved: Router2 was configured to require the password cisco before permitting Telnet access, and the Telnet password prompt was displayed successfully.
