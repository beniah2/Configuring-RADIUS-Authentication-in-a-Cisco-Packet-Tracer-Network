# Configuring RADIUS Authentication in a Cisco Packet Tracer Network

I am building a secure Packet Tracer network with VLANs, inter VLAN routing, and centralized AAA authentication using RADIUS and I will verify access using Telnet or SSH afterwards.

## Step 1. Build the network topology

I built a star topology in Cisco Packet Tracer.

• I added 6 laptops for Users
• I added 1 PC for Admin
• I added 1 Server for RADIUS
• I added 1 Layer 2 switch
• I added 1 router for inter VLAN routing

VLAN layout.
• VLAN 10, Users laptops
• VLAN 20, Admin PC and RADIUS server


<img width="1917" height="930" alt="image" src="https://github.com/user-attachments/assets/69ce1320-3680-4a58-ad11-59693f12d564" />


---


## Step 2. Configure VLANs on the switch

I created VLANs to logically separate the network.

Commands I used.

• enable
• configure terminal
• vlan 10
• name Users
• exit
• vlan 20
• name Admin
• exit

This created two separate broadcast domains.


<img width="1040" height="1015" alt="image" src="https://github.com/user-attachments/assets/3c4cc159-b673-46b8-a15b-075e23558a30" />


---

## Step 3. Assign switch ports to VLANs

I assigned access ports so devices automatically join the correct VLAN.

For VLAN 10.

• interface range fa0/1 - 6
• switchport mode access
• switchport access vlan 10
• exit

For VLAN 20.

• interface range fa0/7 - 8
• switchport mode access
• switchport access vlan 20
• exit

This ensures user devices and admin devices remain isolated.


<img width="1015" height="203" alt="image" src="https://github.com/user-attachments/assets/e2ad8573-0a98-480a-9faa-b145f5461fe0" />


---



