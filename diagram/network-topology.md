NETWORK_TOPOLOGY.md
# Network Topology

This document describes the network topology of my homelab infrastructure, including segmentation, routing, and service exposure. The design is intentionally structured to simulate a small production-style environment with separation of concerns, security boundaries, and service isolation.

---

## Topology Overview

The network follows a simplified hierarchical model adapted for a homelab environment. Instead of full enterprise hardware layers, logical segmentation is implemented using VLANs, firewall rules, and virtual networking.

### Key Design Goals
- Isolate services to reduce blast radius
- Separate management traffic from application traffic
- Enable secure internal-only services
- Allow controlled external access via reverse proxy
- Simulate real-world infrastructure patterns

---

## Core Layer

In this homelab, the core layer is implemented logically through the primary router/firewall rather than dedicated enterprise core switches.

### Responsibilities
- Default gateway for all VLANs
- Inter-VLAN routing
- Firewall policy enforcement
- NAT for outbound internet access

### Implementation
- Router/Firewall: *(e.g., pfSense / OPNsense / UniFi Gateway — replace with yours)*
- Handles:
  - Routing between VLANs
  - WAN connectivity
  - Port forwarding (limited and controlled)
  - DNS forwarding (optional internal DNS)

### Design Decisions
- Centralized routing simplifies control and troubleshooting
- Firewall rules enforce least-privilege communication between segments
- All traffic between VLANs must pass through the firewall (no lateral movement by default)

---

## Distribution Layer

The distribution layer is implemented using a managed switch with VLAN support.

### Responsibilities
- VLAN segmentation
- Traffic aggregation between physical hosts
- Forwarding traffic to the core (firewall/router)

### Implementation
- Managed Switch: *(VLAN-capable switch)*
- Configured VLANs:
  - VLAN 10 – Management Network
  - VLAN 20 – Server/VM Network
  - VLAN 30 – Container/Services Network
  - VLAN 40 – Client Devices
  - VLAN 50 – Isolated/Test Network

### Design Decisions
- VLAN segmentation improves security and organization
- Services are grouped by function rather than host
- Test/experimental workloads are isolated to prevent impact on stable services

---

## Access Layer

The access layer consists of physical and virtual endpoints connected to the network.

### Physical Devices
- Proxmox Host 1 (multi-VM workloads)
- Proxmox Host 2 (optional / additional compute)
- Personal workstation
- Miscellaneous client devices

### Virtualized Resources
- Linux VMs (application servers, utility systems)
- Docker containers (self-hosted services)
- Monitoring and logging services

### Network Integration
- Proxmox bridges connect VMs to VLANs
- Containers are attached via host networking or Docker bridge networks
- Static IPs or DHCP reservations used for critical services

---

## VLAN Segmentation

| VLAN | Purpose              | Example Systems                    |
|------|--------------------|----------------------------------|
| 10   | Management          | Proxmox UI, SSH access            |
| 20   | Servers/VMs         | Linux VMs, backend services       |
| 30   | Containers/Apps     | Docker services, internal apps    |
| 40   | Clients             | Workstations, personal devices    |
| 50   | Lab/Test            | Experimental VMs and services     |

### Security Model
- Management VLAN is restricted to admin devices only
- Client VLAN cannot directly access management interfaces
- Lab VLAN has limited outbound/inbound rules
- Only required ports are opened between VLANs

---

## Service Exposure Model

### Internal Services
- Hosted on VM/Container VLANs
- Accessible only within internal network
- Examples:
  - Internal dashboards
  - Development services
  - Monitoring tools

### External Access
- Routed through a reverse proxy
- Only specific services exposed
- TLS termination handled at proxy layer

### Reverse Proxy
- *(e.g., NGINX / Traefik / Caddy — replace with yours)*
- Provides:
  - Centralized routing
  - SSL/TLS termination
  - Domain-based access control

---

## Network Diagram

Below is a logical representation of the topology:

```mermaid
flowchart TD
    Internet --> Firewall

    Firewall --> VLAN10[Management VLAN]
    Firewall --> VLAN20[Server VLAN]
    Firewall --> VLAN30[Container VLAN]
    Firewall --> VLAN40[Client VLAN]
    Firewall --> VLAN50[Test VLAN]

    VLAN10 --> Proxmox[Proxmox Hosts]
    VLAN20 --> VMs[Linux VMs]
    VLAN30 --> Containers[Docker Services]
    VLAN40 --> Clients[User Devices]
    VLAN50 --> Lab[Test/Experimental]

    Containers --> ReverseProxy
    ReverseProxy --> Internet