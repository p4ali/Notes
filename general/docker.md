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
