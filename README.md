docker-scripts
==============

Instructions and a collection of scripts for using Docker.

## Introduction
To download and for a more comprehensive set of instructions please visit [Docker][1]: 

All following instructions are based on CentOS 7

## Install

Install with package manager (yum for CentOS). After a successful install you will have to manually enable the service to start on startup, and also add any users you wish to permit use of docker by adding them to the docker group (example below permits the user 'developer' to use docker:
  
    yum install docker
    chkconfig docker on
    usermod -a -G docker developer

## Walkthrough

Description | Command
--- | :---: 
List images downloaded | `docker images`
Remove image | `docker rmi <name>`
List containers (running images) | `docker ps -l`
Remove container | `docker rm <id>`
Run interactively (will pull the centos image) | `docker run -i -t centos /bin/bash`
Copy files on/off container | `docker cp centos:~/<file> .` <br> `docker cp <file> centos:~/`
Build from a Docker file (see example below) | `mkdir test` <br> `vi test/Dockerfile` <br> `docker build -t <imagename> <folder containing dockerfile>`

### Example Dockerfile

This dockerfile was created when the contianer could not access the outside world and so it was necessary to copy on the RPM files and install from these rather than via the internet repros.

```
FROM centos
COPY rpm_store/ /tmp/rpm_store
RUN yum -y install /tmp/rpm_store/tar-1.26-29.el7.x86_64.rpm
RUN mkdir node
RUN cd node
RUN tar -xzvf /tmp/rpm_store/node-v0.10.33-linux-x64.tar.gz -C /usr/local
RUN ln -s /usr/local/node-v0.10.33-linux-x64 /usr/local/node
RUN echo PATH=\$PATH:/usr/local/node/bin >>~/.bashrc
RUN /usr/local/node/bin/npm --version
RUN /usr/local/node/bin/node --version
```



[1]: https://docs.docker.com/
