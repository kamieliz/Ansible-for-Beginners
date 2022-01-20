# Ansible for Beginners

# Table of Contents

1. [Ansible](craftdocs://open?blockId=C7DC0473-576E-41EF-BC35-C8723245BA46&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   1. [Ansible Introduction](craftdocs://open?blockId=810898A5-6BC9-429C-8A8B-8BD568675248&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   2. [Set up Ansible local env.](craftdocs://open?blockId=107B911F-0C72-4E38-86DE-D2D572BDF697&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   3. [Install Ansible](craftdocs://open?blockId=E54DB5BC-1C24-44BC-8CE4-1C8C18E1A065&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
2. [Ansible Concepts](craftdocs://open?blockId=DC7DB2AB-FE4C-47E6-84D4-C3EC0DEA74DA&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   1. [Inventory](craftdocs://open?blockId=882B3537-E1A2-4E4E-8066-9CF393B3F784&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   2. [Playbooks](craftdocs://open?blockId=B7C7F663-226C-4A41-B8DF-082899771563&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   3. [Modules](craftdocs://open?blockId=46F1513E-A761-4EE2-B9A4-80149AAC1D3A&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   4. [Variables](craftdocs://open?blockId=0E35256C-AD01-492A-A788-3FDA84D96649&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
3. [Conditionals, Loops and Roles](craftdocs://open?blockId=CEC5893D-7EA9-4B38-8C9A-56A97CADF129&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   1. [Conditionals](craftdocs://open?blockId=7DB9C835-88AC-4D75-80BB-82D447B7127D&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   2. [Loops](craftdocs://open?blockId=CBB264FC-5C75-4646-BF7A-2F5D155226AF&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   3. [Roles](craftdocs://open?blockId=6F1EB8D2-C9C4-4F94-85AF-A1F7F1F77D91&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
4. [Advanced Topics](craftdocs://open?blockId=9BB3B1BE-05C5-418F-B3A7-3C53F5A1A4B9&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   1. [Preparing Windows Server](craftdocs://open?blockId=39F538EA-2403-4DBA-97F3-3FD45A3D4598&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   2. [Ansible-Galaxy](craftdocs://open?blockId=318DAD0F-F9C7-4131-BA10-CA858469E83B&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   3. [Patterns](craftdocs://open?blockId=57929DB8-305A-4EFA-859C-2C6A56307B21&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   4. [Dynamic Inventory](craftdocs://open?blockId=175F56AF-C8E5-4151-AE12-F94657CA6C28&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)
   5. [Developing Custom Modules](craftdocs://open?blockId=6731871E-367C-4AC1-A701-40BC77D3B523&spaceId=d367a179-adcb-7ce8-0b02-ba52d2a7c917)

# Ansible

## Ansible Introduction

- Ansible is good for repetitive tasks:
   - provisioning
   - configuration management
   - continuous delivery
   - application deployment
   - security compliance
- Ansible is a simple, powerful IT automation tool
- Playbooks are the most basic component of Ansible

## Set up Ansible local env.

- Creating VMs on VirtualBox using Centos 7
- Once boxes are set up login to them and use terminal to find their IP addresses (ifconfig)
- Change the hostname and hosts on each box
   - `sudo vi /etc/hostname`
   - `sudo vi /etc/hosts`
- to apply changes restart your system
   - `shutdown now -r`

## Install Ansible

- To install Ansible, check out the [installation guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- On Centos, you can use yum to install it
   - `sudo yum install epel-release`
   - `sudo yum install ansible`
- to confirm that Ansible was installed
   - `ansible —version`
- check the connectivity to target server
   - `ssh ip_address`
- check connectivity to ansible by creating a test project
   - `mkdir test-project`
   - `cd test-project/`
   - `cat > inventory.txt`
      - `target1 ansible_host=192.168.1.139 ansible_ssh_pass=osboxes.org`
   - `ansible target1 -m ping -i inventory.txt`

# Ansible Concepts

## Inventory

- Ansible can work with one or multiple systems in your infrastructure at the same time
   - in order to do this it must establish connectivity to those servers
   - this is done using ssh in linux and powershell remoting for windows
- to access the target machines, you need to create an inventory file or use the default
- default inventory file is located at /etc/ansible/hosts directory
- the inventory file is in an ini format and simply lists a number of servers one after another
   - can also group different servers by putting them under a group name in brackets
   - multiple groups can be defined in a single inventory file
- if you want ansible to refer to different servers by an alias
   - add the parameter ansible_host in front of the server name as well as the alias
      - `web ansible_host=server1.company.com`
   - this parameter is used to specify the FQDN or IP address of a server
- There are other inventory parameters:
   - **ansible_connection** - defines how ansible connects to the target server, options: ssh, winrm, localhost
   - **ansible_port** - defines which port to connect to, options: 22 (default - SSH) and 5986
   - **ansible_user** - defines the user used to make remote connections, options: admin, root (default for linux)
   - **ansible_ssh_pass** - defines ssh password for linux, may not be ideal to store password in plain text
      - use password less authentication for production environments - Ansible Vault
   - **ansible_password** - stores passwords for windows

## Playbooks

- Playbooks are ansibles orchestration language
- in playbooks we define what we want Ansible to do
- Playbook - a single yaml file
   - play - defines a set of activities (tasks) to be run on hosts
      - task - an action to be performed on the host
         - execute a command
         - run a script
         - install a package
         - shutdown/restart
- define the host your playbook runs on at the play level
- Each play is a dictionary with different properties
   - name
   - hosts
      - can have any host or group listed here as long as they are defined in the inventory file
      - if you specify a group all the tasks will be applied to the hosts simultaneously
   - tasks
      - tasks are a list (indicated by -) and here order does matter
      - contain ansible modules: command, script, service are examples of those
         - find more information using ansible-doc -l
   - order doesn’t matter when you switch those labels around
- to execute an ansible playbook
   - `ansible-playbook <playbook file name>`
   - `ansible-playbook —help`
      - for additional parameters

```other
-
	name: Play1
	hosts: localhost
	tasks:
	- name: Execute command 'date'
	  command: date
	- name: Execute script on server
	  command: test_script.sh
	- name: Install httpd service
	  yum:
       name: httpd
       state: present
	- name: start web server
	  service:
		name: httpd
		state: started
```

## Modules

Ansible modules are categorized into various groups based on their functionality

- **system** - actions to be performed at a system level
   - modifying users and groups
   - modifying iptables
   - firewall configurations
   - working with logical volume groups
   - mounting operations
   - working with services
- **commands** - used to execute commands or scripts on a host
   - command - simple command
   - expect - expect module by responding to prompts
   - raw
   - script - run a script on the host
   - shell
- **files** - help work with files
   - acl - set and retrieve acl info on files
   - archive/unarchive - to compress and unpack files
   - copy
   - file
   - find/replace/line in file - to modify the contents of an existing file
   - stat
   - template
- **database** - help in working with databases such as MongoDB, MySQL, MSSQL, or PostgreSQL
   - add or remove databases
   - modify database configurations
- **cloud** - modules for various cloud provides like Amazon, Azure, Docker, google, Openstack, etc.
   - perform tasks such as creating and destroying instances
   - performing configuration changes in networking and security
   - managing containers, datacenters, clusters, virtual networking, VSAN
- **windows** - for using ansible in a windows environment
   - win_copy - copy files
   - win_command - to execute a command
   - other modules available that create files, create a IIS website, install a software using MSI installer, and manage services and users in windows

### Detailed Modules

**command** - executes a command on a remote node

- parameters include:
   - chdir to change into a directory before running
   - creates - a filename or glob pattern if it already exists this step is skipped
   - executable - changes the shell used to execute the command
   - free_form - the command module takes a free form command to run
      - can not input parameters with this, not all modules support this
   - removes - a filename or glob pattern
   - warn - if command warnings exist do not warn about this particular line if set to no
   - parameters are set after the value
- to use create a key value pair, command is the key and the value is the actual command you want to run

**Script** - runs a local script on a remote node after transferring it

- located locally on the Ansible controller machine or one or more remote nodes
- ansible copies the script automatically to remote nodes before executing on remote systems
- can specify location of script
- arguments can be passed for the script as well

**Service** - manage services, start, stop, restart

- takes two parameters name of the service and desired state
- the desired state is in past tense
   - to ensure that the state has already taken place
   - an operation is idempotent if the result of performing it once is exactly the same as the result of performing it repeatedly without any intervening actions
- can write it inline or in dictionary map format

## Variables

- store information that varies with each host
- can have variables in the inventory files and in playbooks
- to store a variable in a playbook
   - use the vars module
   - variable name as the key and then set the value
- can set variables in their own separate file
- to use a variable in a playbook use double curly braces

# Conditionals, Loops and Roles

## Conditionals

- allows you to create one playbook that meets different conditions
- conditions - any check that you can run
   - ex: checking if the ansible_os_family variable is set to Debian
   - use == to check equality
   - or - one or the other condition is met
   - and - both conditions are met
- **when** - only if the condition is true that task is run
- conditionals can be used in a loop
   - specify loop directive under when conditional
   - `loop: “{{ package }}”`
- **Register** directive sets a variable to the output of the task
   - can use these with when to determine if the variable != -1 or is true

## Loops

- task loops - execute the same task multiple number of times.
- Each time it runs it stores the values of each item in the loop in a variable named item
- can call this variable using double curly braces and quotes
- loops allow for more organized code with reduced repetition
- if you want to pass in multiple values from the loop use an array of dictionaries
   - each dictionary has key value pairs according to the values you pass in
   - call the item variable dot property name
      - `‘{{ item.name }}’`
- **loop** directive is used to create simple loops that iterate through a lot of items
- another way to create loops is the **with_*** directive
   - with_items
   - with_files
   - with_url
   - with_mongodb
   - and more available

## Roles

- Can assign roles in Ansible to blank servers
- when you assign a role it comes with a set of tasks to set up the server for its configured role
- roles allow you to package reusable code and organize your tasks into a set of best practices
- Roles can contain tasks, variables, defaults, handlers and templates
- roles help with sharing your code with others in the Ansible community
- Ansible Galaxy is one of those communities where you can find thousands of roles for almost any task you can think of
- its recommended to look for community roles before writing your own playbook
- to use roles, we first create the directory structure required for that role
- **Ansible-galaxy init** command allows you to initialize the basic role structure to create your role from scratch
- You call the role in your playbook by using the role directive
   - need to find the role on your system can do that by creating a role directory in your playbook structure
   - can add them into the /etc/ansible/roles directory where ansible looks by default if it cant find it in the playbook
   - can upload it to ansible galaxy through a github repo
- Find roles: by searching the UI or by using **ansible-galaxy searc**h command
- to install roles: **ansible-galaxy install** command
   - \-p installs it under the current directory
- can specify roles as an array of strings or as an array of dictionaries if you need to pass in additional properties
- Roles make it easy to develop, use and share ansible playbooks
- **ansible-galaxy list** - lists all the created roles

# Advanced Topics

## Preparing Windows Server

- ansible's controller machine can only be Linux and not windows
- windows machines can be targets of ansible and part of the automation
- ansible connects to a windows machine using winrm
- Requirements:
   - pywinrm module installed on the Ansible Control Machine
      - `pip install “pywinrm>=0.2.2”`
   - Setup WinRM
      - `examples/scripts/ConfigureRemotingforAnsible.ps1`
   - Different modes of authentication:
      - basic
      - certificate
      - kerberos
      - ntlm
      - credSSP

## Ansible-Galaxy

- free site for downloading, sharing and writing all kinds of community-developed Ansible roles
- Great way to get a jumpstart on your automation projects
- check the community out [here](Galaxy.ansible.com)
- find all the available roles under the Explore section
- Can share the roles you create on Ansible Galaxy as well

## Patterns

- Patterns allow you to specify hosts in different ways
   - can specify multiple hosts at a time (Host1, Host2, Host3)
   - can specify groups and singular hosts together (Group1, Host1)
   - can run the playbook against all the hosts that begin with the word host (Host*)
   - can run the playbook against all hosts that end in a specific suffix (*.company)
- Additional options for patterns are located on the Ansible documentation site

## Dynamic Inventory

- its not necessary to always define inventory in a static inventory file bc it requires manual changes
- if you want to integrate ansible with any source of inventory in your environment then the inventory will need to be dynamic
- ansible supports having dynamic inventory files
- instead of specifying an inventory file, specify a python script
   - `ansible-playbook -i inventory.py playbook.yml`
   - the script is responsible for reaching out to whatever sources you have for inventory and giving back ansible a list of groups, hosts and their information
- There are multiple scripts available for dynamic inventory for solutions at the ansible documentation

## Developing Custom Modules

- to develop your own ansible module use a python script and place it in the modules directory on your server
- can get templates to start with at the ansible documentation page > Playbook special topics > Ansible developer guide > How to develop a module

