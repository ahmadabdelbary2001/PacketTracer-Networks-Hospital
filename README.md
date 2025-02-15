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

## IP Addressing & DHCP
- **Subnet Schema**: /24 subnet per VLAN
- **DHCP Configuration**:
  - Address pools dynamically assigned per VLAN
  - Reservations for critical medical devices
  - Lease time: 8 hours for staff devices, 2 hours for guest network

- **Key IP Ranges**:
  - VLAN 10: 192.168.10.1-254
  - VLAN 21: 192.168.21.1-254 
  - VLAN 32: 192.168.32.1-254

---

## Security Implementation
1. **Access Control**:
   - Port security on all access switches
   - 802.1X authentication for network devices

2. **Traffic Management**:
   - ACLs for inter-VLAN communication
   - QoS prioritization for medical traffic

3. **Wireless Security**:
   - Separate SSIDs for staff/patients/guests
   - Captive portal for guest access

---

## Network Diagram Note
The actual Packet Tracer file (`Hospital.pkt`) contains:
- Full physical topology layout
- Detailed device configurations
- Complete cable connections
- Testing scenarios for critical pathways

---

## Conclusion
This design provides a future-ready infrastructure that:
✅ Ensures HIPAA-compliant data security  
✅ Supports high-availability medical applications  
✅ Enables seamless staff/patient mobility  
✅ Allows modular expansion as needs evolve

**Last Updated**: [Insert Date]
