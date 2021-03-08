## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.


![](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/Elk-Project.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat. ELk Server Setup


# Jumpbox and Web VM's Setup
- Install ansible in jump box VM. edit the config file to allow remote_user to azadmin and update hosts file in jump box ansible container /etc/ansible/hosts under [webservers] with Web VM IPs.
- Create a new ssh key and copy ssh public key (ida_rsa.pub)
- update public key with username to webservers reset password page in Azure portal
- SSH to Webservers (Web-1, Web-2, Web-3) and make sure u got access
- install docker in jump box and pull ansible container from (sudo docker pull cyberxsecurity/ansible)
- Launch ansible container (docker run -ti cyberxsecurity/ansible:latest bash)
- start and attach the container using (sudo docker attach contained name) you will see root prompt (root@211212323#)
- check /etc/ansible folder for contents open hosts file add ip address of web servers under[webservers]
- update /etc/ansible/install_webservers.yml to install docker,phyton, DVWA on webservers (see install_webservers.yml)
- run playbook using this command (ansible-playbook install_webservers.yml)
- If Successful you will see like attached Image (dvwa_sucess.jpg)

 ![DVWA Success](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/DVWA_Success.PNG)

# Setup Elk Server
- SSh to the jump box
- modify /etc/ansible/hosts file to add new group [Elk].and add ELK-Server IP Address
- Run install_Elk.yml playbook (see below install_Elk.yml)
- Once it is successful, you will see something similar to dvwa_sucess.jpg. and u can access Kibna using URL - http://Kiban's_Public_IP:5601/app/kibana
- 
![Filebeat_Sucess.jpg](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/Kibana.JPG)

- Setup/Config Filebeat and Metricbeat on Webservers
- SSH to jumpbox
- create/download filebeat-config.yml in /etc/ansible/files as per Filebeat_config attached below.
- create/download filebeat-playbook.yml in /etc/ansible/roles as per filebear_playbook.yml attached below check all tasks as required.
- Run Playbook using (ansible-playbook filebeat-playbook.yml), if install successfully you will see as this image as below
- Repeat the same steps to install Metricbeat, download Metricbeat-config.yml and metricbeat-playbook file and modify as required. see attached metricbeat-config.yml and metricbeat-playbook.yml files below.

![Filebeat_Sucess.jpg](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/Installtion.PNG)

-Create/Edit Ansible Config file to allow remote_user to azadmin.

**Refrence files**

[Install_Elk](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/Install_Elk.yml)

[Filebeat_Playbook](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/Filebeat_Playbook.yml)

[Metricbeat_Playbook](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/Metricbeat_Playbook.yml)

[Filebeat_Config](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/filebeat-config.yml)

[Metricbeat_config](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/metricbeat-config.yml)

[Ansible Host File](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/hosts.txt)
# This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

**The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.**
-Load balancing ensures that the application will be high availability, in addition to restricting access to the network.

**What aspect of security do load balancers protect? What is the advantage of a jump box?**
-In this cloud computing world Load Balancer plays inmportent role in security, it protects organisations fro DDoS attacks. Load Balancer provides  High availability.
-Jumpbox with ansible allows managing servers from a single point using SSH (access control) to do automated tasks (automation), we use jump box with ansible in this project to install DVWA, filebeat and Metricbeat also installed Elk-server in different vNet subnet. Jump box will give a more secure way of automating installations.

**Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Data and system Logs.**
- What does Filebeat watch for?--- Filebeat is a lightweight agent which collects system Logs from target servers and forward them to Elastic Search or logstash.
- What does Metricbeat record?  -- Metricbeat is a lightweight agent which collects Metric Logs such as OS metrics like CPU, Memory, network usage from target servers and forward them to Elastic search or logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name       | Function        | IP Address | OS     |
|------------|-----------------|------------|--------|
| Jumpbox    | Gateway Ansibel | 10.2.0.9   | Ubuntu |
| Web-1      | WebServer       | 10.2.0.7   | Ubuntu |
| Web-2      | WebServer       | 10.2.0.8   | Ubuntu |
| Web-3      | WebServer       | 10.2.0.11  | Ubuntu |
| ELK-Server | Kibana          | 10.0.0.4   | Ubuntu |

### Access Policies

- The machines on the internal network are not exposed to the public Internet.
 - Only the Jumpbox machine on port 22 SSH and ELK-Server on port 5601 can accept connections from the Internet. Access to these machines is only allowed from the following IP addresses:
- allowed workstation's public IP in NSG allow rules Source IP.
 - Machines within the network can only be accessed by a jump box machine.
- Which machine did you allow to access your ELK VM? What was its IP address? 
  -Elk VM can access the internal network from the jump box on 10.2.0.9 with SSH 22 and can be accessed on the internet via port 5601 from the workstation.


A summary of the access policies in place can be found in the table below.

| Name          	| Publicly Accessible  	| Allowed IP Addresses               	|
|---------------	|----------------------	|------------------------------------	|
| JumpBox       	| Yes                  	| SSH From Workstation Public IP     	|
| Web-1         	| No                   	| Only SSH from Jumpbox              	|
| Web-2         	| No                   	| Only SSH from Jumpbox              	|
| Web-3         	| No                   	| Only SSH from Jumpbox              	|
| ELK-Server    	| Yes                  	| 5601 From Workstation Public IP    	|
| Load Balancer 	| Yes                  	| Port 80 From Workstation Public IP 	|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?  Ansible uses playbooks to deploy multiple apps, and do setup and configure on multiple machines at the same time without any manual input. in playbook, we can write tasks that can be performed in target machines. this is a more secure and effective way.

The playbook implements the following tasks:
-In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.

# This task to install docker.io
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present
  # This task to install Phyton3
    - name: Install pip3
      apt:
        name: python3-pip
        state: present
  # This task to increase virtual memory
 - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

  #- This task to download and launch a docker elk container
      docker_container:
        name: elk
	 pip:
        name: docker
        state: present

  # This task to use more memory
    - name: Use more memory
      sysctl:
       name: vm.max_map_count
       value: '262144'
       state: present
       reload: yes

  # This task to download and launch docker elk container
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044

  # This task to enable docker service on boot
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes

- The following screenshot displays the result of running "docker ps" after successfully configuring the ELK instance.


![ELK_Docker_PS.JPG](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/ELK_Docker.JPG)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-01 -- 10.2.0.7
- Web-02 -- 10.2.0.8
- Web-03 -- 10.2.0.11


We have installed the following Beats on these machines:
- Web-01 -- 10.2.0.7
- Web-02 -- 10.2.0.8
- Web-03 -- 10.2.0.11
-Elk-Server- 10.0.0.4 (installed only Elk)

These Beats allow us to collect the following information from each machine:
-In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

--We installed filebeat and Metricbeat on all 3 webservers. Filebeat collects system logs and sends it to Elastic Search or logstash for indexing. For each log that Filebeat locates, Filebeat starts a harvester. Each harvester reads a single log for new content and sends the new log data to libbeat, which aggregates the events and sends the aggregated data to the output that you’ve configured for Filebeat.
-Example for filebeat
SSH Login' as we doing jump box to login to all 3 web VMs using SSH we can see the log activity in kibana under Dashboard. it will show you how many successful login attempts and failed attempts

![Filebeat_Sucess.jpg](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/Kibana_SSH%20Log.JPG)

Metricbeat collects operating system metrics like CPU, memory usage, network usage from web servers and sends it to Elastic Search or logstash. -Example for metricbeat 'System Overview' in this example we can see all systems and running processes and resources usage like how much memory it is used in peek usage, how much CPU app is using any bottlenecks exists? this will allows you to optimise servers performance. please see attched Pic for metricbeat.


![Filebeat_Sucess.jpg](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/Kibana_Metricbeat.JPG)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Setup Elk-Server
 - SSh to jumpbox
  - modify  /etc/ansible/hosts file to add new group [Elk] and add ELK-Server IP Address
  - Run install_Elk.yml playbook (ansible-playbook install_Elk.yml)
  - Once it is sucessfull you will see as below.
  -ELK-Server Success

- Setup Filebeat and Metricbeat
  - Download filebeat config file from https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat >  /etc/ansible/files/filebeat-config.yml
  - Modify filebeat-config file specify ELK Server ip address as below
 ** output.elasticsearch:
  hosts: ["10.0.0.4:9200"]
  username: "elastic"
  password: "changeme” **

  setup.kibana:
  host: "10.0.0.4:5601"

  - Copy File from '/etc/ansible/files/filebeat-config.yml' to '/etc/filebeat/filebeat.yml' using playbook tasks
  -  create filebeat.playbook,yml file as below. and create tasks like filebeat.deb download, install filebeat, copy filebeat config file to /etc/filebeat/filebeat.yml, enable system module,setup filebeat, start filebeat service and enable filebeat service on reboot.

  -[Filebeat_Playbook](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/Filebeat_Playbook.yml)
  
  -Run filebeat using this comand from jumpbox (ainsible-playbook filebeat-playbook.yml)
  - once successfull u can see as below

   -![Filebeat Success](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/images/DVWA_Success.PNG)

  - To test the successfull installation of fiebeat  go to Kibana (http://Elk-server_public_ip:5601/app/kibana) : Add log data > System logs > Module Status > Check data and go to dashboard to see log data from webservers.

  -- Repeat same steps to install Metricbeat, download Metricbeat-config.yml and metricbeat-playbook file  and modify as required. see attached metricbeat-config.yml and metricbeat-playbook.yml files below.

[Metricbeat_Config](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/metricbeat-config.yml)

[Metricbeat_Playbook](https://github.com/AK1975/Project-1--ELK-Stack-Deployment-/blob/main/Metricbeat_Playbook.yml)

- after successful metricbeat installation navigate to kibana and click on metric data under metrics > system metrics >> module status > click check data and go to the dashboard to see log data.

_Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
 _ .yml file is playbook and when installing filebeat or metricbeat copy in jump box /etc/ansible/files/filbeat-config.yml file to target machines /etc/filebeat/filebeat.yml using playbook tasks.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus  which to install Filebeat on?
  _ we need to modify /etc/ansible/hosts file and create groups for [webservers] and other group [Elk]
- _Which URL do you navigate to in order to check that the ELK server is running? 
- http://Kiban's_Public_IP:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

- Download filebeat or metricbeat
 - use get-url or curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
- Install filebeat command 
 - dpkg -i /etc/filebeat/filebeat-7.4.0-amd64.deb
- Drop in filebeat.yml
 - copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

- enable and configure system module 
 -name: enable and configure system module
    service:
      name: filebeat
      enabled: yes
      state: started
- Setup Filebeat
  - name: setup filebeat
    command: filebeat setup -e
- Start filebeat
  - name: start filebeat service
    service:
      name: filebeat
      state: started
- To Enable Filebeat on boot
  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes

# Ubuntu Commands we used in Jumpbox
  - sudo apt-get update
-   sudo apt install docker.io
-   sudo service docker start
-   systemctl status docker
-   systemctl status docker
-   sudo docker pull cyberxsecurity/ansible
-   sudo docker run -ti cyberxsecurity/ansible bash
-   sudo docker contaner list -all
-   sudo docker start container_name
-   sudo docker attach container_name
-   sudo ssh-keygen
-   cat ida_rsa.pub
-   ssh azadmin@web-01_Ip
-   sudo ansible -m ping all





# Additional Resouces
- [Ansible playbook documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#playbooks-intro)
- [Kibana Tutorial ](https://www.youtube.com/watch?v=gQ1c1uILyKI)
- [Filebeat Container Documentation](https://www.elastic.co/beats/filebeat)
- [Docker Commands Cheat Sheet](https://phoenixnap.com/kb/list-of-docker-commands-cheat-sheet)


