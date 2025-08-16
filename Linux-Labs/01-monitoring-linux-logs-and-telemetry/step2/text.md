Your team has decided that the data for VictoriaMetrics will be exposed via the tool Node Exporter. Your jobs is to get Node Exporter up and running on your server.

Install Node Exporter.

Verify Node Exporter is running and exposing port 9100.

<br>

### Solution
<details>
<summary>Solution</summary>
Download and unpackage a current version of Node Exporter and move to the extracted folder.

```plain
apt install -y prometheus-node-exporter prometheus-node-exporter-collectors
```{{exec}}

Start Node Exporter
```plain
systemctl start prometheus-node-exporter 
```{{exec}}


Now that you've checked everything, make sure the daemon will restart after reboot.

```plain
systemctl enable prometheus-node-exporter.service --now
```{{exec}}

Verify that Node Exporter is running and exposing the proper port.

```plain
systemctl status prometheus-node-exporter.service --no-pager
sleep 2
curl http://localhost:9100/metrics
```{{exec}}

What data can you see exposed? Don't worry if it's not well formatted for you to process, it's in the correct configuration for Prometheus to scrape.

</details>