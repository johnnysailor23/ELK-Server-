# ELK-Server-
Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml and filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.
This document contains the following details:

Description of the Topology

The topology is a hybrid of fully connected devices and a ring topology. This is because the devices are reliant upon a SSH Key to communicate back and forth across devices and outside of the local network. 

Access Policies

As mentioned before, the access policy is a privilage based policy segmented by the Network Security Groups, Resource Groups, and the Load Balancer to ensure integrity and confidentiality of information within the network. The Load Balancer and the Network Security Group allows or denies the inbound and outbound traffic and the jumpbox allows access to the other devices on the two networks via ssh. The Load Balancer and ELK Server also connect to the internet via HTTP to output information. 

ELK Configuration

The ELK Server was implemented first by creating a seperate network with the following elements: 

A new Virtual Network within the same resource group

A new Peering between the two Virtual Networks, RedTeam vNET and ELK Server vNET

A new VM within the ELK Server vNET

Added the IP of the Elk Server to the hosts file within the ansible container 

Created and ran an ansible playbook that installs docker and configures the ELK Container

Restricted access to the ELK VM by implementing inbound and outbound rules to the NSG

Beats in Use

Filebeat and Metricbeat was used to monitor log files and other files on the systems for metrics and security

Machines Being Monitored

Web-1,2,3 are the machines monitored by the ELK Server

How to Use the Ansible Build

The ansible build is installed first using the ansible command to install ansible. After this, I modified the cfg. file to allow access to the ansible container by replacing the remote user to the admin name created by me. I also modified the hosts file to allow access via the IP addresses of Web-1,2, and 3. 

Description of the Topology/Purpose 

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly , in addition to restricting to the network.

Loadbalancers ensure segmentation of network devices that could potentially be vulnerable to attack such as IoT devices and webservers. Loadbalancers also "balance the load" of a network by monitoring the usage of data across a network and best distributing it out in a fast and effective manner similar to QoS. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the and system .

Filebeat is a logging program that watches changes in files on a system such as when and how they were changed. Filebeat is typically used for watching changes to logiles and other system files. It must be specifically installed on each system that you want to monitor. 

Metricbeat records services and metrics on a device to give you better insight into how well a website is performing or a specific server/server stack. 

The configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.

| Name          | Function          | IP Address              | Operating System |
|---------------|-------------------|-------------------------|------------------|
| Jump Box      | Gateway           | 20.62.195.37            | Linux Ubuntu     |
| Web-1         | Web Server        | 10.1.0.8                | Linux Ubuntu     |
| Web-2         | Web Server        | 10.1.0.9                | Linux Ubuntu     |
| Web-3         | Web Server        | 10.1.0.7                | Linux Ubuntu     |
| ELK Server    | DVWA Host Server  | 10.2.0.4                | Linux Ubuntu     |
| Load Balancer | Segregate devices | 137.117.57.108 10.1.0.8 | Linux Ubuntu     |

Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
89.38.227.142

Machines within the network can only be accessed through the JumpBox VM

All devices contained within the resource group can access the ELK VM.

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses                    |
|------------|---------------------|-----------------------------------------|
| Jump Box   | Yes with SSH        | 89.38.227.142                           |
| ELK Server | No                  | 20.62.195.37                            |
| Web-1      | No                  | 10.1.0.4,10.1.0.7,10.1.0.9,20.62.195.37 |
| Web-2      | No                  | 10.1.0.4,10.1.0.7,10.1.0.8,20.62.195.37 |
| Web-3      | No                  | 10.1.0.4,10.1.0.8,10.1.0.9,20.62.195.37 |






Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it is easier to install and configure the ELK Server using a playbook instead of potentially getting a syntax error and uploading it using ansible allows us to connect the machines as well so the ELK Server can start logging immediately. 

The playbook implements the following tasks:
Install Docker.io
Install Python3
Use Pip to install docker
Increase virtual memory 
Use more memory
Download and Launch Docker
Enable Docker service on boot

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
![image](https://user-images.githubusercontent.com/69168545/119198074-c2051980-ba56-11eb-9d2c-1df7b2e4f8da.png)

Target Machines & Beats

This ELK server is configured to monitor the following machines:

Web-1-10.1.0.8,Web-2-10.1.0.9,Web-3-10.1.0.7

We have installed the following Beats on these machines:

Metricbeats and Filebeats have been installed on the ELK Server to monitor these machines. 

These Beats allow us to collect the following information from each machine:

Filebeats allows us to monitor changes to any files on our webservers, specifially log and config files incase there are any changes in permissions or harmful data added to these files. Metricbeats allows us to see how activity seen by our webservers.

Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Copy the file to /etc/ansible

Update the hosts file to include an uncommented [elk] with the IP address of the server and ansible_python_interpreter=/usr/bin/python3

Run the playbook, and navigate to the ELK Server and run sudo docker ps to check that the installation worked as expected.

The install-elk.yml is the playbook to run to install elk and you run it using ansible-playbook install-elk.yml commmand. This file should be located in the /etc/ansible directory.

Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
The hosts file seperates the webservers and the ELK Server. In the playbook file install-elk.yml, the elk server is chosen from the hosts file. In the filebeat-config.yml file, the webservers are chosen to receive the playbook commands.

_Which URL do you navigate to in order to check that the ELK server is running?
To verify that the ELK Server is running after installing filebeats, navigate to http://[your.VM.IP]:5601/app/kibana
