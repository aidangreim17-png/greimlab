# Phase 10 – Operations

# Uptime Kuma

> This phase documents the deployment and configuration of Uptime Kuma as the availability monitoring platform for GreimLab. Uptime Kuma continuously monitors critical services, verifies their availability, and provides immediate notification when an outage occurs.

---

# Objective

Deploy Uptime Kuma to continuously monitor the availability of GreimLab services.

After completing this phase, the environment gains:

- Continuous service monitoring
- HTTP/HTTPS health checks
- Status dashboard
- Historical uptime statistics
- Response time monitoring
- Early outage detection

Unlike Prometheus, which focuses on system metrics, Uptime Kuma answers a simple question:

> **"Is the service online?"**

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| Authentication | Authelia |
| Reverse Proxy | Nginx Proxy Manager |
| Public Access | Cloudflare Tunnel |

---

# Architecture

```
                    Uptime Kuma
                         │
         ┌───────────────┼───────────────┐
         │               │               │
         ▼               ▼               ▼
     Homepage         Grafana      Prometheus
         │               │               │
         ▼               ▼               ▼
      HTTPS           HTTPS          HTTPS

             Additional Monitors

               Pi-hole
               Authelia
               NPM
               Proxmox
```

Each monitor performs periodic health checks and records response times and availability.

---

# Monitoring Philosophy

GreimLab uses multiple monitoring platforms because no single tool provides complete visibility.

Uptime Kuma answers:

> "Is the service available?"

Prometheus answers:

> "How is the service performing?"

Grafana answers:

> "How can those metrics be visualized and analyzed?"

Together, these platforms provide a comprehensive monitoring solution capable of detecting outages, identifying performance issues, and visualizing infrastructure health.

---

# Prerequisites

Before beginning this phase, the following components were operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Cloudflare Tunnel
- HTTPS
- Nginx Proxy Manager
- Homepage

---

# Deployment

Uptime Kuma was deployed using Docker Compose.

Persistent storage was configured to preserve:

- Monitor configuration
- Historical uptime
- Response time history
- Notification settings
- User configuration

The service was exposed through Nginx Proxy Manager and secured with Authelia.

---

# Monitoring Strategy

Each critical application within GreimLab was configured as an individual monitor.

Examples include:

| Service | Monitor Type |
|----------|--------------|
| Homepage | HTTPS |
| Grafana | HTTPS |
| Prometheus | HTTPS |
| Pi-hole | HTTPS |
| Authelia | HTTPS |
| Nginx Proxy Manager | HTTPS |
| Proxmox | HTTPS |

Monitoring each service individually allows outages to be isolated quickly.

---

# Health Checks

Every monitor performs regular checks to verify:

- Service availability
- HTTP status
- Response time
- SSL validity
- Historical uptime

If a service becomes unavailable, Uptime Kuma immediately records the outage.

---

# Dashboard

The Uptime Kuma dashboard provides:

- Overall monitor status
- Current service availability
- Average response time
- Historical uptime percentage
- Incident timeline

This dashboard serves as the primary availability monitoring interface within GreimLab.

---

# Integration

Uptime Kuma integrates with several GreimLab components.

Examples include:

- Homepage (service widget)
- Discord notifications
- HTTPS monitoring
- Cloudflare Tunnel
- Nginx Proxy Manager

Together these integrations provide immediate visibility into service health.

---

# Verification

Verify the container is running.

```bash
docker ps
```

Verify logs.

```bash
docker logs uptime-kuma
```

Open:

```
https://uptime.greimlab.net
```

Expected result:

The Uptime Kuma dashboard loads successfully.

Verify:

- All monitors appear
- Services report **UP**
- Response times are displayed
- Historical data is being recorded

---

# Validation

The following items were confirmed:

✅ Uptime Kuma container running

✅ Dashboard accessible

✅ HTTPS functioning

✅ Monitors successfully checking services

✅ Historical uptime recording

✅ Response time tracking operational

---

# Troubleshooting

## Monitor Reports DOWN

### Symptoms

A monitor displays:

```
DOWN
```

while the service appears to be running.

### Cause

Possible causes include:

- Incorrect URL
- Wrong port
- Reverse proxy issue
- Cloudflare Tunnel issue
- Service unavailable

### Resolution

Verified:

- Docker container status
- Nginx Proxy Manager
- Cloudflare Tunnel
- Service URL
- HTTPS accessibility

---

## SSL Certificate Warning

### Symptoms

HTTPS monitor fails certificate validation.

### Resolution

Verified:

- SSL certificate validity
- Proxy Host SSL configuration
- Cloudflare DNS
- HTTPS configuration

---

## High Response Time

### Symptoms

Service remains online but response time increases significantly.

### Resolution

Investigated:

- CPU utilization
- Memory utilization
- Docker container health
- Network connectivity

This helped identify potential performance issues before they developed into outages.

---

# Lessons Learned

Uptime Kuma provides a different perspective than Prometheus.

While Prometheus measures resource utilization and performance metrics, Uptime Kuma focuses exclusively on service availability.

Using both platforms together creates a more complete monitoring solution by combining operational metrics with real-time uptime monitoring.

---

# Screenshots

Include:

- Dashboard overview
- Monitor list
- Historical uptime graph
- Response time graph
- Incident history
- Homepage widget integration

---

# Next Phase

➡️ Phase 11 – Pi-hole