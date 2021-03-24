Automated ELK Stack Deployment
*The files in this repository were used to configure the network depicted below.

https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Diagrams/diagram%20for%20elk%20stack.PNG

 
*These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project 1 file may be used to install only certain pieces of it, such as Filebeat.
 
https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Ansible/filebeat-playbook.yml
https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Linux/filebeat-configuration.yml
 
*This document contains the following details:
Description of the Topology
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Dmn Vulnerable Web Application.
Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.
What aspect of security do load balancers protect?
It helps prevent overloading servers as well as optimizes productivity and maximizes uptime.
It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attacks.
What is the advantage of a jump box?
-A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments. Over the last few years, with malicious hackers and malware infesting nearly every enterprise network at will, security admins have been looking for a way to decrease the ability of hackers or their malware creations to steal admin credentials and take over an environment and the concept of a traditional “jump box” has morphed into an even more comprehensive and locked-down “secure admin workstation” (or SAW).
*Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
What does Filebeat watch for?
Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
What does Metricbeat record?
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

*The configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.
Name
Function
IP Address
Operating System
JumpBoxProvisioner
Gateway
10.0.0.4(Private)//52.146.45.27(Public)
Linux
ELK-VM
Server
10.1.0.4(Private)//23.99.8.60(Public)
Linux
Web-1
Server
10.0.0.5(Private)//40.88.124.36(Public)
Linux
Web-2
Server
10.0.0.6(Private)//40.88.124.36(Public)
Linux

Access Policies
*The machines on the internal network are not exposed to the public Internet.
Only the jump-Box-Provisioner machine can accept connections from the Internet.
Access to this machine is only allowed from the following IP addresses:
73.223.214.142(LocalHost IP address)
*Machines within the network can only be accessed by JumpBoxProvisioner
Which machine did you allow to access your ELK VM?
JumpBoxProvisioner
What was its IP address?
10.0.0.4 (Private)//52.146.45.27(Public)
A summary of the access policies in place can be found in the table below.
Name
Publicly Accessible
Allowed IP Addresses
JumpBoxProvisioner
Yes
73.223.214.142
ELK-SERVER*
No
10.0.0.4
Web-1*
No
10.0.0.4
Web-2*
No
10.0.0.4

 
Elk Configuration
*Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it’s very simple to set up and use: No special coding skills are necessary to use Ansible's playbooks 

*The playbook implements the following tasks:
In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
SSH into the JumpBoxProvisioner (ssh azadmin@52.146.45.27)
Start then attach the Ansible docker (sudo docker start dazzling_diffie) then (sudo docker attach dazzling_diffie)
Go to /etc/ansible/roles directory and create the ELK playbook (myplaybook-elk.yml)
Ran the myplaybook-elk.yml in that same directory (ansible-playbook myplaybook-elk.yml)
Then, SSH into the ELK-SERVER to verify the server is up and running.
*The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Images/sudo%20docker%20ps.PNG
 
Target Machines & Beats
*This ELK server is configured to monitor the following machines:
List the IP addresses of the machines you are monitoring
Web-1 (10.0.0.5)
Web-2 (10.0.0.6)
We have installed the following Beats on these machines:
https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Images/filebeat%20data%20success.PNG
https://github.com/Gnarly-Quinn/ElkStackProject/blob/main/Images/metricbeat%20data%20success.PNG
*These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc._
Filebeat is used to collect log files from specific files on remote machines.The azure module retrieves different types of log data from Azure. Some of these include: activitylogs, platformlogs, signinlogs, and auditlogs.
Metricbeat consists of modules and metricsets. A Metricbeat module defines the basic logic for collecting data from a specific service, such as Redis, MySQL, and so on. The module specifies details about the service, including how to connect, how often to collect metrics, and which metrics to collect.
Using the Playbook
*In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:
---Filebeat---
Copy the filebeat-configuration.yml file to /etc/ansible/roles/files.
Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.
Run the playbook, and navigate to http://23.99.8.60:5601/ (ELK-SERVER public IP) to check that the installation worked as expected.
---Metricbeat---
Copy the metricbeat-configuration.yml file to /etc/ansible/roles/files.
Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.
Run the playbook, and navigate to http://23.99.8.60:5601/ (ELK-SERVER public IP) to check that the installation worked as expected.
Answer the following questions to fill in the blanks:
Which file is the playbook? filebeat-playbook.yml
Where do you copy it? /etc/ansible/roles
Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts file.
How do I specify which machine to install the ELK server on versus which to install Filebeat on? I have to specify two separate groups in the etc/ansible/hosts file. One of the groups will be web servers with the IPs of the VMs that I will install Filebeat to. The other group is named elk servers which will have the IP of the VM I will install ELK to.
Which URL do you navigate to in order to check that the ELK server is running? http://23.99.8.60:5601/
As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
 -------Filebeat---------

- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.

- To create the playbook: nano filebeat-playbook.yml

  ---
 - name: installing and launching filebeat
	   hosts: webservers
       become: true
       tasks:

	   - name: download filebeat deb
  	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

	   - name: install filebeat deb
  	     command: dpkg -i filebeat-7.7.1-amd64.deb

	   - name: drop in filebeat.yml
  	     copy:
   	       src: ./files/filebeat-configuration.yml
   	       dest: /etc/filebeat/filebeat.yml

	   - name: enable and configure system module
  	     command: filebeat modules enable system

	   - name: setup filebeat
  	     command: filebeat setup

	   - name: start filebeat service
  	    command: service filebeat start
---
-To run the playbook: ansible-playbook filebeat-playbook.yml

* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml


-------Metricbeat-------

- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.

- To create the playbook: nano metricbeat-playbook.yml

---
  - name: installing and lunching metricbeat
    hosts: webservers
    become: true
    tasks:
    
  - name: download metricbeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
    
  - name: install metricbeat deb
    command: sudo dpkg -i metricbeat-7.7.1-amd64.deb
    
  - name: drop in metricbeat.yml
    copy:
      src: /etc/ansible/roles/files/metricbeat-configuration.yml
      dest: /etc/metricbeat/metricbeat.yml
      
   - name: enable and configure system module
     command: metricbeat modules enable system
     
   - name: setup metricbeat
     command: metricbeat setup
     
   - name: start metricbeat service
     command: service metricbeat start
     
   ---
   
   - To run the playbook: ansible-playbook metricbeat-playbook.yml
   
   * To order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/metricbeat-p

