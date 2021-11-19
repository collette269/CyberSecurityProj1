## SHW PROJECT 1 – RICE UNIVERSITY, CYBERSECURITY BOOTCAMP

Written by Stephanie Wagner 11.20.2021

## Table of Contents
This document contains the following:

-	Purpose of this project
-	Create Virtual Network in Azure
-	Description of the Topography
-	Access Policies
-	Overview of Installation Process
-	What is Docker?
-	What is DVWA? 
-	What is Ansible?
-	What is Elk Stack?
-	Elk Installation and Configuration
-	Install Filebeats
-	Install Metricbeats

## Purpose of this project. 

Design a virtual network for cyber security students with DVWA and ELK stack deployment. 

The goal of this project is to create a virtual network for cyber security students to practice web vulnerabilities based on the cybersecurity founding principles of the CIA Triad: Confidentiality, Integrity and Availability. We will be using the NIST Management Framework: Prepare, Categorize, Select, Implement, Assess, Authorize, Monitor in our policies and procedures. 

![Triad Diagram](https://user-images.githubusercontent.com/88781846/142481603-43ca3728-3d68-4a73-b4cf-c87f734945f7.png)

https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/NIST%20RMF.PNG

Tools: 
-	Microsoft Azure <https://portal.azure.com/>
-	Damn Vulnerable Web App(DVWA) <https://dvwa.co.uk/>
-	Ansible <www.ansible.com>
-	Docker <www.docker.com>
-	Elk Stack <www.elastic.co>

## Create Virtual Network in Azure
This virtual network utilizes Microsoft Azure Portal because of their easy to use graphical user interface and their cloud deployment capabilities to build, manage and monitor the system. This system could be replicated on other networks with different operating systems. 

NAME	FUNCTION	IP ADDRESS	OPERATING SYSTEM
JumpBox	Gateway	10.0.1.0	Linux(ubtntu 18.04)
Web-1	Server	10.0.1.5	Linux(ubtntu 18.04)
Web-2	Server	10.0.1.7	Linux(ubtntu 18.04)
Elk-VM	Elk - Server	10.1.0.4	Linux(ubtntu 18.04)

![ Virtual Network.PNG](https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/Virtual%20Network.PNG)

### Description of the Topology

This virtual network was created to expose a load-balancer and two virtual machine with instance of DVWA, the Damn Vulnerable Web Application. These virtual machines will be monitored with a separate virtual machine and the Kibana ELK Stack program. 

![SHW13DIAGRAM.png](Attached is a diagram of the Red Team Resource Group’s Topology.
https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/SHW13DIAGRAM.png)

### Access Policies

The user, Redadmin, is able to ssh into the jumpbox from the remote work station. The jumpbox includes ansible and docker deployments and is the central point from which updates are issued to the webserver and elk virtual machines.  Only the jumpbox can connect to the public internet. 


Name	Public Access	Allowed IP Addresses 
JumpBox	Yes	redadmin workstation via ssh
Web1	No	http access via port 80
Web2	No	http access via port 80
Elk Stack	No	Jumpbox via ssh & http port 5601, Kibana

![Rules.PNG](https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/Rules.PNG)

## Overview of Installation Process

Step 1: Install Docker.io
Step 2: Download DVWA
Step 3: Download Ansible docker container
Step 4: Start Ansible docker container
Step 5: Open interactive Docker Ansible Shell
Step 6: Create Ansible-Playbook for Elk VM
Step 7: Run Ansible-Playbook
Step 8: Create & Run Ansible-Playbook for Beats


##What is Docker? Container hub that distributes containers fast and easy to install complex server configurations. We will use Docker to install Ansible scripts to ensure they run exactly the same thing, every time. 

hub.docker.com

INSTALL DOCKER.IO
sudo apt install docker.io
sudo systemctl status docker
sudo systemctl start docker


## What is DVWA? 
Damn Vulnverable Web Application (DVWA) is an open sourced web application that is useful for cyber professionals, developers and students to practice web application security. 

sudo systemctl start docker

sudo docker pull cyberxsecurity/ubuntu:bionic

sudo docker exec -ti bionic/ubuntu bash

Run curl localhost/setup.php to test the connection

## What is Ansible? 
Ansible is an open source tool that allows you to automate network configuration updates and maintenance. This human readable, agentless architecture uses OpenSSH and WinRM is very easy to manage and not easy to exploit. In this system, we used Ansible’s play-books to automate configuration of the ELK machine. Controls on administrative duties simplify the process, reduce errors, increase performance and standardize procedures. 

sudo systemctl status docker

sudo systemctl start docker

sudo docker pull cyberxsecurity/ansible

sudo docker exec -ti cyberxsecurity/ansible

Ssh redadmin@jumbboxIP

sudo docker container list -a

sudo docker start _____________(container name)

sudo docker exec -ti ___________(container name)   /bin/bash

You should be in the container root user. 

cd /etc/ansible

nano hosts
  add >>> webservers IP address of web1 & web2 include “ansible_python_interpreter=/usr/bin/python3”

nano ansible.cfg  
   change >>> remote_user = YOUR_USER_NAME

More about the automation at: 
https://www.ansible.com/overview/it-automation

## What is ELK Stack? 
Elk Stack is an open sourced security monitoring and alerting service tool created and managed by Elastic Company. The tools are available on AWS through the Elastic Cloud. The Elastic Company has several data processing tools. For this project, we will be sending the data with their lightweight shipper Beats’ Filebeats for logs and Metricbeats for metrics and analyzing the data with Electricsearch’s Kibana visualization module. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic. 

Ansible-Playbook’s can be found in the Ansible Container, available from the jumpbox, in the directory /etc/ansible. 
What does Filebeat watch for?_Filebeat monitors file systems and records changes in log event files. For example, ssh login attempts and web requests. 

What does Metricbeat record? Metricbeat collects data from operating systems and services on servers. For example, CPU usage. 

For more information about the company and their resources: 
https://www.elastic.co/about/

### Elk Installation and Configuration 

Elk Installation Tasks: 

Ssh redadmin@jumbboxIP

sudo docker container list -a

sudo docker start _____________(ANSIBLE container name)

sudo docker exec -ti ___________(ANSIBLE container name)   /bin/bash

Now in the ANSIBLE container

cd /etc/ansible

touch /etc/ansible/install-elk.yml

nano install-elk.yml

To Run: ansible-playbook install-elk.yml

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
! [Elk Stack VM Docker PS.PNG](https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/Elk%20Stack%20VM%20Docker%20PS.PNG)

## Install Filebeats 

Filebeats will monitor WEB-1 & WEB2 Machines 

curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/filebeat-config.yml

In your ansible container:

cd /etc/ansible

touch /etc/ansible/filebeats.yml

nano install-elk.yml

To Run: ansible-playbook filebeats.yml

Confirm by : http://ELKSTACKVM:5601/app/kibana

![ Filebeats Dashboard.PNG](https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/Filebeats%20Dashboard.PNG)

## Install Metricbeats 

Metricbeats will monitor WEB-1 & WEB2 Machines 

curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/metricbeat-config.yml

In your ansible container:

cd /etc/ansible

touch /etc/ansible/metricbeats.yml

nano install-elk.yml

To Run: ansible-playbook metricbeats.yml

Confirm by : http://ELKSTACKVM:5601/app/kibana

![ Metricbeats Dashboard.PNG](https://github.com/collette269/CyberSecurityProj1/blob/main/Diagrams/Metricbeats%20Dashboard.PNG)
