# Phase 12 – Monitoring

# Prometheus

> This phase documents the deployment and configuration of Prometheus as the primary metrics collection platform for GreimLab. Prometheus continuously scrapes metrics from infrastructure services, exporters, and applications, providing the foundation for dashboards, alerting, and long-term performance monitoring.

---

# Objective

Deploy Prometheus to centralize infrastructure monitoring.

After completing this phase, GreimLab gains:

- Time-series metrics collection
- Infrastructure monitoring
- Service health monitoring
- PromQL querying
- Grafana integration
- Alerting foundation

Prometheus serves as the primary data source for all Grafana dashboards throughout the environment.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Deployment | Docker Compose |
| Data Source | Prometheus |
| Dashboard Platform | Grafana |
| Alerting | Grafana Alerting |
| Storage | Local Persistent Volume |

---

# Architecture

```
                       Exporters
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
  Node Exporter      Pi-hole Exporter    Proxmox Exporter
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                            ▼
                      Prometheus
                            │
                Time-Series Database
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
          Grafana                   Alert Rules
                                          │
                                          ▼
                                    Discord Alerts
```

Prometheus continuously collects metrics from exporters and stores them in its built-in time-series database.

---

# Monitoring Philosophy

Prometheus is responsible for collecting metrics—not displaying them.

Its primary responsibilities include:

- Scraping metrics
- Storing historical data
- Executing PromQL queries
- Evaluating alert rules
- Supplying data to Grafana

This separation allows Prometheus to specialize in data collection while Grafana focuses on visualization.

---

# Prerequisites

Before beginning this phase, the following components were operational:

- Ubuntu Server
- Docker Engine
- Docker Compose
- Homepage
- Pi-hole
- Uptime Kuma

---

# Deployment

Prometheus was deployed using Docker Compose.

Persistent storage was configured for:

- Time-series database
- Configuration files
- Alert rules

The primary configuration file, `prometheus.yml`, defines:

- Global scrape interval
- Scrape targets
- Job names
- Exporters

---

# Scrape Targets

Prometheus was configured to collect metrics from the following services.

| Target | Purpose |
|----------|----------|
| Prometheus | Self-monitoring |
| Node Exporter | Ubuntu Server metrics |
| Pi-hole Exporter | DNS statistics |
| Proxmox Exporter | Virtualization metrics |
| cAdvisor | Docker container metrics |

Each target is monitored continuously.

---

# Metrics Collected

Prometheus collects thousands of metrics including:

## Ubuntu Server

- CPU utilization
- Memory usage
- Disk usage
- Network traffic
- Filesystem statistics
- Load averages

---

## Pi-hole

- DNS queries
- Queries blocked
- Block percentage
- Cache statistics
- Client activity

---

## Proxmox

- Node CPU
- Node memory
- Storage usage
- Virtual machine metrics

---

## Docker

- Running containers
- Container CPU
- Container memory
- Container network traffic
- Container filesystem usage

---

# PromQL

Prometheus uses PromQL to query collected metrics.

Examples include:

CPU Usage

```promql
100 - (avg by(instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

Memory Usage

```promql
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
```

Disk Usage

```promql
100 * (
1 -
(node_filesystem_avail_bytes /
node_filesystem_size_bytes)
)
```

These queries provide the data visualized throughout the Grafana dashboards.

---

# Integration

Prometheus integrates with:

- Grafana
- Exporters
- Docker
- Discord alerting
- Homepage widgets

It serves as the central monitoring platform for GreimLab.

---

# Verification

Verify the Prometheus container.

```bash
docker ps
```

Verify logs.

```bash
docker logs prometheus
```

Open:

```
https://prometheus.greimlab.net
```

Verify:

- Targets are UP
- Configuration loaded
- Metrics available
- Queries return data

---

# Validation

The following items were confirmed:

✅ Prometheus container running

✅ All configured targets reachable

✅ Metrics being collected

✅ Queries returning data

✅ Grafana connected successfully

---

# Troubleshooting

## Target Down

### Symptoms

Target displayed:

```
DOWN
```

### Resolution

Verified:

- Exporter running
- Docker container status
- Network connectivity
- Scrape target configuration
- Port availability

---

## No Data

### Symptoms

PromQL queries returned:

```
No Data
```

### Resolution

Verified:

- Metric name
- Exporter status
- Target health
- Prometheus configuration

Used the Explore page to identify the correct metric names before updating dashboard queries.

---

## Failed Targets

### Symptoms

One or more scrape targets failed.

### Resolution

Reviewed:

- `/targets`
- Prometheus logs
- Docker logs
- Exporter configuration

Restarted affected services when necessary.

---

# Lessons Learned

Prometheus became the central monitoring platform within GreimLab.

Unlike Uptime Kuma, which focuses on service availability, Prometheus collects detailed operational metrics from every monitored system.

Learning PromQL and understanding how exporters expose metrics provided valuable insight into enterprise monitoring platforms and laid the foundation for advanced dashboards and alerting.

---

# Screenshots

Include:

- Prometheus Home
- Targets Page
- Status → Configuration
- Status → Runtime Information
- Graph Page
- Successful PromQL Query

---

# Next Phase

➡️ Phase 13 – Grafana