# Hospital Network Infrastructure - Packet Tracer Design

## Network Overview
The hospital's network infrastructure is designed to support critical medical operations, secure data exchange, and reliable connectivity across three buildings (A, B, and C). This Cisco-based solution ensures:
- Secure segregation of sensitive medical data (EMRs, lab results)
- Scalable architecture for future expansion
- Robust wireless coverage for staff and patients
- Efficient traffic management through VLANs

---

## Network Design
### Core Networking Devices
| Device               | Model           | Quantity | Role                                                                 |
|----------------------|-----------------|----------|----------------------------------------------------------------------|
| Core Router          | Cisco 2901      | 1        | Inter-VLAN routing & DHCP services                                   |
| Core Switch          | 2960-24TT       | 1        | Central backbone connecting all buildings                           |
| Layer 3 Switches     | 3560-24PS       | 3        | VLAN routing (1 per building)                                       |
| Access Switches      | 2960-24TT       | 8        | Device connectivity (Computers/Medical Equipment)                   |
| Wireless Access Points | AP-PT-A        | 5        | Wi-Fi coverage in common areas                                      |

### Physical Infrastructure
- **Building Interconnections**:
  - Fiber connections between core switch and each building's Layer 3 switch
  - Redundant links for critical pathways

- **Wireless Coverage**:
  - Dual-band support (2.4GHz & 5GHz)
  - WPA2-Enterprise authentication
  - Guest network isolation

---

## VLAN Architecture
### Building A
| VLAN ID | Name               | Purpose                          |
|---------|--------------------|----------------------------------|
| 10      | ReceptionAndWaiting| Patient check-in & waiting areas |
| 11      | Consultation       | Doctor offices & exam rooms      |
| 12      | AdminsBuildingA    | Administrative staff             |

### Building B
| VLAN ID | Name               | Purpose                          |
|---------|--------------------|----------------------------------|
| 20      | SpecialistServices | Specialty clinics                |
| 21      | Laboratories       | Lab equipment & data systems     |
| 23      | Pharmacy           | Medication management systems    |
| 24      | AdminsBuildingB    | Building B administration        |

### Building C
| VLAN ID | Name               | Purpose                          |
|---------|--------------------|----------------------------------|
| 30      | Finance            | Financial systems & payroll      |
| 31      | HR                 | Human Resources                  |
| 32      | DirectorAndAdmins  | Executive offices                |
| 33      | Conference         | Meeting rooms & AV systems       |
| 34      | Training           | Medical training facilities      |
| 35      | Cafe               | Public Wi-Fi & POS systems       |

---

## IP Addressing Architecture

### Subnet Design Strategy
The network utilizes **192.168.1.0/22** (192.168.1.1 - 192.168.3.198) providing **1,024 addresses** with:
- Efficient VLAN-based subnetting
- Scalable address reservation (30% of total space reserved)
- Hierarchical allocation for easy expansion

### VLAN IP Allocation Table
| VLAN ID | Purpose                  | Network Address   | Subnet Mask | Usable IP Range            | Hosts | Broadcast     |
|---------|--------------------------|-------------------|-------------|----------------------------|-------|---------------|
| 10      | Reception & Waiting      | 192.168.1.0       | /24         | 192.168.1.1-254            | 254   | 192.168.1.255 |
| 11      | Consultation Rooms       | 192.168.2.128     | /26         | 192.168.2.129-190          | 64    | 192.168.2.191 |
| 12      | Building A Administration| 192.168.3.96      | /28         | 192.168.3.97-110           | 16    | 192.168.3.111 |
| 20      | Specialist Services      | 192.168.2.192     | /26         | 192.168.2.193-254          | 64    | 192.168.2.255 |
| 21      | Laboratories             | 192.168.3.64      | /27         | 192.168.3.65-94            | 32    | 192.168.3.95  |
| 23      | Pharmacy                 | 192.168.3.128     | /28         | 192.168.3.129-142          | 16    | 192.168.3.143 |
| 30      | Finance                  | 192.168.3.144     | /28         | 192.168.3.145-158          | 16    | 192.168.3.159 |
| 31      | HR Department            | 192.168.3.176     | /29         | 192.168.3.177-182          | 8     | 192.168.3.183 |
| 32      | Executive Offices        | 192.168.3.160     | /28         | 192.168.3.161-174          | 16    | 192.168.3.175 |
| 33      | Conference Rooms         | 192.168.3.0       | /26         | 192.168.3.1-62             | 64    | 192.168.3.63  |
| 34      | Training Facilities      | 192.168.3.184     | /28         | 192.168.3.185-198          | 16    | 192.168.3.199 |
| 35      | Café & Public Access     | 192.168.2.0       | /25         | 192.168.2.1-126            | 128   | 192.168.2.127 |

