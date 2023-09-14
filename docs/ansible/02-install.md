# Installation and Setup

## Installing Ansible on Ubuntu

Ubuntu builds are available in a PPA here.

```shell
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

> PPA加速地址: `launchpad.proxy.ustclug.org`

## Setting Up a Server for Testing

Generate ssh key pair for remote exec.

```shell
$ ssh-keygen -t rsa
$ cat ~/.ssh/id_rsa.pub  >> ~/.ssh/authorized_keys 
$ chmod  600  ~/.ssh/authorized_keys
$ ssh 192.168.33.100 "hostname"
testserver
```

Telling Ansible About Your Servers

```shell
$ mkdir inventory

$ vim inventory/testserve.ini 
[webservers]
192.168.33.100 ansible_port=22

[webservers:vars]
ansible_host=192.168.33.100
ansible_user=vagrant
ansible_private_key_file=~/.ssh/id_rsa
```
> Edit `/etc/ansible/ansible.cfg` args to: `host_key_checking = False`

Revoke ping module 

```
$ ansible webservers -i inventory/testserve.ini -m ping
192.168.33.100 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
```

> Defaut Set:
> - inventory: /etc/ansible/hosts
> - passing -i parames to exec: ansible webservers -m ping

Revoke command module

```shell
$ sudo cp inventory/testserve.ini /etc/ansible/hosts

$ ansible webservers -m command -a uptime
192.168.33.100 | CHANGED | rc=0 >>
 05:38:18 up  2:28,  3 users,  load average: 0.07, 0.02, 0.00
```
```shell
$ ansible webservers -m command -a 'ls /var/log/unattended-upgrades'
192.168.33.100 | FAILED | rc=2 >>
ls: cannot open directory '/var/log/unattended-upgrades': Permission deniednon-zero return code
vagrant@ubuntu-bionic:~$ ansible webservers -b -m command -a 'ls /var/log/unattended-upgrades'
192.168.33.100 | CHANGED | rc=0 >>
unattended-upgrades-shutdown.log
```
> e -b or --become flag to tell Ansible to become the root user