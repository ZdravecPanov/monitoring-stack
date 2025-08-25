# Monitoring Stack: Prometheus + Grafana + Node Exporter + Windows Exporter

This project deploys a complete monitoring stack for Linux + Windows virtual machines on a
self-hosted ESXi lab using Docker Compose on Ubuntu.

---

## Features
- Prometheus for metrics collection and storage
- Grafana for visualization
- Node Exporter for Linux VM metrics
- Windows Exporter for Windows VM metrics
- Docker Compose for simple, reproducible deployment

---

## Architecture

Linux VM (Node Exporter)   --->
                              ---> Prometheus ---> Grafana
Windows VM (Windows Exporter) --->

---

## Quick Start

Requirements on the Ubuntu monitoring VM: Docker, Docker Compose, internet access.

```bash
# 1) Clone or copy this repo
git clone https://github.com/ZdravecPanov/monitoring-stack.git
cd monitoring-stack

# 2) Launch the stack
docker-compose up -d

# 3) Open the services
# Prometheus: http://<server-ip>:9090
# Grafana (default admin/admin): http://<server-ip>:3000
```

---

## Configure Prometheus Targets

Edit `prometheus.yml` to add your VM IPs:

- For extra Linux VMs (Node Exporter default port 9100):

```yaml
- job_name: "linux_vm"
  static_configs:
    - targets: ["<linux-vm-ip>:9100"]
```

- For Windows VM (Windows Exporter default port 9182):

```yaml
- job_name: "windows_vm"
  static_configs:
    - targets: ["<windows-vm-ip>:9182"]
```

Restart Prometheus after changes:

```bash
docker compose restart prometheus  # or: docker-compose restart prometheus
```

---

## Grafana Setup

1. Open Grafana -> Add data source -> Prometheus -> URL: http://prometheus:9090 -> Save & test.
2. Import dashboards (search by ID on grafana.com -> Dashboards -> Import):
   - Linux Node Exporter: ID 1860
   - Windows Exporter: ID 10427

Take screenshots of your dashboards and place them in ./screenshots/ for your portfolio.

---

## Install Windows Exporter (on your Windows VM)

1. Download from the official releases page.
2. Run the installer (accept defaults). Metrics endpoint should be available at:
   http://<windows-vm-ip>:9182/metrics

---

## Health Checks

- Prometheus targets page: http://<server-ip>:9090/targets
- Node Exporter metrics page: http://<server-ip>:9100/metrics
- Windows Exporter metrics page: http://<windows-vm-ip>:9182/metrics

---

## Maintenance

```bash
# Stop stack
docker compose down

# Start/stop single service
docker compose (start|stop|restart) <service>

# View logs
docker compose logs -f <service>
```

---

## Technologies
Ubuntu | Docker | Docker Compose | Prometheus | Grafana | Node Exporter | Windows Exporter

---

## Author
Zdravec Panov — https://zdravkopanov.online
