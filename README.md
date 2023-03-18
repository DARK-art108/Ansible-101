## Ansible-101

This repository contains the code for the Ansible 101 workshop.

### Prerequisites

* Ansible 2.2 or higher

### Setup

* Clone this repository

#### Perform these commands in master node:

* `sudo apt-get update`
* `sudo -i`
* `apt-get install python3`
* `apt-get install python3-pip`
* `python3 --version`
* `pip3 --version or pip --version`
* `apt-get update`
* `apt-get install software-properties-common`
* `apt-add-repository ppa:ansible/ansible`
* `apt-get update`
* `apt-get install ansible`
* `ansible --version`

#### Add hosts to /etc/ansible/hosts - 

* `vi /etc/ansible/hosts`

```bash
[web]
192.168.2.1
192.168.2.2
```
* `ansible all --list-hosts` - to check if hosts are added

* `ssh-keygen`
* `cat /root/.ssh/id_rsa.pub`

#### Add the public key to the remote hosts

Here we are going to add the copied key to the slave nodes.

* login to the slave nodes
* `vi /root/.ssh/authorized_keys` or `sudo vi /root/.ssh/authorized_keys`
* paste the copied key in both the slave nodes

#### Perform these commands in master node:

* ssh <ip-of-slave> then enter yes - to check if the connection is established, and you will be logged in to the slave node, do same for the other slave node

* `ansible all -m ping` - to check if the connection is established

* `ansible all -m shell -a "free -m"` - to check the memory of the slave nodes.

* `ansible all -m shell -a "df -h"` - to check the disk space of the slave nodes.

* `ansible all -m shell -a "uptime"` - to check the uptime of the slave nodes.

* `ansible all -m shell -a "cat /etc/os-release"` - to check the OS of the slave nodes.

* `ansible all -m shell -a "cat /etc/hostname"` - to check the hostname of the slave nodes.

#### Setup ansible.cfg

* `vi /etc/ansible/ansible.cfg`

```bash
[defaults]
inventory = /etc/ansible/hosts
host_key_checking = False
```

#### Create Playbook

* `vi playbook.yml`

```bash
---
- name: Install nginx 
  hosts: web  
  tasks:    
    - name: install nginx 
      apt: 
        name: nginx
        state: latest
        update_cache: yes
    - name: start nginx 
      service:
        name: nginx 
        state: started 
```

* `ansible-playbook playbook.yml` - to run the playbook

* `ansible-playbook playbook.yml --check` - to check the playbook

* Check the nginx installation in the slave nodes, go to the browser and type the ip of the slave nodes, you will see the nginx welcome page.

if the page is not displayed, then check the inbound rules in the security group of the slave nodes.

These are inbound rules:

* `HTTP 80`
* Access Anywhere - IPv4
* Access Anywhere - IPv6

This is the instruction to setup the Ansible 101 workshop.



