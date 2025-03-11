# CCNA-Cybersecurity
 
## About This Repository

Welcome to the CCNA Cybersecurity Scenarios repository, a comprehensive resource for students, networking enthusiasts, and professionals aiming to strengthen their cybersecurity skills at the CCNA level. This repository features lab scenarios, configuration examples, and troubleshooting exercises focused on securing network infrastructures. More advanced scenarios such as VPN and Firewalls would be in the advanced CCNA and CCNP security repository.

Key concepts include:  
- Network security fundamentals    
- Access control lists (ACLs)  
- NAT and PAT configurations   
- Configuring Network Management Services
- Secure router and switch configurations  

Labs are designed to be compatible with **Cisco Packet Tracer**, **GNS3**, or **EVE-NG**, with detailed instructions for setup and execution.  

### Important Notice  
This repository may contain IP addresses, usernames, and passwords used for lab scenarios. Handle this information responsibly and ensure it is used strictly for educational purposes. 


## Scenario 1 Summary  
This guide covers how to configure and verify network security using **Access Control Lists (ACLs)** on Cisco devices. You'll set up a basic network topology, configure routing, and apply both standard and extended ACLs to control traffic and enhance security.  


## Key Steps  

### **Part 1: Set Up the Topology and Initialize Devices**  
Start by creating the network topology and ensuring all devices are initialized correctly.  

1.2 **Initialize Devices**  
   - Assign hostnames to devices for easy identification.  
   - Set passwords for console, VTY, and enable modes to secure access.  
   - Configure basic IP settings on routers, switches, and PCs.  

---

### **Part 2: Configure Devices and Verify Connectivity**  

1. **Configure Basic Settings**  
   - Assign IP addresses to interfaces on routers and PCs.  
   - Enable interfaces using the `no shutdown` command.  
   - Set up basic routing information and hostnames.  

2. **Configure OSPF Routing**  
   - Enable OSPF on routers (R1, ISP, R3):  
     ```plaintext
     configure terminal  
     router ospf 1  
     network <network_address> <wildcard_mask> area 0  
     ```  
   - Verify OSPF neighbors with:  
     ```plaintext
     show ip ospf neighbor  
     ```  

3. **Verify Connectivity**  
   - Test connectivity using:  
     ```plaintext
     ping <destination_ip>  
     traceroute <destination_ip>  
     ```  

---

### **Part 3: Configure and Verify Standard Numbered and Named ACLs**  

1. **Configure a Numbered Standard ACL**  
   - Create and apply a basic ACL to permit or deny traffic:  
     ```plaintext
     access-list 10 permit 192.168.1.0 0.0.0.255  
     interface <interface_name>  
     ip access-group 10 in  
     ```  

2. **Configure a Named Standard ACL**  
   - Use named ACLs for better readability:  
     ```plaintext
     ip access-list standard ALLOWED_DEVICES  
     permit 192.168.2.0 0.0.0.255  
     interface <interface_name>  
     ip access-group ALLOWED_DEVICES in  
     ```  

3. **Verify ACLs**  
   - Confirm ACLs are applied and functioning as intended:  
     ```plaintext
     show access-lists  
     show ip interface <interface_name>  
     ```  

---

### **Part 4: Configure and Verify Extended Numbered and Named ACLs**  

1. **Configure a Numbered Extended ACL**  
   - Apply advanced filtering based on protocols and ports:  
     ```plaintext
     access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80  
     interface <interface_name>  
     ip access-group 100 in  
     ```  

2. **Configure a Named Extended ACL**  
   - Create a descriptive and flexible ACL:  
     ```plaintext
     ip access-list extended WEB_ACCESS  
     permit tcp 192.168.2.0 0.0.0.255 any eq 443  
     interface <interface_name>  
     ip access-group WEB_ACCESS in  
     ```  

3. **Verify Extended ACLs**  
   - Ensure ACLs are functioning correctly:  
     ```plaintext
     show access-lists  
     show running-config  
     ```  

---

### **Part 5: Modify and Verify Extended ACLs**  

1. **Modify an Existing ACL**  
   - Adjust ACLs as needed using sequence numbers:  
     ```plaintext
     ip access-list extended WEB_ACCESS  
     no 10  
     10 permit tcp 192.168.2.0 0.0.0.255 any eq 8080  
     ```  

2. **Verify the Modified ACLs**  
   - Check the updated ACL to ensure proper filtering:  
     ```plaintext
     show access-lists  
     ```  

---

## Why Use ACLs?  
Access Control Lists (ACLs) are critical for enhancing network security. They allow administrators to control traffic based on IP addresses, protocols, and port numbers, reducing the attack surface and limiting unauthorized access. Proper ACL configuration helps ensure that only legitimate traffic flows through the network, strengthening overall security posture.  



