# Unit 11 Killecoda - monitoring-linux-logs


## Install and configure Grafana

Your team has determined they need to offload and monitor Linux logs in your environment. 
You know that Grafana can do that as a visualization tool, so you plan to deploy it on a server as a daemon.

Deploy Grafana and ensure that it is running on your server.

### Solution

Refer to the Grafana Docs for latest installation instructions.

Install the required packages and Grafana GPG key.

apt install -y apt-transport-https
apt install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
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

https://7cecdf82983e-10-244-3-86-3000.saci.r.killercoda.com

Change the password. Default User: admin and Password: admin

Feel free to look around in the Web UI and then continue on to the next part of the lab.

## Install and configure Loki

Your team has decided the log aggregator will be a tool called Loki. 
Your job is to get Loki up and running on your server.

Install Loki.

Verify Loki is running and exposing port 3100.

### Solution

Create the directory where we will install Loki
mkdir /opt/loki
cd into that directory to set up the server

cd /opt/loki
Download and unpackage a current version of Loki

curl -O -L "https://github.com/grafana/loki/releases/download/v2.9.7/loki-linux-amd64.zip"
unzip "loki-linux-amd64.zip"
chmod a+x "loki-linux-amd64"

Copy over the loki config file from the /answers directory

cp /answers/loki-local-config.yaml /opt/loki
Copy over the loki.service file and restart the systemctl daemon so that Loki can run on your system.

cp /answers/loki.service /etc/systemd/system/loki.service
systemctl daemon-reload
Review the config file for Loki before starting the server.

cat /opt/loki/loki-local-config.yaml
Review the service file so that you are confident it is going to properly start Loki.

cat /etc/systemd/system/loki.service
Now that you've checked everything, start Loki daemon.

systemctl enable loki.service --now
Verify that Loki is running and exposing the proper port.

systemctl status loki.service --no-pager
ss -ntulp | grep 3100

## Install and configure Promtail

In your reseach you find that the Promtail tool can push logs over to Loki in real time. 
Your tasks will be to deploy and configure Promtail to push logs into loki server.

Install Promtail.

Configure Promtail to push /var/log/auth.log and /var/log/syslog off the server to the Loki aggregator.

Ensure that Promtail is running correctly.

### Solution

Create the directory where we will install Promtail.

mkdir /opt/promtail
Change to that directory and get ready to install promtail

cd /opt/promtail
Download and extract the executable

curl -O -L "https://github.com/grafana/loki/releases/download/v2.7.1/promtail-linux-amd64.zip"
unzip promtail-linux-amd64.zip 
Copy over the provided configuration and verify that it is pointing to the correct log files that you want to review.

cp /answers/promtail-local-config.yaml /opt/promtail
cat /opt/promtail/promtail-local-config.yaml
What do you notice about the configuration file? What are the scrape configs? What is the format that the file is in?

Setup the Promtail service configuration file for systemd to use to start the service

cp /answers/promtail.service /etc/systemd/system/promtail.service
cat /etc/systemd/system/promtail.service
Reload the systemd daemon and start the Promtail service

systemctl daemon-reload
systemctl enable promtail.service --now
Verify that the Promtail service is running on your system.

systemctl status promtail.service --no-pager
ps -ef | grep [p]romtail

## Configure Dashboard and view logs

You've setup all the pieces, now you have to create a dashboard in Grafana and 
verify that everything is working end to end.

Log into Grafana (and change the password if you didn't do it earlier)

Create the datasource for Loki in the the Datasource page. URL = http://127.0.0.1:3100

Create a dashboard (import 13639) that shows the log files for your server.

### Solution

Connect to Grafana and log in https://7cecdf82983e-10-244-3-86-3000.saci.r.killercoda.com

Create the datasource for Loki in the the Datasource page. URL = http://127.0.0.1:3100

Import the dashboard 13639 to view logs.

Verify the dashboard is working properly.

## End

Look at you, learning Linux Configuration! You created a log monitoring solution with 
Grafana, Loki, and Promtail. You created a dashboard to visualize your logs.