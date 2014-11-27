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
--- | :---
List images downloaded | `docker images`
Remove image | `docker rmi <name>`
List containers (running images) | `docker ps -l`
Remove container | `docker rm <id>`
Run interactively (will pull the centos image) | `docker run -i -t centos /bin/bash`
Copy files on/off container | `docker cp centos:~/<file> .` <br> `docker cp <file> centos:~/`
Build from a Docker file (see example below) | `mkdir test` <br> `vi test/Dockerfile` <br> `docker build -t <imagename> <folder containing dockerfile>`

### Opening a Port

#### Finding the IP Address and Connecting
    docker ps
    docker inspect <container> | grep IPAddress
    nc <contianer_ip> <container_port>

#### NATing the port locally
    iptables -t nat -A DOCKER -p tcp --dport <port> -j DNAT --to-destination <contianer_ip>:<container_port>

### Example Dockerfile

This dockerfile was created when the contianer could not access the outside world and so it was necessary to copy on the RPM files and install from these rather than via the internet repros. The local repos was made using the command `yumdownloader` to download the RPMs.

```
FROM centos
COPY rpm_store/ /tmp/rpm_store
RUN yum -y install /tmp/rpm_store/tar-1.26-29.el7.x86_64.rpm
RUN mkdir node
RUN cd node
RUN tar -xzvf /tmp/rpm_store/node-v0.10.33-linux-x64.tar.gz -C /usr/local
RUN ln -s /usr/local/node-v0.10.33-linux-x64 /usr/local/node
RUN useradd server
USER server
RUN whoami
RUN echo PATH=\$PATH:/usr/local/node/bin >>~/.bashrc
RUN /usr/local/node/bin/npm --version
RUN /usr/local/node/bin/node --version
```
Complete documentation can be found [here][2]

## Usage
```
Usage: docker [OPTIONS] COMMAND [arg...]
 -H=[unix:///var/run/docker.sock]: tcp://host:port to bind/connect to or unix://path/to/socket to use

A self-sufficient runtime for linux containers.

Commands:
    attach    Attach to a running container
    build     Build an image from a Dockerfile
    commit    Create a new image from a container's changes
    cp        Copy files/folders from a container's filesystem to the host path
    diff      Inspect changes on a container's filesystem
    events    Get real time events from the server
    export    Stream the contents of a container as a tar archive
    history   Show the history of an image
    images    List images
    import    Create a new filesystem image from the contents of a tarball
    info      Display system-wide information
    inspect   Return low-level information on a container
    kill      Kill a running container
    load      Load an image from a tar archive
    login     Register or log in to a Docker registry server
    logout    Log out from a Docker registry server
    logs      Fetch the logs of a container
    port      Lookup the public-facing port that is NAT-ed to PRIVATE_PORT
    pause     Pause all processes within a container
    ps        List containers
    pull      Pull an image or a repository from a Docker registry server
    push      Push an image or a repository to a Docker registry server
    restart   Restart a running container
    rm        Remove one or more containers
    rmi       Remove one or more images
    run       Run a command in a new container
    save      Save an image to a tar archive
    search    Search for an image on the Docker Hub
    start     Start a stopped container
    stop      Stop a running container
    tag       Tag an image into a repository
    top       Lookup the running processes of a container
    unpause   Unpause a paused container
    version   Show the Docker version information
    wait      Block until a container stops, then print its exit code

```


[1]: https://docs.docker.com/
[2]: http://docs.docker.com/reference/builder/
