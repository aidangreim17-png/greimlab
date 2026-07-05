# 01 – Proxmox VE Installation

## Overview

This document describes the installation and initial configuration of the Proxmox Virtual Environment (VE) server used as the virtualization platform for GreimLab.

Proxmox VE provides the foundation for all virtual machines and containers used throughout the lab, including the Ubuntu monitoring server and Windows virtual machines.

---

## Objectives

- Install Proxmox VE
- Configure networking
- Configure local storage
- Verify management interface
- Prepare the host for virtual machine deployment

---

## Environment

| Component | Value |
|-----------|------|
| Hypervisor | Proxmox VE |
| Management Interface | HTTPS |
| Primary Storage | Local SSD |
| Virtualization | KVM |
| Container Support | LXC |

---

## Installation

1. Download the latest Proxmox VE ISO.
2. Create a bootable USB drive.
3. Boot the server.
4. Install Proxmox VE.
5. Configure:
   - Hostname
   - Static IP address
   - Gateway
   - DNS
6. Reboot.
7. Access the web interface.

---

## Initial Configuration

After installation:

- Updated package repositories
- Configured storage
- Created virtual machine templates
- Enabled SSH
- Verified networking

---

## Validation

Verified:

- Web UI accessible
- SSH access functional
- Storage available
- Network connectivity
- VM creation successful

---

## Lessons Learned

- Static IP addresses simplify infrastructure management.
- Proxmox's web interface makes VM administration significantly easier.
- Resource planning early in the project reduces later reconfiguration.

---

## Next Phase

Proceed to:

02 – Ubuntu Server 26.04 Installation