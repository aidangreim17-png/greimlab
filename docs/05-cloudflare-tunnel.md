# Phase 5 – Cloudflare Tunnel

> This phase documents the deployment and configuration of Cloudflare Tunnel within GreimLab. Cloudflare Tunnel provides secure remote access to self-hosted services without exposing inbound ports on the firewall or requiring traditional port forwarding.

---

# Objective

Deploy Cloudflare Tunnel to securely publish GreimLab services to the Internet while minimizing the attack surface.

By completing this phase, the environment gains:

- Secure remote access
- Encrypted HTTPS connections
- No inbound port forwarding
- Cloudflare edge protection
- Integration with custom domain names

---

# Why Cloudflare Tunnel?

Traditional home-hosted services often require opening ports such as:

```
80
443
```

This creates unnecessary exposure to the Internet.

Cloudflare Tunnel eliminates this requirement by creating an outbound encrypted tunnel from the Ubuntu server to Cloudflare's network.

Benefits include:

- No firewall port forwarding
- Reduced attack surface
- Automatic TLS encryption
- Easy integration with Cloudflare DNS
- Secure access from anywhere

Cloudflare acts as a secure gateway between users and the GreimLab infrastructure.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Tunnel Software | cloudflared |
| DNS Provider | Cloudflare |
| Domain | greimlab.net |
| Reverse Proxy | Nginx Proxy Manager |

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
                  Ubuntu Server
                        │
              Nginx Proxy Manager
                        │
     ┌────────────────────────────────┐
     │ Homepage                       │
     │ Grafana                        │
     │ Prometheus                     │
     │ Pi-hole                        │
     │ Authelia                       │
     │ Uptime Kuma                    │
     └────────────────────────────────┘
```

All inbound requests terminate at Cloudflare before being securely forwarded through the encrypted tunnel.

---

# Prerequisites

Before beginning this phase, the following components were already operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Custom domain (greimlab.net)
- Cloudflare account
- DNS delegated to Cloudflare

---

# Deployment

A Cloudflare Tunnel was created through the Cloudflare Zero Trust dashboard.

The tunnel connector was deployed as a Docker container using Docker Compose.

Once authenticated, the connector established a persistent encrypted outbound connection to Cloudflare.

---

# DNS Configuration

Public hostnames were created for each service.

Examples include:

| Service | Hostname |
|----------|----------|
| Homepage | homepage.greimlab.net |
| Grafana | grafana.greimlab.net |
| Prometheus | prometheus.greimlab.net |
| Authelia | auth.greimlab.net |
| Uptime Kuma | uptime.greimlab.net |

Cloudflare automatically routes these hostnames through the tunnel.

---

# Verification

Verify the connector is running.

```bash
docker ps
```

Verify the tunnel logs.

```bash
docker logs cloudflared
```

Expected behavior:

- Tunnel connected
- Connector registered
- No authentication errors

Verify public connectivity by accessing:

```
https://homepage.greimlab.net
```

The Homepage dashboard should load successfully over HTTPS.

---

# Troubleshooting

## Tunnel Address Missing

### Symptoms

The Cloudflare dashboard did not display a Tunnel Address.

### Cause

The tunnel connector container had not been deployed successfully.

### Resolution

The cloudflared Docker container was installed and connected to the existing tunnel.

After the connector authenticated successfully, the Tunnel Address appeared automatically.

---

## Connection Timed Out

### Symptoms

Public hostnames failed to load.

### Cause

The connector was offline or the public hostname configuration was incomplete.

### Resolution

Verified:

- Tunnel status
- Docker container health
- Public hostname configuration
- DNS propagation

---

## Router Configuration

During troubleshooting it was confirmed that traditional port forwarding on ports 80 and 443 was **not required**.

This validated that Cloudflare Tunnel was functioning as intended by creating outbound-only encrypted connections.

---

# Validation

The following items were successfully verified:

✅ Tunnel connector online

✅ Public hostnames resolving

✅ HTTPS functioning

✅ No firewall port forwarding required

✅ Secure remote access operational

---

# Lessons Learned

Cloudflare Tunnel significantly simplified secure remote access.

Instead of exposing services directly to the Internet, the environment maintains outbound-only encrypted connections to Cloudflare.

This approach reduces administrative overhead while improving security and eliminating the need for inbound firewall rules.

Understanding how DNS, Cloudflare Zero Trust, Docker containers, and reverse proxies interact was a major milestone in the development of GreimLab.

---

# Next Phase

➡️ Phase 6 – Nginx Proxy Manager