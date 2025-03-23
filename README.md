Simple-Monitoring
https://roadmap.sh/projects/simple-monitoring-dashboard

Installing Netdata
Update the system:

bash
sudo apt update && sudo apt upgrade -y
Install dependencies:

```bash
sudo apt install curl netcat-openbsd -y
```
Download and install Netdata:

```bash
bash <(wget -qO- https://my-netdata.io/kickstart.sh)
```
2. Setting Up Monitoring

After installing, Netdata will automatically start monitoring key system metrics such as CPU load, memory usage, and disk I/O. The default configuration should be sufficient for most scenarios.

Accessing the Dashboard
Open a web browser and enter:

If locally:
    http://localhost:19999

If remotely:
    http://IP-SERVER:19999

This will open the Netdata dashboard.

3. Configuring the Dashboard
To add a new chart or modify an existing one, you need to edit the configuration files in /opt/netdata/usr/lib/netdata/conf.d and restart the service:

```bash
sudo systemctl restart netdata
```
4. Configuring Alerts
Open the alert configuration file:

```bash
sudo nano opt/netdata/etc/netdata/health.d/health.conf
```
Add an alert for CPU load above 80%:

```ini
alert cpu_usage
    on cpu
    lookup = average -30s unaligned of system.cpu
    warn if > 80
    info = CPU usage is above 80%
```
Save and close the file. Restart Netdata:

Plain Text
```bash
sudo systemctl restart netdata
```

5. Creating Shell Scripts
Now let's create three scripts to automate processes.

setup.sh

```bash
#!/bin/bash
# Installing Netdata
sudo apt update && sudo apt upgrade -y
sudo apt install curl netcat -y
bash <(wget -qO- https://my-netdata.io/kickstart.sh)
echo "Netdata installed."
```
test_dashboard.sh

```bash
#!/bin/bash
# Script to increase system load
echo "Increasing system load ...."
stress --cpu 4 --timeout 60 &
echo "Load increased for 60 seconds."
```
Make sure to install stress if it's not installed:

```bash
sudo apt install stress -y
```
cleanup.sh

```bash
#!/bin/bash
# Removing Netdata
sudo systemctl stop netdata
sudo apt remove netdata -y
sudo rm -rf /opt/netdata
echo "Netdata removed."
```

6. Testing
Now you can test your installation:

Run setup.sh to install Netdata.
Run test_dashboard.sh to create a load.
Open a web browser at http://localhost:19999 or http://<IP-SERVER>:19999 to check the dashboard.
Run cleanup.sh to remove Netdata.


