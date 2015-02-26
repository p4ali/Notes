## Problem
To setup a private cloud on VMware Cloud Air. Which has following structure:
* An Organization (M123456789-1111) with api url https://p3v10-vcd.vchs.vmware.com:443/cloud/org/M123456789-1111/
* A Virtual Data Center (M123456789-1111)
  * VDC Gateway M123456789-111-default-routed
* 2 public ips are available: 11.86.53.56 and 11.86.53.58

To goal is to setup a webApp which has two VMs, a web server (11.86.53.56) and db server (11.86.53.58). each has a different public IP.

## Goal
A script to build above components from scratch, and also use puppet to provision web server and db server.

## Setup DNS
Once vm is powered on, and ip translation is configured correctly, the last thing to do is to setup dns, this was done:
```bash
# change config
echo 'dns-nameservers 8.8.8.8 8.8.4.4' >> /etc/network/interfaces
# restart nic
ifdown eth0 ; ifup eth0'
```
