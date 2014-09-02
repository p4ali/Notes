# Docker

> Docker is an open-source engine which automates the deployment of applicaitons as highly portable, self-sufficient containers which are independent of hardware, language, framework, jpackaging ssytem and hosting provider.
> Containers are like extremely lightweight VMs - they allow code to run in isolation from other containers but safely share the machine's resources, all without the overhead of a hypervisor.

## Docker on Macosx with boot2docker
Becuase the Docker engine uses Linux-specific kernel features, you will need to use a lightweight virtual machine (VM) to run it on OSX. Then you use the OSX docker client to control the virtualized docker engine to build, run, and manage Docker containers.

Start boot2docker client (double click from Application folder), then run:
```
$ docker run hello-world
Hello from Docker.
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (Assuming it was not already locally available.)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

# Overview

* A **broker** is running, each request to the **broker** is delegate to a **p4d** running within **docker** container.

* The setup is scripted as **vagrant** and **puppet** script to orchestrate a VM, the VM then start running a docker/p4d.
* 
```bash
## running as daemon
p4broker -c /etc/perforce/p4broker.conf -d
/usr/bin/docker -d

## only when access specific project, and gone after a while
docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 49153 -container-ip 172.17.0.2 -container-port 1666

## dns to map url to docker
/usr/sbin/dnsmasq -x /var/run/dnsmasq/dnsmasq.pid -u dnsmasq -r /var/run/dnsmasq/resolv.conf -7 /etc/dnsmasq.d,.dp

## mysqld root user root
/usr/sbin/mysqld
```


# Basic Docker command

## Start docker 

```bash
docker start myproject >/dev/null 2>&1
```

## Run command
Start a docker container named **myproject** and run the cloundspace/p4d quitely.
```bash
# --name : assign a name to the container 
# -v: Bind mount a volume, e.g., map from host /var/lib/perforce/myproject to container /srv
# -p: bind a container's port 1666 to the host port
docker run -d --dns 172.17.42.1 -v /var/lib/perforce/myproject:/srv -p 1666 --name myproject cloudspace/p4d:1.5.0 -qr /srv/p4d -p 1666 >/dev/null 2>&1
```

# A Usecase

## Step 1 User issue a connection
The following parameters will send to broker:

```
clientPort: mahattan_12def234.dev:1666
workspace: y_client_herbie_fully_loaded_1
user: herbie_fully_loaded_1
cwd: /Users/x/workspace/y
command: dirs
Arg0: //mahattan_12def234/main/*
```

mv ~/VirtualBox\ VMs/boot2docker-vm/boot2docker-vm.vbox ~/VirtualBox\ VMs/boot2docker-vm/boot2docker-vm.vbox.bak
# Reference
* [Docker installation](http://docs.docker.com/installation/mac/)
* [Docker Command Line](http://docs.docker.com/reference/commandline/cli/)
