# 🚀 Reverse Proxy Monitoring Suite

## 📌 Overview

This project sets up a **monitoring suite** using **Traefik, Prometheus, and Grafana** to track real-time metrics, analyze traffic, and ensure security for web applications. The setup is containerized with **Docker**, making it easy to deploy and manage.

## 🔥 Features

- ✅ **Traefik as a Reverse Proxy and Load Balancer** with automatic HTTPS (TLS/SSL).
- ✅ **Prometheus for scraping metrics** from Traefik.
- ✅ **Grafana for visualization** and real-time monitoring.
- ✅ **Security-focused setup** with TLS, HSTS, and firewall rules.
- ✅ **Alerting & logging** for traffic anomalies and failures.

## 🏗 Architecture Diagram
![Architecture Diagram](https://github.com/user-attachments/assets/d3e423c6-f885-4ac6-9f11-fed5fb4d0f19)


## 🛠 Installation Guide

### **1️⃣ Prerequisites**

- Docker installed
- Basic Linux knowledge
- Domain name (for HTTPS, if applicable)

### **2️⃣ Setup Steps**

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/Andrew-Opp/Reverse-Proxy-Monitoring-Suite.git
   cd Reverse-Proxy-Monitoring-Suite
   ```
2. **Create Docker Networks:**
   ```bash
   docker network create --driver=bridge --subnet=192.168.200.0/24 internal-net
   docker network create --driver=bridge --subnet=192.168.100.0/24 dmz-net
   ```
3. **Start the Containers in dmz-net Network:**
   ```bash
   docker run -d --name traefik --network dmz-net -p 80:80 -p 443:443 traefik:v2.9
   docker run -d --name prometheus --network dmz-net -p 9090:9090 -v $(pwd)/config/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
   docker run -d --name grafana --network dmz-net -p 3000:3000 grafana/grafana
   ```
4. **Connect Traefik to the internal network:**
   ```bash
   docker network connect internal-net traefik
   ```
5. **Configure Grafana to Use Prometheus:**
   - Open Grafana: [http://localhost:3000](http://localhost:3000)
   - Login with `admin/admin`
   - Go to **Configuration > Data Sources**
   - Add **Prometheus** as a source (`http://prometheus:9090`)
   - Save & Test
6. **Import a Pre-Built Dashboard:**
   - Go to **Dashboards > Import**
   - Enter Dashboard ID: `17346` *(or any other Traefik dashboard)*
   - Select Prometheus as the data source and **Import**

## 📊 Monitoring & Metrics

- **Traefik Metrics**: `http://localhost:8080/metrics`
- **Prometheus UI**: `http://localhost:9090`
- **Grafana Dashboard**: `http://localhost:3000`

## 🔒 Security & Hardening

- **Enable HTTPS (TLS/SSL) using OpenSSL-generated certificates**
- **Force HTTP to HTTPS redirection**
- **Enable HSTS for extra security**
- **Fine-tune firewall rules to prevent bypassing Traefik**
- **Implement logging & monitoring alerts**

## 🔥 Firewall Rules

```bash
sudo iptables -A DOCKER-USER -s 192.168.100.2 -d 192.168.200.0/24 -j ACCEPT
sudo iptables -A DOCKER-USER -i eth0 -s 0.0.0.0/0 -d 192.168.200.0/24 -j DROP
sudo iptables -A DOCKER-USER -s 192.168.200.0/24 -d 192.168.200.0/24 -j ACCEPT
```

## 🛠 Troubleshooting

### **Common Issues & Fixes**

| Issue                             | Solution                                  |
| --------------------------------- | ----------------------------------------- |
| `Traefik not starting`            | Check logs: `docker logs traefik`         |
| `Prometheus can’t scrape metrics` | Verify `prometheus.yml` config            |
| `Grafana dashboards empty`        | Ensure Prometheus is set as a data source |

## 🎯 Skills Learned

- **Containerization** with Docker
- **Reverse Proxy Configuration** using Traefik
- **Load Balancing** using Traefik
- **Monitoring & Metrics Collection** with Prometheus
- **Data Visualization** using Grafana
- **Security Enhancements** (TLS, HSTS, firewall rules)
- **Logging & Alerting**
- **Networking Concepts** in Docker environments

## 🛠 Tools Used

- **Traefik** – Reverse Proxy and Load Balancer
- **Prometheus** – Metrics Collection and Monitoring
- **Grafana** – Data Visualization
- **Docker** – Containerization
- **Linux** – Server Environment

## 🚀 Future Enhancements

- **Implement Log Aggregation (ELK/Fluentd)**
- **Add Alerting with Prometheus & Grafana**
- **Include Docker Swarm/Kubernetes deployment**
- **Implement mTLS (Mutual TLS) for stronger authentication**
- **Automate OpenSSL certificate renewal**

## 📜 License

This project is open-source under the **MIT License**.

## 🤝 Contributing

If you’d like to contribute, feel free to open an issue or submit a pull request!

## 📢 Contact

For questions or discussions, reach out[ via ]([https://linkedin.com/in/yourprofile](https://www.linkedin.com/in/andrew-oppong-asante-663368286/))**[LinkedIn](https://www.linkedin.com/in/andrew-oppong-asante-663368286/)** or **GitHub Issues**.

---

