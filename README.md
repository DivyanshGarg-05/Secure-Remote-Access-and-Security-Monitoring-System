# Secure-Remote-Access-and-Security-Monitoring-System
A virtualized corporate network with a WireGuard VPN, isolated internal servers, and centralized Grafana security monitoring.

# Virtual Machine Networking Lab

## Project Overview
This repository contains the configuration files, scripts, and documentation for a 3-node Ubuntu virtual machine lab environment. 

## Phase 0: Initial Setup and Network Configuration
During Phase 0, three Ubuntu virtual machines were deployed. Each machine was configured with two network adapters: a NAT adapter for outbound internet access (`enp0s3`) and a Host-Only adapter (`enp0s8`) for secure, internal communication between the VMs and the host machine. 

Netplan was used to assign static IP addresses to the Host-Only network to ensure reliable connectivity for future lab phases. Git was also initialized on the host machine to version-control the server configurations.

### Network Topology
The virtual machines are configured with the following static IP addresses on the `192.168.56.0/24` network:

| Machine Name | Interface | IP Address | Subnet Mask |
| :--- | :--- | :--- | :--- |
| **VM1 (VPN-Server)** | `enp0s8` | `192.168.56.10` | `/24` |
| **VM2 (Internal Host Server)** | `enp0s8` | `192.168.56.20` | `/24` |
| **VM3 (Monitoring System)** | `enp0s8` | `192.168.56.30` | `/24` |

### Tracked Configuration Files
* **Netplan Configs:** The YAML configuration files used to establish the static IP routing rules (e.g., `50-cloud-init.yaml`).

---
*Document last updated: Phase 0 Completion*