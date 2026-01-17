# Node-Exporter

# Create node exporter user
```bash
sudo useradd \
  --system \
  --no-create-home \
  --shell /bin/false \
  node_exporter
```
## Download Node Exporter
Check latest:
👉 https://github.com/prometheus/node_exporter/releases

```bash
cd /tmp
NODE_VERSION="1.7.0"
wget https://github.com/prometheus/node_exporter/releases/download/v${NODE_VERSION}/node_exporter-${NODE_VERSION}.linux-amd64.tar.gz
tar -xvf node_exporter-${NODE_VERSION}.linux-amd64.tar.gz
```

# Move binary
```bash
sudo mv node_exporter-${NODE_VERSION}.linux-amd64/node_exporter /usr/local/bin/
```
```bash
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
sudo chmod 755 /usr/local/bin/node_exporter
```
# Create systemd service
```bash
sudo vim /etc/systemd/system/node_exporter.service       #past this belo contain
```
# Paste this 👇
```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple

ExecStart=/usr/local/bin/node_exporter

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter
sudo nano /etc/prometheus/prometheus.yml
```
# Add under scrape_configs
```bash
scrape_configs:
  - job_name: "node_exporter"
    static_configs:
      - targets:
          - "localhost:9100"
```
# Validate config
```bash
promtool check config /etc/prometheus/prometheus.yml
```
