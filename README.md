# Cisco Multi-VLAN Network Design

## Network Design Overview

### VLAN Segmentation
The network is divided into six VLANs, each assigned a dedicated IPv4 /28 subnet to provide logical separation between departments. Each subnet supports up to 14 usable IPv4 addresses. Two PCs are connected to each VLAN in this Cisco Packet Tracer simulation.

- VLAN 10 - HR(10.10.0.0/28)
- VLAN 20 - Management(10.10.0.16/28)
- VLAN 30 - Finance/Accounting(10.10.0.32/28)
- VLAN 40 - Commercial(10.10.0.48/28)
- VLAN 50 - Technical/IT(10.10.0.64/28)
- VLAN 60 - Guests(10.10.0.80/28)
- VLAN 99 - Native(UNUSED_PORTS)

## Switch Configuration 

### Creating & Naming the VLANS
```
en

conf

vlan 10
name HR
exit

vlan 20
name Management
exit

vlan 30
name Finance/Accounting
exit

vlan 40
name Commercial
exit

vlan 50
name Techincal/IT
exit

vlan 60
name Guests
exit

vlan 99
name UNUSED_PORTS
exit
```

### Assigning IP address to each VLANS
```
interface vlan10
ip address 10.10.0.1 255.255.255.240
exit

interface vlan20
ip address 10.10.0.17 255.255.255.240
exit

interface vlan30
ip address 10.10.0.33 255.255.255.240
exit

interface vlan40
ip address 10.10.0.49 255.255.255.240
exit

interface vlan50
ip address 10.10.0.65 255.255.255.240
exit

interface vlan60
ip address 10.10.0.81 255.255.255.240
exit
```

### Assigning ports to VLANS
```
interface range FastEthernet 0/1-2
switchport mode access
switchport access vlan10
exit

interface range FastEthernet 0/3-4
switchport mode access
switchport access vlan20
exit

interface range FastEthernet 0/5-6
switchport mode access
switchport access vlan30
exit

interface range FastEthernet 0/7-8
switchport mode access
switchport access vlan40
exit

interface range FastEthernet 0/9-10
switchport mode access
switchport access vlan50
exit

interface range FastEthernet 0/11-12
switchport mode access
switchport access vlan60
exit
```

### Enabling Inter-VLAN Routing
```
ip routing
```
IP routing was enabled on the multilayer switch to allow IPv4 traffic to be routed between the departmental VLANs through their respective SVIs.

## 5. Network Architecture
- The project implements a centralized network architecture using a Cisco Catalyst 3560-24PS multilayer switch in Cisco Packet Tracer.

- Six departmental VLANs provide logical separation between HR, Management, Finance/Accounting, Commercial, Technical/IT, and Guest devices.

- Each VLAN uses a dedicated IPv4 /28 subnet. Switch Virtual Interfaces (SVIs) provide the default gateways, while the multilayer switch performs inter-VLAN routing.

- Two end-user PCs are connected to each departmental VLAN through dedicated access ports.

- Unused switch ports are assigned to VLAN 99 and administratively shut down.

The design demonstrates VLAN segmentation, IPv4 addressing, inter-VLAN routing, and extended ACL configuration in a simulated environment.

### 5.1 Logical Network Design
  | Department | VLAN ID | IPv4 Network | Default Gateway |
- | HR | 10 | 10.10.0.0/28 | 10.10.0.1 |
- | Management | 20 | 10.10.0.16/28 | 10.10.0.17 |
- | Finance/Accounting | 30 | 10.10.0.32/28 | 10.10.0.33 |
- | Commercial | 40 | 10.10.0.48/28 | 10.10.0.49 |
- | Technical/IT | 50 | 10.10.0.64/28 | 10.10.0.65 |
- | Guests | 60 | 10.10.0.80/28 | 10.10.0.81 |

### 5.2 IPv4 Addressing Standards
All departmental networks use a /28 prefix.
- | Subnet mask | 255.255.255.240 |
- | Total addresses per subnet | 16 |
- | Usable host addresses per subnet | 14 |
- | Address allocation | One subnet per department |

Each subnet provides sufficient address space for two end-user devices, a default gateway, and additional devices as the lab expands.
The network address and broadcast address are reserved and are not assigned to end-user devices or SVIs.

