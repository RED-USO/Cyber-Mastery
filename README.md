# ELK STACK PROJECT
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![ELK Cloud](https://github.com/RED-USO/Cyber-Mastery/blob/7fff0157d485854a8526b4faddc67fcfebfc80bb/Diagrams/Red-Net%20Cloud%20With%20Elk.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

* [Install ELK](https://github.com/RED-USO/Cyber-Mastery/blob/722cf1f153e85e157b3e50b1832db1283de02a48/Ansible/install-Elk.jpg)
* [DVWA](https://github.com/RED-USO/Cyber-Mastery/blob/7577f0a0bca0ca6f01d6fae47f015062e2090900/Ansible/DVWA%20Setup%20yml%20script.jpg)

This document contains the following details:

* Description of the Topology
* Access Policies
* ELK Configuration 
  * Beats in Use
  * Machines Being Monitored
* How to Use the Ansible Build

## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

* Security-wise a load balancer can have integrated intrusion prevention and web application firewall services to add another layer of protection against DDos attacks.  The load balancer is meant to reroute live traffic from one server to another if a server is hit with a DDoS attack or becomes unavailable.
* The advantage of a jump box is that it’s a secured computer that only admins first connect to before they do any administrative tasks.  It can also serve as a gateway to gain entry into a remote network.
            
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

* Filebeat monitors the log files or locations that are specified.
* Metricbeat collects metrics and statistics, from the OS, and sends them to a specified location.

The configuration details of each machine may be found below.

Name	Function | IP Address | Operating System
-------------- | ---------- | ----------------
Jump-Box VM	Gateway | 10.1.0.4 | Linux
Web1 | Webserver | 10.1.0.6 | Linux
Web2 | Webserver | 10.1.0.5 | Linux
ELK | Elk Server | 10.2.0.5 | Linux

## Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump-Box VM machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

* Access to Jump-Box VM is only allowed from my public IP.
      
Machines within the network can only be accessed by the Jump-Box VM.

* The Jump-Box VM private IP 10.1.0.4 was only allowed access to the ELK VM, which is accessed through the ELK’s private IP 10.2.0.5.
 
A summary of the access policies in place can be found in the table below.

Name | Publicly Accessible | Allowed IP Addresses
---- | ------------------- | --------------------
Jump-Box VM | SSH - 22 - YES | Personal Public IP
Web1 & Web2 | NO | 10.1.0.4 & Red-Team LB IP: 52.183.4.37
Red-Team LB | HTTP - 80 - YES | Personal Public IP
ELK | SSH - 22 – YES & HTTP - 80 - YES | 10.1.0.4, 10.1.0.5, 10.1.0.6 Personal Public IP

## Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

* Ansible has portable playbooks, YAML and non-YAML scripts, which can be used anywhere as well as repeatable.
* Ansible can easily track containers along with knowing what’s in them incase you need to rebuild them.
* Ansible can maintain the environment in which all the containers are in, even high complex environments.

The playbook implements the following tasks:

* Installs Docker Engine used for running containers
* Installs python3-pip which is the package used to install the Python software
* Installs Docker module which is a Python client for Docker and will default to pip3
* Increase Memory to the VM
* Configures the container to start with specific port mappings
* Enables the docker service on boot which if you restart the ELK VM, the docker service will automatically start up
          
The following screenshot displays the result of running `sudo docker ps -a` after successfully configuring the ELK instance.

![Docker ps](https://github.com/RED-USO/Cyber-Mastery/blob/cd77a00750f4b536a3b94799ea05de7b780f0eb1/Ansible/Docker%20ps%20-a.jpg)
 
## Target Machines & Beats

This ELK server is configured to monitor the following machines:

* Web1: 10.1.0.6
* Web2: 10.1.0.5

We have installed the following Beats on these machines:

* Filebeat
* Metricbeat

These Beats allow us to collect the following information from each machine:

* Filebeat and Metricbeat have been installed on both Web1 and Web2 servers.
* Filebeat collects system type events such as who’s actively logging into the system.
  * [Filebeat Log Example in Kibana](https://github.com/RED-USO/Cyber-Mastery/blob/3c2fb2d0196f3e5e44fb4050d731d6aca4d5257b/Images/Filebeat%20Log%20Pic.jpg)
* Metricbeat collects useful information such as cpu usage and memory. This is useful when there are programs or behaviors taking system resources.
  * [Metricbeat CPU Usage in Kibana](https://github.com/RED-USO/Cyber-Mastery/blob/3c2fb2d0196f3e5e44fb4050d731d6aca4d5257b/Images/Metricbeat%20Metric%20Pic.jpg)

## Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

* Copy the configuration file to the Ansible container
* Update the hosts file to include the Web1, Web2 and ELK VM IP addresses
* Run the playbook, and navigate to http://[public IP address of ELK VM]/app/kibana to check that the installation worked as expected.
      
* The playbook file is called filebeat-playbook.yml and you copy it to /etc/ansible/roles/ directory.
* You update the filebeat-config.yml to make Ansible run the playbook on a specific machine.
* The URL you navigate to, in order to check that the ELK server is running, is http://[public IP address of ELK VM]/app/kibana.


### What the User Will Need to Run ELK

* First ssh into Jump-Box VM `ssh Admin@52.158.231.29(Jump-Box Public IP)`
  * Jump-Box command prompt should look like this example: **`Admin@Jump-Box-Provisioner`**
* Start your Ansible Container `sudo docker start example_docker`
* Next attach into your Ansible Container `sudo docker attach example_docker'
  * Ansible Container command prompt should look like this example: **`root@6160a9be360e`**
* Run command `cd /etc/ansible/` to enter the ansible directory and locate **install-Elk.yml**
* Locate the `hosts` file in `/etc/ansible/` and edit this file with command `nano hosts`
![Host File Example](https://github.com/RED-USO/Cyber-Mastery/blob/cf4e70ec95a84db2b0730b41b511b5fa8fd87f44/Ansible/Hosts%20File%20Example.jpg)
* To run the Playbook run command `ansible-playbook install-Elk.yml`
* Verify you can access your server by inputting `http://[your_elk_server_ip]:5601/app/kibana` into your browser
  * You should see something similar to this:
![Kibana Success](https://github.com/RED-USO/Cyber-Mastery/blob/b31c3b5e6bda058f23477d00a9c367cd7ea44b9c/Images/Kibana%20Successful.jpg)

### What the User Will Need to Run Filebeat

* First ssh into Jump-Box VM `ssh Admin@52.158.231.29(Jump-Box Public IP)`
  * Jump-Box command prompt should look like this example: **`Admin@Jump-Box-Provisioner`**
* Start your Ansible Container `sudo docker start example_docker`
* Next attach into your Ansible Container `sudo docker attach example_docker'
  * Ansible Container command prompt should look like this example: **`root@6160a9be360e`**
* Run command `cd /etc/ansible/roles` to enter the roles directory and locate **filebeat-playbook.yml**
  * The filebeat-playbook.yml should look similar to this:
![filebeat-playbook](https://github.com/RED-USO/Cyber-Mastery/blob/eeb85eb7d54e7369078505f94de716c23e54b3ac/Ansible/filebeat-playbook%20yml%20script.jpg)
* To run the Playbook run command `ansible-playbook filebeat-playbook.yml`
* Verify in kibana that it was successful
  * You should see something similar to this:
![Filebeat Success](https://github.com/RED-USO/Cyber-Mastery/blob/e5a7aa22da81b56c09329a8860bf195a572dabe5/Images/Filebeat%20Successfull%20Data.jpg)

### What the User Will Need to Run Metricbeat

* First ssh into Jump-Box VM `ssh Admin@52.158.231.29(Jump-Box Public IP)`
  * Jump-Box command prompt should look like this example: **`Admin@Jump-Box-Provisioner`**
* Start your Ansible Container `sudo docker start example_docker`
* Next attach into your Ansible Container `sudo docker attach example_docker'
  * Ansible Container command prompt should look like this example: **`root@6160a9be360e`**
* Run command `cd /etc/ansible/roles` to enter the roles directory and locate **metricbeat-playbook.yml**
  * The metricbeat-playbook.yml should look similar to this:
![metricbeat-playbook](https://github.com/RED-USO/Cyber-Mastery/blob/eeb85eb7d54e7369078505f94de716c23e54b3ac/Ansible/metricbeat-playbook%20yml%20script.jpg)
* To run the Playbook run command `ansible-playbook metricbeat-playbook.yml`
* Verify in kibana that it was successful
  * You should see something similar to this:
![Metricbeat Success](https://github.com/RED-USO/Cyber-Mastery/blob/e5a7aa22da81b56c09329a8860bf195a572dabe5/Images/Metricbeat%20Successfull%20Data.jpg)
