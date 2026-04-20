# 🚀 Cloud-Based Monitoring System using Prometheus & Grafana on AWS

## 📌 Project Overview

This project demonstrates the design and deployment of a **cloud-native monitoring system** using **Prometheus** and **Grafana** on an AWS EC2 instance. The goal was to build a **real-time observability pipeline** to monitor system performance metrics such as CPU, memory, disk usage, and network activity.

The entire setup was containerized using **Docker**, ensuring portability and consistency across environments. A **custom Docker network** was implemented to enable seamless communication between services, following production-level practices.

---

## 🧱 Architecture

```
Node Exporter  →  Prometheus  →  Grafana
     │                │              │
 System Metrics   Collects Data   Visualizes Data
```

* **Node Exporter**: Collects system-level metrics
* **Prometheus**: Scrapes and stores metrics
* **Grafana**: Visualizes metrics via dashboards

---

## ⚙️ Tech Stack

* **Cloud**: AWS EC2 (Ubuntu 22.04)
* **Containerization**: Docker
* **Monitoring**: Prometheus
* **Visualization**: Grafana
* **Metrics Exporter**: Node Exporter
* **Version Control**: Git & GitHub

---

## 🔥 Key Features

* Real-time monitoring of system metrics (CPU, RAM, Disk, Network)
* Custom Prometheus configuration for metric scraping
* Docker-based deployment with isolated networking
* Grafana dashboards with professional visualization
* Custom PromQL queries for advanced metric analysis
* Alert setup for high CPU usage
* Fully reproducible setup via GitHub

---

## 🛠️ Setup Instructions (Step-by-Step)

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/chaitanyaaaa/prometheus-grafana-monitoring.git
cd prometheus-grafana-monitoring/prometheus
```

---

### 2️⃣ Install Docker (if not installed)

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker ubuntu
```

---

### 3️⃣ Create Docker Network

```bash
docker network create monitoring
```

---

### 4️⃣ Run Node Exporter

```bash
docker run -d \
  --name=node-exporter \
  --network=monitoring \
  -p 9100:9100 \
  prom/node-exporter
```

---

### 5️⃣ Configure Prometheus

Ensure `prometheus.yml` contains:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

---

### 6️⃣ Run Prometheus

```bash
docker run -d \
  --name prometheus \
  --network=monitoring \
  -p 9090:9090 \
  -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```

---

### 7️⃣ Run Grafana

```bash
docker run -d \
  --name grafana \
  --network=monitoring \
  -p 3000:3000 \
  grafana/grafana
```

---

### 8️⃣ Access Services

* Prometheus → `http://<EC2-PUBLIC-IP>:9090`
* Grafana → `http://<EC2-PUBLIC-IP>:3000`

Grafana Login:

* Username: `admin`
* Password: `admin`

---

### 9️⃣ Configure Grafana

* Add Prometheus Data Source:

  ```
  http://prometheus:9090
  ```

* Import Dashboard:

  * Dashboard ID: `1860` (Node Exporter Full)

---

## 📊 Sample Metrics Visualized

* CPU Utilization (%)
* Memory Usage (%)
* Disk Usage (%)
* Network Traffic

---

## 📸 Screenshots

*Add your screenshots here*

```
screenshots/
├── grafana-dashboard.png
├── prometheus-targets.png
├── ec2-instance.png
```

---

## ⚠️ Challenges & Solutions

### ❌ Issue: Prometheus container exiting immediately

**Cause:** YAML syntax error in configuration file
**Solution:** Fixed indentation and array syntax in `prometheus.yml`

---

### ❌ Issue: Grafana unable to connect to Prometheus

**Cause:** Incorrect use of `localhost` inside containers
**Solution:** Used Docker network and container name (`prometheus:9090`)

---

### ❌ Issue: Metrics not showing in dashboard

**Cause:** Incorrect target configuration
**Solution:** Updated target to `node-exporter:9100`

---

## 🧠 Key Learnings

* Understanding of monitoring pipeline (Exporter → Prometheus → Grafana)
* Hands-on experience with PromQL queries
* Docker networking for service communication
* Debugging containerized applications using logs
* Designing production-like dashboards

---
