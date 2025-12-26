# 🛡️ Hybrid Cloud HomeLab Infrastructure (Zero Trust)

![Status](https://img.shields.io/badge/Status-Production-success?style=for-the-badge&color=2ea44f)
![Security](https://img.shields.io/badge/Security-Zero%20Trust-blue?style=for-the-badge&color=0051c3)
![Tech](https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker&logoColor=white)

## 📖 About the Project
This repository contains **Infrastructure-as-Code (IaC)** configurations for a self-hosted homelab server designed with a **Zero Trust Network Access (ZTNA)** approach. 

The goal was to create an environment that is **completely invisible to public network scanners** (shodan/nmap) while maintaining high availability of services via Cloudflare Proxy network.

## 🏗️ Architecture

### Network Diagram
`User` ➡️ `Cloudflare Proxy` ➡️ `Secure Tunnel` ➡️ `Localhost (127.0.0.1)` ➡️ `Docker Containers`

<img width="3720" height="1702" alt="diagram_en" src="https://github.com/user-attachments/assets/3c858edf-31ee-4e03-8aec-a93842e482b4" />

### Key Security Features
1.  **Hermetic Networking:** Docker containers are bound **exclusively** to the loopback interface (`127.0.0.1:port`). This prevents Docker from bypassing the firewall using raw iptables.
2.  **Firewall Hardening (UFW):**
    * Policy: `DEFAULT_INPUT_POLICY="DROP"`
    * **NO open inbound ports** (No port 22, 80, or 443 opened on the router/firewall).
3.  **SSH Security:** Access is possible only via Cloudflare Access (Identity-Aware Proxy) using `ProxyCommand`.

## 🛠️ Tech Stack
* **OS:** Ubuntu Server / Debian
* **Orchestration:** Docker Compose (Per-service isolation strategy)
* **Networking:** Cloudflare Tunnel (`cloudflared`), UFW
* **Monitoring:** Uptime Kuma

## 🚀 Performance & Monitoring

The system is optimized for low resource usage. Below is the `htop` output showing the server under load with active Cloudflare Tunnels and Docker containers.

<img width="1480" height="736" alt="de076d9e-f45d-4238-9d7e-80d10cfd5deb" src="https://github.com/user-attachments/assets/87cffc33-e7e4-48f4-90df-87fb5056b96f" />
*Snapshot of system resources: CPU usage is minimal (~5%) despite multiple active services.*

## 🚀 Configuration Snippets

### 1. Network Isolation (docker-compose.yml)
Binding ports to `127.0.0.1` ensures the service is not reachable via the server's public LAN IP.

```yaml
services:
  webapp:
    image: nginx:alpine
    ports:
      # CRITICAL: Bind to localhost only.
      # Blocks direct access bypassing the Tunnel.
      - "127.0.0.1:8080:80"
    restart: always
```

### 2. Firewall Strategy (UFW)
The server relies on outbound connections for the Tunnel.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow in on lo  # Allow localhost communication for Tunnel <-> Docker
sudo ufw enable
```

## 📊 Live Status
You can view the live status of this infrastructure here: [monitor.czerks.pl](https://monitor.czerks.pl/status/portfolio)

---
### 📬 Contact
Created & Maintained by **Karol Wójcik** [🌐 Website](https://karol.czerks.pl/en/) • [💼 LinkedIn](https://www.linkedin.com/in/karolwojcik573) • [📧 Email](mailto:kwojcikv2@gmail.com)