## Scenario 2 Summary  
This guide focuses on configuring **Network Address Translation (NAT)** on Cisco devices. It covers building the network, configuring both **Static and Dynamic NAT**, and verifying proper connectivity and address translation.  


## Key Steps  

##**Part 1: Build the Network and Verify Connectivity**  

1. **Basic Device Configuration**  
   - Set hostnames, passwords, and IP addressing.  
   - Enable interfaces with the `no shutdown` command.  

2. **Verify Initial Connectivity**  
   - Confirm end-to-end connectivity between internal and external devices:  
     ```plaintext
     ping <destination_ip>  
     traceroute <destination_ip>  

---
# 
### **Part 2: Configure and Verify Static NAT**  

1. **Configure Static NAT**  
   - Map a specific private IP address to a public IP address:  
     ```plaintext
     ip nat inside source static <private_ip> <public_ip>  
     ```  
   - Identify NAT interfaces:  
     ```plaintext
     interface <inside_interface>  
     ip nat inside  
       
     interface <outside_interface>  
     ip nat outside  
     ```  

2. **Verify Static NAT Configuration**  
   - Test connectivity from an external device to the internal server using the public IP.  
   - Use the following command to verify NAT translation:  
     ```plaintext
     show ip nat translations  
     ```  
   - Check the NAT statistics for any issues:  
     ```plaintext
     show ip nat statistics  
     ```  

---

### **Part 3: Configure and Verify Dynamic NAT**  

1. **Define a NAT Pool**  
   - Create a pool of public IP addresses for dynamic translation:  
     ```plaintext
     ip nat pool NAT_POOL <start_ip> <end_ip> netmask <subnet_mask>  
     ```  

2. **Create an Access Control List (ACL)**  
   - Define which internal addresses are eligible for NAT translation:  
     ```plaintext
     access-list 1 permit 192.168.1.0 0.0.0.255  
     ```  

3. **Configure Dynamic NAT**  
   - Link the ACL with the NAT pool:  
     ```plaintext
     ip nat inside source list 1 pool NAT_POOL  
     ```  
   - Set interfaces for NAT as in the Static NAT section.  

4. **Verify Dynamic NAT Configuration**  
   - Test connectivity from internal devices to an external network.  
   - Check NAT translations and statistics:  
     ```plaintext
     show ip nat translations  
     show ip nat statistics  
     ```  

---

## Why Use NAT?  
**Network Address Translation (NAT)** is essential for allowing multiple devices in a private network to access external networks (like the internet) using limited public IP addresses. NAT enhances security by masking internal IPs and helps in conserving IPv4 address space.  


## Scenario 3 Summary  
This guide focuses on configuring **NAT Pool Overload** and **Port Address Translation (PAT)** on Cisco devices. These techniques enable multiple internal devices to share a single public IP address, ensuring efficient IP address utilization and secure connectivity.  


## Key Steps  

### **Part 1: Build the Network and Verify Connectivity**  
 Same as the NAT scenario setup.

---

### **Part 2: Configure and Verify NAT Pool Overload**  

1. **Define a NAT Pool**  
   - Create a pool of public IP addresses:  
     ```plaintext
     ip nat pool OVERLOAD_POOL <start_ip> <end_ip> netmask <subnet_mask>  
     ```  

2. **Create an Access Control List (ACL)**  
   - Specify the internal network to be translated:  
     ```plaintext
     access-list 1 permit 192.168.1.0 0.0.0.255  
     ```  

3. **Configure NAT Pool Overload**  
   - Link the ACL with the NAT pool and enable overload:  
     ```plaintext
     ip nat inside source list 1 pool OVERLOAD_POOL overload  
     ```  

4. **Set NAT Interfaces**  
   - Define the inside and outside interfaces:  
     ```plaintext
     interface <inside_interface>  
     ip nat inside  
       
     interface <outside_interface>  
     ip nat outside  
     ```  

5. **Verify NAT Pool Overload**  
   - Test connectivity from internal devices to an external network.  
   - View NAT translations and statistics:  
     ```plaintext
     show ip nat translations  
     show ip nat statistics  
     ```  

---

### **Part 3: Configure and Verify PAT (Port Address Translation)**  

1. **Configure PAT Using Interface Overload**  
   - Instead of a pool, use the router's external interface IP for PAT:  
     ```plaintext
     ip nat inside source list 1 interface <outside_interface> overload  
     ```  

2. **Verify PAT Configuration**  
   - Test by initiating connections from multiple internal devices.  
   - Use these commands to confirm active translations and overload status:  
     ```plaintext
     show ip nat translations  
     show ip nat statistics  
     ```  

---

## Why Use NAT Pool Overload and PAT?  
- **NAT Pool Overload** enables multiple internal devices to share a limited pool of public IPs efficiently.  
- **PAT (Port Address Translation)** allows numerous internal hosts to share a single public IP address by distinguishing sessions using port numbers.  
- These methods conserve IPv4 addresses, enhance security by masking internal IPs, and ensure seamless external connectivity.



## Scenarios 4 and 5 Summary  
This guide focuses on **network discovery protocols** and **time synchronization** on Cisco devices. It includes configuring and verifying **Cisco Discovery Protocol (CDP)**, **Link Layer Discovery Protocol (LLDP)**, and **Network Time Protocol (NTP)** to ensure efficient network management and accurate device timekeeping.  


## Key Steps  

### **Part 1: Build the Network and Verify Connectivity**  

 **Basic Device Configuration**  
   - Configure hostnames, IP addresses, and enable interfaces.  
   - Ensure all devices can communicate by performing basic connectivity tests:  
     ```plaintext
     ping <destination_ip>  
     traceroute <destination_ip>  
     ```  

---

### **Part 2: Network Discovery with CDP**  

1. **Verify CDP Status**  
   - CDP is enabled by default on Cisco devices. Verify with:  
     ```plaintext
     show cdp  
     show cdp neighbors  
     ```  

2. **Enable or Disable CDP**  
   - Enable CDP globally and on interfaces (if disabled):  
     ```plaintext
     cdp run  
     interface <interface_name>  
     cdp enable  
     ```  
   - To disable CDP on an interface (for security purposes):  
     ```plaintext
     no cdp enable  
     ```  

3. **View Detailed Neighbor Information**  
   - Get detailed information about connected neighbors:  
     ```plaintext
     show cdp neighbors detail  
     ```  

---

### **Part 3: Network Discovery with LLDP**  

1. **Enable LLDP**  
   - Unlike CDP, **LLDP** is disabled by default. Enable it globally:  
     ```plaintext
     lldp run  
     ```  
   - Enable LLDP on specific interfaces:  
     ```plaintext
     interface <interface_name>  
     lldp transmit  
     lldp receive  
     ```  

2. **Verify LLDP Status**  
   - Display LLDP neighbor information:  
     ```plaintext
     show lldp neighbors  
     show lldp neighbors detail  
     ```  

3. **Disable LLDP (Optional)**  
   - If needed, disable LLDP globally or on specific interfaces:  
     ```plaintext
     no lldp run  
     interface <interface_name>  
     no lldp transmit  
     no lldp receive  
     ```  

---

### **Part 4: Configure and Verify NTP**  

1. **Configure an NTP Server**  
   - Set the device to synchronize with an external NTP server:  
     ```plaintext
     ntp server <ntp_server_ip>  
     ```  

2. **Configure NTP Authentication (Optional)**  
   - For secure NTP configuration:  
     ```plaintext
     ntp authenticate  
     ntp authentication-key 1 md5 <key_value>  
     ntp trusted-key 1  
     ntp server <ntp_server_ip> key 1  
     ```  

3. **Verify NTP Synchronization**  
   - Check synchronization status:  
     ```plaintext
     show ntp associations  
     show ntp status  
     ```  
   - Display the current time:  
     ```plaintext
     show clock  
     ```  

---

## Why Use CDP, LLDP, and NTP?  

- **CDP (Cisco Discovery Protocol):** Allows devices to discover directly connected Cisco devices, aiding in network topology mapping and troubleshooting.  
- **LLDP (Link Layer Discovery Protocol):** An open standard for device discovery, useful in multi-vendor environments.  
- **NTP (Network Time Protocol):** Ensures accurate timekeeping across network devices, which is essential for log accuracy, security protocols, and system operations. 

## Scenario 6 Summary  
This guide covers the essential steps to **secure Layer 2 switches** in a network. It includes configuring **basic switch settings**, enabling **SSH access**, securing **trunk and access ports**, and implementing **DHCP snooping** for enhanced network security.  

---

## Key Steps  

### **Part 1: Configure Hostname, IP Address, and Access Passwords**  

1. **Set the Hostnames**  
   ```plaintext
   configure terminal  
   hostname S1  
   ```  

2. **Configure the Management Interface**  
   - Assign an IP address to the VLAN interface for remote management:  
   ```plaintext
   interface vlan 1  
   ip address 192.168.1.2 255.255.255.0  
   no shutdown  
   exit  
   ```  

3. **Configure Console and VTY Access Passwords**  
   ```plaintext
   line console 0  
   password cisco  
   login  
   exit  

   line vty 0 4  
   password cisco  
   login  
   transport input ssh  
   exit  
   ```  

4. **Encrypt Passwords**  
   ```plaintext
   service password-encryption  
   ```  

5. **Set an Enable Secret Password**  
   ```plaintext
   enable secret cisco  
   ```  

---

### **Part 2: Configure SSH Access to the Switches**  

1. **Set Domain Name and Generate RSA Keys**  
   ```plaintext
   ip domain-name example.com  
   crypto key generate rsa  
   modulus 1024  
   ```  

2. **Configure SSH Version 2**  
   ```plaintext
   ip ssh version 2  
   ```  

3. **Create a Local User for SSH Access**  
   ```plaintext
   username admin privilege 15 secret cisco
   ```  

4. **Restrict VTY Access to SSH Only**  
   ```plaintext
   line vty 0 4  
   login local  
   transport input ssh  
   exit  
   ```  

5. **Verify SSH Configuration**  
   - Use the following commands to confirm SSH is active:  
     ```plaintext
     show ip ssh  
     show ssh  
     ```  

6. **Test SSH Access**  
   - From an SSH client or terminal, attempt to connect:  
     ```plaintext
     ssh -l admin 192.168.1.2  
     ```  

---

### **Part 3: Configure Secure Trunks and Access Ports**  

1. **Configure Trunk Port Mode**  
   ```plaintext
   interface fastEthernet 0/1  
   switchport mode trunk  
   ```  

2. **Change the Native VLAN for Trunks**  
   ```plaintext
   switchport trunk native vlan 99  
   ```  

3. **Verify Trunk Configuration**  
   ```plaintext
   show interfaces trunk  
   ```  

4. **Enable Storm Control for Broadcasts**  
   ```plaintext
   storm-control broadcast level 10.00  
   ```  

5. **Configure Access Ports**  
   ```plaintext
   interface range gigabitEthernet 0/2-24  
   switchport mode access  
   switchport access vlan 10  
   ```  

6. **Enable PortFast and BPDU Guard**  
   ```plaintext
   spanning-tree portfast  
   spanning-tree bpduguard enable  
   ```  

   > **Why Enable BPDU Guard?**  
   > - BPDUs (Bridge Protocol Data Units) are used in **Spanning Tree Protocol (STP)** to prevent loops.  
   > - If a rogue switch is connected, it could send malicious BPDUs and compromise network stability.  
   > - **BPDU Guard** ensures that if any BPDU is received on a **PortFast**-enabled port, the port will be **automatically disabled**, protecting the network.  

7. **Verify BPDU Guard**  
   ```plaintext
   show spanning-tree interface detail  
   ```  

8. **Enable Root Guard**  
   ```plaintext
   spanning-tree guard root  
   ```  

9. **Enable Loop Guard**  
   ```plaintext
   spanning-tree guard loop  
   ```  

10. **Configure and Verify Port Security**  
   ```plaintext
   switchport port-security  
   switchport port-security maximum 2  
   switchport port-security violation restrict  
   switchport port-security mac-address sticky  
   show port-security interface <interface>  
   ```  

11. **Disable Unused Ports**  
   ```plaintext
   interface range gigabitEthernet 0/25-48  
   shutdown  
   ```  

12. **Move Ports from VLAN 1 to an Alternate VLAN**  
   ```plaintext
   switchport access vlan 99 
   ```  

13. **Configure PVLAN Edge (Private VLAN Edge)**  
   ```plaintext
   switchport protected  
   ```  

---

### **Part 4: Configure IP DHCP Snooping**  

1. **Configure DHCP on R1**  
   ```plaintext
   ip dhcp pool name  
   network 192.168.10.0 255.255.255.0  
   default-router 192.168.1.1  
   dns-server 8.8.8.8  
   ```  

2. **Configure Inter-VLAN Routing on R1**  
   ```plaintext
   interface gigabitEthernet 0/1.10  
   encapsulation dot1Q 10  
   ip address 192.168.10.1 255.255.255.0  
   ```  

3. **Configure Trunk on S1**  
   ```plaintext
   interface fastEthernet 0/5  
   switchport mode trunk  
   ```  

4. **Enable DHCP Snooping**  
   ```plaintext
   ip dhcp snooping  
   ip dhcp snooping vlan 10,20  
   interface fastEthernet 0/5  
   ip dhcp snooping trust  
   ```  

   > **Why Enable DHCP Snooping?**  
   > - DHCP Snooping helps prevent rogue DHCP servers from assigning incorrect IP addresses.  
   > - It ensures that only **trusted ports** can offer DHCP services, enhancing network security.  

5. **Verify DHCP Snooping**  
   ```plaintext
   show ip dhcp snooping  
   show ip dhcp snooping binding  
   ```



## why we use Layer 2 Switch Security
Securing Layer 2 switches is essential to prevent vulnerabilities and attacks in a network. Key elements include configuring basic switch settings to ensure proper initialization, enabling SSH access for secure management, securing trunk and access ports to prevent unauthorized connections and attacks like VLAN hopping, and implementing DHCP snooping to block rogue DHCP servers from disrupting network services. These steps help protect the network from common security threats and ensure that only authorized devices and traffic can access critical resources.
