# Service Dependency Map

This document outlines the dependencies between services in my homelab infrastructure. Understanding these relationships helps with troubleshooting, maintenance, recovery planning, and future scaling.

The environment includes virtualization, self-hosted services, reverse proxy routing, monitoring, and supporting network services. Some services are core infrastructure components, while others depend on those components to function correctly.

---

## Purpose

This dependency map exists to document:

- which services are foundational
- which services depend on shared infrastructure
- what breaks when a dependency fails
- how service restoration should be prioritized

This is especially useful during outages, upgrades, migrations, and backup/restore testing.

---

## Service Overview

### Core Infrastructure Services

- **Router/Firewall**
  - Provides internet access, inter-VLAN routing, NAT, and firewall policy enforcement.
  - Acts as a foundational dependency for most network-connected services.

- **Managed Switch / VLAN Infrastructure**
  - Provides network connectivity and segmentation between management, server, client, and lab networks.
  - Required for proper communication between hosts and service segments.

- **Proxmox Hosts**
  - Provide the virtualization platform for running virtual machines and containers.
  - Many services in the lab depend on these hosts being available.

---

### Platform and Supporting Services

- **DNS Service**
  - Provides hostname resolution for internal services.
  - Used by applications, reverse proxy configuration, and administrative access.

- **Reverse Proxy**
  - Routes incoming requests to internal services.
  - Handles centralized access and, where applicable, TLS termination.

- **Docker / Container Runtime**
  - Runs self-hosted applications and supporting services.
  - Dependency for all containerized applications.

- **Storage / Backup Target**
  - Stores backups, exported configs, or persistent service data.
  - Important for recovery and long-term resilience.

---

### Application and Operational Services

- **Monitoring Service**
  - Tracks host or service health, uptime, resource usage, or alerts.
  - Depends on network connectivity and monitored endpoints being reachable.

- **Logging / Observability Services**
  - Collect logs or telemetry from services and hosts.
  - Depend on core infrastructure and source systems being online.

- **Internal Applications**
  - Self-hosted apps, dashboards, utilities, or test services running on VMs or containers.
  - Often depend on reverse proxy, DNS, storage, and the container or VM platform.

---

## Dependency Relationships

### Foundational Layer

The following services form the base of the environment:

- **Router/Firewall**
- **Managed Switch / VLAN Infrastructure**
- **Proxmox Hosts**
- **Storage**

These are considered critical infrastructure dependencies. If any of these fail, multiple downstream services may be affected at the same time.

---

## Dependency Visualization

```mermaid
flowchart TD
    Internet --> Firewall
    Firewall --> Switch
    Switch --> Proxmox
    Switch --> Clients

    Proxmox --> VMServices[Virtual Machines]
    Proxmox --> Containers[Container Services]

    Firewall --> DNS
    Containers --> ReverseProxy
    DNS --> ReverseProxy

    ReverseProxy --> InternalApps[Internal Applications]
    VMServices --> Monitoring
    VMServices --> Logging
    Storage --> Proxmox
    Storage --> InternalApps
    Storage --> Monitoring

    DNS --> InternalApps
    DNS --> Monitoring
