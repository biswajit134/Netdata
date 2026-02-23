# TASK 7: Monitor System Resources Using Netdata
* Objective: Install Netdata and visualize system and app performance metrics.
* Tools: Netdata (free, open-source monitoring tool), Docker
* Deliverables: Screenshot of the dashboard and running metrics

## Mini-Guide (Hints):

**Run via Docker: docker run -d --name=netdata -p 19999:19999 netdata/netdata**

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20150523.png?raw=true)

![image_url](https://github.com/biswajit134/Netdata/blob/main/SS/Screenshot%202026-02-23%20151034.png?raw=true)
* To connect the docker containers in Netdata Dashboard, we need to create docker-compose.yml file and run it """docker compose up -d"""
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