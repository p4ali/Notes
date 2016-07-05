# [Docker](https://docs.docker.com/engine/understanding-docker/#what-is-docker-s-architecture)
> "Docker is a tool that can package an application and its dependencies in a virtual container that can run on any Linux server. This helps enable flexibility and portability on where the application can run, whether on premise [sic], public cloud, private cloud, bare metal, etc"

> Docker is an open-source engine which automates the deployment of applicaitons as highly portable, self-sufficient containers which are independent of hardware, language, framework, jpackaging ssytem and hosting provider.

> Containers are like extremely lightweight VMs - they allow code to run in isolation from other containers but safely share the machine's resources, all without the overhead of a hypervisor.

## Docker on Macosx with boot2docker
Becuase the Docker engine uses Linux-specific kernel features, you will need to use a lightweight virtual machine (VM) to run it on OSX. Then you use the OSX docker client to control the virtualized docker engine to build, run, and manage Docker containers.

Boot2Docker is a lightweight Linux distribution made specifically to run Docker containers. It runs completely from RAM, is a small ~24MB download and boots in ~5s (YMMV).

Start boot2docker client (double click from Application folder), then run:
```
## start boot2docker
$ boot2docker init
$ boot2docker up

## download docker image
$ docker pull ubuntu

## run docker in interactive mode
$ docker run -i -t ubuntu /bin/bash

## run docker in non-interactive mode
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

![Docker architecuture](https://docs.docker.com/engine/article-img/architecture.svg)
[Docker Architecture](https://docs.docker.com/engine/understanding-docker/#what-is-docker-s-architecture) includes:
* Docker **client**: by which user interact with containers through docker Daemon.
* Docker **host**: where docker **Daemon** runs and manages one or more **container**s.
* Docker **registry**: similar to maven repository, which stores registered images, official or not
* Docker **container** runs on top of readonly layered images, with writable file system. More than one containers can share images.
* Docker must run on top of linux kernel.
```

# Basic Docker command

## Start docker 
```bash
docker run
docker inspect
```

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
In a more traditional Linux boot the root filesystem is mounted read-only and then switched to read-write
after boot and integrity check. In the docker world, however, the root filesystem stays in read-only mode
and docker takes advantage of a union mount to add more read-only filesystems onto the root filesystem.
Docker callls each of these filesystems **images**. Images can be layered on tope of one another. The
image below is called the 'parent' image, until you reach the bottom of the image stack where the final
image is called the base image. Finally, docker mount a read-write filesystem on top of any layers below.
This is where whatever processes we want our docker container to run will execute.

## Commands
```bash
docker images  ## image format of repository:tag, such as ubuntu:12.04.
docker run -i -t ubuntu:12.04 /bin/bash
docker images ubuntu ## list at specific repository 'ubuntu'
docker search puppet ## search docker dependency
docker pull cloudspace/p4d
# doker user repository takes the form of a username and repository name, e.g., cloudspace/p4d.
docker search p4d ## returns following 
  REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
  cloudspace/p4d      latest              f67a6b963191        3 days ago          266.5 MB
  cloudspace/p4d      1.5.0               b13b6ee47b1f        4 weeks ago         262.4 MB

# create a container from p4d master image
docker run -i -t cloudspace/p4d /bin/bash

# list containers
docker ps -a

  CONTAINER ID        IMAGE                   COMMAND                CREATED             STATUS                  PORTS               NAMES
  bb27ef795c30        cloudspace/p4d:latest   "/usr/local/bin/run.   2 days ago          Exited (0) 2 days ago                       atlas-sanity.atlas.dev   

# making changes to images, say install apache to containter
docker commit bb27ef795c30 cloudspace/p4d # commits the diff between the image the container was created from and the current state of the container.

```

## Bash into container

Two different way: one is open a new term to teh container, another is share the same term with previous.

```bash
$ eval $(docker-machine env)
$ docker ps 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                           NAMES
7b6071563893        rest/spark          "/code/run.sh"           2 minutes ago       Up 2 minutes        0.0.0.0:32776->4567/tcp         evil_goldberg
38615322e5c0        nginx               "nginx -g 'daemon off"   25 hours ago        Up 25 hours         443/tcp, 0.0.0.0:8000->80/tcp   modest_banach

# open a seperate term and login to the same container
$ docker exec -i -t evil_goldberg /bin/bash

# or login to the same term of the container (different from above)
$ docker attach evil_goldberg
```

# A Usecase
## User create a project
```
curl -s --data-urlencode name=atlas-sanity.atlas.dev --data-urlencode username=ali@perforce.com 
     --data-urlencode password=password http://192.168.33.10/api/v1/depot
     {"depot_id":1,"name":"atlas-sanity.atlas.dev"}
```

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
* [How to use Docker on Macosx](https://www.viget.com/articles/how-to-use-docker-on-os-x-the-missing-guide)
