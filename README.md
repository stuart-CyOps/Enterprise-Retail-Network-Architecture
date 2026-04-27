# Enterprise-Retail-Network-Architecture
This project simulates a highly secure, resilient, and highly available network infrastructure for a major retail branch connecting to a centralized Data Center (simulating the Shoprite Brackenfell DC).

Retail networks require absolute stability for Point-of-Sale (POS) systems, strict segmentation to meet PCI-DSS compliance, and centralized management. This lab demonstrates practical competence in achieving these business-critical objectives using Cisco enterprise networking principles.

## Network Architecture & Topology
The topology consists of a localized Branch Network (Layer 2 Switching & Layer 3 Routing) traversing an ISP public network to reach the Corporate Data Center.

(images/1 Architecture.png)

## Phase 1: Enterprise Device Hardening
All network infrastructure must be secured against unauthorized access before routing begins. Passwords are encrypted, and SSH is enforced for remote VTY access.

(Screenshot 2: CLI showing the custom Warning Banner and User Access Verification prompt)

## Phase 2: Branch Network Segmentation (PCI-DSS Compliance)
To prevent the lateral movement of malware from Back Office PCs to the payment systems, the network is segmented using VLANs and an 802.1Q "Router-on-a-Stick" configuration.

(Screenshot 3: Output of show ip interface brief showing virtual subinterfaces dynamically routing VLAN traffic)

## Phase 3: Core Routing & ISP Setup
A simulated ISP router was configured to handle public traffic. Default static routes were configured on the Branch and HQ edge routers to push all non-local traffic out to the internet.

(Screenshot 4: Successful ping from Branch-R1 to HQ-R1's Public WAN IP 203.0.113.2, proving ISP traversal)

## Phase 4: Site-to-Site IPsec VPN (Zero-Trust Transport)
All financial POS data traversing the public ISP must be encrypted. A Site-to-Site IPsec tunnel was built using AES-256 encryption and SHA hashing (Diffie-Hellman Group 5) to guarantee data integrity and confidentiality.

(Screenshot 6: Output of show crypto ipsec sa proving packets are being actively encapsulated/encrypted)

## Phase 5: Testing, Verification & DHCP Fulfillment

Because IPsec VPNs are policy-based, an extended ping was used to simulate POS traffic and wake the tunnel up. Once established, the POS PC successfully pulled its IP address centrally from the Brackenfell Data Center via DHCP.

(Screenshot 5: POS-PC1 successfully receiving 192.168.10.10 via DHCP through the encrypted tunnel)

## Phase 6: Threat Mitigation (Port Security)

To protect against physical breaches (e.g., an attacker unplugging a till and connecting a rogue device), Port Security was enforced on the access switch, locking the port to the Till's specific MAC address.

(Screenshot 7: Output of show port-security int f0/1 showing the port secured and actively restricting unauthorized MAC addresses)

# Key Takeaways & Business Value

### Uptime & Resilience: Designed a standardized, scalable branch networking model.

### PCI-DSS Adherence: Successfully segmented credit card processing networks from standard administrative networks.

### Centralized Management: Demonstrated the ability to manage branch IP allocations centrally via ip helper-address forwarding over an encrypted WAN.

### Cybersecurity Proficiency: Applied foundational zero-trust concepts at both the Layer 2 (Port Security) and Layer 3 (IPsec) boundaries.


