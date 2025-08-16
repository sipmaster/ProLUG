In your reseach you find that the VictoriaMetrics can read telemetry data from Node Exporter using vmagent. Your tasks will be to deploy and configure VictoriaMetrics to poll telemetry data from Node Exporter.

Install VictoriaMetrics.

Configure VictoriaMetrics to gather telemetry data.

Ensure that VictoriaMetrics is running correctly.

<br>

### Solution
<details>
<summary>Solution</summary>

Setup your server for VictoriaMetrics install

Download and extract VictoriaMetrics.

```plain
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.123.0/victoria-metrics-linux-amd64-v1.123.0.tar.gz
tar xvfz victoria-metrics-linux-amd64-v1.123.0.tar.gz
```{{exec}}

Start VictoriaMetrics

```plain
nohup ./victoria-matrics-prod &
```{{exec}}

Create a directory and copy over the configuration for prometheus

```plain
mkdir /etc/prometheus
cp /answers/prometheus.yaml /etc/prometheus/prometheus.yaml
```{{exec}}

View the file and look at the configuration

```plain
cat /etc/prometheus/prometheus.yaml
```{{exec}}

Change the port used by the scraper

```plain
sed 's/9090/9100/' /etc/prometheus/prometheus.yaml
```{{exec}}

Install vmagent

```plain
wget https://github.com/VictoriaMetrics/VictoriaMetrics/releases/download/v1.123.0/vmutils-linux-amd64-v1.123.0.tar.gz
tar xvfz vmutils-linux-amd64-v1.123.0.tar.gz
```{{exec}}

Start vmagent

```plain
nohup ./vmagent-prod -promscrape.config=/etc/prometheus/prometheus.yaml -promscrape.config.strictParse=false -remoteWrite.url=http://127.0.0.1:8428/api/v1/write &
```{{exec}}

Verify that VictoriaMetrics and vmagent are working

```plain
cat nohup.out
ps -ef | grep [v]ictoria
```{{exec}}

You can also access VictoriaMetrics via this link:

{{TRAFFIC_HOST1_8428}}

</details>