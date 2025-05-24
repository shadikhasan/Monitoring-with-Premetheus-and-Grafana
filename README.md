
# 🚀 Monitoring with Prometheus and Grafana

This project demonstrates a basic monitoring setup using **Prometheus**, **Grafana**, and exporters like **Node Exporter** and **Blackbox Exporter**, all containerized with **Docker Compose**.

---

## 📦 Stack Overview

| Service            | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **Prometheus**     | Collects metrics from configured exporters and stores them.                |
| **Grafana**        | Visualizes metrics and allows dashboard creation from Prometheus data.     |
| **Node Exporter**  | Exposes machine-level metrics such as CPU, memory, and disk usage.         |
| **Blackbox Exporter** | Probes endpoints like HTTP, HTTPS, TCP, ICMP and exposes probe results. |

---

## 🧾 Features

- 📊 Real-time visualization of server metrics
- 🌐 Probing of external/internal services with Blackbox Exporter
- 🐳 Easy deployment with Docker Compose
- 💾 Persisted Grafana dashboards with Docker volume

---

## 🛠️ Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/)

---

## 🚀 Getting Started

1. **Clone the Repository:**

```bash
git clone https://github.com/shadikhasan/Monitoring-with-Premetheus-and-Grafana.git
cd Monitoring-with-Premetheus-and-Grafana
```

2. **Run the Stack:**

```bash
docker-compose up -d
```

3. **Access the Services:**

| Service    | URL                      |
|------------|--------------------------|
| Prometheus | http://localhost:9090    |
| Grafana    | http://localhost:3200    |
| Node Exporter | http://localhost:9100 |
| Blackbox Exporter | http://localhost:9115 |

---

## 🔐 Grafana Credentials

- **Username:** `admin`
- **Password:** `admin`  
(You will be prompted to change it on first login)

---

## 📥 Configurations

### 🔧 `prometheus.yml`

Prometheus is configured to scrape:

- Itself
- Node Exporter
- Blackbox Exporter targets (e.g. `http://google.com`)

### ⚙️ `blackbox.yml`

Blackbox Exporter is configured with:

```yaml
preferred_ip_protocol: "ip4"
```

This forces IPv4 and avoids Docker's common IPv6 routing issues.

---

## 📊 Dashboards

After logging into Grafana:

1. Go to **Configuration → Data Sources** → Add **Prometheus**:
   - URL: `http://prometheus:9090`

2. Import a dashboard:
   - Click **"+" → Import**
   - Use Dashboard ID: `1860` for Node Exporter Full
   - Select your Prometheus data source and import

---

## 🧪 Example Blackbox Target

To add external probes (like `http://google.com`), define them in `prometheus.yml`:

```yaml
- job_name: 'blackbox'
  metrics_path: /probe
  params:
    module: [http_2xx]
  static_configs:
    - targets:
      - http://google.com
  relabel_configs:
    - source_labels: [__address__]
      target_label: __param_target
    - source_labels: [__param_target]
      target_label: instance
    - target_label: __address__
      replacement: blackbox-exporter:9115
```

---

## 📂 Project Structure

```
.
├── docker-compose.yml
├── prometheus/
│   ├── prometheus.yml
│   └── blackbox.yml
└── README.md
```

---

## 🧹 Cleanup

To stop and remove all containers:

```bash
docker-compose down
```

To remove volumes as well:

```bash
docker-compose down -v
```

---

## ✍️ Author

**Shadik Hasan**  
📫 [GitHub](https://github.com/shadikhasan)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
