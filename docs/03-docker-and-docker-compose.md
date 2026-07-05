# Phase 3 – Docker & Docker Compose

## Objective

Deploy Docker Engine and Docker Compose on the Ubuntu Server to provide a standardized, containerized environment for hosting GreimLab services.

Docker allows every application in the homelab to run independently while remaining easy to update, back up, and recreate. Docker Compose was chosen to define the entire infrastructure using version-controlled configuration files.

---

# Why Docker?

Docker became the foundation of GreimLab because it provides:

- Lightweight containerization
- Consistent deployments
- Simplified upgrades
- Service isolation
- Easy backups
- Infrastructure as Code (IaC)
- Large community support

Instead of manually configuring every application, services can be deployed using a single Docker Compose file.

---

# Environment

| Component | Value |
|-----------|------|
| Operating System | Ubuntu Server 26.04 LTS |
| Platform | Proxmox Virtual Machine |
| Container Runtime | Docker Engine |
| Orchestration | Docker Compose |
| Hostname | ubuntu-server |

---

# Architecture

```
                Ubuntu Server
                      │
                Docker Engine
                      │
          Docker Compose Stack
                      │
 ┌────────────────────────────────────┐
 │                                    │
 │  Homepage                          │
 │  Grafana                           │
 │  Prometheus                        │
 │  Pi-hole                           │
 │  Authelia                          │
 │  Uptime Kuma                       │
 │  Nginx Proxy Manager               │
 │  Node Exporter                     │
 │  Proxmox Exporter                  │
 │  cAdvisor                          │
 │                                    │
 └────────────────────────────────────┘
```

Docker provides the runtime environment while Docker Compose manages the lifecycle of every container.

---

# Prerequisites

Before beginning this phase, the following were completed:

- Proxmox VE installed
- Ubuntu Server deployed
- Static IP configured
- Internet connectivity verified
- SSH access configured
- System packages updated

---

# Installing Docker

Update package repositories.

```bash
sudo apt update
sudo apt upgrade -y
```

Install Docker Engine and Docker Compose.

```bash
sudo apt install docker.io docker-compose-v2 -y
```

Enable Docker to start automatically.

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

# Verification

Verify Docker installation.

```bash
docker --version
```

Expected output

```
Docker version xx.x.x
```

Verify Docker Compose.

```bash
docker compose version
```

Expected output

```
Docker Compose version x.x.x
```

Verify Docker service.

```bash
sudo systemctl status docker
```

Expected state

```
Active (running)
```

---

# Docker Test

Run a test container.

```bash
docker run hello-world
```

Expected output

```
Hello from Docker!
```

This confirms:

- Docker Engine is installed.
- Images can be downloaded.
- Containers can execute successfully.

---

# Creating the Monitoring Directory

A dedicated directory was created to store all Docker Compose files and supporting configuration.

Example:

```bash
mkdir ~/monitoring
cd ~/monitoring
```

This directory became the central location for:

- docker-compose.yml
- prometheus.yml
- Grafana provisioning
- Exporter configuration
- Supporting files

---

# Docker Compose

GreimLab uses Docker Compose to deploy all infrastructure services.

Benefits include:

- Single command deployment
- Version-controlled infrastructure
- Consistent environments
- Simplified updates
- Easy backups

Containers can be started with:

```bash
docker compose up -d
```

Stopped with:

```bash
docker compose down
```

View running containers.

```bash
docker ps
```

View all containers.

```bash
docker ps -a
```

View container logs.

```bash
docker logs <container-name>
```

Restart a container.

```bash
docker restart <container-name>
```

---

# Validation

The following checks were completed:

✅ Docker service running

✅ Docker Compose installed

✅ Hello World container executed

✅ Monitoring directory created

✅ Docker accessible over SSH

---

# Troubleshooting

## Docker Service Failed to Start

### Cause

Docker service was not enabled.

### Resolution

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

---

## Permission Denied

### Cause

Current user not in Docker group.

### Resolution

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## Container Failed to Start

Useful commands:

```bash
docker ps -a

docker logs <container>

docker inspect <container>
```

These commands became the primary troubleshooting tools throughout the remainder of the project.

---

# Lessons Learned

Docker dramatically simplified infrastructure management.

Instead of manually configuring every application, each service could be defined within a Docker Compose file and recreated within minutes.

Containerization also isolated services from one another, reducing dependency conflicts while making updates and backups significantly easier.

Understanding Docker commands such as `docker ps`, `docker logs`, and `docker compose up` became essential skills for managing the GreimLab environment.

---

# Next Phase

➡️ Phase 4 – Cloudflare Tunnel