
# Project Setup Guide

This guide details setting up WordPress with NGINX on VirtualBox using Bitnami and installing Prometheus with Node Exporters and Pushgateway.

## WordPress with NGINX on VirtualBox

### Bitnami Package
- Utilize the [Bitnami WordPress with NGINX and SSL package](https://bitnami.com/stack/wordpress-pro) for easy setup.

## Installing Prometheus

### Initial Steps
- **Update and Install Tools**
  ```bash
  yum update -y
  yum install wget vim -y
  wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
  ```

### Setting Up User and Directories
- **Configure Prometheus User and Directories**
  ```bash
  useradd --no-create-home --shell /bin/false prometheus
  mkdir /etc/prometheus /var/lib/prometheus
  chown prometheus:prometheus /etc/prometheus /var/lib/prometheus
  ```

### Configuring Prometheus
- **Extract and Set Up Prometheus Files**
  ```bash
  tar -xvzf prometheus-2.44.0.linux-amd64.tar.gz
  mv prometheus-2.44.0.linux-amd64 prometheuspackage
  cp -r prometheuspackage/{prometheus,promtool} /usr/local/bin/
  cp -r prometheuspackage/{consoles,console_libraries} /etc/prometheus
  chown -R prometheus:prometheus /usr/local/bin/{prometheus,promtool} /etc/prometheus/{consoles,console_libraries}
  ```

- **Edit Configuration File**
  - Modify `/etc/prometheus/prometheus.yml` as needed.
  - Change file ownership:
    ```bash
    chown prometheus:prometheus /etc/prometheus/prometheus.yml
    ```

- **Setting Up Prometheus Service**
  - Create and configure `/etc/systemd/system/prometheus.service`.
  - Enable and start the service:
    ```bash
    systemctl daemon-reload
    systemctl start prometheus
    systemctl status prometheus
    ```

- **Verification**
  - Access the web interface at `http://Server-IP:9090/graph`.

## Node Exporters Installation

### Download and Setup
- **Download Node Exporter**
  ```bash
  wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
  tar xzfv node_exporter-1.5.0.linux-amd64.tar.gz
  ```

- **Configure User and Move Binaries**
  ```bash
  useradd -rs /bin/false nodeusr
  mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin/
  ```

- **Create Node Exporter Service and Start It**
  - Configure the service.
  - Enable and start the service.

- **Update Prometheus Configuration**
  - Include Node Exporter in the configuration.
  - Restart Prometheus service.

- **Verification**
  - Prometheus: `http://localhost:9090/targets`
  - Node Exporter: `http://localhost:9100/metrics`

## Installing Pushgateway

### Setup and Configuration
- **Download and Prepare Pushgateway**
  ```bash
  wget https://github.com/prometheus/pushgateway/releases/download/v1.7.0/pushgateway-1.7.0.linux-amd64.tar.gz
  tar zxvf pushgateway-*.tar.gz
  cp pushgateway-*/pushgateway /usr/local/bin/
  useradd --no-create-home --shell /bin/false pushgateway
  chown pushgateway:pushgateway /usr/local/bin/pushgateway
  ```

- **Run Pushgateway**
  - Execute with specified options.

- **Update Prometheus Configuration**
  - Include Pushgateway in the configuration.

- **Verification**
  - Prometheus: `http://localhost:9090/targets`
  - Pushgateway: `http://localhost:9091/metrics`
