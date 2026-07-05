# Phase 8 – Authelia Authentication

> This phase documents the deployment and configuration of Authelia as the centralized authentication platform for GreimLab. Authelia provides Single Sign-On (SSO) and Multi-Factor Authentication (MFA) capabilities, protecting publicly accessible services behind a unified authentication portal.

---

# Objective

Deploy Authelia to secure GreimLab services by requiring authentication before granting access.

By completing this phase, GreimLab gains:

- Centralized authentication
- Single Sign-On (SSO)
- Session management
- Access control policies
- Optional Multi-Factor Authentication (MFA)

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| Reverse Proxy | Nginx Proxy Manager |
| Public Access | Cloudflare Tunnel |
| Authentication | Authelia |
| User Database | File-based |

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
          ┌──────────────┴──────────────┐
          │                             │
      Authentication Successful     Authentication Failed
          │                             │
          ▼                             ▼
   Requested Application          Login Page
```

Every protected request passes through Authelia before reaching the destination application.

---

# Prerequisites

Before beginning this phase, the following components were already operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Cloudflare Tunnel
- Nginx Proxy Manager
- HTTPS configured

---

# Deployment

Authelia was deployed as a Docker container using Docker Compose.

Persistent volumes were configured to retain:

- Configuration
- User database
- Session data
- Notification files

Once deployed, the Authelia container became responsible for authenticating requests to protected services.

---

# Configuration

Authelia was configured using a YAML configuration file.

The configuration included:

- Session settings
- Access control rules
- Storage configuration
- Authentication backend
- Identity validation
- Notification settings

User accounts were stored in a local file-based database using hashed passwords.

---

# Integration with Nginx Proxy Manager

Protected services were configured to use Authelia through Nginx Proxy Manager.

Applications protected by Authelia include:

- Homepage
- Grafana
- Prometheus
- Uptime Kuma

When a user requests one of these services:

1. NPM forwards the request.
2. Authelia checks for a valid session.
3. If authenticated, access is granted.
4. If not authenticated, the user is redirected to the login page.

---

# Password Hashes

User passwords were stored as secure password hashes.

Password hashes were generated using the Authelia CLI before being added to the user database.

Plain-text passwords are never stored.

---

# Verification

Verify the container is running.

```bash
docker ps
```

Verify container logs.

```bash
docker logs authelia
```

Open:

```
https://auth.greimlab.net
```

Expected result:

The Authelia login page loads successfully.

After successful authentication:

- Homepage loads
- Grafana loads
- Prometheus loads
- Uptime Kuma loads

---

# Validation

The following items were verified:

✅ Authelia container running

✅ Login page accessible

✅ User authentication successful

✅ Protected services accessible after login

✅ Session persistence functioning correctly

---

# Troubleshooting

## Incorrect Username or Password

### Symptoms

Authelia repeatedly displayed:

```
Incorrect username or password
```

### Cause

The password hash stored in the user database did not match the configured password.

### Resolution

A new password hash was generated using the Authelia CLI.

The updated hash replaced the previous value in the users database.

The Authelia container was restarted.

Authentication succeeded after the configuration update.

---

## Configuration Permissions Reset

### Symptoms

Configuration file permissions repeatedly changed after editing.

### Cause

Container ownership and file permissions were inconsistent with the expected Docker user.

### Resolution

Verified ownership of the configuration directory.

Corrected file permissions.

Restarted the container.

Confirmed configuration was successfully loaded.

---

## Login Redirect Loop

### Symptoms

Successful login immediately redirected back to the login page.

### Cause

Session or proxy configuration mismatch.

### Resolution

Verified:

- Session configuration
- Nginx Proxy Manager integration
- Cookie settings
- Forward authentication configuration

After correcting the configuration, authenticated sessions persisted correctly.

---

# Lessons Learned

Authelia introduced enterprise-style authentication into the GreimLab environment.

Implementing centralized authentication demonstrated how reverse proxies, session management, cookies, and identity providers work together to secure self-hosted applications.

Troubleshooting configuration issues reinforced the importance of file permissions, password hashing, and proxy integration when deploying authentication services.

---

# Next Phase

➡️ Phase 9 – Homepage Dashboard