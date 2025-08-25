# SETUP_GUIDE.md — Full End-to-End Instructions

This guide walks you from a clean Ubuntu VM on ESXi to a working monitoring stack,
GitHub repo, and portfolio update.

---

## 1) Prepare Ubuntu Monitoring VM

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io docker-compose git
sudo systemctl enable docker
sudo systemctl start docker
```

Create project folder:

```bash
mkdir -p ~/monitoring-stack && cd ~/monitoring-stack
```

Add files from this pack (docker-compose.yml, prometheus.yml, README.md).

---

## 2) Bring Up the Stack

```bash
docker-compose up -d
```

Check services:
- Prometheus -> http://<server-ip>:9090
- Grafana -> http://<server-ip>:3000 (user: admin, pass: admin at first run)
- Node Exporter -> http://<server-ip>:9100/metrics

---

## 3) Configure Grafana

- Data source: Prometheus with URL http://prometheus:9090
- Import dashboards by ID:
  - Linux Node Exporter: 1860
  - Windows Exporter: 10427

---

## 4) Add Windows VM Metrics

- Install Windows Exporter on the Windows VM.
- Confirm metrics at http://<win-ip>:9182/metrics.
- Edit prometheus.yml to add <win-ip>:9182 under the windows_vm job.
- Apply change:
  ```bash
  docker compose restart prometheus
  ```

---

## 5) GitHub Repository (Portfolio-Ready)

From the ~/monitoring-stack folder that contains your files:

```bash
git init
git add .
git commit -m "Initial commit: Monitoring Stack project"
# Create a new repo at https://github.com/new named: monitoring-stack
git remote add origin https://github.com/ZdravecPanov/monitoring-stack.git
git branch -M main
git push -u origin main
```

Add screenshots after you have dashboards:

```bash
mkdir -p screenshots
# save images as grafana-linux-dashboard.png / grafana-windows-dashboard.png
git add screenshots/
git commit -m "Add Grafana screenshots"
git push
```

---

## 6) Update Your Portfolio Website

Add the project snippet to your Projects section:

### Full Monitoring Stack Deployment
Deployed a self-hosted monitoring solution with Prometheus, Grafana, Node Exporter, and Windows Exporter to monitor both Linux and Windows VMs on an ESXi server. The setup includes metrics collection, visualization dashboards, and alerting — replicating a real DevOps monitoring environment.
Link: https://github.com/ZdravecPanov/monitoring-stack

If your site is static and hosted from a GitHub repo, commit the content changes and push.
If using a CMS/site builder, paste the snippet into the appropriate section and link the repo.

---

## 7) Troubleshooting Tips

- docker: command not found -> reinstall Docker: sudo apt install -y docker.io
- Grafana "bad gateway" -> wait 30–60s after first start; check logs:
  docker compose logs -f grafana
- Prometheus target DOWN -> verify IP/port and firewall rules; try curl from the VM:
  curl http://<target-ip>:<port>/metrics
- Port already in use -> change the left-hand host port in docker-compose.yml or stop the conflicting service.

---

## 8) Next Projects (Placeholders & Snippets)

Use docs/PROJECT_SNIPPETS.md to quickly populate your portfolio with In Progress items.
Replace links with real repos as you complete them.
