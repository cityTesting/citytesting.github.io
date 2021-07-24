---
layout: post
title: Run Teamcity in Ubuntu
categories: [Teamcity]
---

In this post we are going to show how to install a Teamcity instance using Ubuntu.

**Prerequisites:**
* Ubuntu machine.

**Steps**
* Download TeamCity distribution [here](https://www.jetbrains.com/teamcity/download/#section=on-premises): 
  ```
  wget -i https://download.jetbrains.com/teamcity/TeamCity-2021.1.1.tar.gz
  ```
* Extract the file:
  ```
  tar xyz TeamCity-2021.1.1.tar.gz
  ```
  
* Now run the following command:
  ```
  cd Teamcity
  ./bin.runAll.sh start
  ```
* After a while Teamcity will be installed in our machine with the default configurations. For more information visit the link below.
  

Useful links:
* [Teamcity on premises installation guide.](https://www.jetbrains.com/help/teamcity/installing-and-configuring-the-teamcity-server.html).