---

## Network Configuration

### Building A - Layer 2 Switch Configurations

#### **Switch 1 (Reception & Waiting Area)**
```cisco
! Core Configuration
vlan 10
 name ReceptionAndWaiting
!
interface range FastEthernet0/2-24
 switchport mode access
 switchport access vlan 10
!
interface FastEthernet0/1
 switchport mode trunk
!
do write

! VLAN Configuration
vlan 11
 name Consultation
vlan 12
 name AdminBuildingA

! Port Assignments
interface range FastEthernet0/2-16
 switchport mode access
 switchport access vlan 11

interface range FastEthernet0/17-24
 switchport mode access
 switchport access vlan 12

! Trunk Configuration
interface FastEthernet0/1
 switchport mode trunk
!
do write
```

## Building B - Layer 3 Switch Configuration

### Core Switch Configuration (Multilayer Switch 0)
```cisco
! VLAN Definitions
vlan 20
 name SpecialistServices
vlan 21
 name Laboratories
vlan 22
 name Pharmacy
vlan 23
 name AdminisBuildingB

! Trunk Port Configuration
interface range FastEthernet0/1-3
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
do write

vlan 20
 name SpecialistServices

interface range FastEthernet0/2-24
 switchport mode access
 switchport access vlan 20

interface FastEthernet0/1
 switchport mode trunk
!
do write

vlan 21
 name Laboratories

interface range FastEthernet0/2-24
 switchport mode access
 switchport access vlan 21

interface FastEthernet0/1
 switchport mode trunk
!
do write

! Dual VLAN Configuration
vlan 22
 name Pharmacy
vlan 23
 name AdminisBuildingB

! Port Assignments
interface range FastEthernet0/2-9
 switchport mode access
 switchport access vlan 22

interface range FastEthernet0/10-24
 switchport mode access
 switchport access vlan 23

! Trunk Setup
interface FastEthernet0/1
 switchport mode trunk
!
do write
```

## Building C - Layer 3 Switch Configuration

### Core Switch Configuration (Multilayer Switch 2)
```cisco
! VLAN Definitions for Building C
vlan 30
 name Finance
vlan 31
 name HR
vlan 32
 name DirectorAndAdmins
vlan 33
 name Conference
vlan 34
 name Training
vlan 35
 name Cafe

! Trunk Port Configuration
interface range FastEthernet0/1-4
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
do write

vlan 30
 name Finance
vlan 31
 name HR

interface range FastEthernet0/3-7
 switchport mode access
 switchport access vlan 30

interface range FastEthernet0/9-12
 switchport mode access
 switchport access vlan 31

interface FastEthernet0/1
 switchport mode trunk
!
do write

vlan 33
 name Conference

interface range FastEthernet0/2-24
 switchport mode access
 switchport access vlan 33

interface FastEthernet0/1
 switchport mode trunk
!
do write

! Dual VLAN Configuration
vlan 34
 name Training
vlan 35
 name Cafe

interface range FastEthernet0/2-17
 switchport mode access
 switchport access vlan 34

interface range FastEthernet0/18-24
 switchport mode access
 switchport access vlan 35

interface FastEthernet0/1
 switchport mode trunk
!
do write
```


## Core Network Configuration

### Core Switch Configuration (Multilayer Switch)
```cisco
! VLAN Definitions for All Buildings
vlan 10
 name ReceptionAndWaiting
vlan 11
 name Consultation
vlan 12
 name AdminBuildingA
vlan 20
 name SpecialistServices
vlan 21
 name Laboratories
vlan 22
 name Pharmacy
vlan 23
 name AdminBuildingB
vlan 30
 name Finance
vlan 31
 name HR
vlan 32
 name DirectorAndAdmins
vlan 33
 name Conference
vlan 34
 name Training
vlan 35
 name Cafe

! Trunk Port Configuration
interface FastEthernet0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
do write
```

