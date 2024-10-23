## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(ELK_network_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-configuration.yml or metricbeat_configuration.yml 
file may be used to install only certain pieces of it.

elk-playbook.yml
filebeat-configuration.yml
filebeat-playbook.yml
metricbeat-configuration.yml
metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.5   | Linux            |
|DVWA-VM1  |          | 10.0.0.6   | Linux            |
|DVWA-VM2  |          | 10.0.0.7   | Linux            |
|ELKProject|          | 10.0.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-72.177.57.80

Machines within the network can only be accessed by the virtual network and my home IP address.
-the ELK server can only be accessed by other machines in the vnet or my home IP: 72.177.57.80

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Vnet and 72.177.57.80|
| DVWA-VM1 | No                  | Vnet 		|
| DVWA-VM2 | No                  | Vnet 		|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
Ansible and containers enables implementation of the configuration remotely over ssh and from command line (kali linux in this case).

The elk-playbook.yml implements the following tasks:
- Install Docker
- Install Python-pip
- Install Docker python module
- Increase virtual memory 
- Download and launch Docker container named "elk" 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

ELK-container.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DVWA-VM1: 10.0.0.6
- DVWA-Vm2: 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
Filebeat monitors system logs and will provide info like Syslog events by hostname and Syslog events and processes by VM
Metricbeat monitors docker metrics and will provide info like # of Containers and CPU usage


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control (JumpBox) node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-configuration.yml/metricbeat-configuration.yml file to /etc/ansible/files .
- Update the above config files to include the private IP of the ELKProject VM (10.0.0.8)
- Copy the filebeat-playbook.yml/metricbeat-playbook.yml to /etc/ansible/roles
- Update the playbooks to refer back to the filepath for the config files. 
- Run the playbooks, and navigate to ELKProject VM Public IP to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ filebeat-playbook.yml/metricbeat-playbook.yml copy to /etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine? Update /etc/ansible/hosts file and 10.0.0.6 and 10.0.0.7 to 'webservers'section
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ Update /etc/ansible/hosts and add the 'elkservers' section with ip address 10.0.0.8
- Which URL do you navigate to in order to check that the ELK server is running? ELKProject public IP: 104.42.8.157:5601

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
ssh to jumpbox
sudo docker container start beautiful_mcnulty
sudo docker attach beautiful_mcnulty

inside the beautiful-mcnulty container:
cd /etc/ansible
ansible-playbook pentest.yml

ansible-playbook elk-playbook.yml

nano hosts: add private IP of DVWA-VM1 and DVWA-VM2 to 'webservers'
ansible-playbook pentest.yml

curl localhost/setup.php to make sure the webserver is up and 'exit'

ssh RedTeam@10.0.0.7 (DVWA-VM2)
and curl localhost/setup.php

ping public IP for DVWA to check if it is up, use login: RedTeam

ssh RedTeam@10.0.0.8  **elk container**

set NSG rule in Azure to access Elk VM over 5601 from home IP

test by visiting Elk "**publicIP":5601

eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

back in ansible:
update filebeat-configuration.yml/metricbeat-configuration.yml to include private IP address of ELKProject VM (10.0.0.8) and save in /etc/ansible/files
update filebeat-playbook.yml/metricbeat-playbook.yml to refer back to /etc/ansible/files directory for the respective configuration files

ansible-playbook filebeat-playbook.yml
ansible-playbook metricbeat-playbook.yml

I
