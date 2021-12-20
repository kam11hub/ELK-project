# ELK-project
This repo provides an in-depth analysis and description of how I deployed an ELK stack server within a virtual network. 


## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/kam11hub/Cybersecurity/blob/main/Diagrams/Azure%20Cloud%20Diagram.drawio.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be distributed between the available Web servers, in addition to restricting inbound access to the network by connecting only through the Jump Box VM.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data logs and system metrics.

The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operating System  |
|----------|------------|------------|-------------------|
| Jump Box | Gateway    | 10.0.0.4   | Linux             |
| DVWA 1   | Web Server | 10.0.0.5   | Linux             |
| DVWA 2   | Web Server | 10.0.0.6   | Linux             |
| DVWA 3   | Web Server | 10.0.0.7   | Linux             |
| ELK      | Monitoring | 10.1.0.4   | Linux             |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
- 20.118.33.131

Machines within the network can only be accessed by each other.

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses |
|-----------|---------------------|----------------------|
| Jump Box  | Yes                 | 55.118.7.146         |
| ELK       | No                  | 10.1.0.1-254         |
| DVWA 1    | No                  | 10.0.0.1-254         |
| DVWA 2    | No                  | 10.0.0.1-254         |
| DVWA 3    | No                  | 10.0.0.1-254         | 

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because this made deployment faster and more efficient.

The playbook implements the following tasks:

∙ Configures the VM to use more memory so that it will be able to run. Specifically sets the vm.max_map_count to 262144

∙ installs apt packages:
-	docker.io - an engine used for running containers
-	python3-pip - Python software

∙ installs pip packages:
-	docker - Python client for Docker

∙ Downloads the Docker container called sebp/elk:761. Sebp is the organization that created the container. Elk is the name of the container and 761 is the version. 

∙ Container starts with these port mappings:
-	5601:5601
-	9200:9200
-	5044:5044 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/kam11hub/ELK-project/blob/main/Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DVWA Web 1 – 10.0.0.5
- DVWA Web 2 – 10.0.0.6
- DVWA Web 3 – 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to examine log events such as Apache or audit logs.  It detects changes within the filesystem. Metricbeat monitors the metrics such as the server’s CPU usage, SSH login attempts, RAM statistics, and sudo escalations. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:
- Copy the playbook file to the Ansible Control Node (The jump-box has the docker container and ansible installed to run these playbooks).

Open your git. 

- $ cd /etc/ansible
- $ mkdir files
# Clone Repository + IaC Files
- $ git clone https://github.com/yourusername/ELK-project.git
# move playbooks and hosts file into /etc/ansible 
- $ cp ELK-project/playbooks/*
- $ cp project ELK-project/* ./files



# Create and update the hosts file to include the target VMs...

- $ cd /etc/ansible
- $ nano hosts 

[webservers]

- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

[elk]

- 10.1.0.4

 

- Run the playbook, and navigate to its URL to check that the installation worked as expected.

$ cd /etc/ansible
$ ansible-playbook install_elk.yml elk
$ ansible-playbook filebeat-playbook.yml webservers
$ ansible-playbook metricbeat-playbook.yml webservers

Give ELK a few minutes to boot up to verify success.

Run curl http://10.1.0.4:5601. This is the address of Kibana. The HTML will show on the console if installed correctly.  



