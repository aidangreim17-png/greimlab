# 02 – Ubuntu Server 26.04 LTS

> Ubuntu Server serves as the primary application host for GreimLab. It runs the Docker engine and hosts all self-managed infrastructure services including Grafana, Prometheus, Homepage, Authelia, Pi-hole, Uptime Kuma, Nginx Proxy Manager, and supporting exporters.

---

# Objectives

The Ubuntu server was deployed to provide a stable, lightweight Linux environment capable of hosting multiple Docker containers while serving as the central management server for the homelab.

Primary goals included:

- Deploy a secure Linux server
- Configure static networking
- Enable remote administration via SSH
- Prepare the system for Docker
- Provide a reliable platform for monitoring and authentication services

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Operating System | Ubuntu Server 26.04 LTS |
| Platform | Proxmox Virtual Machine |
| Hostname | ubuntu-server |
| Purpose | Docker Host |
| Network | Static IPv4 |
| Administration | SSH |

---

# Virtual Machine Configuration

| Resource | Allocation |
|----------|-----------|
| CPU | 4 vCPU |
| Memory | 8 GB RAM |
| Storage | 100 GB SSD |
| Network | VirtIO |

These resources were selected to support multiple Docker containers while maintaining sufficient overhead for future expansion.

---

# Initial Configuration

After installation, the following tasks were completed:

- Updated all system packages
- Configured a static IP address
- Verified DNS resolution
- Enabled OpenSSH Server
- Configured automatic time synchronization
- Verified internet connectivity

Example commands:

```bash
sudo apt update
sudo apt upgrade -y
hostnamectl
timedatectl
ip addr
ip route
```

---

# Why Ubuntu?

Ubuntu Server was selected because:

- Excellent Docker support
- Large community
- Long-Term Support (LTS)
- Extensive documentation
- Enterprise adoption

The operating system provides a stable foundation while minimizing unnecessary overhead.

---

# Role Within GreimLab

Ubuntu functions as the central application server.

```
Internet
        │
Cloudflare Tunnel
        │
Nginx Proxy Manager
        │
Authelia
        │
Docker Containers
```

All monitoring, authentication, dashboards, and infrastructure services are hosted from this server.

---

# Validation

The following checks were performed after installation:

✅ SSH accessible

✅ Internet connectivity

✅ DNS resolution

✅ Package updates successful

✅ Static IP verified

✅ System time synchronized

---

# Lessons Learned

Deploying a dedicated Linux server dramatically simplified infrastructure management compared to running individual virtual machines for every service.

Using a static IP address from the beginning avoided future configuration changes within Docker Compose, Prometheus, Grafana, and Cloudflare Tunnel.

Planning networking before deploying services reduced troubleshooting later in the project.

---

# Next Phase

➡️ 03 – Docker & Docker Compose