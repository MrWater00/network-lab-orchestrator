# 🌐 Automated Network Lab Orchestrator

> A fully automated, infrastructure-as-code network lab platform that deploys, configures, and monitors complete network topologies — built and rebuilt in minutes, with zero manual effort.

![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Containerlab](https://img.shields.io/badge/Containerlab-Topology-00ADD8?style=for-the-badge)
![Ansible](https://img.shields.io/badge/Ansible-Automation-EE0000?style=for-the-badge&logo=ansible&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-Dashboards-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?style=for-the-badge&logo=docker&logoColor=white)

---

## 📌 Overview

This project automates the full lifecycle of a network lab environment — from topology deployment to device configuration and real-time monitoring — using modern DevOps and network automation tools.

Instead of manually configuring routers, switches, and links one by one, this platform lets you **define your entire network in a YAML file** and spin it up automatically. It demonstrates real-world skills used in enterprise networking, data center operations, and network reliability engineering.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Network Lab Orchestrator                  │
│                                                             │
│  ┌─────────────┐    ┌─────────────┐    ┌────────────────┐  │
│  │  Topology   │    │   Ansible   │    │   Monitoring   │  │
│  │  (YAML IaC) │───▶│ (Config Mgmt│───▶│ Stack          │  │
│  │             │    │  & Verify)  │    │                │  │
│  └─────────────┘    └─────────────┘    └────────────────┘  │
│         │                                      │            │
│         ▼                                      ▼            │
│  ┌─────────────┐                    ┌────────────────────┐  │
│  │ Containerlab│                    │ Prometheus         │  │
│  │  pc1 pc2    │                    │ Grafana            │  │
│  │  pc3 pc4    │                    │ cAdvisor           │  │
│  │  sw1 sw2 r1 │                    │                    │  │
│  └─────────────┘                    └────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

- **Infrastructure as Code** — entire network topology defined in YAML, version-controlled and reproducible
- **Automated Deployment** — spin up routers, switches, and hosts with a single command using Containerlab
- **Configuration Management** — push device configs, verify connectivity, and apply changes across all devices using Ansible
- **Dynamic Routing** — FRRouting (FRR) handles BGP/routing protocols between network nodes
- **Full Observability** — Prometheus collects metrics, Grafana visualizes them in real-time dashboards
- **Container Monitoring** — cAdvisor tracks CPU, memory, and network I/O for every running container
- **Repeatable & Scalable** — tear down and rebuild the entire lab in minutes with no manual steps

---

## 🧰 Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| **Topology** | [Containerlab](https://containerlab.dev/) | Define and deploy network topologies as containers |
| **Routing** | [FRRouting (FRR)](https://frrouting.org/) | BGP, OSPF, and other routing protocols |
| **Config Management** | [Ansible](https://www.ansible.com/) | Push configs and verify connectivity across devices |
| **Metrics Collection** | [Prometheus](https://prometheus.io/) | Scrape and store time-series metrics |
| **Visualization** | [Grafana](https://grafana.com/) | Real-time dashboards for lab health |
| **Container Metrics** | [cAdvisor](https://github.com/google/cadvisor) | Per-container resource usage monitoring |
| **Orchestration** | [Docker + Docker Compose](https://docs.docker.com/compose/) | Run monitoring stack as containers |

---

## 📁 Project Structure

```
network-lab/
│
├── topology/                   # Network topology definitions
│   └── network-lab.yml         # Containerlab topology file (nodes, links, IPs)
│
├── ansible/                    # Automation & configuration management
│   ├── inventory/              # Host inventory files
│   ├── playbooks/              # Ansible playbooks
│   │   ├── configure.yml       # Push device configurations
│   │   └── verify.yml          # Verify connectivity & routing
│   └── ansible.cfg             # Ansible configuration
│
└── monitoring/                 # Observability stack
    ├── docker-compose.yml      # Prometheus + Grafana + cAdvisor setup
    └── prometheus/
        └── prometheus.yml      # Scrape configs and targets
```

---

## 🚀 Getting Started

### Prerequisites

- Linux (Kali Linux recommended) or any Debian-based distro
- Docker & Docker Compose
- Containerlab
- Ansible
- Python 3

### 1. Clone the repository

```bash
git clone https://github.com/MrWater00/network-lab-orchestrator.git
cd network-lab-orchestrator
```

### 2. Deploy the network topology

```bash
cd topology
sudo containerlab deploy -t network-lab.yml
```

This spins up all routers, switches, and hosts as Docker containers with the defined topology.

### 3. Push configurations with Ansible

```bash
cd ../ansible
ansible-playbook playbooks/configure.yml
```

### 4. Verify connectivity

```bash
ansible-playbook playbooks/verify.yml
```

### 5. Start the monitoring stack

```bash
cd ../monitoring
sudo docker-compose up -d
```

### 6. Access dashboards

| Service | URL | Credentials |
|---------|-----|-------------|
| Grafana | http://localhost:3000 | admin / networklab123 |
| Prometheus | http://localhost:9090 | — |
| cAdvisor | http://localhost:8080 | — |

---

## 📊 Monitoring Dashboard

The Grafana dashboard (cAdvisor Exporter) provides real-time visibility into:

- **CPU Usage** — per container, with mean and max values
- **Memory Usage** — RAM consumption across all lab nodes
- **Network I/O** — traffic in/out per container
- **Container Info** — running status, uptime, and image details for all nodes

### Live Containers Monitored

| Container | Role | Image |
|-----------|------|-------|
| `clab-network-lab-pc1` to `pc4` | End hosts | alpine:latest |
| `clab-network-lab-sw1`, `sw2` | Switches | alpine:latest |
| `clab-network-lab-r1` | Router | frrouting/frr:latest |
| `prometheus` | Metrics DB | prom/prometheus:latest |
| `grafana` | Dashboards | grafana/grafana:latest |
| `cadvisor` | Container metrics | gcr.io/cadvisor/cadvisor:latest |

---

## 🔧 Key Concepts Demonstrated

### Infrastructure as Code (IaC)
The entire network topology — nodes, links, IP addressing, device roles — is declared in a YAML file. This means the lab is version-controlled, shareable, and reproducible on any machine.

### Network Automation
Ansible playbooks automate pushing configurations to all devices simultaneously, eliminating manual CLI work and human error. Changes can be applied across the entire lab in seconds.

### Observability
The monitoring stack collects real performance data from every container in the lab. Prometheus stores time-series metrics and Grafana renders them into actionable dashboards — the same pattern used in production environments.

---

## 📚 Skills Showcased

- `Network Automation` — automated config push and verification
- `Infrastructure as Code` — YAML-defined reproducible environments
- `Container Networking` — Docker-based network simulation
- `Routing Protocols` — FRR-based dynamic routing (BGP/OSPF)
- `Observability` — Prometheus + Grafana monitoring pipeline
- `Configuration Management` — Ansible playbooks and inventories
- `DevOps` — Docker Compose, containerized services

---

## 👤 Author

**MrWater00**
- GitHub: [@MrWater00](https://github.com/MrWater00)

---

> *Built to demonstrate practical network automation skills used in modern enterprise and data center networking.*
