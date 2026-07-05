# Phase 13 – Monitoring

# Grafana

> This phase documents the deployment, configuration, and customization of Grafana within GreimLab. Grafana serves as the primary visualization platform, transforming Prometheus metrics into interactive dashboards, operational insights, and real-time alerts.

---

# Objective

Deploy Grafana as the visualization platform for GreimLab.

After completing this phase, the environment gains:

- Interactive dashboards
- Real-time infrastructure visualization
- Alert management
- Dashboard organization
- Historical metric analysis
- Centralized monitoring interface

Grafana became the primary operational dashboard used to monitor the entire homelab.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| Data Source | Prometheus |
| Authentication | Authelia |
| Public Access | Cloudflare Tunnel |
| Dashboard Platform | Grafana |

---

# Architecture

```
                  Exporters
                       │
                  Prometheus
                       │
                       ▼
                  Grafana
                       │
     ┌─────────────────┼──────────────────┐
     │                 │                  │
     ▼                 ▼                  ▼
 Mission Control   Pi-hole Dashboard   Proxmox Dashboard
     │                 │                  │
     ▼                 ▼                  ▼
Prometheus Dashboard  Target Status   Discord Alerts
```

Grafana retrieves metrics from Prometheus and presents them through customized dashboards designed for different operational purposes.

---

# Dashboard Philosophy

Rather than creating one extremely large dashboard, GreimLab organizes monitoring into multiple focused dashboards.

Each dashboard answers a specific operational question.

| Dashboard | Purpose |
|-----------|----------|
| Mission Control | Overall infrastructure health |
| Pi-hole | DNS performance and statistics |
| Proxmox | Hypervisor resource utilization |
| Prometheus | Monitoring platform health |
| Target Status | Exporter and scrape target health |

This approach simplifies troubleshooting and reduces unnecessary information.

---

# Prerequisites

Before beginning this phase, the following components were operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Prometheus
- Exporters
- Homepage
- Uptime Kuma

---

# Deployment

Grafana was deployed using Docker Compose.

Persistent Docker volumes were configured to retain:

- Dashboards
- Data sources
- Users
- Alert rules
- Preferences

Grafana was configured to automatically connect to Prometheus as its primary data source.

---

# Data Source Configuration

Prometheus was added as the default Grafana data source.

Configuration included:

- Prometheus URL
- Default data source
- Connectivity verification

Successful integration allowed Grafana panels to query Prometheus metrics using PromQL.

---

# Dashboard Development

Rather than importing community dashboards, GreimLab dashboards were designed specifically for the environment.

The dashboards evolved throughout the project as monitoring requirements changed.

The final dashboard collection includes:

## Mission Control

Provides an overall health summary.

Panels include:

- Service Status
- CPU Usage
- Memory Usage
- Disk Usage
- Network Activity

This dashboard serves as the primary operational view.

---

## Pi-hole Dashboard

Displays DNS activity.

Panels include:

- Total Queries
- Queries Blocked
- Block Percentage
- Top Clients
- Top Domains
- DNS Activity

---

## Proxmox Dashboard

Monitors the virtualization host.

Panels include:

- CPU Usage
- Memory Utilization
- Storage Usage
- Network Activity

---

## Prometheus Dashboard

Monitors the monitoring platform itself.

Panels include:

- Active Alerts
- Targets Up
- Failed Targets
- Prometheus Health
- Scrape Duration
- Target Status Table

---

## Target Status Dashboard

Provides detailed visibility into Prometheus scrape targets.

Displays:

- Job Name
- Instance
- Health Status
- Last Scrape
- Scrape Duration

This dashboard assists with exporter troubleshooting.

---

# Dashboard Design Standards

All dashboards follow consistent design principles.

Panels use:

- Standardized colors
- Consistent thresholds
- Clear panel titles
- Appropriate visualizations
- Logical grouping

Examples:

Gauge Panels

- CPU
- Memory
- Disk

Stat Panels

- Service Status
- Targets Up
- Active Alerts

Time Series

- Network Traffic
- CPU History
- Memory History

Tables

- Target Status
- Failed Targets

This consistency improves readability and simplifies monitoring.

---

# Alerting

Grafana Alerting was configured to monitor critical infrastructure metrics.

Configured alerts include:

- High CPU Usage
- High Memory Usage
- High Disk Usage
- Prometheus Offline
- Exporter Down

Alerts are delivered through Discord.

---

# Dashboard Organization

Dashboards were separated by operational responsibility rather than combining every panel into a single interface.

Benefits include:

- Easier navigation
- Reduced visual clutter
- Faster troubleshooting
- Better scalability

Each dashboard has a clearly defined purpose.

---

# Verification

Verify Grafana container.

```bash
docker ps
```

Verify logs.

```bash
docker logs grafana
```

Open:

```
https://grafana.greimlab.net
```

Verify:

- Dashboards load
- Prometheus connected
- Panels display data
- Alerts configured
- No panel errors

---

# Validation

The following items were confirmed:

✅ Grafana container running

✅ Prometheus connected

✅ Dashboards loading

✅ Metrics updating

✅ Alerts functioning

✅ HTTPS enabled

---

# Troubleshooting

## No Data

### Symptoms

Panels displayed:

```
No Data
```

### Resolution

Verified:

- Prometheus targets
- Metric names
- PromQL queries
- Time range
- Exporter status

The Explore page was used extensively to identify valid metrics before updating dashboard panels.

---

## Incorrect Panel Layout

### Symptoms

Panels were misaligned or displayed inconsistent sizing.

### Resolution

Adjusted Grafana grid positioning to improve dashboard organization and maintain a consistent visual layout.

---

## Threshold Colors Incorrect

### Symptoms

Gauge colors did not accurately reflect system health.

### Resolution

Adjusted threshold values based on expected operating ranges for CPU, memory, and disk utilization.

---

## Alert Not Triggering

### Symptoms

Alert rule remained in the Normal state despite test conditions.

### Resolution

Verified:

- PromQL expression
- Evaluation interval
- Alert condition
- Contact point
- Notification policy

---

# Lessons Learned

Grafana transformed raw infrastructure metrics into actionable operational dashboards.

Developing custom dashboards required understanding PromQL, panel types, thresholds, field overrides, and dashboard organization.

Rather than relying on imported dashboards, building dashboards from scratch provided a much deeper understanding of infrastructure monitoring and resulted in visualizations tailored specifically to GreimLab.

---

# Screenshots

Include:

- Mission Control Dashboard
- Pi-hole Dashboard
- Proxmox Dashboard
- Prometheus Dashboard
- Target Status Dashboard
- Alert Rules
- Contact Points
- Notification Policies

---

# Next Phase

➡️ Phase 14 – Monitoring Exporters