## Router Configuration (Cisco 2901)

### Base Interface Activation
```cisco
interface GigabitEthernet0/0
 no shutdown
!

interface g0/0.10
 encapsulation dot1q 10
 ip address 192.168.1.1 255.255.255.0
!
interface g0/0.11
 encapsulation dot1q 11
 ip address 192.168.2.129 255.255.255.192
!
... [Similar configuration for other sub-interfaces]
!
do write
```

## DHCP Configuration for Automated IP Management

### Global DHCP Activation
```cisco
service dhcp
!
```

### VLAN-Specific DHCP Pools

| VLAN ID | Pool Name               | Network            | Subnet Mask       | Default Gateway   | DNS Server       |
|---------|-------------------------|--------------------|-------------------|-------------------|------------------|
| 10      | ReceptionAndWaiting-Pool| 192.168.1.0        | 255.255.255.0     | 192.168.1.1       | 192.168.1.1      |
| 11      | Consultation-Pool       | 192.168.2.128      | 255.255.255.192   | 192.168.2.129     | 192.168.2.129    |
| 12      | AdminBuildingA-Pool     | 192.168.3.96       | 255.255.255.240   | 192.168.3.97      | 192.168.3.97     |
| 20      | SpecialistsServices-Pool| 192.168.2.192      | 255.255.255.192   | 192.168.2.193     | 192.168.2.193    |
| 21      | Laboratories-Pool       | 192.168.3.64       | 255.255.255.224   | 192.168.3.65      | 192.168.3.65     |
| 22      | Pharmacy-Pool           | 192.168.3.112      | 255.255.255.240   | 192.168.3.113     | 192.168.3.113    |
| 23      | AdminBuildingB-Pool     | 192.168.3.128      | 255.255.255.240   | 192.168.3.129     | 192.168.3.129    |
| 30      | Finance-Pool            | 192.168.3.144      | 255.255.255.240   | 192.168.3.145     | 192.168.3.145    |
| 31      | HR-Pool                 | 192.168.3.176      | 255.255.255.248   | 192.168.3.177     | 192.168.3.177    |
| 32      | DirectorsAndAdmins-Pool | 192.168.3.160      | 255.255.255.240   | 192.168.3.161     | 192.168.3.161    |
| 33      | Conference-Pool         | 192.168.3.0        | 255.255.255.192   | 192.168.3.1       | 192.168.3.1      |
| 34      | Training-Pool           | 192.168.3.184      | 255.255.255.240   | 192.168.3.185     | 192.168.3.185    |
| 35      | Cafe-Pool               | 192.168.2.0        | 255.255.255.128   | 192.168.2.1       | 192.168.2.1      |

### Sample Configuration for VLAN 10
```cisco
ip dhcp pool ReceptionAndWaiting-Pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1 
 dns-server 192.168.1.1
```
---

## Wireless Network Configuration
For network security, **WPA-PSK (Wi-Fi Protected Access - Pre-Shared Key)** was chosen as the authentication method, which uses **AES (Advanced Encryption Standard)** for encrypting the data. This approach ensures that users must enter a shared password to connect to the network.

### Key Wireless Settings:
- **SSID for Waiting Rooms**: `Hospital`  
- **Authentication**: WPA-PSK  
- **Encryption**: AES  
- **Passphrase**: `++123456789++`  

---

## Testing Results

### DHCP and VLAN Validation
1. **PC1 (VLAN 10 - Reception/Waiting Area)**  
   - Assigned IP: `192.168.1.2`  
   - Subnet Mask: `255.255.255.0`  
   - Default Gateway: `192.168.1.1`  
   - DNS Server: `192.168.1.1`  

2. **PC8 (VLAN 20 - Specialist Services)**  
   - Assigned IP: `192.168.2.194`  
   - Subnet Mask: `255.255.255.192`  
   - Default Gateway: `192.168.2.193`  
   - DNS Server: `192.168.2.193`  

### Inter-VLAN Communication Test
A successful ping from **PC8 (VLAN 20)** to **PC1 (VLAN 10)** confirmed proper routing:  
```powershell
C:\> ping 192.168.1.2

Ping statistics for 192.168.1.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss)
    Approximate round trip times:
    Minimum = 0ms, Maximum = 2ms, Average = 1ms
