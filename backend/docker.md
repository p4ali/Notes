# Overview

* A **broker** is running, each request to the **broker** is delegate to a **p4d** running within **docker**.

* The setup is scripted as **vagrant** and **puppet** script to orchestrate a VM, the VM then start running a docker/p4d.


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
