# VM/Container Layout

This document describes the layout and organization of virtual machines and containers in my homelab infrastructure. The environment is designed to simulate a production-like setup with separation between infrastructure, platform services, and applications.

---

## VM Overview

### Hypervisor Hosts

#### Host 1 – Primary Proxmox Node
- **Role**: Main compute host for VMs and containers
- **CPU**: Multi-core CPU (e.g., 8–16 cores)
- **RAM**: 64–128 GB
- **Storage**: Local SSD/NVMe + additional storage pool
- **Network**: VLAN-aware bridge interfaces
- **Notes**:
  - Hosts majority of production-like services
  - Runs both VMs and container workloads
  - Configured for backups and snapshots

---

#### Host 2 – Secondary Proxmox Node
- **Role**: Additional compute / redundancy / testing
- **CPU**: Mid-range multi-core CPU
- **RAM**: 32–64 GB
- **Storage**: SSD-based storage
- **Network**: Integrated into same VLAN architecture
- **Notes**:
  - Used for lab/testing workloads
  - Supports failover scenarios (manual)
  - Used for experimentation and upgrades

---

#### Host 3 – Storage / Utility Node (Optional)
- **Role**: Backup target / storage services
- **CPU**: Low to mid-range CPU
- **RAM**: 16–32 GB
- **Storage**: High-capacity HDD/SSD array
- **Notes**:
  - Stores VM backups and container data
  - Supports recovery and restore testing

---

## Virtual Machines

### VM-1 – Infrastructure Services VM
- **OS**: Ubuntu Server LTS
- **vCPU**: 2–4 cores
- **RAM**: 4–8 GB
- **Storage**: 40–80 GB
- **Purpose**:
  - DNS services
  - Utility services
  - Internal infrastructure support
- **Network**: Management or server VLAN

---

### VM-2 – Application Server VM
- **OS**: Ubuntu Server LTS
- **vCPU**: 4–8 cores
- **RAM**: 8–16 GB
- **Storage**: 100+ GB
- **Purpose**:
  - Hosts Docker containers
  - Runs self-hosted applications
- **Network**: Container/Services VLAN

---

### VM-3 – Monitoring / Logging VM
- **OS**: Ubuntu Server LTS
- **vCPU**: 2–4 cores
- **RAM**: 4–8 GB
- **Storage**: 50–100 GB
- **Purpose**:
  - Monitoring tools (uptime, metrics)
  - Logging and observability
- **Network**: Server VLAN

---

### VM-4 – Test / Lab Environment
- **OS**: Varies (Linux/Windows)
- **vCPU**: 2–6 cores
- **RAM**: 4–12 GB
- **Storage**: Variable
- **Purpose**:
  - Testing new configurations
  - Simulating failures
  - Learning new technologies
- **Network**: Isolated/Test VLAN

---

## Container Overview

### Container Orchestration Platform

- **Platform**: Docker (standalone, optionally Docker Compose)
- **Version**: *(e.g., Docker 24.x — replace with your version)*
- **Configuration**:
  - Containers grouped by service type
  - Compose files used for multi-service deployments
  - Restart policies enabled (`unless-stopped`)
  - Logs managed via default Docker logging (or centralized logging if configured)

---

### Container Images

Examples of deployed images:

- Reverse proxy (e.g., nginx, traefik, or caddy)
- Monitoring tools (e.g., uptime monitoring, metrics)
- Self-hosted applications (dashboards, utilities)
- Supporting services (databases, cache if used)

**Image Sources**:
- Docker Hub (primary)
- Official vendor registries (when applicable)

**Update Strategy**:
- Manual updates during maintenance windows
- Images reviewed before upgrade
- Critical services tested in lab environment before production update

---

### Container Instances

#### Reverse Proxy Container
- **Purpose**: Central routing for internal/external services
- **CPU**: 1–2 cores
- **RAM**: 512MB – 1GB
- **Ports**:
  - 80 (HTTP)
  - 443 (HTTPS)
- **Network**: Container VLAN
- **Notes**:
  - Handles routing and TLS termination
  - Entry point for most user-facing services

---

#### Application Containers
- **Purpose**: Host self-hosted apps and services
- **CPU**: 1–4 cores per container (depending on service)
- **RAM**: 512MB – 4GB
- **Storage**: Persistent volumes where required
- **Network**: Internal container network + reverse proxy access

---

#### Monitoring Container(s)
- **Purpose**: Track uptime and service health
- **CPU**: 1–2 cores
- **RAM**: 512MB – 2GB
- **Network**: Access to all service endpoints
- **Notes**:
  - Used for operational visibility
  - Alerts on service failure or downtime

---

#### Media / High-Compute Containers (Optional)
- **Purpose**: GPU or high-performance workloads (e.g., media server, AI workloads)
- **CPU/GPU**: High allocation depending on workload
- **RAM**: 4–16 GB+
- **Notes**:
  - Runs on hosts with GPU acceleration
  - Isolated from critical infrastructure services

---

## Network Configuration

- Containers communicate via:
  - Docker bridge networks
  - Reverse proxy routing
- VMs are connected via:
  - Proxmox VLAN-aware bridges
- Critical services use:
  - Static IPs or DHCP reservations
- Public-facing services:
  - Routed through reverse proxy only

---

## Resource Allocation

| Resource       | Total | Allocated | Available |
|----------------|-------|-----------|-----------|
| CPU Cores      | 32    | 20–26     | 6–12      |
| RAM (GB)       | 128   | 80–100    | 28–48     |
| Storage (TB)   | 20–50 | 15–40     | 5–10      |

> Note: Resource allocation varies depending on active workloads and lab experiments.

---

## Design Considerations

### Separation of Concerns
- Infrastructure services separated from applications
- Monitoring isolated from primary workloads
- Test environments isolated from stable services

### Scalability
- Additional VMs can be provisioned quickly
- Containers allow lightweight scaling of services
- Secondary host supports workload distribution

### Resilience
- Backups in place for VMs and critical data
- Services can be redeployed from configuration
- Some workloads can be moved between hosts

---

## Lessons Learned

- Overcommitting CPU is manageable; RAM overcommitment is more risky
- Containers simplify deployment but still require proper networking design
- Separating test and production-like workloads prevents instability
- Resource visibility is critical for maintaining performance
- Documentation becomes essential as the number of services grows

---

## Summary

The VM and container layout reflects a hybrid infrastructure model combining virtualization and containerization. This approach enables flexibility, efficient resource usage, and realistic simulation of production environments while maintaining control and isolation.
