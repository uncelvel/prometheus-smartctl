# Prometheus S.M.A.R.T ctl metrics exporter

![build](https://github.com/uncelvel/prometheus-smartctl/workflows/build/badge.svg)

This is a simple exporter for the [Prometheus metrics](https://prometheus.io/) using [smartctl](https://www.smartmontools.org/). The script `smartprom.py` also comes with `smartprom.service` so that you can run this script in the background on your Linux OS via `systemctl`. The script will use port `9902`, you can change it by changing it directly in the script. This script exports all of the data available from the smartctl.

Docker image here: <https://hub.docker.com/r/matusnovak/prometheus-smartctl>

## Running under Systemd services
1. Install smartctl 
```
yum install smartmontools -y
```
2. Clone project 
```
git clone https://github.com/uncelvel/prometheus-smartctl
cd prometheus-smartctl
```
3. Copy the `smartprom.service` file into `/etc/systemd/system` folder.
```
cp smartprom.service /etc/systemd/system
```
4. Copy the `smartprom.py` file anywhere into your system.
```sh 
cp smartprom.py /usr/sbin/
chmod +x /usr/sbin/smartprom.py
```
5. Install `prometheus_client` for the root user
```
yum install python3 -y 
sudo -H python3 -m pip install prometheus_client
```
6. Run 
```
systemctl enable smartprom
systemctl start smartprom
```
7. Your metrics will now be available at 
```
http://localhost:9902
```

## Docker usage

No extra configuration needed, should work out of the box. The `privileged: true` is needed because then `smartctl` can't access drvies from the host.

```yml
version: '3'
services:
  smartctl-metrics:
    image: matusnovak/prometheus-smartctl:latest
    restart: unless-stopped
    privileged: true
    ports:
      - 9902:9902
```

Your metrics will be available at <http://localhost:9902/metrics>
