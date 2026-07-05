# Phase 14 – Monitoring

# Monitoring Exporters

> This phase documents the deployment and configuration of the monitoring exporters used throughout GreimLab. Exporters expose infrastructure and application metrics in a Prometheus-compatible format, enabling comprehensive monitoring through Prometheus and Grafana.

---

# Objective

Deploy monitoring exporters to collect detailed operational metrics from infrastructure services.

After completing this phase, GreimLab gains visibility into:

- Ubuntu Server performance
- Docker container statistics
- Pi-hole DNS metrics
- Proxmox virtualization metrics

These exporters provide the telemetry that powers every Grafana dashboard.

---

# Environment

| Component | Configuration |
|-----------|--------------|
| Platform | Ubuntu Server 26.04 LTS |
| Metrics Platform | Prometheus |
| Visualization | Grafana |
| Deployment | Docker Compose |

---

# Architecture

```
                    Ubuntu Server
                          │
     ┌────────────────────┼────────────────────┐
     │                    │                    │
     ▼                    ▼                    ▼
Node Exporter        cAdvisor         Pi-hole Exporter
     │                    │                    │
     └────────────────────┼────────────────────┘
                          │
                          ▼
                  Proxmox Exporter
                          │
                          ▼
                     Prometheus
                          │
                          ▼
                      Grafana
```

Every exporter exposes metrics using an HTTP endpoint that Prometheus scrapes at regular intervals.

---

# Monitoring Philosophy

Each exporter has a single responsibility.

Rather than collecting every metric from one application, GreimLab follows a modular monitoring approach.

Benefits include:

- Easier troubleshooting
- Independent updates
- Better scalability
- Simplified configuration
- Service-specific metrics

---

# Exporters Deployed

## Node Exporter

Node Exporter provides operating system metrics for the Ubuntu Server.

Metrics collected include:

- CPU utilization
- Memory usage
- Disk usage
- Filesystem statistics
- Network traffic
- System load
- Uptime

These metrics power the Mission Control dashboard.

---

## cAdvisor

cAdvisor provides container-level monitoring.

Metrics include:

- Container CPU utilization
- Container memory usage
- Network usage
- Filesystem usage
- Running containers

These metrics provide visibility into Docker workloads.

---

## Pi-hole Exporter

Pi-hole Exporter exposes DNS statistics.

Metrics include:

- Total DNS queries
- Queries blocked
- Block percentage
- Cache statistics
- Client activity
- Blocklist size

These metrics power the dedicated Pi-hole dashboard.

---

## Proxmox Exporter

The Proxmox Exporter exposes virtualization metrics from the Proxmox API.

Metrics include:

- Node CPU
- Node memory
- Storage utilization
- Network statistics
- Virtual machine status
- Virtual machine resource usage

These metrics provide insight into the health of the virtualization platform hosting GreimLab.

---

# Exporter Endpoints

Each exporter exposes metrics through an HTTP endpoint.

Examples include:

| Exporter | Default Port |
|----------|--------------|
| Node Exporter | 9100 |
| cAdvisor | 8080 |
| Pi-hole Exporter | 9617 |
| Proxmox Exporter | 9221 |

Prometheus periodically scrapes these endpoints according to the configured scrape interval.

---

# Prometheus Integration

Each exporter was added as a scrape target within `prometheus.yml`.

Prometheus continuously polls each endpoint and stores the returned metrics within its time-series database.

Once collected, the metrics become immediately available to:

- Grafana
- Alert Rules
- PromQL Queries

---

# Verification

Verify running containers.

```bash
docker ps
```

Verify Prometheus targets.

```
https://prometheus.greimlab.net/targets
```

Expected result:

All exporters display:

```
UP
```

Verify metrics.

```
https://prometheus.greimlab.net/graph
```

Search for metrics such as:

```
node_cpu_seconds_total

container_cpu_usage_seconds_total

pihole_dns_queries_today

pve_up
```

Successful queries confirm exporter functionality.

---

# Validation

The following items were confirmed:

✅ Node Exporter operational

✅ cAdvisor operational

✅ Pi-hole Exporter operational

✅ Proxmox Exporter operational

✅ Prometheus scraping all exporters

✅ Metrics available within Grafana

---

# Troubleshooting

## Exporter Displays DOWN

### Symptoms

Prometheus reports:

```
DOWN
```

### Resolution

Verified:

- Docker container status
- Correct exporter port
- Docker networking
- Prometheus target configuration

Restarted the exporter when necessary.

---

## No Metrics Available

### Symptoms

PromQL queries returned:

```
No Data
```

### Resolution

Verified:

- Exporter running
- Correct metric names
- Prometheus target status
- Grafana data source

Used the Prometheus **Explore** page to identify valid metrics before updating dashboard panels.

---

## Incorrect Port Configuration

### Symptoms

Exporter container running but Prometheus unable to scrape metrics.

### Resolution

Compared Docker Compose port mappings with Prometheus scrape targets.

Updated the configuration and restarted Prometheus.

---

# Lessons Learned

Exporters form the foundation of the GreimLab monitoring platform.

Each exporter specializes in collecting metrics for a specific component, allowing Prometheus to build a comprehensive view of infrastructure health.

Separating telemetry into dedicated exporters simplified troubleshooting and made the monitoring environment easier to expand as additional services are deployed.

---

# Screenshots

Include:

- Prometheus Targets page
- Node Exporter target (UP)
- cAdvisor target (UP)
- Pi-hole Exporter target (UP)
- Proxmox Exporter target (UP)
- Example PromQL query results
- Mission Control dashboard using exporter metrics

---

# Next Phase

➡️ Phase 15 – Grafana Dashboards