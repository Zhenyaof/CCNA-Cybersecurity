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
Securing Layer 2 switches is essential to prevent vulnerabilities and attacks in a network. Key elements include configuring basic switch settings to ensure proper initialization, enabling SSH access for secure management, securing trunk and access ports to prevent unauthorized connections and attacks like VLAN hopping, and implementing DHCP snooping to block rogue DHCP servers from disrupting network services. These steps help protect the network from common security threats and ensure that only authorized devices and traffic can access critical resources




# Scenario 7

This guide provides a comprehensive, step-by-step configuration for securing routers, ensuring both connectivity and enhanced security measures are implemented effectively.

## Part 1: Basic Router Configuration and Connectivity

### Step 1: Configure IP Addresses on Routers
```
R1(config)# interface g0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown

R1(config)# interface s0/0/0
R1(config-if)# ip address 10.1.1.1 255.255.255.252
R1(config-if)# clock rate 64000
R1(config-if)# no shutdown

R2(config)# interface s0/0/0
R2(config-if)# ip address 10.1.1.2 255.255.255.252
R2(config-if)# no shutdown

R2(config)# interface s0/0/1
R2(config-if)# ip address 10.2.2.2 255.255.255.252
R2(config-if)# clock rate 64000
R2(config-if)# no shutdown

R3(config)# interface g0/1
R3(config-if)# ip address 192.168.3.1 255.255.255.0
R3(config-if)# no shutdown

R3(config)# interface s0/0/1
R3(config-if)# ip address 10.2.2.1 255.255.255.252
R3(config-if)# no shutdown
```

### Step 2: Configure OSPF Routing
```
R1(config)# router ospf 1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.1.1.0 0.0.0.3 area 0

R2(config)# router ospf 1
R2(config-router)# network 10.1.1.0 0.0.0.3 area 0
R2(config-router)# network 10.2.2.0 0.0.0.3 area 0

R3(config)# router ospf 1
R3(config-router)# network 192.168.3.0 0.0.0.255 area 0
R3(config-router)# network 10.2.2.0 0.0.0.3 area 0
```

### Step 3: Configure PC Hosts
- Set IP, subnet mask, and gateway on PCs

### Step 4: Verify Connectivity
```
ping 192.168.3.3
traceroute 192.168.3.3
```

---

## Part 2: Control Administrative Access for Routers

### Step 1: Encrypt Passwords
```
R1(config)# enable secret cisco12345
R1(config)# service password-encryption
R1(config)# security passwords min-length 10
```

### Step 2: Login Warning Banner
```
R1(config)# banner motd # Unauthorized Access Prohibited #
```

### Step 3: Configure SSH
```
R1(config)# hostname R1
R1(config)# ip domain-name example.com
R1(config)# crypto key generate rsa modulus 2048
R1(config)# username admin secret Adm!n1234
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
```

### Step 4: Enable SCP
```
R1(config)# ip scp server enable
```

### Step 5: Configure AAA for Enhanced Security
```
R1(config)# aaa new-model
R1(config)# aaa authentication login default local
R1(config)# aaa authorization exec default local
```

- AAA (Authentication, Authorization, and Accounting) manages user access and activities. Authentication verifies identity, Authorization defines user permissions, and Accounting tracks actions. This configuration enables local authentication, authorization, and activity logging.

## Part 3: Configure Administrative Roles
```
R1(config)# parser view AdminView
R1(config-view)# secret admin123
R1(config-view)# commands exec include all

R1(config)# parser view UserView
R1(config-view)# secret user123
R1(config-view)# commands exec include show
```

- Views in Cisco devices allow defining user roles and command access. AdminView grants full access, while UserView limits access to show commands. This setup enhances security by restricting command usage based on roles.

## Part 4: Cisco IOS Resilience and Management Reporting

### Step 1: Configure SNMPv3
```
R1(config)# snmp-server group SNMPv3Group v3 priv
R1(config)# snmp-server user SNMPUser SNMPv3Group v3 auth sha authpass priv aes 128 privpass
R1(config)# snmp-server host 192.168.3.1 version 3 priv SNMPUser
```
- SNMPv3 user has access to all interfaces for monitoring purposes.

### 1.2 Configure Access Control for SNMP:

To ensure SNMP access is limited to R1's LAN (192.168.1.0/24), configure an access control list (ACL) to permit SNMP packets only from this network:

R1(config)# ip access-list standard PERMIT-SNMP
R1(config-std-nacl)# permit 192.168.1.0 0.0.0.255
R1(config-std-nacl)# exit
This ACL allows SNMP traffic only from the LAN network 192.168.1.0/24, securing access to SNMP data.

### Step 2: NTP

