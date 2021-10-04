# Docker-Raspberry-PI-Monitoring

## Introduction

A monitoring solution for Docker hosts and containers with [Prometheus](https://prometheus.io/), [Grafana](http://grafana.org/), [cAdvisor](https://github.com/google/cadvisor), [NodeExporter](https://github.com/prometheus/node_exporter).

## Screenshot

![screencapture-192-168-1-100-1013-d-Ss3q6hSZk-docker-and-os-metrics-2021-10-04-21_55_16](https://user-images.githubusercontent.com/18188407/135915942-3ed65215-aa99-4068-a66a-491d822e9363.png)

## Installation

Clone this repository on your Docker host, cd into Docker-Raspberry-PI-Monitoring directory and run docker-compose up:

```sh
git clone https://github.com/oijkn/Docker-Raspberry-PI-Monitoring.git
cd Docker-Raspberry-PI-Monitoring
docker-compose up -d
```

Containers:

* Prometheus (metrics database) http://<host-ip>:9090
* Grafana (visualize metrics) http://<host-ip>:3000
* NodeExporter (host metrics collector)
* cAdvisor (containers metrics collector)

## Setup Grafana

Navigate to `http://<host-ip>:3000` and login with user ***admin*** password ***admin***. You can change the credentials in the compose file or by supplying the `ADMIN_USER` and `ADMIN_PASSWORD` environment variables on compose up. 

```yaml
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=changeme
GF_USERS_ALLOW_SIGN_UP=false
```

Grafana is not preconfigured with dashboard, so you have to import it from the [json](https://github.com/oijkn/Docker-Raspberry-PI-Monitoring/blob/main/grafana/dashboard_by_oijkn.json) file. And set Prometheus as the default data source.

## Setup Prometheus

```yaml
scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 30s
    static_configs:
      - targets: ['localhost:9090', 'cadvisor:8080', 'node-exporter:9100']
```
