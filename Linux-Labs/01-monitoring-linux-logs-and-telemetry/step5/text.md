Now, since you have some metrics, it's time to take care of logging. Your task is to install VictoriaLogs.

Install VictoriaLogs.

Verify VictoriaLogs is running and exposing port 9428.

<br>

### Solution
<details>
<summary>Solution</summary>
Create the directory where we will install Node Exporter.

Download and unpackage a current version of VictoriaLogs.

```plain
wget https://github.com/VictoriaMetrics/VictoriaLogs/releases/download/v1.28.0/victoria-logs-linux-amd64-v1.28.0.tar.gz
tar xvfz victoria-logs-linux-amd64-v1.28.0.tar.gz
```{{exec}}

Start VictoriaLogs
```plain
nohup ./victoria-logs-prod &
```{{exec}}


Install the Grafana plugin for VictoriLogs and restart the server

```plain
grafana-cli plugins install victoriametrics-logs-datasource
systemctl restart grafana-server
```{{exec}}

Next, we need to install promtail to gather the information from the server.

```plain
apt install promtail
```{{exec}}

On Debian/Ubuntu, the logging is done in the file syslog not messages.
We need to change this in the configuration of promtail.

```plain
sed -i 's/messages/syslog/' /etc/promtail/config.yml
```{{exec}}

One problem that will occur is that promtail will not be able to read the syslog file. Let's fix this.

```plain
cd /var/
apt install acl
setfacl -R -m u:promtail:rX log
setfacl -R -m d:u:promtail:rX log
systemctl start promtail
```{{exec}}

We need to change the url of the client where promtail will send the data.

```plain
sed -i '/http\:\/\/localhost\:3100\/loki\/api\/v1\/push/c \- url\: http\:\/\/127\.0\.0\.1\:9428\/insert\/loki\/api\/v1\/push' /etc/promtail/config.yml
systemctl restart promtail
```{{exec}}

What data can you see exposed? Don't worry if it's not well formatted for you to process, it's in the correct configuration for Prometheus to scrape.

</details>