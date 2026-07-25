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


## Phase 1: Infrastructure & Secure Tunneling (Completed)
In this phase, I built a secure, encrypted bridge between a physical Windows host and a virtualized Linux server.

**Key Accomplishments:**
* Configured a dedicated VirtualBox Host-Only network adapter.
* Set up static IP routing on a headless Ubuntu server using Netplan.
* Generated cryptographic key pairs and deployed a WireGuard VPN tunnel.
* Implemented a strict Windows Defender Firewall rule via PowerShell to allow secure lateral ICMP traffic while blocking unauthorized local access.


## Phase 2 & 3: Secure Access, Routing & SSH Hardening (Completed)
In this phase, the internal server (VM2) was isolated and secured so that it is completely invisible to the host network and only accessible via cryptographic keys routed through the WireGuard VPN tunnel (VM1).

**Key Accomplishments:**
* **Cloud-Init Override:** Disabled Ubuntu's default cloud-init network capabilities to prevent static IP configurations from resetting on reboot.
* **Gateway IP Forwarding:** Converted VM1 into a functional network router by enabling IPv4 forwarding, allowing it to pass traffic between the VPN subnet (`10.0.0.x`) and the Host-Only subnet (`192.168.56.x`).
* **Static Kernel Routing:** Configured a custom Netplan routing rule on VM2 to ensure return traffic bound for the VPN is correctly routed back through the VM1 gateway.
* **SSH Daemon Hardening:** Secured VM2 by enforcing `ed25519` SSH key authentication, disabling password logins entirely, and implementing an `AllowUsers` firewall rule to drop any SSH traffic not originating from the VPN subnet.


## Phases 4, 5 & 6: Auditing & Security Monitoring (Completed)
In this phase, internal security monitoring was established on the secure server (VM2) using the Linux Audit Daemon (`auditd`). The system was configured to intercept kernel system calls and generate an immutable, searchable audit trail of all commands executed by any user who successfully breaches the network perimeter.

**Key Accomplishments:**
* **Audit Engine Deployment:** Installed and enabled the core `auditd` service and support plugins to continuously monitor the Linux kernel for security events.
* **Custom Kernel Rules:** Injected a custom configuration rule (`-S execve`) bypassing default wipe configurations, to strictly track all 64-bit program executions and tag them with a unique `command_tracking` identifier.
* **Forensic Log Analysis:** Utilized `ausearch` to filter and translate raw, dense kernel logs into human-readable data, successfully proving the system captures the exact User ID, working directory, and command arguments of any system activity.
* **Privilege Escalation Tracking:** Analyzed the master authentication ledger (`/var/log/auth.log`) to verify secure SSH key logins and isolate `sudo` usage, establishing a complete and un-hackable timeline of administrator access.

---
*Document last updated: Phase 4-6 Completion*