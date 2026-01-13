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

## Step 4. Configure trunk link to the router

I configured the switch port connected to the router as a trunk.

• interface fa0/9
• switchport mode trunk
• exit

This allows multiple VLANs to pass between the switch and router.

<img width="1009" height="188" alt="image" src="https://github.com/user-attachments/assets/47471d88-7a20-4a88-b790-977525c10b5d" />


---

## Step 5. Configure router on a stick

I enabled inter VLAN routing using router subinterfaces.

For VLAN 10.

• interface g0/0.10
• encapsulation dot1Q 10
• ip address 192.168.10.1 255.255.255.0

For VLAN 20.

• interface g0/0.20
• encapsulation dot1Q 20
• ip address 192.168.20.1 255.255.255.0

Then I enabled the physical interface.

• interface fa0/0
• no shutdown

This allowed devices in different VLANs to communicate.


<img width="1042" height="901" alt="image" src="https://github.com/user-attachments/assets/ee6a4c7b-11c8-4c79-b9c7-1086f0025847" />

---

## Step 6. Configure the RADIUS server

I configured centralized authentication on the server.

Steps I followed on the server.

• Opened the Server device
• Went to Services
• Selected AAA
• Enabled AAA and RADIUS

I added the router as a network client.

Network entry.

• Client Name: Router
• Client IP: 192.168.20.1
• Secret: Cisco123
• Server Type: RADIUS

I then added a user.

User entry.

• Username: admin
• Password: strongpassword12345


<img width="1045" height="1000" alt="image" src="https://github.com/user-attachments/assets/c761f0b3-ac15-45dc-8809-02232142e7e5" />

---

## Step 7. Enable AAA on the router

I enabled the AAA framework.

• aaa new-model

This allowed the router to use external authentication methods.

<img width="1037" height="86" alt="image" src="https://github.com/user-attachments/assets/3199790b-eee6-43ac-86cd-f70de370f83b" />

---
## Step 8. Configure RADIUS authentication methods

I forced the router to use RADIUS for login authentication.

• aaa authentication login default group radius

I also protected privileged EXEC mode.

• aaa authentication enable default group radius

This ensured both login and enable access were validated by the RADIUS server.

<img width="1031" height="35" alt="image" src="https://github.com/user-attachments/assets/9d6030cf-7290-4a0c-a929-1cbc19481e62" />


---

## Step 9. Define the RADIUS server on the router

I pointed the router to the RADIUS server.

• radius-server host 192.168.20.3 key Cisco123

This shared secret must match the one configured on the server.

<img width="1022" height="861" alt="image" src="https://github.com/user-attachments/assets/cd1f46a0-bebd-40e5-86b3-c29faa998e21" />

---

## Step 10. Apply AAA to VTY lines

I applied AAA authentication to remote access lines.

• line vty 0 4
• login authentication default
• transport input telnet
• exit

This enforced RADIUS authentication for all Telnet sessions.

<img width="1038" height="291" alt="image" src="https://github.com/user-attachments/assets/4a9be9fa-95c7-4c47-ac49-f92af5f6d7b4" />

---

## Step 11. Configure local fallback user

I created a local admin account for emergency access.

• username admin secret Cisco123

This account works only if the RADIUS server is unreachable.

<img width="1034" height="306" alt="image" src="https://github.com/user-attachments/assets/d961e8e9-548a-4fd4-b5f3-e550753c3bec" />


