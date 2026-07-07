# My First Main Lab Project Practice

This is my first major Cisco Packet Tracer project outside of regular laboratory exercises. Although it has limited features and lacks several advanced security and traffic control protocols found in production environments, it is the best networking project I have built so far. I am far from being an expert, but I believe I am making steady progress with every project.

---

# Certification & Training Center LAN/WAN Infrastructure

## 1. Project Overview

The goal of this project is to simulate the network infrastructure of a small-to-medium Certification & Training Center company. It demonstrates the implementation of a network that is intended to be secure, scalable, and highly available while supporting multiple departments, centralized services, and reliable communication.

Although this project is only a simulation, it attempts to reflect real-world enterprise networking concepts such as redundancy, segmentation, routing, and security. I understand that production environments involve far more complex requirements, stricter security policies, and additional technologies beyond what is implemented here.

---

## 2. Objectives

* Design a scalable campus network for multiple departments.
* Implement redundancy and high availability.
* Explore Zero Trust concepts using ACLs and firewall policies.
* Simulate Internet (Cloud) connectivity.
* Explore networking protocols and enterprise design practices.

---

## 3. Logical Topology

The network follows a **Hierarchical Campus Network Design** using a **Collapsed Core Architecture**.

The **SW8-CORE** switch serves as the centralized Layer 3 routing and distribution device for all access switches. On the WAN side, the design demonstrates Internet connectivity, gateway redundancy through HSRP, firewall security policies, NAT, and ISP connectivity.

Each laboratory VLAN contains only one PC for simulation purposes. In a real deployment, each lab would represent multiple client devices, but using a single PC per VLAN keeps the topology manageable while still demonstrating the intended functionality.

---

# 4. Network Devices, Concepts, Protocols & Features

## A. Access Layer Switches

### Description

* Consists of eight Layer 2 access switches (seven internal and one external).
* Each switch represents a single laboratory VLAN.

---

## B. Core Layer Switch

### Description

* Acts as the central Layer 3 routing and distribution device.
* Performs Inter-VLAN Routing.
* Centralizes ACL implementation instead of configuring ACLs on every Layer 2 switch.

### Current Limitations

The Core Switch acts as the aggregation point for every access switch. While it functions correctly in this topology, a production environment would normally include a second Layer 3 core switch to improve scalability, eliminate bottlenecks, and remove single points of failure—particularly on the WAN-facing side.

---

## C. Access Control Lists (ACLs)

### Description

ACLs were configured to control communication between VLANs.

* VLANs 10, 20, 30, and 40 are allowed access to the LMS and FTP servers through specific services.
* VLAN 50 is restricted from accessing other networks except its own default gateway.
* VLAN 90 is permitted to access only the FTP service.
* Internal clients receive IP addresses through DHCP.
* DNS allows authorized users to access internal services using domain names instead of IP addresses.

### Current Limitations

One configuration issue I have not fully resolved is that removing a permit statement from the ACL still allows traffic because it eventually matches the final:

`permit ip any any`

statement.

This is an area I continue to study to better understand ACL processing and policy design.

---

## D. Firewall

### Description

The firewall separates the internal and external networks using security levels.

* Inside Interface — Security Level **100**
* Outside Interface — Security Level **0**

Current inbound firewall policy:

**Permitted**

* ICMP Echo Replies
* ICMP Error Messages responding to internal hosts

**Denied**

* All other unsolicited inbound traffic

### Current Limitations

While learning more about enterprise security, I realized this implementation does not fully follow a Zero Trust approach.

Instead of assuming outbound traffic is trusted, a stricter implementation should validate and inspect both inbound and outbound communications according to least-privilege principles.

---

## E. HSRP Routers & NAT

### Description

The HSRP routers provide gateway redundancy while also performing NAT before traffic exits toward the simulated Internet.

This design allows private internal addresses to be translated into public addresses while maintaining high availability.

### Current Limitations

A significant issue identified during project evaluation was that if the Active HSRP router loses its connection to the ISP, it continues to remain the Active gateway because the LAN interface participating in HSRP is still operational.

As a result, client traffic continues to be forwarded to the Active router even though it no longer has Internet connectivity.

Based on feedback from our professor, a better implementation would use **HSRP Interface Tracking**, allowing the router to monitor its upstream interface. If that interface fails, the router automatically lowers its HSRP priority, allowing the Standby router to become the Active gateway and preventing traffic blackholing.

---

## F. ISP Routers

### Description

Separate ISP routers were included to better simulate a real-world deployment.

Rather than connecting enterprise routers directly to the Internet, organizations typically connect to ISP-owned routers. NAT is therefore implemented on the enterprise HSRP routers before traffic reaches the ISP infrastructure.

---

## G. Cloud Interfaces

### Description

The Cloud devices serve only as simulated transmission media.

Their purpose is to bridge Ethernet connectivity toward the DSL modem and simulate Internet infrastructure.

---

## H. DSL Modem

### Description

The modem forwards traffic between Ethernet and telephone connections by converting Ethernet frames into signals suitable for transmission over telephone lines.

---

## I. External Network

### Description

The external network represents the Certification Authority or certifying organization's infrastructure.

It contains external services that students may access after completing their certification requirements.

A DNS server was also included so internal users can access services using domain names instead of manually entering IP addresses.

---

# 5. Future Improvements

## A. Remove Single Points of Failure

Improve redundancy between:

* Core Switch
* Firewall
* WAN Infrastructure

---

## B. Improve Simulation Realism

This project focuses primarily on logical network design.

Future improvements include:

* Physical topology considerations
* Realistic hardware deployment
* Live attack simulations
* Link failures
* Performance testing

---

## C. Improve Redundancy

Instead of relying entirely on dual links protected by STP, future versions could implement EtherChannel where appropriate.

Due to Packet Tracer limitations on supported port channels, a hybrid approach could be adopted:

* EtherChannel for laboratory access switches
* STP-protected redundant links where EtherChannel is impractical

Firewall redundancy could also be implemented.

---

## D. Expand Security & Monitoring

Future versions may include:

* Intrusion Detection/Prevention Systems (IDS/IPS)
* Centralized logging
* SIEM integration
* AAA services
* Network monitoring
* Additional enterprise security controls

---

# Takeaways

Working on this project taught me that designing an enterprise network involves much more than simply connecting devices together. High availability, redundancy, scalability, and security all require careful planning and consideration.

While this simulation only represents a small portion of what exists in production environments, it has given me a stronger understanding of enterprise networking concepts and highlighted areas where I still have much to learn.

As I continue studying networking and cybersecurity, I plan to improve this project by incorporating more advanced technologies, better security practices, and more realistic enterprise designs.
