WordPress with NGINX on VirtualBox
Using Bitnami Package
For setup, use the Bitnami WordPress with NGINX and SSL package.
Installing Prometheus
Update and Install Tools
bash
Copy code
yum update -y
yum install wget vim -y
wget https://github.com/prometheus/prometheus/releases/download/v2.44.0/prometheus-2.44.0.linux-amd64.tar.gz
Setup User and Directories
bash
Copy code
useradd --no-create-home --shell /bin/false prometheus
mkdir /etc/prometheus /var/lib/prometheus
chown prometheus:prometheus /etc/prometheus /var/lib/prometheus
Extract and Move Prometheus Files
bash
Copy code
tar -xvzf prometheus-2.44.0.linux-amd64.tar.gz
mv prometheus-2.44.0.linux-amd64 prometheuspackage
cp -r prometheuspackage/{prometheus,promtool} /usr/local/bin/
cp -r prometheuspackage/{consoles,console_libraries} /etc/prometheus
chown -R prometheus:prometheus /usr/local/bin/{prometheus,promtool} /etc/prometheus/{consoles,console_libraries}
Configure Prometheus
Edit /etc/prometheus/prometheus.yml with the specified settings and change ownership:

bash
Copy code
chown prometheus:prometheus /etc/prometheus/prometheus.yml
Setting Up Prometheus as a Service
Create and configure /etc/systemd/system/prometheus.service. Then enable and start the service:

bash
Copy code
systemctl daemon-reload
systemctl start prometheus
systemctl status prometheus
Verification
Access Prometheus at http://Server-IP:9090/graph.

Installing Node Exporters
Download and Extract
bash
Copy code
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xzfv node_exporter-1.5.0.linux-amd64.tar.gz
Setup and Move Binaries
bash
Copy code
useradd -rs /bin/false nodeusr
mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin/
Create and Start Service
Configure Node Exporter service and start it.

Update Prometheus Configuration
Include Node Exporter in the Prometheus configuration.

Restart Prometheus Service
Restart the service and check its status.

Verification
Prometheus: http://localhost:9090/targets
Node Exporter: http://localhost:9100/metrics
Install Pushgateway
Download and Setup
bash
Copy code
wget https://github.com/prometheus/pushgateway/releases/download/v1.7.0/pushgateway-1.7.0.linux-amd64.tar.gz
tar zxvf pushgateway-*.tar.gz
cp pushgateway-*/pushgateway /usr/local/bin/
useradd --no-create-home --shell /bin/false pushgateway
chown pushgateway:pushgateway /usr/local/bin/pushgateway
Run Pushgateway
Run Pushgateway with the specified options.

Update Prometheus Configuration
Include Pushgateway in the Prometheus configuration.

Verification
Prometheus: http://localhost:9090/targets
Pushgateway: http://localhost:9091/metrics
