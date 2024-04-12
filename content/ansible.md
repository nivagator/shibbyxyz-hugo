---
title: "Ansible"
date: 2024-03-19T12:33:02-05:00
draft: true
---

# Jeff Geerling Ansible 101
- ansible for dev ops 
https://www.jeffgeerling.com/blog/2020/ansible-101-jeff-geerling-youtube-streaming-series

## Episode 1
- 2024-03-19
- https://www.youtube.com/watch?v=goclfp6a2IQ

### chapter 1
docs.ansible.com
ansible community google group
ansible.com/blog

all ansible needs is to be able to connect via ssh and keys

inventory file is where the servers are
.ini file syntax

ansible.cfg file to set defaults

-m for modules if no module, defaults to -m command (command module)
-a for attributes 
-u for user
-K for password or --ask-pass

### chapter 2
build vms locally with vagrant

vagrant init geerlingguy/centos7
creates vagrant file. written in ruby
vagrant up

add ansible as provisioner in vagrant file

vagrant provision to run playbook

itempotence 

vagrant destroy