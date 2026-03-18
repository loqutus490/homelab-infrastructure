# Architecture Overview

## Environment Summary
My homelab is designed to simulate a small production-like environment using multiple physical servers, virtual machines, and containers.

## Major Components
- Hypervisor: Proxmox
- Virtual Machines: Linux servers, utility VMs, test environments
- Containers: Self-hosted applications and supporting services
- Storage: Local and/or shared storage depending on host role
- Networking: VLANs, firewall segmentation, reverse proxy, internal DNS
- Monitoring: Uptime and system visibility tools
- Backups: Scheduled backups with restore validation

## Design Decisions

### Why Proxmox
I chose Proxmox because it provides enterprise-style virtualization features, supports both VMs and containers, and gave me a practical platform for learning infrastructure operations.

### Why Containers for Some Services
I used containers where lightweight deployment and portability made more sense than full virtual machines. This reduced overhead and made service updates easier.

### Why Separate Services Across VMs/Containers
I separated roles to improve isolation, reduce blast radius, and better mirror real-world infrastructure practices.

### Security Considerations
- Limited public exposure
- Firewall rules between segments
- Minimal service permissions
- Backups and recovery planning
- Configuration sanitization before publishing to GitHub
