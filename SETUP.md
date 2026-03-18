# Setup Guide

## Prerequisites
- Proxmox host installed
- Managed switch or VLAN-capable networking
- Linux ISO images
- Storage prepared
- Git installed for documentation and version control

## Initial Host Setup
1. Install Proxmox on bare metal
2. Configure management IP
3. Apply updates
4. Create storage pools
5. Configure bridge networking

## Creating a VM
1. Upload ISO
2. Create VM
3. Allocate CPU, RAM, and disk
4. Install Linux
5. Configure static IP or DHCP reservation
6. Harden SSH access
7. Install required packages

## Deploying Containers
1. Install Docker
2. Create service directory
3. Add docker-compose file
4. Start service
5. Validate logs
6. Route through reverse proxy if needed

## Backup Setup
1. Create backup job
2. Set retention policy
3. Test restore workflow
4. Document recovery time and issues
