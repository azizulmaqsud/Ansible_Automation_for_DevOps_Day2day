# Why Ansible is a MUST Now?
- Ansible is an open-source tool that automates application deployment, intra-service orchestration, and cloud provisioning with complex automation to support your project requirements.
- It is simple to set up and use. No special coding skills are necessary to use Ansible’s playbooks. 
- It is Powerful: Ansible lets you model even highly complex IT workflows. 
- It is Flexible: You can orchestrate the entire application environment no matter where it’s deployed. You can also customize it based on your needs.
- It is Agentless: NO need to install any other software or firewall ports on the client systems you want to automate. You also don’t have to set up a separate management structure. 
- It is Efficient: Because you don’t need to install any extra software, there’s more room for application resources on your server.

## Ansible Architecture 
- Ansible has a management node to control the entire execution of the playbook and also run the installation
- Ansible has an inventory file with the list of hosts to run the Ansible modules 
- The management node has an SSH connection to execute the modules on the host's machine and install the product/software

## Ansible SETUP
- Ansible can be run from any machine with Python2 (versions 2.6 or 2.7) or Python 3 (versions 3.5 and higher) installed.
- Note − Windows does not support a control machine.

# Ansible INSTALLATION
- Add a user on each machine named, for example, 'Ansible'
- Configure SSH login between two servers, control, and remotes, without a password
- Install Ansible: 

[root@ansible-control~] # yum install -y ansible

ssh root@servera useradd ansible passwd ansible ssh-keygen
cd /etc/suders.d/

Machine − A physical server, VM (virtual machine), or a container. 

Target machine − A machine we are about to configure with Ansible. 

Task − An action (run this, delete that), etc. managed by Ansible.

Playbook − The YML file where Ansible commands are written and YML is executed on a machine. 

Ansible.cfg – ansible configuration file 

Inventory File – a file that contains all the remote ansible nodes

# Ad hoc Commands
- ansible <host-pattern> -m <module-name> -a “<module-command>”
- Transferring file to many servers/machines
- $ansible abc -m copy -a "src = /etc/yum.conf dest = /tmp/yum.conf”

# Creating new directory
- $ ansible abc -m file -a "dest = /path/user1/new mode = 777 owner = user1 group = user1 state = directory”

# Transferring file to many servers/machines
- $ Ansible abc -m copy -a "src = /etc/yum.conf dest = /tmp/yum.conf"

# The following command checks if yum package is installed or not, but does not update it.
- $ Ansible abc -m yum -a "name = demo-tomcat-1 state = present"

# Ansible YAML Tags
- name
- hosts
- vars
- tasks

# Ansible – Playbooks
- Playbooks are one of the core features of Ansible and tell Ansible what to execute.

## Why Ansible role is important?
- Role provides an interdependent collections of variables, tasks, files, templates, and modules.
- Role is the primary mechanism for breaking a playbook into multiple files. It simplifies writing complex playbooks
- Each role is assigned to a particular functionality or desired output

## How many Ansible Roles?
tasks – contains the main list of tasks to be executed by the role.

handlers – contains handlers, which may be used by this role or even anywhere outside this role.

defaults – default variables for the role.

vars – other variables for the role. Vars has the higher priority than defaults.

files – contains files required to transfer or deployed to the target machines via this role.

templates – contains templates which can be deployed via this role. 

meta – defines some data/information about this role (author, dependency, versions, examples, etc,.)
 
## Creating a Role Directory
- $ mkdir roles
- $ cd roles
- $ ansible-galaxy role init tomcat

A role for tomcat is created successfully

## Ansible Playbook to Copy A File
---
- name: ply to collect host info

  hosts: servera,serverb,serverc,serverd become: true

  user: devops tasks:

  - name: collect host info

    copy:

       content: "{{ ansible_hostname }} {{ ansible_processor_count }} {{ansible_default_ipv4.address }} {{ ansible_default_ipv4.macaddress }}"
  dest: /root/hostinfo.txt

## Download artifact and unzip it
---
-	name: play to download the the jar file from jfrog and unarchive 

	hosts: devops

	become: true 

	user: azizul

	tasks:

	 - name: create a folder 

	   file:

		path: /var/deploy 

		state: directory

	 - name: download the tart

	   get_url:

	   url: https://artifactory.com/flipcart.zip 

	   dest: /var/tmp/

	 - name: uzipz

	   command: unzip -o /var/tmp/flipcart.zip -d /var/deploy

## ANSIBLE TAGS SCENARIO TO DEPLOY
---
      - name: play to deploy files in grouped servers hosts: all

	become: true 

	user: azizul

	tasks:

	- name: create a tar file

	  command: tar cfz /var/tmp/production.tar.gz /var/www/html 

	  when: inventory_hostname in groups['production’]

	  name: create a tar file

	  command: tar cfz /var/tmp/backup.tar.gz /var/log/httpd 

	  when: inventory_hostname in groups['backup']

## Install APACHE and configure the files using ansible modules

      - name: play to install apache 

	hosts: devops

	become: true 

	user: azizul 

	tasks:

	  - name: Install http package 

	    yum:

	    name: httpd 

	    state: present

	  - name: Download httpd.conf 

	  get_url:

	     url: https://artifact/azizul/httpd.conf.j2 

	     dest: /etc/httpd/conf/httpd.conf

	     force: yes

	  - name: create index.html 

	     lineinfile:

		path: /var/www/html/index.html

		line: "Hello from {{ ansible_hostname }}" 

		create: yes

	   - name: start and enable httpd 

	   service:

	   name: httpd

	   state: started 

	   enabled: true


## Ansible VAULT

 ansible-playbook unarchive.yml \ --vault-password-file=.file1 File - unarchive.yml

## Thank You!
# Stay Connected !!
https://www.youtube.com/channel/UCNwP7KEElaJ7cdDTLP-KbBg

https://www.linkedin.com/in/azizul-maqsud/

https://azizulmaqsud-1684501031000.hashnode.dev/

https://medium.com/@azizulmaqsud

https://twitter.com/Sohail2me

https://github.com/azizulmaqsud
