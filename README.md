# ELK-Stack-Deployment
The files in this repository can be used to automatically deploy an ELK stack.

Note: The following image link needs to be updated. Replace diagram_filename.png with the name of your diagram image file.

![Elk Stack Deployment](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Diagrams/ELK%20Deployment%20Network%20Diagram.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the [Ansible files](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/tree/main/Ansible) may be used to install only certain pieces of it, such as Filebeat.

* [Filebeat Playbook](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/filebeat-playbook.yml)
* [Metricbeat Playbook](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/metricbeat-playbook.yml)

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

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system OS & service metrics.

The configuration details of each machine may be found below.

Name | Function | IP Address | Operating System |
----- | ---- | ---- | ---- |
Jump-Box-Provisioner | Gateway | 10.1.0.4 | Linux (ubuntu 20.04) |
ELK | Runs ELK stack for monitoring | 10.2.0.4 | Linux (ubuntu 20.04) |
Web-1 | Runs DVWA | 10.1.0.5 | Linux (ubuntu 20.04) |
Web-2 | Runs DVWA | 10.1.0.6 | Linux (ubuntu 20.04) |
DVWA-VM3 | Runs DVWA | 10.1.0.7 | Linux (ubuntu 20.04)|


## Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

* 73.229.49.154

Machines within the network can only be accessed by SSH connection from the Ansible container in the Jump-Box-Provisioner machine, IP address 10.1.0.4.

A summary of the access policies in place can be found in the table below.

Name | Publicly Accessible | Allowed IP Addresses |
---- | ---- | ---- |
Jump Box | Yes | 73.229.49.154 |
ELK | No | 10.1.0.4 |
Web-1 | No | 10.1.0.4 |
Web-2 | No | 10.1.0.4 |
DWVA-VM3 | No | 10.1.0.4 |


## Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this means that the entire configuration can be deployed again within minutes, using the provided [Ansible files](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/tree/main/Ansible).

The [Install-Elk playbook](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/install-elk.yml) implements the following tasks:

TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
* here
* here
* here
* here
* here

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
![docker ps result after running install-elk.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Diagrams/dock_ps_output.png)

## Target Machines & Beats
This ELK server is configured to monitor the following machines:

* 10.1.0.5 (Web-1)
* 10.1.0.6 (Web-2)
* 10.1.0.7 (DVWA-VM3)

We have installed the following Beats on these machines:

* Filebeat
* Metricbeat

These Beats allow us to collect the following information from each machine:

### Filebeat

Filebeat monitors the log files on a machine, collecting the log events and sending them to a centralized location. You can explore this log information in a tool such as Kibana.

### Metricbeat

Metric monitors the Operating System and service metrics on a machine. It captures information such as CPU or memory usage, and load and networking metrics.

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

Copy the [install-elk.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/install-elk.yml) file to your /etc/ansible directory.
Update the /etc/ansible/hosts file to include a group called [elk] with the IP address of your intended ELK server and specifying using python3. See below example:
* 10.2.0.4 ansible_python_interpreter=/usr/bin/python3

Use the following command to run the playbook:

_ansible-playbook install-elk.yml_

To confirm that everything has been correctly configured, navigate to http://20.127.133.95:5601/app/kibana to check that the installation worked as expected.

