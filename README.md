# Project1Submission

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Project Diagram ELK Stack.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

- name: Configure Elk VM with Docker
  hosts: webservers
  remote_user: sysadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _____, in addition to restricting _____ to the network.
- Reliable, traffic

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- Files
- Metrics

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Web App  | 10.0.0.5   | Linux            |
| Web-2    | Web App  | 10.0.0.6   | Linux            |
| ELK Stack| Container| 10.0.0.7   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Jump Box, 174.53.156.107

Machines within the network can only be accessed by _____.
- Protocol was SSH, access was from Jump Box machine

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 174.53.156.107       |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |
| Load Bal | Yes                 | All IP Addresses     |
| Elk      | Both                | 10.0.0.4/All IP for Elk panel |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Scalability 

The playbook implements the following tasks:
- Install docker.io
- Instal pip3
- Install Docker python module 
- set memory usage with sysctl
- download adn launch elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.  --wasn't in instructions of setup

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 and Web-2, 10.0.0.5 and 10.0.0.6

We have installed the following Beats on these machines:
- filebeat and metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat looks for file changes, and metric beat monitors the metric that we set i.e. cpu usage

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- elk playbook, copied into the /etc/ansible/ folder
- update the ansible.cfg file to allow sysadmin user access, and then change the hosts file to add the IP of the elk server
- _Which URL do you navigate to in order to check that the ELK server is running?  The IP of the elk: 5601

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml

dpkg -i filebeat-7.4.0-amd64.deb

ansible-playbook filebeat-playbook.yml
