# ELK STACK PROJECT
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

![Diagrams](/Diagrams/Red-Net Cloud With Elk.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

Install ELK

This document contains the following details:

      •	Description of the Topology
      •	Access Policies
      •	ELK Configuration 
            o   Beats in Use
            o   Machines Being Monitored
      •	How to Use the Ansible Build

## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

      •	Security-wise a load balancer can have integrated intrusion prevention and web application
            firewall services to add another layer of protection against DDos attacks.  The load balancer
            is meant to reroute live traffic from one server to another if a server is hit with a DDoS
            attack or becomes unavailable.
      •	The advantage of a jump box is that it’s a secured computer that only admins first connect to
            before they do any administrative tasks.  It can also serve as a gateway to gain entry into a
            remote network.
            
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

      •	Filebeat monitors the log files or locations that are specified.
      •	Metricbeat collects metrics and statistics, from the OS, and sends them to a specified location.

The configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.

Name	Function | IP Address | Operating System
-------------- | ---------- | ----------------
Jump-Box VM	Gateway | 10.1.0.4 | Linux
Web1 | Webserver | 10.1.0.6 | Linux
Web2 | Webserver | 10.1.0.5 | Linux
ELK | Elk Server | 10.2.0.5 | Linux

## Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump-Box VM machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

      •   Access to Jump-Box VM is only allowed from my public IP.
      
Machines within the network can only be accessed by the Jump-Box VM.

      •   The Jump-Box VM private IP 10.1.0.4 was only allowed access to the ELK VM, which is accessed
          through the ELK’s private IP 10.2.0.5.
 
A summary of the access policies in place can be found in the table below.

Name | Publicly Accessible | Allowed IP Addresses
---- | ------------------- | --------------------
Jump-Box VM | SSH - 22 - YES | Personal Public IP
Web1 & Web2 | NO | 10.1.0.4 & Red-Team LB IP: 52.183.4.37
Red-Team LB | HTTP - 80 - YES | Personal Public IP
ELK | SSH - 22 – YES HTTP - 80 - YES | 10.1.0.4, 10.1.0.5, 10.1.0.6 Personal Public IP

## Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

      •   Ansible has portable playbooks, YAML and non-YAML scripts, which can be used anywhere as well as repeatable.
      •   Ansible can easily track containers along with knowing what’s in them incase you need to rebuild them.
      •   Ansible can maintain the environment in which all the containers are in, even high complex environments.

The playbook implements the following tasks:

      •   Installs Docker Engine used for running containers
      •   Installs python3-pip which is the package used to install the Python software
      •   Installs Docker module which is a Python client for Docker and will default to pip3
      •   Increase Memory to the VM
      •   Configures the container to start with specific port mappings
      •   Enables the docker service on boot which if you restart the ELK VM, the docker service will
          automatically start up
          
The following screenshot displays the result of running sudo docker ps -a after successfully configuring the ELK instance.

Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.
 
## Target Machines & Beats

This ELK server is configured to monitor the following machines:

      •   Web1: 10.1.0.6
      •   Web2: 10.1.0.5

We have installed the following Beats on these machines:

      •   Filebeat
      •   Metricbeat

These Beats allow us to collect the following information from each machine:

      •   Filebeat and Metricbeat have been installed on both Web1 and Web2 servers.
      •   Filebeat collects system type events such as who’s actively logging into the system.
      •   Metricbeat collects useful information such as cpu usage and memory. This is useful when there
          are programs or behaviors taking system resources.
          
*TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.*

## Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

      •   Copy the configuration file to the Ansible container
      •   Update the hosts file to include the Web1, Web2 and ELK VM IP addresses
      •   Run the playbook, and navigate to http://[public IP address of ELK VM]/app/kibana to check that
          the installation worked as expected.
      
      •   The playbook file is called filebeat-playbook.yml and you copy it to /etc/ansible/roles/ directory.
      *Which file is the playbook? Where do you copy it?*
      •   You update the filebeat-config.yml to make Ansible run the playbook on a specific machine.
      *How do I specify which machine to install the ELK server on versus which to install Filebeat on?*
      •   The URL you navigate to, in order to check that the ELK server is running, is
          http://[public IP address of ELK VM]/app/kibana.
      
*As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.*
