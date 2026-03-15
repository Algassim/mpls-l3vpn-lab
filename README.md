# MPLS Layer 3 VPN Lab

## Overview

This project demonstrates the implementation of a **Service Provider MPLS Layer 3 VPN network** supporting multiple customers over a shared backbone.

The lab focuses on the core MPLS architecture and the interaction between:

- **Provider Core Routers (P)**
- **Provider Edge Routers (PE)**
- **Customer Edge Routers (CE)**

Two customers are connected to the provider network:

- **Customer A (OGI)** using **OSPF**
- **Customer B (SON)** using **EIGRP**

Each customer has two remote sites connected to different PE routers. The MPLS backbone ensures secure and isolated communication between customer sites using **VRF and MP-BGP VPNv4**.

---

## Network Architecture

### Provider Network

The service provider backbone contains:

- **4 Core Routers**
  - P1
  - P2
  - P3
  - P4

- **2 Provider Edge Routers**
  - PE1
  - PE2
  
  **4 Customer Networks**
  - CE-A1
  - CE-A2
  - CE-B1
  - CE-B2
  
Core characteristics:

- OSPF used as the **provider IGP**
- MPLS enabled on core interfaces  
- **LDP** used for label distribution
- **MP-BGP VPNv4** used to exchange VPN routes between PE routers

---

### Customer Networks

Two customers are connected to the provider infrastructure.

#### Customer A – OGI

- CE-A1 connected to **PE1**
- CE-A2 connected to **PE2**
- Routing protocol: **OSPF**

#### Customer B – SON

- CE-B1 connected to **PE1**
- CE-B2 connected to **PE2**
- Routing protocol: **EIGRP**

Each customer operates in an independent **VRF** ensuring full routing isolation.

---

## Key Technologies Used

This lab demonstrates several important technologies used in Service Provider networks:

### MPLS Core

- MPLS forwarding
- Label Distribution Protocol (**LDP**)
- MPLS label switching
- OSPF for core routing

### MPLS L3VPN

- Virtual Routing and Forwarding (**VRF**)
- Route Distinguisher (**RD**)
- Route Target (**RT**)
- **MP-BGP VPNv4**

### Customer Routing

- **OSPF** between CE and PE for Customer A

- **EIGRP** between CE and PE for Customer B

- Route redistribution between BGP and customer protocols

---

## Addressing Overview

### Provider Loopbacks

| Router | Loopback |

| PE1 | 1.1.1.1 |

| PE2 | 2.2.2.2 |

| P1 | 4.4.4.4 |

| P2 | 5.5.5.5 |

| P3 | 6.6.6.6 |

| P4 | 7.7.7.7 |


These loopbacks are used for:

- OSPF router IDs
- LDP sessions
- MP-BGP peering
---
### Customers Loopbacks

| CE-A1 | 172.16.1.1/24 |

| CE-A2 | 172.16.2.1/24 |

| CE-B1 | 172.17.1.1/24 |

| CE-B2 | 172.17.2.1/24 |

## VRF Configuration

Two VRFs are configured on the Provider Edge routers.

### VRF OGI

RD: 100:1

RT export: 100:1

RT import: 100:1

### VRF SON

RD: 200:1

RT export: 200:1

RT import: 200:1

### Verification commands
  
  Configure **OSPF** across the provider backbone.
  
  Verify:
  
  show ip ospf neighbor
  
  show ip route
  
### Provider Core Routing

  Configure **OSPF** across the provider backbone.
  
  Verify:
  
  ip cef
  
  mpls label range 100 199
  
  mpls ip
  
### Verify LDP neighbors:

  show mpls ldp neighbor
  
  show mpls forwarding-table

### VRF Configuration

  Create VRFs on PE routers and associate CE-facing interfaces.
  
  Example:
  
  ip vrf OGI
  
  rd 100:1
  
  route-target export 100:1
  
  route-target import 100:1

### MP-BGP Configuration
  Establish iBGP between PE routers using loopbacks.
  Activate VPNv4 address-family.
  
  Verify:
  
  show ip bgp vpnv4 all

---

## Expected Results
- OGI Site 1 communicates with OGI Site 2
- SON Site 1 communicates with SON Site 2
- OGI and SON remain completely isolated
- MPLS labels are used for forwarding across the provider core

---

## Tools Used

- **EVE-NG**
- **Cisco IOSv / IOSvL2**
- MPLS L3VPN configuration

