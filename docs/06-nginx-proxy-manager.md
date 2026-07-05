# Phase 6 – Nginx Proxy Manager

> This phase documents the deployment and configuration of Nginx Proxy Manager (NPM). NPM provides centralized reverse proxy functionality for GreimLab, allowing multiple self-hosted services to be securely accessed using individual subdomains while simplifying SSL certificate management.

---

# Objective

Deploy Nginx Proxy Manager to provide:

- Reverse proxy services
- HTTPS termination
- Automatic SSL certificate management
- Centralized application routing
- Integration with Cloudflare Tunnel

After completing this phase, every public application can be accessed using a dedicated subdomain instead of an IP address and port number.

---

# Why Nginx Proxy Manager?

Without a reverse proxy, services would be accessed using URLs such as:

```
http://192.168.1.xxx:3000
```

```
http://192.168.1.xxx:9090
```

```
http://192.168.1.xxx:3001
```

This quickly becomes difficult to manage.

Using Nginx Proxy Manager allows services to be accessed through friendly URLs such as:

```
https://homepage.greimlab.net
```

```
https://grafana.greimlab.net
```

```
https://prometheus.greimlab.net
```

Benefits include:

- Centralized reverse proxy
- HTTPS support
- Automatic certificate management
- Easy service administration
- Docker integration
- Cloudflare compatibility

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Reverse Proxy | Nginx Proxy Manager |
| Deployment | Docker Compose |
| Public DNS | Cloudflare |
| Tunnel | Cloudflare Tunnel |

---

# Architecture

```
                   Internet
                        │
                        ▼
                 Cloudflare Edge
                        │
                Cloudflare Tunnel
                        │
                        ▼
            Nginx Proxy Manager
                        │
 ┌──────────────────────────────────────────┐
 │ Homepage                                 │
 │ Grafana                                  │
 │ Prometheus                               │
 │ Authelia                                 │
 │ Pi-hole                                  │
 │ Uptime Kuma                              │
 └──────────────────────────────────────────┘
```

NPM receives incoming requests and forwards them to the correct Docker container based on the requested hostname.

---

# Prerequisites

Before beginning this phase, the following components were already configured:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Cloudflare Tunnel
- Public domain
- Cloudflare DNS

---

# Deployment

Nginx Proxy Manager was deployed using Docker Compose.

Persistent Docker volumes were configured to retain:

- Proxy hosts
- SSL certificates
- Configuration
- Database

The container was started using:

```bash
docker compose up -d
```

---

# Initial Configuration

After deployment:

- Logged into the NPM web interface
- Changed the default administrator password
- Verified container health
- Confirmed dashboard accessibility

---

# Proxy Hosts

A proxy host was created for each public service.

Examples include:

| Service | Destination |
|----------|-------------|
| Homepage | homepage:3000 |
| Grafana | grafana:3000 |
| Prometheus | prometheus:9090 |
| Uptime Kuma | uptime-kuma:3001 |
| Authelia | authelia:9091 |

Each proxy host included:

- Domain name
- Forward hostname
- Forward port
- WebSocket support (when required)

---

# SSL Configuration

HTTPS was enabled for each proxy host.

Certificates were requested through Cloudflare.

SSL options enabled:

- Force SSL
- HTTP/2
- HSTS (when appropriate)

All services became accessible using encrypted HTTPS connections.

---

# Verification

Verify the NPM container is running.

```bash
docker ps
```

Verify logs.

```bash
docker logs npm
```

Open:

```
https://homepage.greimlab.net
```

Expected result:

Homepage loads successfully over HTTPS.

Repeat for:

- Grafana
- Prometheus
- Uptime Kuma
- Authelia

---

# Troubleshooting

## ERR_TOO_MANY_REDIRECTS

### Symptoms

Browser displayed:

```
ERR_TOO_MANY_REDIRECTS
```

Homepage continuously redirected without loading.

### Cause

An SSL configuration mismatch existed between Cloudflare and Nginx Proxy Manager.

The browser became trapped in an HTTPS redirect loop.

### Resolution

Verified:

- Cloudflare SSL mode
- Proxy host SSL settings
- HTTPS redirection
- Cloudflare Tunnel routing

After correcting the SSL configuration, the redirect loop was eliminated.

---

## Service Unreachable

### Symptoms

Proxy host returned:

```
502 Bad Gateway
```

or

```
Connection Timed Out
```

### Cause

Incorrect Docker hostname or port.

### Resolution

Verified:

- Docker container running
- Correct internal hostname
- Correct internal port
- Docker network connectivity

---

## SSL Certificate Failed

### Symptoms

Certificate request unsuccessful.

### Resolution

Verified:

- DNS records
- Cloudflare Tunnel
- Public hostname
- Proxy host configuration

---

# Validation

The following items were verified:

✅ NPM container operational

✅ Proxy hosts functioning

✅ HTTPS enabled

✅ Cloudflare Tunnel routing correctly

✅ Individual service subdomains operational

---

# Lessons Learned

Nginx Proxy Manager greatly simplified service management.

Instead of remembering multiple IP addresses and ports, every application became accessible using a consistent naming convention.

Understanding reverse proxies, SSL certificates, Cloudflare Tunnel, and Docker networking proved essential for building a secure self-hosted environment.

Troubleshooting the redirect loop also reinforced the importance of ensuring SSL settings remain consistent across Cloudflare, Nginx Proxy Manager, and the hosted applications.

---

# Next Phase

➡️ Phase 7 – HTTPS & SSL Certificates