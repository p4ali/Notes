# [Docker](https://docs.docker.com/engine/understanding-docker/#what-is-docker-s-architecture)
> The Docker platform enables multiple applications to run concurrently on a single copy of an OS, either deployed directly onto a physical server or as a virtual machine (VM).

> This is achieved by providing the capability to execute multiple copies of "user space", which is the place where applications run on a Linux or Unix platform (system or privileged code runs in the kernel).

> "Docker is a tool that can package an application and its dependencies in a virtual container that can run on any Linux server. This helps enable flexibility and portability on where the application can run, whether on premise [sic], public cloud, private cloud, bare metal, etc"

> Docker is an open-source engine which automates the deployment of applicaitons as highly portable, self-sufficient containers which are independent of hardware, language, framework, packaging sytem and hosting provider.

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

![Docker architecuture from docker website](https://docs.docker.com/engine/article-img/architecture.svg)

(From: https://docs.docker.com/engine/article-img/architecture.svg) 

[Docker Architecture](https://docs.docker.com/engine/understanding-docker/#what-is-docker-s-architecture) includes:
* Docker **client**: by which user interact with containers through docker Daemon.
* Docker **host**: where docker **Daemon** runs and manages one or more **container**s.
* Docker **registry**: similar to maven repository, which stores registered images, official or not
* Docker **container** runs on top of readonly layered images, with topmost writable file system. More than one containers can share images. At runtime, you can also mount volume from other images.
* Docker must run on top of linux kernel.

## [Dockerfile](https://docs.docker.com/engine/reference/builder/)

**Dockerfile** contains instructions and commands for building images from the base image. Instructions include actions like:

* Run a command
* Add a file or directory
* Create an environment variable
* What process to run when launching a container from this image

The format of Dockerfile:
```
# Comment
INSTRUCTION arguments
```
Where the INSTRUCTION:

|instruction|description|
|:-----------|:------------|
|FROM image:tag|set the base image for subsequent instructions. FROM must be the first non-comment instruction in the Dockerfile. FROM can appear multiple times within a single Dockerfile in order to create multiple images. |
|MAINTAINER name|maintainer of the image|
|RUN ["executable", "param1", "param2"]|run a command and commit the result|
|CMD ["executable","param1","param2"]|CMD sets default command and/or parameters, which can be overwritten from command line when docker container runs.|
|LABEL key=value|add metadata into the image|
|EXPOSE port|informs Docker that the container listens on the specified network ports(of the Docker Host) at runtime.|
|ENV key=value|set environment variable. This value will be in the environment of all “descendant” Dockerfile commands and can be replaced inline in many as well.|
|ADD src dest|copy new files, directories or remote file url to the dest. **dest** can be an absolute path, or relative path to WORKDIR|
|COPY src dest|copy new files from src to dest|
|ENTRYPOINT ["executable", "param1", "param2"]|allows you to configure a container that will run as an executable|
|VOLUME|creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.|

## How does a container work?
A container consists of an operating system, user-added files, and meta-data. Each container is built from an image. That image tells Docker what the container holds, what process to run when the container is launched, and a variety of other configuration data. The Docker image is read-only. When Docker runs a container from an image, it adds a read-write layer on top of the image (using a union file system) in which user application can then run.

When running following command:

```bash
$ docker run -i -t ubuntu /bin/bash
```

In order, Docker Engine does the following:

* Pulls the ubuntu image: Docker Engine checks for the presence of the ubuntu image. If the image already exists, then Docker Engine uses it for the new container. If it doesn’t exist locally on the host, then Docker Engine pulls it from Docker Hub.
* Creates a new container: Once Docker Engine has the image, it uses it to create a container.
* Allocates a filesystem and mounts a read-write layer: The container is created in the file system and a read-write layer is added to the image.
* Allocates a network / bridge interface: Creates a network interface that allows the Docker container to talk to the local host.
* Sets up an IP address: Finds and attaches an available IP address from a pool.
* Executes a process that you specify: Runs your application, and;
* Captures and provides application output: Connects and logs standard input, outputs and errors for you to see how your application is running.

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

# docker port to see which port are binded to
docker port insane_murdock 4567

# docker history to list the image creation
docker history --no-trunc=true java:8

```

## Stop and remove containers

```bash
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
# docker rm -v $(docker ps -a -q -f status=exited) # remove exited docker, also remove the volume associated with the container(`-v`)
(docker ps -a -q -f status=exited) | xargs -r docker rm -v # this will avoid error when no containers. Above line not.
docker rmi $(docker images -f "dangling=true" -q) # remove dangling image
```

## display docker full command

```bash
docker ps -a --no-trunc # will display the full command along with the other details of the running containers.
```

## Bash into container

Two different way: one is open a new term to teh container, another is share the same term with previous.

```bash
$ eval $(docker-machine env)
$ docker ps 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                           NAMES
7b6071563893        rest/spark          "/code/run.sh"           2 minutes ago       Up 2 minutes        0.0.0.0:32776->4567/tcp         evil_goldberg
38615322e5c0        nginx               "nginx -g 'daemon off"   25 hours ago        Up 25 hours         443/tcp, 0.0.0.0:8000->80/tcp   modest_banach

# open a seperate term and login to the same container(Use EXEC not RUN)
$ docker exec -i -t evil_goldberg /bin/bash

# or login to the same term of the container (different from above)
$ docker attach evil_goldberg
```

## Run vs Exec
* `run`  - run a command in a new container
* `exec` - run a command in a running container

# [Reverse engineering an image](https://abdelrahmanhosny.com/2015/07/11/how-to-merge-two-docker-images/)

```
docker history --no-trunc=true java:8
```

# An example Dockerfile
```
# Firefox over VNC
#
# VERSION               0.3

FROM ubuntu

# Install vnc, xvfb in order to create a 'fake' display and firefox
RUN apt-get update && apt-get install -y x11vnc xvfb firefox
RUN mkdir ~/.vnc
# Setup a password
RUN x11vnc -storepasswd 1234 ~/.vnc/passwd
# Autostart firefox (might not be the best way, but it does the trick)
RUN bash -c 'echo "firefox" >> /.bashrc'

EXPOSE 5900
CMD    ["x11vnc", "-forever", "-usepw", "-create"]
```

# [RUN vs CMD vs ENTRYPOINT](http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)
* RUN executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.
* CMD sets default command and/or parameters, which can be overwritten from command line when docker container runs.
* ENTRYPOINT configures a container that will run as an executable.

# Usecase
* [Create a reusable volume](http://www.davidwong.com.au/blog/tag/gradle/)
* [Docker features](https://docs.docker.com/compose/overview/#features)

# Reference
* [Docker installation](http://docs.docker.com/installation/mac/)
* [Docker Command Line](http://docs.docker.com/reference/commandline/cli/)
* [How to use Docker on Macosx](https://www.viget.com/articles/how-to-use-docker-on-os-x-the-missing-guide)
* [Docker storage](http://www.computerweekly.com/feature/Docker-storage-101-How-storage-works-in-Docker)
* [Clean up after yourself](http://blog.yohanliyanage.com/2015/05/docker-clean-up-after-yourself/)