1- **Configure NTP with Authentication:**

   **On R2 (NTP Master Configuration):**
   
   First, configure NTP authentication on R2 by defining the authentication key, hashing type, and password. The password is case-sensitive.

   ```bash
   R2(config)# ntp authentication-key 1 md5 NTPpassword
   ```

   Then, configure the trusted key on R2, which will be used for authentication:

   ```bash
   R2(config)# ntp trusted-key 1
   ```

   Enable the NTP authentication feature:

   ```bash
   R2(config)# ntp authenticate
   ```

   Configure R2 as the NTP master with a stratum number of 3. The stratum number indicates the distance from the original time source. For this lab, we use stratum 3 on R2:

   ```bash
   R2(config)# ntp master 3
   ```

   **On R1 and R3 (NTP Clients Configuration):**
   
   For both R1 and R3, configure NTP authentication using the same authentication key and password as on R2:

   ```bash
   R1(config)# ntp authentication-key 1 md5 NTPpassword
   R3(config)# ntp authentication-key 1 md5 NTPpassword
   ```

   Configure the trusted key on R1 and R3 to ensure they only trust the time source from R2:

   ```bash
   R1(config)# ntp trusted-key 1
   R3(config)# ntp trusted-key 1
   ```

   Enable the NTP authentication feature on both devices:

   ```bash
   R1(config)# ntp authenticate
   R3(config)# ntp authenticate
   ```

   Then, configure R1 and R3 as NTP clients of R2, and enable calendar synchronization using the `ntp update-calendar` command:

   ```bash
   R1(config)# ntp server 10.1.1.2
   R1(config)# ntp update-calendar

   R3(config)# ntp server 10.1.1.2
   R3(config)# ntp update-calendar
   ```

   - R1 and R3 will now sync their time with R2, using NTP authentication for secure time synchronization.
```

---

This update provides the full NTP authentication configuration for R1, R2, and R3, ensuring secure synchronization of time across devices.

### Step 3: Syslog Setup
```
R1(config)# logging 192.168.1.3
```
- Ensure the syslog server is configured to receive and store logs effectively.

---

## Part 5: Securing the Control Plane

In this part of the lab, you will configure OSPF routing protocol authentication using SHA256 to ensure the integrity and authenticity of OSPF messages. Additionally, you will verify that OSPF authentication is functioning correctly.

### Task 1: Configure OSPF Routing Protocol Authentication using SHA256 Hashing

**Step 1: Configure a key chain on all three routers.**

a. **Assign a key chain name and number:**
   
   On each router (R1, R2, and R3), configure the key chain with a name (`NetAcad`) and key number (`1`):

   ```bash
   R1(config)# key chain NetAcad
   R1(config-keychain)# key 1
   ```

b. **Assign the authentication key string:**

   Set the key string to be used for OSPF authentication (ensure it is the same on all routers):

   ```bash
   R1(config-keychain-key)# key-string CCNASkeystring
   ```

c. **Configure the encryption algorithm to be used for authentication:**

   To use SHA256 encryption for OSPF authentication, configure the cryptographic algorithm:

   ```bash
   R1(config-keychain-key)# cryptographic-algorithm hmac-sha-256
   ```

**Step 2: Configure the serial interfaces to use OSPF authentication.**

a. **Assign the key-chain to the serial interfaces:**

   Now configure the OSPF authentication for the serial interfaces on R1 and R3 by using the `ip ospf authentication` command to assign the key chain:

   ```bash
   R1(config)# interface s0/0/0
   R1(config-if)# ip ospf authentication message-digest
   R1(config-if)# ip ospf message-digest-key 1 md5 CCNASkeystring
   ```

   Repeat the same configuration for R3:

   ```bash
   R3(config)# interface s0/0/1
   R3(config-if)# ip ospf authentication message-digest
   R3(config-if)# ip ospf message-digest-key 1 md5 CCNASkeystring
   ```

b. **Verify OSPF Authentication:**

   After configuring the authentication, verify that OSPF is using the configured authentication by checking the OSPF neighbors:

   ```bash
   R1# show ip ospf neighbor
   ```

   You should see that the OSPF neighbors are authenticated, confirming that the SHA256 OSPF authentication is working correctly.

---

## Part 6: Secure the Control Plane with OSPF Authentication

### Step 1: Configure OSPF Message Digest Key

```
R1(config-router)# area 0 authentication message-digest
R1(config-if)# ip ospf message-digest-key 1 sha256 OSPFpass
```
- Using SHA256 ensures stronger OSPF authentication, enhancing security.

---

## Part 7: Configure Automated Security Features

### Step 1: Enable AutoSecure

```
R1# auto secure
```

### Step 2: Verify Configuration

```
show running-config
```


## Why we use Securing the Router for Administrative Access

Securing administrative access to routers is crucial to protect network infrastructure from unauthorized access and potential attacks. By implementing strong authentication, encryption, and access control mechanisms, we ensure that only authorized personnel can configure or manage the device. This minimizes the risk of malicious configurations, data breaches, or downtime caused by unauthorized changes. Methods such as password encryption, SSH for secure remote access, and the use of role-based views help protect sensitive network devices and maintain the integrity of the network.
