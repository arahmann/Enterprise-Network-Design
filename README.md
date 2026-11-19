# Enterprise Network Design & Implementation

## 1. Project Overview

This project demonstrates the design and implementation of a multi-department enterprise network using Cisco Packet Tracer.

The network consists of four departments: HR, Finance, IT, and Sales. Each department operates in a separate VLAN to provide logical network segmentation.

Inter-VLAN communication is enabled through Router-on-a-Stick routing. DHCP provides automatic IP addressing, DNS enables hostname resolution, and Extended Access Control Lists (ACLs) enforce communication restrictions between departments.

### Objectives

- Design a multi-department enterprise network.
- Implement VLAN-based network segmentation.
- Configure 802.1Q trunk links.
- Enable inter-VLAN routing.
- Configure DHCP for automatic IP assignment.
- Implement DNS hostname resolution.
- Apply extended ACLs for network security.
- Verify connectivity and security policies.

## 2. Technologies Used

- Cisco Packet Tracer
- Cisco 2911 Router
- Cisco Catalyst 2960 Switches
- VLANs and 802.1Q Trunking
- Router-on-a-Stick
- DHCP
- DNS
- Extended ACLs
- IPv4 Addressing and Subnetting

## 3. Network Topology

The network consists of:

- 1 × Cisco 2911 Router (R1)
- 2 × Cisco 2960 Switches (SW1 and SW2)
- 8 × PCs across four departments
- 1 × Server providing DNS services

### Topology Diagram

![Enterprise network topology](<./Complete network topology.png>)

### Department Distribution

| Department | VLAN | Devices |
|---|---:|---|
| HR | 10 | HR-01, HR-02 |
| Finance | 20 | Finance-01, Finance-02 |
| IT | 30 | IT-01, IT-02 |
| Sales | 40 | Sales-01, Sales-02 |

### Network Addressing

| Department | Network | Default Gateway |
|---|---|---|
| HR | 192.168.10.0/24 | 192.168.10.1 |
| Finance | 192.168.20.0/24 | 192.168.20.1 |
| IT | 192.168.30.0/24 | 192.168.30.1 |
| Sales | 192.168.40.0/24 | 192.168.40.1 |

### Infrastructure Addressing

| Device | IP Address | Purpose |
|---|---|---|
| R1 G0/0.10 | 192.168.10.1 | HR Gateway |
| R1 G0/0.20 | 192.168.20.1 | Finance Gateway |
| R1 G0/0.30 | 192.168.30.1 | IT Gateway |
| R1 G0/0.40 | 192.168.40.1 | Sales Gateway |
| DNS Server | 192.168.30.100 | DNS Service |

### Network Connections

- SW1 Gi0/1 ↔ SW2 Gi0/1: Switch trunk link.
- SW1 Gi0/2 ↔ R1 Gi0/0: Router trunk link.
- SW1 Fa0/5: DNS Server (VLAN 30).
- PC access ports: Fa0/1–Fa0/4 on both switches.

## 4. Network Services Configuration

### DHCP

Configured DHCP on R1 with four separate address pools:

- HR: 192.168.10.0/24
- Finance: 192.168.20.0/24
- IT: 192.168.30.0/24
- Sales: 192.168.40.0/24

The first ten addresses in each subnet were excluded from dynamic allocation to reserve addresses for gateways and infrastructure.

Each DHCP pool distributes:

- IP address
- Subnet mask
- Default gateway
- DNS server address (192.168.30.100)

### DNS

A dedicated server was configured with:

- IP address: 192.168.30.100
- Default gateway: 192.168.30.1
- DNS service: Enabled

DNS record:

| Hostname | Record Type | IP Address |
|---|---|---|
| www.company.local | A | 192.168.30.100 |

## 5. Network Security — Extended ACLs

Two extended ACLs were configured on R1.

### Security Policy

| Source | Destination | Policy |
|---|---|---|
| HR | Finance | Deny |
| Finance | HR | Deny |
| HR | DNS Server | Permit |
| Finance | DNS Server | Permit |
| Other traffic | Other destinations | Permit |

### ACL Placement

- HR-FILTER: Applied inbound on R1 G0/0.10.
- FINANCE-FILTER: Applied inbound on R1 G0/0.20.

The ACLs enforce department-level communication restrictions while preserving access to the DNS server.

## 6. Testing and Verification

The network was tested to verify connectivity, DHCP allocation, DNS resolution, inter-VLAN routing, and ACL enforcement.

### VLAN Verification

![VLAN configuration on SW1](./show%20vlan%20brief%20on%20SW1.png)

### Trunk Verification

![Trunk interfaces on SW1](./show%20interfaces%20trunk%20on%20SW1.png)

### Router Interface Verification

![Router interface status](./show%20ip%20interface%20brief%20on%20R1.png)

### DNS Resolution Test

![Successful DNS ping from HR-01](./Successful%20DNS%20ping%20from%20HR-01.png)

### ACL Verification

![ACL match counters on R1](<./show access-lists on R1, showing match counters.png>)

The `show access-lists` command confirmed that the configured ACL rules matched traffic.

Observed match counters:

- HR → DNS Server: 9 matches
- HR → Finance: 8 matches
- Finance → DNS Server: 4 matches
- Finance → HR: 10 matches

### Test Results

| Test | Expected Result | Actual Result |
|---|---|---|
| HR-01 → HR-02 | Successful | PASS |
| Finance-01 → Finance-02 | Successful | PASS |
| HR → Finance | Blocked | PASS |
| Finance → HR | Blocked | PASS |
| HR → DNS Server | Successful | PASS |
| Finance → DNS Server | Successful | PASS |
| DNS hostname resolution | Successful | PASS |
| DHCP address assignment | Successful | PASS |

### Conclusion

Successfully designed and implemented a segmented enterprise network with centralized routing, automated IP addressing, DNS resolution, and department-level access control.

All configured network services and security policies passed their respective tests.

## 7. Project Files

- [Download the Cisco Packet Tracer simulation](./Enterprise_Network_Design_VLAN_DHCP_DNS_ACL.pkt)
- [View the topology image](<./Complete network topology.png>)

Open the `.pkt` file using Cisco Packet Tracer to inspect and test the network.

## 8. Future Improvements

- Implement HSRP for gateway redundancy.
- Configure a second router for high availability.
- Add static routing or dynamic routing for additional network segments.
- Implement additional security policies and monitoring.
