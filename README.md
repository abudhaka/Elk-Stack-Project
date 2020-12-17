## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![https://github.com/abudhaka/Elk-Stack-Project/blob/main/](Images/Project_1.2_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml files may be used to install only certain pieces of it, such as Filebeat.

- elk.yml
- filebeat-plybook.yml
- metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly availability, in addition to restricting Denial-of-Service (DoS) attack to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network traffic and system CPU, memory, and load.
- Filebeat monitors the log files or locations that you specify, as well as collects log events, and forwards them either to Elasticsearch or Logstash.
- Metricbeat monitors and analyzes system CPU, memmory and load.

The configuration details of each machine may be found below.

| Name          | Function   			     | IP Address          | Operating System |
|---------------|------------------------------------|---------------------|------------------|
| Jump Box      | Gateway                            | 10.0.0.4            | Linux            |
| Web 1 Server  | Process & deliver web pages        | 10.0.0.5            | Linux            |
| Web 2 Server  | Process & deliver web pages        | 10.0.0.6            | Linux            |
| ELK Server    | Log data & monitor network traffic | 10.1.0.4            | Linux            |
| Load Balancer | Distribute network traffic         | 104.42.75.65        | Linux            |
| My Computer   | Log data & monitor network traffic | author's IP address | Windows          |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer machine can accept connections from the Internet. Access to this machine is allowed from an author's personal IP address.

Machines within the network can only be accessed by SSH with authenticated SSH keys.  And, in the current setting, a SSH access to ELK Server is allowed only from Jump Box which has an IP address of 10.0.0.4.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible| Allowed IP Addresses|  
|--------------|--------------------|---------------------|
| Jump Box     | No                 | author's IP address (port 22) access only|
| Web 1 Server | No                 | 10.0.0.4:22, 104.42.75.65:80, author's IP address (port 80 for testing purpose)|    	
| Web 2 Server | No                 | 10.0.0.4:22, 104.42.75.65:80, author's IP address (port 80 for testing purpose)|   	
| Load Balancer| Yes		    | author's IP address (port 80)|
| ELK Server   | Yes                | 10.0.0.4:22, 10.0.0.5:9200 (Elasticsearch), 10.0.0.5:5601 (Kibana), 10.0.0.6:9200 (Elasticsearch), 10.0.0.6:5601 (Kibana), author's IP address (port 5601)|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible can significantly reduce maintenance overheads and performance degradation. Ansible is simple to learn with easily understandable Python language.

The playbook implements the following tasks:
- Install docker on on Ubuntu
- Install package installer for Python3
- Install docker python module with pip
- Increase max_map_count parameter to result an error when starting ElasticSearch
- Download and launch a docker container for ELK Server

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the following files to the /etc/ansible directory.
  - [filebeat-playbook.yml](https://github.com/abudhaka/Elk-Stack-Project/blob/main/Ansible/filebeat-playbook.yml)
  - [filebeat-config.yml](https://github.com/abudhaka/Elk-Stack-Project/blob/main/Ansible/filebeat-config.yml)
  - [metricbeat-playbook.yml](https://github.com/abudhaka/Elk-Stack-Project/blob/main/Ansible/metricbeat-playbook.yml)
  - [metricbeat-config.yml](https://github.com/abudhaka/Elk-Stack-Project/blob/main/Ansible/metricbeat-config.yml)

- Update the default ansible hosts file to include 10.1.0.4 IP address. This hosts file specifies to the ansible playbook to install and configure ELK Server on 10.1.0.4 IP address and install Filebeat and Metricbeat on the two web servers.

- Run the playbook, and navigate to the Filebeat or Metricbeat installation page on the ELK server GUI on Kibana website (http://[ELK public IP address]:5601/app/kibana) to check that the installation worked as expected.  In this implementation, 104.211.28.24 is the public IP address of ELK Server.

The specific commands the user will need to run to download the playbook, update the files, etc.
- sudo docker container list --all
- sudo docker start <name of the docker container>
- sudo docker attach <name of the docker container>
- cd /etc/ansible
- ansible-playbook elk.yml
- ansible-playbook filebeat-playbook.yml
- ansible-playbook metricbeat-playbook.yml
