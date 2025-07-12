# Unit 11 Killecoda - monitoring-linux-telemetry

## Install and configure Grafana

Your team has determined they need to monitor telemetry in your environment. 
You know that Grafana can do that as a visualization tool, so you plan to deploy it on a server as a daemon.

Deploy Grafana and ensure that it is running on your server.

### Solution

Refer to the Grafana Docs for latest installation instructions.

Install the required packages and Grafana GPG key.

apt install -y apt-transport-https
apt install -y software-properties-common wget
wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
Add the Grafana repository.

echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
Finally, we're ready to install Grafana:

apt update
# Install the latest Enterprise release:
apt install -y grafana-enterprise
Now that you've installed Grafana, let's make sure it's started.

systemctl daemon-reload
systemctl start grafana-server
systemctl status grafana-server --no-pager
Verify that the server is serving on port 3000 (the default port)

systemctl status grafana-server --no-pager
ss -ntulp | grep grafana
ss -ntulp | grep 3000
We can also check that the external Web UI is available and change the default password.

https://4588a59573a0-10-244-6-183-3000.saci.r.killercoda.com

Change the password. Default User: admin and Password: admin

Feel free to look around in the Web UI and then continue on to the next part of the lab.

## Install and configure node exporter

Your team has decided that the data for Prometheus will be exposed via the tool Node Exporter. 
Your jobs is to get Node Exporter up and running on your server.

Install Node Exporter.

Verify Node Exporter is running and exposing port 9100.

### Solution

Create the directory where we will install Node Exporter.
mkdir /opt/node_exporter
cd into that directory to set up the server.

cd /opt/node_exporter
Download and unpackage a current version of Node Exporter and move to the extracted folder.

wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xvfz node_exporter-*.*-amd64.tar.gz
cd node_exporter-*.*-amd64
cp /answers/node_exporter.service /etc/systemd/system/node_exporter.service
Review the service file so that you are confident it is going to properly start Node Exporter.

cat /etc/systemd/system/node_exporter.service
Now that you've checked everything reload the systemd configuration manager and start Node Exporter daemon.

systemctl daemon-reload
systemctl enable node_exporter.service --now
Verify that Node Exporter is running and exposing the proper port.

systemctl status node_exporter --no-pager
sleep 2
curl http://localhost:9100/metrics
What data can you see exposed? Don't worry if it's not well formatted for you to process, it's in the correct configuration for Prometheus to scrape.

## Install and configure Prometheus

In your reseach you find that the Prometheus can read telemetry data from Node Exporter. 
Your tasks will be to deploy and configure Prometheus to poll telemetry data from Node Exporter.

Install Prometheus.

Configure Prometheus to gather telemetry data.

Ensure that Prometheus is running correctly.

### Solution

Setup your server for Prometheus install

useradd prometheus
mkdir /var/lib/prometheus
Download and extract Prometheus and required tools.

wget https://github.com/prometheus/prometheus/releases/download/v2.42.0-rc.0/prometheus-2.42.0-rc.0.linux-amd64.tar.gz  -P /tmp
tar xvfz /tmp/prometheus-2.42.0-rc.0.linux-amd64.tar.gz -C /var/lib/prometheus/ --strip-components=1
cp /var/lib/prometheus/prometheus /usr/bin/prometheus
Change ownership of /var/lib/prometheus so that prometheus user can start the service

chown -R prometheus:prometheus /var/lib/prometheus/
Create a directory and copy over the configuration for prometheus

mkdir /etc/prometheus
cp /answers/prometheus.yml /etc/prometheus/prometheus.yml
View the file and look at the configuration

cat /etc/prometheus/prometheus.yml
Copy over the prometheus.service file so that systemd can control and start prometheus

cp /answers/prometheus.service /etc/systemd/system/prometheus.service
View the file and look at the configuration

cat /etc/systemd/system/prometheus.service
Start the Prometheus Service

systemctl daemon-reload
systemctl start prometheus
Verify that Prometheus is working

systemctl status prometheus.service --no-pager
ps -ef | grep [p]rometheus
You can also access prometheus via this link:

https://4588a59573a0-10-244-6-183-9090.saci.r.killercoda.com

## Configure Dashboard and view telemetry data

You've setup all the pieces, now you have to create a dashboard in Grafana and 
verify that everything is working end to end.

Log into Grafana (and change the password if you didn't do it earlier)

Create the datasource for Prometheus in the the Datasource page. URL = http://127.0.0.1:9090

Create a dashboard (import 159) that shows the telemetry information for your server.

### Solution

Connect to Grafana and log in https://4588a59573a0-10-244-6-183-3000.saci.r.killercoda.com

Create the datasource for Prometheus in the the Datasource page. URL = http://127.0.0.1:9090

Import the dashboard 159 to view the metric data.

Verify the dashboard is working properly.

## END

Look at you, learning Linux Configuration! 
You created a telemetry monitoring solution with Grafana, Prometheus and Node Exporter. 
You imported a dashboard to visualize your metrics on your server.
