# Enterprise Network Plan

This project involves the design and implementation of a structured enterprise network using Cisco Packet Tracer. The goal was to create a scalable, secure, and organized network infrastructure separating different departments into their own Virtual Local Area Networks (VLANs).

## Project Overview

The network is designed to support multiple departments, ensuring traffic isolation and efficient management. The core of the implementation revolves around a Cisco 3560 multilayer switch and a router to handle inter-VLAN routing and external connectivity.

## Key Implementations

### 1. VLAN Segmentation
Different departments were assigned to specific VLANs to improve security and reduce broadcast domains:
- **HR Department**: VLAN 20
- **Finance Department**: VLAN 30
- **Sales Department**: VLAN 40

### 2. Network Configuration
- **Layer 2 Switching**: Configured VLANs on the Cisco 3560 switch and assigned specific ports to these VLANs.
- **Trunking**: Implemented 802.1Q trunking between switches to allow VLAN traffic to pass across the network backbone.
- **Inter-VLAN Routing**: Configured the router (Router-on-a-Stick or Layer 3 switching) to enable communication between the different departmental VLANs.
- **IP Addressing**: Implemented a structured IP addressing scheme, including the use of Class A addressing for specific departments (e.g., HR).
- **Security**: Basic firewall and router configurations were applied to secure the network perimeter.

## Files in this Repository
- `enterprise network plan.pkt`: The primary Cisco Packet Tracer project file containing the complete network topology and configurations.
- `*.png`: A collection of screenshots documenting the configuration process, including:
    - VLAN assignments for HR, Finance, and Sales.
    - Trunking configurations.
    - Router and Firewall setup.
    - Verification of connectivity (e.g., VLAN 20 verification).

## Tools Used
- **Cisco Packet Tracer**: For network simulation and configuration.
