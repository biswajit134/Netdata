# TASK 7: Monitor System Resources Using Netdata
* Objective: Install Netdata and visualize system and app performance metrics.
* Tools: Netdata (free, open-source monitoring tool), Docker
* Deliverables: Screenshot of the dashboard and running metrics

## Mini-Guide (Hints):

**Run via Docker: docker run -d --name=netdata -p 19999:19999 netdata/netdata**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20150523.png?raw=true)

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151034.png?raw=true)
To connect the docker containers in Netdata Dashboard, we need to create docker-compose.yml file :
```
version: '3'
services:
  netdata:
    image: netdata/netdata:stable
    container_name: netdata
    pid: host
    network_mode: host
    restart: unless-stopped
    cap_add:
      - SYS_PTRACE
      - SYS_ADMIN
    security_opt:
      - apparmor:unconfined
    volumes:
      - netdataconfig:/etc/netdata
      - netdatalib:/var/lib/netdata
      - netdatacache:/var/cache/netdata
      - /:/host/root:ro,rslave
      - /etc/passwd:/host/etc/passwd:ro
      - /etc/group:/host/etc/group:ro
      - /etc/localtime:/etc/localtime:ro
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /etc/os-release:/host/etc/os-release:ro
      - /var/log:/host/var/log:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /run/dbus:/run/dbus:ro
    environment:
      - NETDATA_CLAIM_TOKEN=F5mGxmat59vjAUGs3_Jg_fWSp2TnehwLkbEEbdhlCJuMUPG2SWXTYK-Ztn0nbJewr12s8sHATTzbeviFG8YM3EqBW-xlt0WQ1BmjRmjN3Lf2w6K-A6egDySQRZsE8K-J_qhY4SM
      - NETDATA_CLAIM_URL=https://app.netdata.cloud
      - NETDATA_CLAIM_ROOMS=2263066d-2ed8-4720-9aba-ff1aab8a7d9b
volumes:
  netdataconfig:
  netdatalib:
  netdatacache:
```
```
docker compose up -d
```
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20152019.png?raw=true)

**Access at http://localhost:19999**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20152858.png?raw=true)

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151154.png?raw=true)

**Monitor CPU, memory, disk, Docker containers**
* CPU Usage
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151509.png?raw=true)

* Memory Usage
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151536.png?raw=true)

* Disk Usage
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151622.png?raw=true)

* Docker Containers
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151434.png?raw=true)

**Explore alerts and chart panels**

* Alerts
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151719.png?raw=true)

* Charts
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20152210.png?raw=true)

**Explore logs in /var/log/netdata**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151936.png?raw=true)

## Interview Questions:

**1. What does Netdata monitor?**
Netdata is a highly granular, real-time monitoring tool that tracks the health and performance of systems, hardware, containers, and applications.

* System/Hardware: CPU utilization, RAM usage, swap memory, disk I/O (read/write latency), and network interfaces (bandwidth, dropped packets).

* Infrastructure: Virtual machines, Docker containers, and Kubernetes clusters.

* Applications & Services: Web servers (Nginx, Apache), databases (MySQL, PostgreSQL, Redis), message brokers (Kafka, RabbitMQ), and custom application metrics.
It does this automatically, collecting thousands of metrics per second out-of-the-box.

**2. How do you view real-time metrics?**

You can view real-time metrics primarily through the Netdata Web Dashboard.
By default, the Netdata daemon runs a lightweight web server on port 19999. You can access it by navigating to http://<your-server-ip>:19999 in a web browser. Alternatively, if your nodes are connected to Netdata Cloud, you can view and aggregate metrics across multiple nodes in real-time through their centralized cloud interface.

**3. How is Netdata different from Prometheus?**

While both are excellent monitoring tools, they serve different primary use cases and have different architectures:

* Resolution & Setup: Netdata collects metrics every second (1s resolution) with zero configuration and auto-detects most services. Prometheus typically scrapes metrics every 10–15 seconds and requires manual configuration (targets, exporters) and a separate visualization tool (like Grafana).

* Architecture (Push vs. Pull): Netdata uses a distributed architecture where each node collects and stores its own metrics (though it can stream them). Prometheus relies on a central server that pulls (scrapes) metrics from endpoints.

* Focus: Netdata is built for high-fidelity, immediate troubleshooting of a node. Prometheus is designed for massive-scale, long-term aggregation, and complex querying (PromQL) across distributed systems.

**4. What is a collector?**

In Netdata, a collector (often referred to as a plugin or module) is a lightweight script or program responsible for gathering metrics from a specific source. For example, there is a MySQL collector, a Docker collector, and a Nginx collector. These collectors auto-detect running services, extract their performance data, and feed it into the core Netdata daemon to be stored and visualized.

**5. What are some performance KPIs to watch?**

While it depends on the specific workload, some universal Key Performance Indicators (KPIs) to monitor include:

* CPU: iowait (indicates the CPU is waiting on slow disk storage) and steal time (in VMs, indicates the hypervisor is taking resources).

* Memory: Available RAM and swap usage (high swap usage indicates you are out of physical memory, severely degrading performance).

* Disk I/O: Disk latency/await time and IOPS (Input/Output Operations Per Second).

* Network: Dropped packets, errors, and saturated bandwidth.

* Application-specific: HTTP 5xx errors (web servers) or slow query rates (databases).

**6. How to deploy Netdata on a VM?**

Netdata provides a highly streamlined "kickstart" script that handles dependencies, package managers, and installation automatically. On most Linux distributions, you can deploy it by running a single command in the terminal:

Bash:
```
wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/netdata-kickstart.sh
```
Alternatively, it can be deployed as a Docker container or installed manually via standard package managers (apt, yum, pacman).

**7. How does Netdata alerting work?**

Netdata features an advanced, built-in health watchdog.

* It ships with hundreds of pre-configured health alarms for common issues (e.g., running out of disk space, high CPU usage).

* The system evaluates metrics continuously against thresholds defined in .conf files.

* When a threshold is breached, the alarm changes state (from CLEAR to WARNING or CRITICAL).

* Netdata then pushes notifications to configured channels like Email, Slack, PagerDuty, Discord, or webhooks.

**8. What is a dashboard in this context?**

In the context of Netdata, the dashboard is the interactive, web-based graphical user interface (GUI). It visualizes the thousands of collected metrics using dynamic charts and graphs. The dashboard is structured hierarchically (System -> Applications -> Network, etc.), allows users to pan and zoom through time natively, and updates automatically every second without requiring a page refresh.