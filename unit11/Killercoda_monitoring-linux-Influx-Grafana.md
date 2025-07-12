# Unit 11 Killecoda - monitoring-linux-Influx-Grafana

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

https://8494489e37dd-10-244-3-87-3000.saci.r.killercoda.com

Change the password. Default User: admin and Password: admin

Feel free to look around in the Web UI and then continue on to the next part of the lab.

## Install InfluxDB

Your team has decided that the data will be collected by Telegraf and sent for 
aggregation to influxdb version 2.7.

Install InfluxDB2.

Verify that InfluxDB2 is running on your system.

Setup an organization, bucket, and token. Collect and copy your data toke from the UI.

### Solution

Install the InfluxDB2 repository.
wget -q https://repos.influxdata.com/influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | tee /etc/apt/sources.list.d/influxdata.list
Install InfluxDB2

apt-get update && apt-get -y install influxdb2
Start InfluxDB2

systemctl start influxdb      
systemctl enable influxdb
Verify InfluxDB2 is listening on the correct port.

ss -ntulp | grep 8086
lsof -i :8086
Connect to InfluxDB, set up your organization, bucket, and token. Copy those pieces of information out to a notepad, you will need them shortly.

https://8494489e37dd-10-244-3-87-8086.saci.r.killercoda.com

InfluxDB2.png

Once this is complete you have completed this section of the lab.

## Install and configure Telegraf

In your reseach you find that Telegraf can write out to InfluxDB2. 
So you need to install and configure telegraf to write telemetry data into influxDB2.

Install Telegraf.

Configure Telegraf to push data into InfluxDB2.

Verify that telegraf is running and pushing data into InfluxDB2

### Solution

Install the respository for telegraf.

wget -q https://repos.influxdata.com/influxdata-archive_compat.key
echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | tee /etc/apt/sources.list.d/influxdata.list
Install telegraf

apt update && apt -y install telegraf
Setup the telegraf configuration file to write to the output producer for influxdb2

vi /etc/telegraf/telegraf.conf
Set the information as follows: (Replace with your url, token, organization, and bucket)

###############################################################################
#                            OUTPUT PLUGINS                                   #
###############################################################################


# # Configuration for sending metrics to InfluxDB 2.0
 [[outputs.influxdb_v2]]
#   ## The URLs of the InfluxDB cluster nodes.
#   ##
#   ## Multiple URLs can be specified for a single cluster, only ONE of the
#   ## urls will be written to each interval.
#   ##   ex: urls = ["https://us-west-2-1.aws.cloud2.influxdata.com"]
   urls = ["http://127.0.0.1:8086"]
#
#   ## Token for authentication.
   token = "mXA4HiqkssvKaNMtGmEyGPa7h8bpV7hwgjqRJKtBz79qpbQSbIzsaClRJgyuIhBxyw5Lb8qF2Jt1yy_-2qUTA=="
#
#   ## Organization is the name of the organization you wish to write to.
   organization = "influxtest"
#
#   ## Destination bucket to write into.
   bucket = "influxdata"
Restart Telegraf and verify it's writing to InfluxDB2

systemctl restart telegraf
systemctl status telegraf --no-pager -l
Look at the output above and verify that telegraf is properly writing out to InfluxDB2.

## Configure Dashboard and view telemetry data

You've setup all the pieces, now you have to create a dashboard in Grafana and verify that everything is working end to end.

Log into Grafana (and change the password if you didn't do it earlier)

Create the datasource for InfluxDB2 in the the Datasource page. URL = http://127.0.0.1:8086 You also need to know all of your client token, organization, and bucket information.

Create a dashboard that shows the telemetry information for your server from InfluxDB2.

### Solution

Connect to Grafana and log in https://8494489e37dd-10-244-3-87-3000.saci.r.killercoda.com

Create the datasource for InfluxDB in the the Datasource page. Change the Query Language to Flux Set the URL = http://127.0.0.1:8086 Scroll all the way down and set: Organization, Bucket, and Token.

If you see "Datasource is working: 3 Buckets found" You know you connected properly.

Create a new Dashboard. Select a new Panel. Pick the data source InfluxDB. Choose the sample query of "Filter by Value".

Verify the dashboard is working properly.



## End

Look at you, learning Linux Configuration! You created a telemetry monitoring solution with 
Grafana, InfluxDB and Telegraf. You imported a dashboard to visualize your metrics on your server.

