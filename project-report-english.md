# Project Report - English

**1. Project Objective**

The objective of this project is to create a segmented, secure, and functional computer network using Cisco Packet Tracer 8.2.2. The network includes an IDS (simulated with ACLs), internet connectivity, an email server, an Active Directory server, a wireless network with mobile devices, and a firewall for additional security.

**2. Network Components**

* **Cisco 4331 ISR Router**
* **Cisco ASA 5505 Firewall**
* **Cisco Catalyst 3560 Layer 3 Switch**
* **Layer 2 Switches (for each network segment)**
* **Servers (simulated IDS, Email/AD)**
* **PCs and Mobile Devices (Smartphones and Laptops)**
* **Access Point for wireless network**

**3. Network Diagram**

<figure><img src=".gitbook/assets/Captura de ecrã de 2024-07-04 21-46-04.png" alt=""><figcaption></figcaption></figure>

**4. Device Configurations**

**Cisco 4331 ISR Router**

* **GigabitEthernet0/0 Interface:**
  * IP Address: 192.168.1.1 (connected to ISP)
* **GigabitEthernet0/1 Interface:**
  * IP Address: 192.168.4.1 (connected to firewall)

**Cisco ASA 5505 Firewall**

* **Ethernet0/0 Interface:**
  * IP Address: 192.168.4.2 (connected to router)
* **Ethernet0/1 Interface:**
  * Connected to Layer 3 Switch

**Cisco Catalyst 3560 Layer 3 Switch**

* **GigabitEthernet0/1 Interface:**
  * Connected to firewall
* **VLAN Configurations:**
  * VLAN 10: Sales Network (192.168.2.0/24)
  * VLAN 20: Finance Network (192.168.3.0/24)
  * VLAN 30: DMZ (192.168.4.0/24)
  * VLAN 40: Wireless Network (192.168.5.0/24)

**Access Switches**

* **Switch1 (Sales):**
  * Connected to Layer 3 Switch
  * VLAN 10
* **Switch2 (Finance):**
  * Connected to Layer 3 Switch
  * VLAN 20
* **Switch3 (DMZ):**
  * Connected to Layer 3 Switch
  * VLAN 30
* **Switch4 (Wireless):**
  * Connected to Layer 3 Switch and Access Point
  * VLAN 40

**Servers**

* **Email/AD Server:**
  * IP Address: 192.168.4.200
  * Services: Active Directory (AAA), Email
* **Simulated IDS Server:**
  * IDS simulation using ACLs on the router/firewall

**5. End Device Configurations**

**PCs and Mobile Devices**

* **Static IP Configurations:**
  * PCs in Sales Network (192.168.2.x):
    * Default Gateway: 192.168.2.254
    * DNS Server: 192.168.4.200
  * PCs in Finance Network (192.168.3.x):
    * Default Gateway: 192.168.3.254
    * DNS Server: 192.168.4.200
  * Devices in DMZ (192.168.4.x):
    * Default Gateway: 192.168.4.254
    * DNS Server: 192.168.4.200
  * Devices in Wireless Network (192.168.5.x):
    * Default Gateway: 192.168.5.254
    * DNS Server: 192.168.4.200

**6. ACL Configuration for IDS Simulation**

**On the Router**

```
Router> enable
Router# configure terminal
! Create ACL to allow HTTP/HTTPS traffic
Router(config)# access-list 100 permit tcp any any eq 80
Router(config)# access-list 100 permit tcp any any eq 443
! Create ACL to deny all ICMP traffic (ping)
Router(config)# access-list 100 deny icmp any any
! Allow all other traffic
Router(config)# access-list 100 permit ip any any
! Apply ACL to internal interface
Router(config)# interface gigabitEthernet0/1
Router(config-if)# ip access-group 100 in
Router(config-if)# exit
Router(config)# exit
```

On the Firewall

```
Router> enable
Router# configure terminal
Router(config)# access-list 101 permit tcp 192.168.2.0 0.0.0.255 192.168.3.0 0.0.0.255 eq 80
Router(config)# end
! Allowing HTTP Traffic from Vendas VLAN to Finanças VLAN
```

**7. Testing and Verification**

* **Internal Connectivity:**
  * Test ping between PCs in different networks.
* **Internet Access:**
  * Verify connectivity to the ISP server.
* **Email/AD Functionality:**
  * Authenticate users on the configured AD server and send emails between configured accounts.
* **ACL Monitoring:**
  * Check logs for permitted/denied traffic as per ACLs.

#### Conclusion

This project sets up a securely segmented network with appropriate internal and external connectivity. Using ACLs to simulate IDS provides an additional layer of monitoring and security, while the Email/AD servers ensure essential communication and user authentication functionality.
