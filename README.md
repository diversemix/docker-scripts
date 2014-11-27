docker-scripts
==============

Instructions and a collection of scripts for using Docker.

## Introduction
To download and for a more comprehensive set of instructions please visit: https://docs.docker.com/

All following instructions are based on CentOS 7

## Install

Install with package manager (yum for CentOS). After a successful install you will have to manually enable the service to start on startup, and also add any users you wish to permit use of docker by adding them to the docker group (example below permits the user 'developer' to use docker:
  
  yum install docker
  chkconfig docker on
  usermod -a -G docker developer

## Walkthrough

Description | Command
--- | :---: 
Run interactively (will pull the centos image) | `docker run -i -t centos /bin/bash`
