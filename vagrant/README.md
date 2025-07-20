# Vagrant Infrastructure Setup

This directory contains the Vagrant configuration for setting up a complete Kubernetes infrastructure with supporting services.

## Architecture Overview

The Vagrant setup creates the following virtual machines:

### Load Balancer
- **VM Name**: `lb1`
- **IP**: `192.168.56.10`
- **Resources**: 2 CPU cores, 2GB RAM
- **Purpose**: Load balancing for Kubernetes API servers

### Kubernetes Master Nodes (3x)
- **VM Names**: `master1`, `master2`, `master3`
- **IPs**: `192.168.56.11`, `192.168.56.12`, `192.168.56.13`
- **Resources**: 4 CPU cores, 8GB RAM each
- **Purpose**: Kubernetes control plane nodes for high availability

### Kubernetes Worker Nodes (3x)
- **VM Names**: `worker1`, `worker2`, `worker3`
- **IPs**: `192.168.56.21`, `192.168.56.22`, `192.168.56.23`
- **Resources**: 8 CPU cores, 12GB RAM each
- **Purpose**: Kubernetes worker nodes for running applications

### NFS Server
- **VM Name**: `nfs`
- **IP**: `192.168.56.40`
- **Resources**: 2 CPU cores, 4GB RAM
- **Purpose**: Network File System for persistent storage

### Vault Server
- **VM Name**: `vault`
- **IP**: `192.168.56.50`
- **Resources**: 2 CPU cores, 4GB RAM
- **Purpose**: HashiCorp Vault for secrets management

## Prerequisites

### System Requirements
- **CPU**: Multi-core processor (minimum 8 cores recommended)
- **RAM**: Minimum 32GB (recommended 64GB)
- **Storage**: At least 100GB free space
- **Network**: Private network `192.168.56.0/24` available

## Quick Start

### 2. Start All VMs
```bash
# Navigate to vagrant directory
cd vagrant

# Start all VMs (this will take some time)
vagrant up

# Check status
vagrant status
```

### 3. Start Individual VMs
```bash
# Start specific VMs
vagrant up lb1
vagrant up master1 master2 master3
```

## VM Management

### SSH Access
```bash
# SSH into any VM
vagrant ssh lb1
```

### VM Operations
```bash
# Stop all VMs
vagrant halt

# Stop specific VM
vagrant halt master1

# Restart VM
vagrant reload master1

# Destroy and recreate VM
vagrant destroy master1
vagrant up master1

# Destroy all VMs
vagrant destroy -f
```

### VM Status
```bash
# Check status of all VMs
vagrant status

# Get detailed info
vagrant ssh-config
```
