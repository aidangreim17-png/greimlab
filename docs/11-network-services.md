# Phase 11 – Network Services

# Pi-hole

> This phase documents the deployment, configuration, and monitoring of Pi-hole within GreimLab. Pi-hole functions as the primary DNS server for the homelab, providing network-wide advertisement blocking, DNS filtering, query logging, and performance monitoring.

---

# Objective

Deploy Pi-hole as the centralized DNS server for GreimLab.

After completing this phase, the environment provides:

- Network-wide DNS filtering
- Advertisement blocking
- DNS query logging
- Local DNS resolution
- DNS performance metrics
- Integration with Prometheus and Grafana

Pi-hole became one of the most frequently used administrative services within the environment.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| DNS Platform | Pi-hole |
| Reverse Proxy | Nginx Proxy Manager |
| Authentication | Authelia |
| Public Access | Cloudflare Tunnel |

---

# Architecture

```
                    Client Devices
                           │
                           ▼
                      Pi-hole DNS
                           │
          ┌────────────────┴────────────────┐
          │                                 │
     Local DNS Cache                  Upstream DNS
          │                                 │
          └────────────────┬────────────────┘
                           ▼
                     Internet Resources
```

Pi-hole acts as the first DNS server contacted by client devices, filtering unwanted domains before forwarding approved requests to upstream DNS servers.

---

# DNS Philosophy

GreimLab uses Pi-hole as its primary DNS resolver because DNS is a foundational network service.

Rather than blocking advertisements within individual browsers, Pi-hole filters DNS requests before unwanted content is downloaded.

Benefits include:

- Reduced advertisements
- Improved browsing performance
- Reduced unnecessary network traffic
- Centralized DNS management
- Visibility into DNS activity

Because filtering occurs at the DNS level, every compatible client benefits without requiring browser extensions or additional software.

---

# Prerequisites

Before beginning this phase, the following components were operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Cloudflare Tunnel
- Nginx Proxy Manager
- HTTPS
- Homepage
- Uptime Kuma

---

# Deployment

Pi-hole was deployed using Docker Compose.

Persistent Docker volumes were configured to retain:

- DNS configuration
- Query history
- Blocklists
- Local DNS records
- Application settings

The service was then integrated into GreimLab through Nginx Proxy Manager and protected with Authelia for administrative access.

---

# Initial Configuration

After deployment, the following configuration tasks were completed:

- Configured upstream DNS servers
- Verified DNS resolution
- Configured administrative password
- Updated gravity database
- Verified web interface accessibility

---

# Dashboard

The Pi-hole dashboard provides real-time visibility into DNS activity.

Key metrics include:

- Total DNS Queries
- Queries Blocked
- Percent Blocked
- Domains on Blocklist
- Cached Queries
- Top Clients
- Top Permitted Domains
- Top Blocked Domains

These metrics provide immediate insight into network activity and filtering effectiveness.

---

# Monitoring Integration

Pi-hole integrates with the GreimLab monitoring stack through the Pi-hole Exporter.

Metrics collected include:

- DNS queries
- Block percentage
- Cached queries
- Client statistics
- Blocklist size
- Query types

Prometheus collects these metrics and Grafana visualizes them through the dedicated Pi-hole dashboard.

---

# Verification

Verify the Pi-hole container.

```bash
docker ps
```

Verify logs.

```bash
docker logs pihole
```

Verify DNS resolution.

```bash
nslookup google.com
```

Verify the Pi-hole web interface.

```
https://pihole.greimlab.net
```

Expected result:

- Dashboard loads successfully
- DNS queries increase in real time
- Advertisements are filtered
- Statistics update continuously

---

# Validation

The following items were confirmed:

✅ Pi-hole container running

✅ DNS resolution functioning

✅ Dashboard accessible

✅ Query statistics updating

✅ Blocklists functioning

✅ HTTPS enabled

✅ Pi-hole Exporter operational

---

# Troubleshooting

## DNS Resolution Failed

### Symptoms

Clients were unable to resolve hostnames.

### Resolution

Verified:

- Pi-hole container
- Docker networking
- Upstream DNS configuration
- Client DNS settings

---

## Dashboard Not Updating

### Symptoms

Query statistics remained static.

### Resolution

Verified:

- DNS traffic reaching Pi-hole
- Exporter connectivity
- Prometheus target status

---

## Advertisements Not Blocked

### Symptoms

Advertisements continued to appear.

### Resolution

Verified:

- Client DNS server
- Gravity database
- Blocklists
- DNS cache

Flushed the client DNS cache and repeated testing.

---

# Lessons Learned

Deploying Pi-hole demonstrated the importance of DNS within modern infrastructure.

Rather than simply blocking advertisements, Pi-hole provides valuable visibility into network activity while serving as a centralized DNS platform.

Integrating Pi-hole with Prometheus and Grafana transformed DNS from a basic network service into a fully monitored component of the GreimLab infrastructure.

---

# Screenshots

Include:

- Pi-hole Dashboard
- Query Statistics
- Top Clients
- Top Blocked Domains
- Grafana Pi-hole Dashboard
- Homepage Pi-hole Card

---

# Next Phase

➡️ Phase 12 – Prometheus