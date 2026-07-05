# Phase 4 – Domain Name & DNS Configuration

> This phase documents the acquisition, configuration, and integration of the GreimLab domain. A custom domain provides a professional method of accessing self-hosted services while enabling HTTPS, Cloudflare Tunnel, and centralized authentication.

---

# Objective

Configure a custom domain for GreimLab to provide:

- Secure HTTPS access
- Friendly URLs
- Cloudflare integration
- Future wildcard certificate support
- Centralized DNS management

This phase establishes the public identity of the homelab before exposing services through Cloudflare Tunnel.

---

# Why Use a Custom Domain?

Instead of accessing services using IP addresses such as:

```
http://192.168.1.xxx:3000
```

GreimLab uses fully qualified domain names such as:

```
https://homepage.greimlab.net
```

Benefits include:

- Easier to remember
- Professional appearance
- HTTPS support
- Reverse proxy compatibility
- Single Sign-On integration
- Simplified service management

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Domain Registrar | Porkbun |
| Domain | greimlab.net |
| DNS Provider | Cloudflare |
| Public Access | Cloudflare Tunnel |
| SSL | Cloudflare Edge Certificates |

---

# Architecture

```
                    Internet
                         │
                         ▼
                   Porkbun Domain
                         │
                Authoritative Nameservers
                         │
                         ▼
                    Cloudflare DNS
                         │
                    Cloudflare Tunnel
                         │
                    Ubuntu Server
                         │
                 Docker Containers
```

The domain acts as the public entry point for every service hosted within GreimLab.

---

# Domain Registration

A custom domain was purchased through Porkbun.

Responsibilities of the registrar include:

- Domain ownership
- Registration renewal
- Nameserver delegation

After purchasing the domain, authoritative nameservers were updated to point to Cloudflare.

---

# Cloudflare Integration

The domain was added to Cloudflare.

Cloudflare became responsible for:

- DNS management
- SSL certificates
- Cloudflare Tunnel
- Edge security
- Public routing

---

# DNS Strategy

Each application is assigned its own subdomain.

Examples include:

| Service | Subdomain |
|----------|-----------|
| Homepage | homepage.greimlab.net |
| Grafana | grafana.greimlab.net |
| Prometheus | prometheus.greimlab.net |
| Uptime Kuma | uptime.greimlab.net |
| Authelia | auth.greimlab.net |

Using individual subdomains simplifies:

- Reverse proxy configuration
- Authentication policies
- Monitoring
- Troubleshooting

---

# DNS Verification

The following checks were performed.

Verify DNS resolution.

```bash
nslookup homepage.greimlab.net
```

or

```bash
dig homepage.greimlab.net
```

Expected result:

```
Cloudflare-managed DNS records returned successfully.
```

---

# Validation

The following items were verified:

✅ Domain registered successfully

✅ Nameservers delegated

✅ Cloudflare active

✅ DNS records created

✅ Public DNS resolution functioning

---

# Lessons Learned

Separating domain registration from DNS management provides flexibility and simplifies future migrations.

Using Cloudflare as the authoritative DNS provider allows additional security features such as Cloudflare Tunnel, HTTPS, and DNS management without changing registrars.

Planning the subdomain structure before deploying services significantly reduced future configuration changes.

---

# Next Phase

➡️ Phase 5 – Cloudflare Tunnel