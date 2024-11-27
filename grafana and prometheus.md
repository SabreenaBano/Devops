linux commands :
 List all running processes:
     ps aux
Using ss to list listening ports:
        sudo ss -tuln or sudo netstat -tuln | grep LISTEN
Find MySQL exporter (port 9104 by default)
      sudo ss -tuln | grep :9104


https://docs.google.com/document/d/1i5Du5eFPTwjvUkhTKutAGzmBBvN1el0Ww6wSP35mjkc/edit?usp=sharing
# prometheus installation 
https://www.cherryservers.com/blog/install-prometheus-ubuntu
https://www.atlantic.net/dedicated-server-hosting/how-to-install-grafana-and-prometheus-on-ubuntu-22-04/
4. Reload Systemd and Restart the Service
Reload the systemd daemon and restart Prometheus:
sudo systemctl daemon-reload
sudo systemctl restart prometheus
5. Check the Service Status
Verify if Prometheus is running:
sudo systemctl status prometheus)
# Node Exporter installation:(http://public-ip:9100/metrics)
https://medium.com/@abdullah.eid.2604/node-exporter-installation-on-linux-ubuntu-8203d033f69c
(curl http://localhost:9100/metrics) to see locally

## Install Grafana on your system.
Start the Grafana service and verify it's running.
Access the Grafana Web UI and log in with default credentials.
Add Prometheus as a data source in Grafana.
Create a new dashboard and add at least 2 panels to display metrics from Prometheus (e.g., CPU usage, memory usage)

https://medium.com/@abdullah.eid.2604/grafana-installation-on-linux-ubuntu-afbecf5b21dc
         http://pub-ip:3000 (access via web)
    4. Add Prometheus as a Data Source
     After logging in, go to Configuration > Data Sources.
     Click "Add data source" and select Prometheus.
     In the URL field, enter your Prometheus server's address (e.g., http://localhost:9090).
Click "Save & Test" to verify the connection.

5. Create a New Dashboard and Add Panels
Create a New Dashboard:

Go to Dashboards > New Dashboard > Add a New Panel.
Add Panels for CPU and Memory Usage:

CPU Usage Panel:
Query:
rate(node_cpu_seconds_total[5m])
Add appropriate filters or legends for clarity.
Memory Usage Panel:
Query:
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes
Choose "Gauge" or "Graph" as the visualization type.
Customize the dashboard with titles, thresholds, and legends.

Save the dashboard by clicking Save (disk icon at the top).

6. Verify Metrics on Panels
Confirm that the panels are displaying data from Prometheus.
Adjust time ranges (top right corner) to view historical or real-time metrics.


## Install Apache Exporter on your system.
    https://techexpert.tips/prometheus/prometheus-monitoring-apache-ubuntu-linux/
Configure Apache to expose server status at http://localhost/server-status.
Start Apache Exporter and verify it's scraping Apache metrics.
Configure Prometheus to scrape metrics from the Apache Exporter

1. Install Apache Exporter
Download Apache Exporter: Visit the Apache Exporter GitHub releases page and download the latest 
wget https://github.com/Lusitaniae/apache_exporter/releases/download/v0.11.0/apache_exporter-0.11.0.linux-amd64.tar.gz
2.Extract the Archive:
   tar -xvzf apache_exporter-0.11.0.linux-amd64.tar.gz
   cd apache_exporter-0.11.0.linux-amd64
3 Move the Binary:
sudo mv apache_exporter /usr/local/bin/
4 Verify Installation:

2. Configure Apache to Expose Server Status
Enable Apache Modules:
sudo a2enmod status
sudo systemctl restart apache2
Update Apache Configuration: Edit the Apache configuration file to allow server-status:

sudo nano /etc/apache2/conf-available/status.conf
Add or modify the configuration:
<Location "/server-status">
    SetHandler server-status
    Require local
</Location>
Test Configuration and Restart Apache:


sudo apachectl configtest
sudo systemctl restart apache2
Verify Server Status: Open a browser or use curl to check:


curl http://localhost/server-status?auto
3. Start Apache Exporter
Run Apache Exporter: Start the exporter, pointing it to the Apache server-status endpoint:
apache_exporter --scrape_uri=http://localhost/server-status?auto
Optional: Run Apache Exporter as a Service: Create a systemd service file for Apache Exporter:
nano /etc/systemd/system/apache_exporter.service
Add the following content:

[Unit]
Description=Apache Exporter
After=network.target

[Service]
User=nobody
ExecStart=/usr/local/bin/apache_exporter --scrape_uri=http://localhost/server-status?auto
Restart=always

[Install]
WantedBy=multi-user.target
Reload systemd and start the service:

sudo systemctl daemon-reload
sudo systemctl start apache_exporter
sudo systemctl enable apache_exporter
Verify Apache Exporter: Open the Apache Exporter metrics endpoint:
curl http://localhost:9117/metrics

youtube :https://youtu.be/VfNk5GxHi98?si=0LWxvgazsVjD3W8j
-------------------------------------------------------------------------------------------------
# 4.Node Exporter
Install Node Exporter on your system.
Start the Node Exporter service.
Verify Node Exporter is exposing metrics by navigating to http://localhost:9100/metrics.
Configure Prometheus to scrape metrics from the Node Exporter.
-1. Install Apache Exporter
Download the Apache Exporter
  wget https://github.com/Lusitaniae/apache_exporter/releases/download/v0.13.4/apache_exporter-0.13.4.linux-amd64.tar.gz
2.Extract the Binar
---------------------------------------------------------------------------
sudo apt update
sudo apt install apache2 -y
3. Enable and Start Apache
For Ubuntu/Debian:

sudo systemctl enable apache2
sudo systemctl start apache2
For CentOS/RHEL:
sudo systemctl enable httpd
sudo systemctl start httpd
4. Enable mod_status
After confirming Apache is running, enable mod_status:

For Ubuntu/Debian:
sudo a2enmod status
sudo systemctl restart apache2
For CentOS/RHEL: Add the configuration directly in the Apache configuration file (usually /etc/httpd/conf/httpd.conf):
<Location "/server-status">
    SetHandler server-status
    Require all granted
</Location>
ExtendedStatus On
Then restart Apache:

sudo systemctl restart httpd
5. Verify Apache Server Status
Test the /server-status endpoint:

curl http://localhost/server-status?auto
     ------------------------------------------------------------------------------------------
## Monitor Nginx with Prometheus (nginx exporter)
   (https://www.fosstechnix.com/how-to-monitor-nginx-with-prometheus/)
Step#1:Install Ngnix on Ubuntu Server
     sudo apt update
     sudo apt install nginx -y
Step#2:Configure Nginx Status Module
      Edit the NGINX configuration to enable the status module by creating a configuration file
      sudo nano /etc/nginx/conf.d/status.conf
             server {
    listen 80;
    # Optionally: allow access only from localhost
    # listen localhost:80;

    server_name _;

    location /status {
        stub_status;
    }
}
    Always verify if the configuration is valid before restarting Nginx.
       nginx -t
# To update the Nginx config without downtime, you can use reload command.
        systemctl reload nginx
# Install Nginx Prometheus Exporter
    First of all, let's create a folder for the exporter and switch directory
         mkdir /opt/nginx-exporter
         cd /opt/nginx-exporter
As a best practice, you should always create a dedicated user for each application that you want to run. Let's call it an nginx-exporter user and a group
        sudo useradd --system --no-create-home --shell /bin/false nginx-exporter
use curl to download the exporter on the Ubuntu machine.
      curl -L https://github.com/nginxinc/nginx-prometheus-exporter/releases/download/v0.11.0/nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz -o nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
Extract the prometheus exporter from the archive.
      rm nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
# make sure that we downloaded the correct binary by checking the version of the exporter.
      ./nginx-prometheus-exporter --version
#optional; let's update the ownership on the exporter folder.
      chown -R nginx-exporter:nginx-exporter /opt/nginx-exporter
#To run it, let's also create a systemd service file. In case it exits systemd manager can restart it. It's the standard way to run Linux daemons.
# (https://antonputra.com/monitoring/monitor-nginx-with-prometheus/#expose-basic-nginx-metrics)
----------------------------------------------------------------------------------------------
# blackbox exporter :
  https://www.junosnotes.com/guide/how-to-install-and-configure-blackbox-exporter-for-prometheus/
# Installing and Configuring Blackbox Exporter for Uptime Monitoring with Prometheus
Step 1: Install Blackbox Exporter
Download the Blackbox Exporter:
wget https://github.com/prometheus/blackbox_exporter/releases/download/v0.24.0/blackbox_exporter-0.24.0.linux-amd64.tar.gz
(Update the version if a newer one is available.)
# Extract the Archive:
  tar -xvf blackbox_exporter-0.24.0.linux-amd64.tar.gz
    cd blackbox_exporter-0.24.0.linux-amd64
Move the Binary
sudo mv blackbox_exporter /usr/local/bin/
Create a Service File: Create /etc/systemd/system/blackbox_exporter.service with the following content:
[Unit]
Description=Prometheus Blackbox Exporter
After=network.target

[Service]
User=blackbox
Group=blackbox
ExecStart=/usr/local/bin/blackbox_exporter --config.file=/etc/blackbox_exporter/config.yml
Restart=on-failure

[Install]
WantedBy=multi-user.target
# Create a Dedicated User:
sudo useradd --no-create-home --shell /bin/false blackbox
sudo mkdir /etc/blackbox_exporter
sudo chown blackbox:blackbox /etc/blackbox_exporter
Step 2: Configure Blackbox Exporter
Create the Configuration File: Create /etc/blackbox_exporter/config.yml:
modules:
  http_2xx:
    prober: http
    timeout: 5s
    http:
      valid_http_versions: [ "HTTP/1.1", "HTTP/2" ]
      valid_status_codes: []  # Defaults to 2xx
      follow_redirects: true
      fail_if_ssl: false
      fail_if_not_ssl: false

  https_2xx:
    prober: http
    timeout: 5s
    http:
      valid_http_versions: [ "HTTP/1.1", "HTTP/2" ]
      valid_status_codes: []  # Defaults to 2xx
      follow_redirects: true
      fail_if_ssl: true
      fail_if_not_ssl: false

  icmp:
    prober: icmp
    timeout: 5s
# Set Permissions:
sudo chown blackbox:blackbox /etc/blackbox_exporter/config.yml
Step 3: Start the Blackbox Exporter
# Reload Systemd:
sudo systemctl daemon-reload
# Start the Service:
sudo systemctl start blackbox_exporter
sudo systemctl enable blackbox_exporter
# Verify the Service:
sudo systemctl status blackbox_exporter
curl localhost:9115/metrics (9116)
# Step 4: Configure Prometheus to Scrape Blackbox Exporter
Edit Prometheus Configuration: Add the following job in the Prometheus prometheus.yml file:
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Change to https_2xx or icmp as needed
    static_configs:
      - targets:
          - http://example.com
          - https://anotherexample.com
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9115  # Blackbox Exporter address
Reload Prometheus:
sudo systemctl reload prometheus
Step 5: Verify Configuration
Check Blackbox Exporter Logs:
sudo journalctl -u blackbox_exporter
Test Targets in Prometheus: Visit http://<prometheus-server>:9090/targets and ensure the Blackbox Exporter targets are listed and show as "UP."

Visualize Data: Use Grafana or Prometheus’ graph to query and monitor Blackbox Exporter metrics:
probe_success{job="blackbox"}
------------------------------------------------------------------------------------------
# MYSQL Exporter:
     step 1: Update the System
Update your package index to ensure you have access to the latest software versions:
sudo apt update && sudo apt upgrade -y
Step 2: Install MySQL Server
Install MySQL Server using the default package manager:
sudo apt install mysql-server -y
Step 3: Secure MySQL Installation
Run the security script to improve MySQL’s default security:
sudo mysql_secure_installation
You will be prompted to configure several options:

Set a root password (important if not already set).
Remove anonymous users (recommended).
Disallow root login remotely (recommended for security).
Remove test database (optional).
Reload privilege tables (select Yes).
Step 4: Verify MySQL Service
Ensure the MySQL service is running:
sudo systemctl status mysql
# If it’s not active, start it:
sudo systemctl start mysql
You can also enable MySQL to start on boot:
sudo systemctl enable mysql
Step 5: Log in to MySQL
Log in as the root user:
      sudo mysql -u root -p
Enter the password you set during the secure installation process.

Step 6: Create a User for the MySQL Exporter
Follow these steps to create a dedicated user for the MySQL Exporter:

Create the user:
CREATE USER 'msql_exporter'@'localhost' IDENTIFIED BY 'Exp0rt3r@2024!';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'msql_exporter'@'localhost';
FLUSH PRIVILEGES;
Step 7: Exit MySQL
Step 2: Configure MySQL Access for Exporter
Cr
Allow Remote Access in MySQL: Edit the MySQL configuration file (/etc/mysql/my.cnf or /etc/my.cnf) and ensure the following line is present under [mysqld]:

ini
Copy code
bind-address = 0.0.0.0
Restart MySQL:

bash
Copy code
sudo systemctl restart mysql
Create a MySQL Exporter Configuration File: Create /etc/.mysqld_exporter.cnf with the following content:

ini
Copy code
[client]
user=exporter
password=exporter_password
Secure the file:

bash
Copy code
sudo chown mysql_exporter:mysql_exporter /etc/.mysqld_exporter.cnf
sudo chmod 600 /etc/.mysqld_exporter.cnf
Step 3: Start the MySQL Exporter Service
Reload Systemd:

bash
Copy code
sudo systemctl daemon-reload
Start and Enable the Service:

bash
Copy code
sudo systemctl start mysqld_exporter
sudo systemctl enable mysqld_exporter
Verify Exporter is Running:

bash
Copy code
sudo systemctl status mysqld_exporter
curl localhost:9104/metrics
Step 4: Configure Prometheus to Scrape MySQL Metrics
Edit Prometheus Configuration: Update the prometheus.yml file:

yaml
Copy code
scrape_configs:
  - job_name: 'mysql'
    static_configs:
      - targets: ['localhost:9104']  # Update the host and port if different
Reload Prometheus:

bash
Copy code
sudo systemctl reload prometheus
Step 5: Verify the Setup
Check Prometheus Targets: Visit Prometheus at http://<prometheus-server>:9090/targets and confirm the MySQL Exporter is listed and "UP."

Query Metrics in Prometheus: Use PromQL to explore MySQL metrics, e.g.:

promql
Copy code
mysql_up
mysql_global_status_connections
mysql_global_status_threads_running






















































    
    
    
    
    
    
    
    
    
    
    




   
 

            
 