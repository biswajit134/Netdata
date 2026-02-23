# TASK 7: Monitor System Resources Using Netdata
* Objective: Install Netdata and visualize system and app performance metrics.
* Tools: Netdata (free, open-source monitoring tool), Docker
* Deliverables: Screenshot of the dashboard and running metrics

## Mini-Guide (Hints):

**Run via Docker: docker run -d --name=netdata -p 19999:19999 netdata/netdata**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20150523.png?raw=true)

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151034.png?raw=true)
To connect the docker containers in Netdata Dashboard, we need to create docker-compose.yml file and run it "docker compose up -d":
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

**Access at http://localhost:19999**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20152858.png?raw=true)
![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151154.png?raw=true)

**Monitor CPU, memory, disk, Docker containers**


![image_url]()
![image_url]()
![image_url]()
![image_url]()
![image_url]()
**Explore alerts and chart panels**
![image_url]()
![image_url]()
![image_url]()
![image_url]()
![image_url]()

**Explore logs in /var/log/netdata**
![image_url]()
![image_url]()
![image_url]()
![image_url]()
![image_url]()