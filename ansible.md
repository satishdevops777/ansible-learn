Ansible:

Simple
Agentless

Ansible configuration Files

/etc/ansible/ansible.cfg

Custom Config File

$ANSIBLE_CONFIG=/opt/ansible-web.cfg ansible-playbook playbok.yml


1. $ANSIBLE_CONFIG
2. ansible.cfg in current dir
3. .asnible.cfg in user home dir
4. /etc/ansible/ansible.cfg


ansible-config list #Lists all configurations
ansible-config view # Shows the current config file
ansible-config dump # Shows the current settings


WHAT IS YAML?

key, value pairs
exam:

fruit: Apple # Key-value pair

Fruits:		#Array or list
- Orange
- Banana
- Apple

Banana:			#Map or Dictionary
	Calories: 105
	Fat: 10



Linux: ssh
Windows: Powershell


default inventory file: /etc/ansible/hosts

[group1]
web ansible_host=server1 ansible_user=root
db ansible_host=server2 ansible_connection=ssh
server3


ansible_ssh_pass
ansible_port


ansible inventory formats

INI
The INI format is the simplest and most straightforward

Example:

[web]
web1.server.com
web2.server.com

[dbservers]
db1.server.com
db2.server.com

YAML

all:
- children:
  - webservers:
    - hosts:
	    - web1.server.com
	    - web2.server.com
  - dbservers:
    - hosts:
	    - db1.server.com
	    - db2.server.com

playbook.yml

variables

string
 username: "admin"
int
 max_connection: 100
boolean (true or false)
 debug_mode: true
dictionary
 packages:
    - nginx
    - httpd

"{{ packages[0] }}"	


Variable precedence

Group vars
Host vars

/etc/ansibel/hosts

web1 ansible_host=1.2.3.4 dns_server=10.10.10.10 #Host vars

[web_servers]
web1
web2

[web_servers:vars]	#Group vars
dns_server=10.2.3.4


playlevel variables


extra variable

ansible-playbook --extra-vars "dns_server=10.2.2.2"



ANSIBLE PLAYBOOKS

set of instructions 


1. extra vars
2. play level
3. host level
4. group level


name:
hosts:
tasks:
  shell: cat /etc/passwd
  register: result

  debug:
    vars: result

# ansible-playbook -i inventory -v


magic variables

we can retrive anything using magic vars

hostvars
msg: '{{ hostvars['webserver'].ansible_facts.architecture }}' = msg: '{{ hostvars['webserver']['ansible_facts']['architecture'] }}'


groups
 msg: '{{ groups['group1'] }}'

group_names
  msg: '{{ groups_names }}'

inventory_hostname
  mesg: '{{ groups_names }}' # Inventory name configured for the host in inventory file


ansible_facts

gather_facts: no # In ansible confing gathering =implicit means it gathers facts explicit means dont gather facts

Playbook
 
Single YAML file

play

Defines a set of activities (tasks) to be run on hosts

task : an action to be performed on the host

modules

command
script
yum
service

ansible-doc -l
ansible-playbook --help
ansible-playbook plabook_name


Verify ansible playbooks

Check Mode
ansible "dry run" where no actual changes are made on hosts
allows previe of playbook chnages without applying them
Use the --check option to run a playbook in check mode

Diff Mode
provides a before and after comparison of playbook chnages
Understand and verify the impact of playbook changes before applying them
Use the --diff option to run a playbook in diff mode

syntax check

Ensures playbook syntax is error-free
Use --syntax-check


Ansible-linit

Ansible lint is a command line tool that performs linting on ansible playbooks, roles and collections

CONDITIONS

when: ansible_os_family == "redhat" or ansible_os_family == "SUSE"


To install multiple packages

loops:
- joe
- ram
- krish

with_items:
- joe
- ram
- mani

with_files:
- "/etc/hosts"
- "/etc/resolv/conf"

with_url:
- "https://site1.com"
- "https://site2.com"



ansible modules

system
user, group, hostnames, iptables, lvg,lvol,make,mount, ping timexone, systemd, service

command
script
shell

files
lineinfile
replace

database
mysql
mongodb etc

cloud
amazon
azure
gcp
docker


windows modules

win_copy
win_command
win_domain

ansible plugins

a piece of code that modifies the functionality of ansible
enhance various aspects of ansible

Inventory 
modules
callbacks

Dynamic Inventory Plugin
Up to date view of infra

Module Plugin
integrates seemlessly with our cloud api

action plugin
ssl certificates
load balancer rules etc.

lookup plugins
Filter plugins
connection plugins
Inventory plugins
callback Plugins


modules & plugins index

cisco.ios

handlers

Tasks triggered by events/notifications


Ansible Roles

re using the code

ansible-galaxy init rolename

/etc/ansible/roles : default dir where ansible check for roles

roles_path= 

ansible-galaxy  list
ansibl-config dumo | grep ROLE

ansible-galaxy install role -p ./roles


Ansible Collections

Package and distribute , roles, plugins


Templating
















