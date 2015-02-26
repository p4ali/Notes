## Problem
To setup a private cloud on VMware Cloud Air. Which has following structure:

* An Organization (M123456789-1111) with api url https://p3v10-vcd.vchs.vmware.com:443/cloud/org/M123456789-1111/
* A Virtual Data Center (M123456789-1111)
  * A vApp (webApp)
    * two VMs, a web server and db server. each has a different public IP.

## Goal
A script to build above components from scratch, and also use puppet to provision.
