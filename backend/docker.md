# Overview

* A **broker** is running, each request to the **broker** is delegate to a **p4d** running within **docker**.

* The setup is scripted as **vagrant** and **puppet** script to orchestrate a VM, the VM then start running a docker/p4d.
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

```bash
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
