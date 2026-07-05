# Phase 9 – Homepage Dashboard

> This phase documents the deployment and configuration of Homepage as the centralized service portal for GreimLab. Homepage provides a single interface for accessing infrastructure services while displaying live status, resource utilization, and operational information.

---

# Objective

Deploy Homepage to serve as the primary landing page for GreimLab.

After completing this phase, administrators can:

- Access all infrastructure services from a single location
- View service availability
- Monitor system information
- Display widgets with live operational data
- Improve navigation throughout the environment

Homepage became the central dashboard for day-to-day administration.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| Authentication | Authelia |
| Reverse Proxy | Nginx Proxy Manager |
| Public Access | Cloudflare Tunnel |
| Homepage Version | Latest Stable |

---

# Architecture

```
                    Internet
                         │
                 Cloudflare Tunnel
                         │
              Nginx Proxy Manager
                         │
                    Authelia
                         │
                     Homepage
                         │
 ┌────────────────────────────────────────────┐
 │ Grafana                                   │
 │ Prometheus                                │
 │ Pi-hole                                   │
 │ Uptime Kuma                               │
 │ Nginx Proxy Manager                       │
 │ Portainer                                 │
 │ Proxmox                                   │
 └────────────────────────────────────────────┘
```

Homepage serves as the primary entry point for every major service within GreimLab.

---

# Prerequisites

Before beginning this phase, the following components were operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Cloudflare Tunnel
- Nginx Proxy Manager
- HTTPS
- Authelia

---

# Deployment

Homepage was deployed using Docker Compose.

Persistent configuration files were created for:

- Services
- Bookmarks
- Widgets
- Settings
- Docker integration

The Homepage container automatically loads these configuration files during startup.

---

# Dashboard Organization

Homepage was intentionally organized into functional groups that reflect how the GreimLab infrastructure is managed. Instead of listing services alphabetically, applications are grouped according to their role within the environment.

This organization reduces navigation time and provides a logical workflow when administering the homelab.

---

## Infrastructure

The Infrastructure section contains services responsible for hosting and managing the environment.

Examples include:

- Proxmox VE
- Portainer
- Nginx Proxy Manager

These services are typically used when deploying new applications, managing containers, or maintaining the underlying platform.

---

## Monitoring

The Monitoring section provides visibility into the health and performance of the environment.

Services include:

- Grafana
- Prometheus
- Uptime Kuma

These applications are used daily to monitor resource utilization, verify service availability, and respond to alerts.

---

## Network

Network services are grouped together because they manage connectivity and traffic flowing throughout the environment.

Examples include:

- Pi-hole
- Cloudflare Dashboard

Keeping these services together simplifies DNS management and troubleshooting.

---

## Security

Security-related applications are isolated into their own section.

Examples include:

- Authelia

Since authentication protects multiple applications throughout GreimLab, placing security services in their own category makes administrative tasks more intuitive.

---

## Design Philosophy

Homepage was designed to answer three questions immediately after login:

1. Are all critical services online?
2. Is the infrastructure healthy?
3. Which administrative application needs attention?

By grouping services according to their operational purpose, Homepage serves as both a navigation portal and a high-level operational dashboard.

---

# Widgets

Homepage was configured with several live widgets.

Examples include:

- System information
- CPU usage
- Memory utilization
- Disk usage
- Docker statistics
- Weather (optional)
- Date & Time

These widgets provide a quick overview of the current health of the environment.

---

# Service Cards

Each service card includes:

- Name
- Description
- Icon
- URL
- Health status
- Optional widget integration

Examples:

| Service | Purpose |
|----------|---------|
| Grafana | Monitoring dashboards |
| Prometheus | Metrics collection |
| Pi-hole | DNS filtering |
| Uptime Kuma | Availability monitoring |
| Nginx Proxy Manager | Reverse proxy |
| Portainer | Docker management |
| Proxmox | Virtualization platform |

---

# Integration

Homepage integrates with multiple GreimLab services.

Examples include:

- Docker API
- Grafana
- Pi-hole
- Uptime Kuma

This allows Homepage to display real-time operational data instead of functioning solely as a bookmark page.

---

# Verification

Verify Homepage container.

```bash
docker ps
```

Verify logs.

```bash
docker logs homepage
```

Open:

```
https://homepage.greimlab.net
```

Expected result:

Homepage loads successfully through Authelia.

Verify:

- Service cards visible
- Icons displayed correctly
- Widgets functioning
- Links operational

---

# Validation

The following items were confirmed:

✅ Homepage container running

✅ Protected by Authelia

✅ HTTPS functioning

✅ Widgets updating correctly

✅ Service cards operational

✅ Navigation links verified

---

# Troubleshooting

## Homepage Not Loading

### Symptoms

Homepage unavailable.

### Resolution

Verified:

- Docker container
- Nginx Proxy Manager
- Cloudflare Tunnel
- Docker logs

---

## Widget Displays "Unavailable"

### Cause

API endpoint or service unavailable.

### Resolution

Verified widget configuration.

Confirmed API credentials.

Restarted affected container if necessary.

---

## Incorrect Service URL

### Symptoms

Homepage links opened the wrong application or returned an error.

### Resolution

Reviewed:

- services.yaml
- Docker hostnames
- Ports
- Reverse proxy configuration

Updated the affected service entry and refreshed Homepage.

---

# Lessons Learned

Homepage significantly improved day-to-day administration by consolidating every major infrastructure component into a single interface.

Instead of memorizing multiple URLs and ports, administrators can quickly navigate to services through a unified dashboard.

The use of widgets transformed Homepage from a simple launcher into a real operational dashboard by providing live infrastructure information directly on the landing page.

---

# Screenshots

Include:

- Homepage dashboard
- Service groups
- Widget panel
- Authelia redirect
- Mobile view (optional)

---

# Next Phase

➡️ Phase 10 – Uptime Kuma