---
title: "Ansible lab on Docker"
subtitle: "This article provides steps to setup ansible lab on docker"
date: "2025-02-06"
---

This article provides steps to set up the ansible lab on the docker platform with Ubuntu image and with one Ansible master and 3 Managed hosts.

Prerequisites:

- Docker Desktop should be installed on the local system.

Ansible Setup:

- We will be using Ubuntu image for the hosts, pull the Ubuntu docker image.

```
        docker pull ubuntu

```

- Create docker network for ansible with specific subnet (suggested use different subnet with /24 than the local private CIDR range)

```
    docker network create ansible-network --subnet 192.168.31.0/24
```

* Once we are ready with image and network we can proceed with Ansible master container creation.
* Run Ansible Master container with Ubuntu inage and on ansible network.

```
    docker run -dit --name ansible-master --net ansible-network --hostname ansmst ubuntu
```

* Once mster container is created login to the container.

```
    docker exec -it ansible-master /bin/bash 
```

* In the Master container do apt update and install required packages(below)

````
    apt update
    apt install vim python-is-python3 openssh-client iputils-ping ansible -y 

````
* once the required packages are install use ssh-keygen to setup passwors less authentication.

```
    ssh-keygen
```

* Now create 3 containers for the managed hosts

```
    docker run -dit --name managed01 --net ansible-network --ip 192.168.31.11 --hostname managed01 ubuntu
    docker run -dit --name managed02 --net ansible-network --ip 192.168.31.12 --hostname managed02 ubuntu
    docker run -dit --name managed03 --net ansible-network --ip 192.168.31.13 --hostname managed03 ubuntu

```
* Login to EACH container (managed hosts) and run below commands for required packages.
 Logging in:

 ```
    docker exec -it managed01 /bin/bash
 ```
 Commands on all three managed containers:

 ```
    apt update
    apt install vim python-is-python3 openssh-client openssh-server -y
    apt update
    service ssh start
 ```
 * Once all the required packages are installed on all hosts, we are ready to set up passwordless authentication.
 * Copy the content of the /pub file on ~/.ssh/ which was created when ssh-keygen is executed on master container.
 * Past the content to ~/.ssh/authorized_keys on all the managed hostes.(if authorized_keys is not preset, create it).
 * We should be good with the password less authentication from master to all managed hosts.
 * Try to ssh managed hosts from master to test, you should be able to login to managed container with out any password prompt.
 * Our lab is ready and we can validate by running ansible.
 * Create inventry.ini file with managed hosts and root as ansible user
 ```
    192.168.31.11 ansible_user=root
    192.168.31.12 ansible_user=root
    192.168.31.13 ansible_user=root
 ```
 * You can validate now by adhoc command "ansible -i inventory.ini -m ping all"

 #### Thats it, now you have your Ansible lab and can experiment and learn  🎉:)