## 6. VLAN and Port Assignment Plan
Each department is assigned a dedicated VLAN. Two access ports are allocated
to each department for end-user connectivity.

  | Department | VLAN ID | VLAN Name | Switch Ports |
- | HR | 10 | HR | Fa0/1-2 |
- | Management | 20 | Management | Fa0/3-4 |
- | Finance/Accounting | 30 | Finance/Accounting | Fa0/5-6 |
- | Commercial | 40 | Commercial | Fa0/7-8 |
- | Technical/IT | 50 | Technical/IT | Fa0/9-10 |
- | Guests | 60 | Guests | Fa0/11-12 |

Ports are configured as access ports and assigned to their respective VLANs.

## 7. Testing and Validation Results
### Test Cases and Results

1. VLAN Connectivity Test

- Communication between two HR devices (10.10.0.2 and 10.10.0.3): Passed
- Inter-VLAN connectivity between tested departmental devices: Passed

2. VLAN and SVI Verification

- VLANs created and access ports assigned: Passed
- SVI IP addresses matched the addressing plan: Passed
- Configured SVIs reported as operational (up/up): Passed

3. Inter-VLAN Routing Test

- Devices can reach their respective default gateways: Passed for the gateways tested
- Inter-VLAN routing between departmental networks: Passed for the connectivity tests previously performed

4. Guest VLAN Security Validation

- Guest PC can reach its default gateway (10.10.0.81): Passed
- Guest-to-HR connectivity blocked: Passed
- Guest-to-Management connectivity blocked: Passed
- Guest-to-Finance connectivity blocked: Passed
- Guest-to-Commercial connectivity blocked: Passed
- Guest-to-Technical connectivity blocked: Passed
- ACL deny counters recorded matches for all five departmental networks: Passed

5. Switch Port Verification

Access ports assigned to the correct VLANs: Passed
Unused switch ports assigned to VLAN 99 and shut down: Passed

## 8. Verification Commands
### Switch CLI Commands
```
show vlan brief
show interfaces status
show interfaces switchport
show ip interface brief
show ip route
show running-config
show access-lists GUEST-ISOLATION
show ip interface vlan 60
```
### PC Command Prompt Tests
#### From HR PC
- ping 10.10.0.3
- ping 10.10.0.18
- ping 10.10.0.35
- ping 10.10.0.51
- ping 10.10.0.67
- ping 10.10.0.83

#### From Guest PC
- ping 10.10.0.81
- ping 10.10.0.2
- ping 10.10.0.18
- ping 10.10.0.34
- ping 10.10.0.50
- ping 10.10.0.66

## 9. Security Implementation
1. VLAN Segmentation and Port Security Practices

Separated six departments into individual VLANs to segment network traffic.
Assigned access ports to their designated departmental VLANs.
Assigned unused switch ports to VLAN 99 (parking VLAN).
Administratively shut down unused switch ports to reduce unauthorized connection risks.

2. Extended ACL and Guest Network Isolation

Created an extended IPv4 ACL named GUEST-ISOLATION.
Configured deny rules to restrict traffic from the Guest subnet (10.10.0.80/28) to the HR, Management, Finance, Commercial, and Technical/IT subnets.
Added a permit rule for other IPv4 traffic originating from the Guest subnet.
Tested Guest-to-department connectivity and observed failed pings to all five departmental networks.
Verified that the ACL recorded matches against all five departmental deny rules.

3. Security Testing

Tested connectivity between devices within the same VLAN.
Tested inter-VLAN connectivity to verify routing between departmental networks.
Tested reachability to the Guest VLAN gateway (10.10.0.81).
Attempted to reach departmental PCs from the Guest VLAN and recorded the results.
Reviewed ACL match counters to identify traffic matching the departmental deny rules.

## 10. Tools and Technologies

- Cisco Packet Tracer
- Cisco Catalyst 3560-24PS multilayer switch
- VLANs and Layer 2 switching
- IPv4 addressing and subnetting
- Switched Virtual Interfaces (SVIs)
- Inter-VLAN routing
- ICMP connectivity testing

## 11. Author

**Gabriel Uwaechie**

Cybersecurity | Network Security | Networking

This project is part of my practical networking portfolio, demonstrating
hands-on experience with enterprise LAN design, VLAN configuration,
IP addressing, and inter-VLAN routing.
