---
layout: post
title: How create your own vpn using a VPS server
categories: [Vpn]
---
**Prerequisites:**
* VPS server (Ubuntu 20.04).

**Steps**
* Connect to the VPS
  For this example we will use an Ubuntu machine to connect an Ubuntu 20.04 VPS server(it is similar with other OS):  
  ```
  ssh  user@vps_name
  ```
  After typing the password we will be connected to the server.

* Install Openvpn server
  Run the following commands to install the Openvpn server in the VPS:
  ```
  apt update && apt -y install ca-certificates wget net-tools gnupg
  wget -qO - https://as-repository.openvpn.net/as-repo-public.gpg | apt-key add -
  echo "deb http://as-repository.openvpn.net/as/debian focal main">/etc/apt/sources.list.d/openvpn-as-repo.list
  apt update && apt -y install openvpn-as
  ```
  After that we should see a message similar to this:
  
  ```
  Please enter "passwd openvpn" to set the initial
  administrative password, then login as "openvpn" to continue
  configuration here: https://xxx.xxx.xxx.xxx:943/admin

  To reconfigure manually, use the /usr/local/openvpn_as/bin/ovpn-init tool.

  +++++++++++++++++++++++++++++++++++++++++++++++
  Access Server 2.9.2 has been successfully installed in /usr/local/openvpn_as
  Configuration log file has been written to /usr/local/openvpn_as/init.log


  Access Server Web UIs are available here:
  Admin  UI: https://xxx.xxx.xxx.xxx:943/admin
  Client UI: https://xxx.xxx.xxx.xxx:943/
  +++++++++++++++++++++++++++++++++++++++++++++++
```
  
  
* Configure the Openvpn server
* Connect to the vpn using a client


Useful links:
* [How to install Openvpn from Openvpn site](https://openvpn.net/vpn-software-packages/).
