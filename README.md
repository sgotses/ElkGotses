# ElkGotses
Fully functional ELK Stack server used to set up a cloud monitoring system.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/sgotses/ElkGotses/blob/main/Diagrams/Diagram.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

```
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start 

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build
```


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable/available, in addition to restricting traffic to the network.
- Load balancers help protect an organization against DoS attacks. 
- A jump box is a computer on a network typically used to access other devices on the network in another security zone. The jumpbox acts as a secure gateway to the corporate network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems and system metrics.
- Filebeat is a logging agent that watches the forwarding and centralization of log data on a machine.
- Metricbeat collects metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.8   | Linux            |
| Web1     |Web Server| 10.0.0.7   | Linux            |
| Web2     |Web Server| 10.0.0.6   | Linux            |
| ELK      |Monitoring| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 72.220.114.29

Machines within the network can only be accessed by the each other.
- The Jump Box (52.247.236.52) is the only machine that can access the Elk server.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |        Yes          | 72.220.114.29        |
| Web1     |        No           | 10.0.0.0/16          |
| Web2     |        No           | 10.0.0.0/16          |
| Elk      |        No           | 10.1.0.0/16          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Ansible allows IT administrators to automate daily tasks allowing the business to spend more time focusing on more important tasks. Ansible is also an open-source tool that is very simple to setup and use.

The playbook implements the following tasks:
- Install Docker
- Install Python3
- Install Docker Module
- Use more memory
- Download and launch a Docker Elk Container 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/sgotses/ElkGotses/blob/main/Screenshots/ELK.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1 10.0.0.7
- Web2 10.0.0.6

We have installed the following Beats on these machines:
- Metricbeat
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat detects changes to the filesystem. Specifically, we use it to collect Apache logs.
- Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook files to the ansible control node.
- Update the hosts file to include which VM's to run each playbook on.
- Run the playbook, and navigate to the url http://40.88.226.66:5601/app/kibana#/home to check that the installation worked as expected.
