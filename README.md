# ELK-Stack-Deployment
The files in this repository can be used to automatically deploy an ELK stack.

![Elk Stack Deployment](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Diagrams/ELK%20Deployment%20Network%20Diagram.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above of the ELK stack and Beats on web servers. Alternatively, select portions of the [Ansible files](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/tree/main/Ansible) may be used to install only certain pieces of it, such as Filebeat.
_Note: this repo only covers the deployment of ELK to an existing server and installing & configuring Beats on existing Web Servers in an Azure cloud network._

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

The configuration details of each machine in the Azure resource group may be found below.

Name | Function | IP Address | Operating System |
----- | ---- | ---- | ---- |
Jump-Box-Provisioner | Gateway | 10.1.0.4 | Linux (ubuntu 20.04) |
ELK | Runs ELK stack for monitoring | 10.2.0.4 | Linux (ubuntu 20.04) |
Web-1 | Runs DVWA | 10.1.0.5 | Linux (ubuntu 20.04) |
Web-2 | Runs DVWA | 10.1.0.6 | Linux (ubuntu 20.04) |
DVWA-VM3 | Runs DVWA | 10.1.0.7 | Linux (ubuntu 20.04)|


## Access Policies
The machines on the internal network are protected by Network Security Groups and are not exposed to the public Internet.
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

The [install-elk.yml playbook](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/install-elk.yml) implements the following tasks:

* Configures the target VM to use more memory so that ELK runs properly
* Installs docker.io & Python
* Installs the Python client for Docker
* Downloads the Docker container from sebp that has ELK v. 761
* Publishes port connections 5601:5601, 9200:9200, and 5044:5044 to allow for connections and monitoring

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
![docker ps result after running install-elk.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Diagrams/dock_ps_output.png)

## Target Machines & Beats
This ELK server is configured to monitor the following machines:

* 10.1.0.5 (Web-1)
* 10.1.0.6 (Web-2)
* 10.1.0.7 (DVWA-VM3)

The following Beats have been installed on these machines:

* Filebeat
* Metricbeat

These Beats allow us to collect the following information from each machine:

### Filebeat

Filebeat monitors the log files on a machine, collecting the log events and sending them to a centralized location. You can explore this log information in a tool such as Kibana.

### Metricbeat

Metric monitors the Operating System and service metrics on a machine. It captures information such as CPU or memory usage, as well as load and networking metrics.

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned, 
SSH into the control node and follow the steps below:

1. Copy the [install-elk.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/install-elk.yml) file to your /etc/ansible directory.

2. Update the /etc/ansible/hosts file to include a group called [elk] with the IP address of your intended ELK server and specifying the use of python3 as shown below:
* 10.2.0.4 ansible_python_interpreter=/usr/bin/python3

3. Use the following command to run the playbook:

_ansible-playbook install-elk.yml_

4. To confirm that everything has been correctly configured, navigate to http://20.127.133.95:5601/app/kibana to check that the installation worked as expected. You should see the Kibana main page load.

## Configuring and installing the Beats
1. Open the /etc/ansible/hosts file. Make sure that the target machine(s) you wish to install Beats on are included in the file under the section [webservers]. Uncomment #[webervers] if necessary and save the file. Remember to specify the use of python3. For the deployment pictured above, the hosts file was edited as following:

_[webservers]
_10.1.0.5 ansible_python_interpreter=/usr/bin/python3_
_10.1.0.6 ansible_python_interpreter=/usr/bin/python3_
_10.1.0.7 ansible_python_interpreter=/usr/bin/python3_

2. Once the hosts file is verified and saved, make a new directory in /etc/ansible called Files. Copy the Filebeat and Metricbeat config and playbook files to this directory.

* [filebeat-playbook.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/filebeat-playbook.yml)
* [filbeat-config.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/filebeat-config.yml)
* [metricbeat-playbook.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/metricbeat-playbook.yml)
* [metricbeat-config.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/metricbeat-config.yml)

3. In the [filbeat-config.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/filebeat-config.yml) file, update the IP address so that it matches the private IP address (if it does not match the IP for the Jump-Box-Provisioner in this repo) for the machine where your Ansible control node is configured in the following spots:

_Line 1106_ : hosts: ["your.private.IP.address:9200"]

_Line 1806_ : host: "your.private.IP.address:5601"

Save the changes to the filebeat-config.yml file.

4. Run the [filebeat-playbook.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/filebeat-playbook.yml) using the command:

_ansible-playbook filebeat-playbook.yml_

This should install and configure Filebeat on your target machine(s).

5. In the [metricbeat-config.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/metricbeat-config.yml) file, update the IP address so that it matches the private IP address for the machine where your Ansible control node is configured in the following spots:

_Line 62_ : host: "you.private.IP.address:5601"

_line 95_ :   hosts: ["your.private.ip.address:9200"]

Save the changes to the metricbeat-config.yml file.

6. Run the [metricbeat-playbook.yml](https://github.com/MauraSullivan-mx/ELK-Stack-Deployment/blob/main/Ansible/Files/metricbeat-playbook.yml) using the command:

_ansible-playbook metricbeat-playbook.yml_

This should install and configure Metricbeat on your target machine(s).

Congratulations! You just deployed an ELK stack and configured two Beats on target machines. You should be able to explore Log and Metric dashboards in your Kibana homepage that you connected to in the previous session.
