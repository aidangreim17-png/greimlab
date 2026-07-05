# Phase 7 – HTTPS & SSL Certificates

> This phase documents the implementation of HTTPS throughout the GreimLab environment. SSL/TLS encryption protects all communication between users and self-hosted services while enabling secure authentication through Authelia and improving the overall security posture of the homelab.

---

# Objective

Configure HTTPS across every public-facing service within GreimLab.

By completing this phase, all services benefit from:

- Encrypted communication
- Secure authentication
- Trusted SSL certificates
- Modern browser compatibility
- Integration with Cloudflare Tunnel
- Improved security

---

# Why HTTPS?

Without HTTPS, all communication between clients and services would be transmitted in plain text.

Potential risks include:

- Credential theft
- Session hijacking
- Traffic interception
- Browser security warnings

HTTPS protects these communications by encrypting all traffic using TLS.

Examples:

```
http://grafana.greimlab.net
```

↓

```
https://grafana.greimlab.net
```

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Domain | greimlab.net |
| DNS Provider | Cloudflare |
| Reverse Proxy | Nginx Proxy Manager |
| Tunnel | Cloudflare Tunnel |
| Certificate Management | Nginx Proxy Manager |
| SSL Provider | Cloudflare Edge Certificates |

---

# Architecture

```
                  Internet
                       │
                       ▼
                Cloudflare Edge
                       │
               TLS Encryption
                       │
               Cloudflare Tunnel
                       │
             Nginx Proxy Manager
                       │
      ┌────────────────────────────────┐
      │ Homepage                       │
      │ Grafana                        │
      │ Prometheus                     │
      │ Authelia                       │
      │ Pi-hole                        │
      │ Uptime Kuma                    │
      └────────────────────────────────┘
```

Every public request is encrypted before reaching the GreimLab infrastructure.

---

# Prerequisites

Before implementing HTTPS, the following components were operational:

- Domain registered
- DNS configured
- Cloudflare Tunnel online
- Nginx Proxy Manager deployed
- Public hostnames configured

---

# SSL Strategy

GreimLab uses a layered HTTPS approach.

Cloudflare provides:

- Public TLS encryption
- Edge certificates
- Secure Internet connectivity

Nginx Proxy Manager provides:

- SSL management
- HTTPS enforcement
- Reverse proxy routing

This design ensures every application is accessed using encrypted HTTPS connections.

---

# Nginx Proxy Manager SSL Configuration

Each Proxy Host was configured with:

- SSL Certificate
- Force SSL
- HTTP/2
- HSTS (where appropriate)

These settings automatically redirect HTTP requests to HTTPS.

---

# Cloudflare SSL Mode

Cloudflare SSL mode was configured to maintain encrypted communication while avoiding SSL negotiation issues.

Proper SSL configuration between Cloudflare and Nginx Proxy Manager is critical to prevent redirect loops.

---

# Verification

Verify HTTPS connectivity.

Open:

```
https://homepage.greimlab.net
```

Verify:

- Secure padlock displayed
- Valid certificate
- HTTPS connection established

Repeat for:

- Grafana
- Prometheus
- Uptime Kuma
- Authelia

---

# Validation

The following items were confirmed:

✅ HTTPS enabled

✅ Valid SSL certificates

✅ Browser trust established

✅ Secure padlock displayed

✅ Automatic HTTP → HTTPS redirection

---

# Troubleshooting

## Browser Reports "Not Secure"

### Symptoms

Browser displays:

```
Not Secure
```

### Cause

No valid SSL certificate or HTTPS configuration.

### Resolution

Verified:

- Proxy Host SSL settings
- Cloudflare configuration
- DNS records
- Tunnel connectivity

---

## Redirect Loop

### Symptoms

Browser displays:

```
ERR_TOO_MANY_REDIRECTS
```

### Cause

Conflicting HTTPS redirection between Cloudflare and Nginx Proxy Manager.

### Resolution

Reviewed:

- Cloudflare SSL mode
- Force SSL
- Proxy Host configuration
- Tunnel routing

Correcting the SSL configuration resolved the issue.

---

## Certificate Request Failed

### Symptoms

Unable to obtain a certificate.

### Resolution

Verified:

- DNS propagation
- Cloudflare DNS
- Public hostname configuration
- Nginx Proxy Manager SSL configuration

---

# Lessons Learned

Implementing HTTPS significantly improved the security of GreimLab.

This phase reinforced several important concepts:

- TLS encryption
- Reverse proxies
- Certificate management
- Cloudflare integration
- Secure service publishing

It also demonstrated how multiple infrastructure components must be configured consistently to avoid SSL-related issues such as redirect loops.

---

# Security Benefits

By implementing HTTPS, GreimLab now provides:

- Encrypted client connections
- Trusted browser sessions
- Secure authentication
- Protection against credential interception
- Foundation for Authelia Single Sign-On

---

# Next Phase

➡️ Phase 8 – Authelia Authentication