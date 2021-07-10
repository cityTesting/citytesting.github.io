---
layout: post
title: Run Teamcity with docker
categories: [Teamcity]

In this post we are going to show how to run Teamcity using docker, and how to start playing with it.

---
**Prerequisites:**
* Docker installed.
* Any linux system.

**Steps**
* Create teamcity folders to save data and logs: 
  ```
  mkdir teamcity
  cd teamcity
  mkdir data
  chmod 777 /data
  mkdir logs
  chmod 777 /logs
  ```
* Run the following docker command: 
  ```
  docker run -d -it --name teamcity-server-instance  -v /teamcity/data:/data/teamcity_server/datadir -v /teamcity/logs:/opt/teamcity/logs -p 8111:8111 jetbrains/teamcity-server
  ```
  

Useful links:
* [Teamcity docker image](https://hub.docker.com/r/jetbrains/teamcity-server/).
