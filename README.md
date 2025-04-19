# monitoring-grafana-prometheus-zabbix-by-dcoker


Unduh Paket Terbaru:
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_6.0+debian11_all.deb

Pasang Paket yang Diunduh:
sudo dpkg -i zabbix-release_latest_6.0+debian11_all.deb
sudo apt update

Instal Zabbix Agent:
sudo apt install zabbix-agent

Konfigurasi Zabbix Agent:
Buka file konfigurasi:
sudo nano /etc/zabbix/zabbix_agentd.conf

Sesuaikan parameter berikut:
Server=IP_SERVER_ZABBIX //pointing ke ip tempat server zabbix diinstall
ServerActive=IP_SERVER_ZABBIX //pointing ke ip tempat serger zabbix diinstall
Hostname=hostnameserver

Mulai dan Aktifkan Layanan Zabbix Agent:
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent


Install Node Exporter (Prometheus)

1. Cari versi terbaru dari Node Exporter:
Kamu bisa langsung mengunjungi halaman releases Prometheus di GitHub untuk melihat versi terbaru Node Exporter di sini: https://github.com/prometheus/node_exporter/releases

Saat ini, versi terbaru yang saya temukan adalah v1.9.1.

2. Download versi ARM64:
Gantilah URL yang mengandung wildcard dengan versi lengkap yang sesuai. Misalnya untuk versi v1.9.1, berikut perintah untuk mengunduhnya:

wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-arm64.tar.gz
3. Ekstrak dan Install:
Setelah berhasil diunduh, ekstrak file yang telah diunduh:

tar -xzf node_exporter-1.9.1.linux-arm64.tar.gz
cd node_exporter-1.9.1.linux-arm64

4. Buat user dan setup systemd service:
sudo useradd -rs /bin/false node_exporter

# Copy binary ke /usr/local/bin
sudo cp node_exporter /usr/local/bin/

# Buat service systemd
sudo nano /etc/systemd/system/node_exporter.service

isi dengan:
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target

5. Enable dan jalankan service
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter

6. Cek status
sudo systemctl status node_exporter






