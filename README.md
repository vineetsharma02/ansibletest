Ansible Playbook
=====================

This is the Ansible Playbook for setting up Oracle JDK 8, Tomcat 8, Activemq 5.14.0 and Nginx.

Requirements
------------

Target server: CentOS 7 only (I think it works with Redhat 7 as well).

Execution server: Environment where ansible runs.

Role Variables
--------------

Default installation mode for Centos/RHEL:

* `oracle_jdk_version: jdk1.8.0_181`
* `tomcat_version: 8.5.9`
* `tomcat_port: 8081`
* `amq_version: 5.14.0`
* `amq_port: 61616`
* `nginx_port: 8080`

Building
--------

### Ansible installation on a CentOS 7

First ensure that the CentOS 7 EPEL repository is installed:
```shell
sudo yum install -y epel-release
```

Once the repository is installed, install Ansible with yum:
```shell
sudo yum install ansible
```

Clone
--------

```shell
git clone https://github.com/vineet/ansibletest.git
```

Configuring Ansible Hosts
-------------------------

Ansible keeps track of all of the servers that it knows about through a "hosts" file. We need to set up this file first before we can begin to communicate with our other servers.

### Open the file with root privileges like this:
```shell
sudo vim ~/ansibletest/hosts
[group_name]
alias ansible_ssh_host=your_server_private_ip ansible_user=your_server_user ansible_ssh_private_key_file=your_user_private_key ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

The group_name is an organizational tag that lets you refer to any servers listed under it with one word. The alias is just a name to refer to that server.

Imagine you have multiple server you want to control with Ansible. Ansible communicates with client computers through SSH, so each server you want to manage should be accessible from the Ansible server by typing:

```shell
ssh your_server_user@your_server_private_ip
```

You should not be prompted for a password. While Ansible certainly has the ability to handle password-based SSH authentication, SSH keys help keep things simple.

### Example:

```shell
sudo vim ~/ansibletest/hosts
[oracle-jdk8]
centos1 ansible_ssh_host=192.168.10.20 ansible_user=centos ansible_ssh_private_key_file=~/centos.pem ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[tomcat8]
centos2 ansible_ssh_host=192.168.10.20 ansible_user=centos ansible_ssh_private_key_file=~/centos.pem ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[activemq]
centos3 ansible_ssh_host=192.168.10.20 ansible_user=centos ansible_ssh_private_key_file=~/centos.pem ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[nginx]
centos4 ansible_ssh_host=192.168.10.21 ansible_user=centos ansible_ssh_private_key_file=~/centos.pem ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
```shell
Note:- You can set up this file in Ansible configuration directory also "/etc/ansible/hosts" & "/etc/ansible/ansible.cfg"
```

Using Simple Ansible Commands
-----------------------------

Now that you have your hosts set up and enough configuration details to allow you to successfully connect to your hosts, you can try out your very first command.

### Ping all of the servers you configured by typing:

```shell
ansible -i hosts -m ping all
```

### Ansible will return output like this:
```shell
Output
centos3 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
centos2 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
centos1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
centos4 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```

By now, you should have your Ansible server configured to communicate with the servers that you would like to control.

This is a basic test to make sure that Ansible has a connection to all of its hosts.

The all portion means "all hosts." You could just as easily specify a group:

```shell
ansible -i hosts -m ping tomcat8
```

Usage
-----

### For running on all servers in one shot
```shell
cd ansibletest; chmod u+x ansible-run.sh; ./ansible-run.sh
```

### For individual server run
```shell
cd ansibletest; ansible-playbook -i hosts oracle-jdk8.yml
cd ansibletest; ansible-playbook -i hosts tomcat8.yml
cd ansibletest; ansible-playbook -i hosts activemq.yml
cd ansibletest; ansible-playbook -i hosts nginx.yml
```
Author Information
------------------
Vineet Sharma <v_vineetsharma@outlook.com>
