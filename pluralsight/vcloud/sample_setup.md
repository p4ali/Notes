## Problem
To setup a private cloud on VMware Cloud Air. Which has following structure:
* An Organization (M123456789-1111) with api url https://p3v10-vcd.vchs.vmware.com:443/cloud/org/M123456789-1111/
* A Virtual Data Center (M123456789-1111)
  * VDC Gateway M123456789-1111-default-routed
* 2 public ips are available: 11.86.53.56 and 11.86.53.58

To goal is to setup a webApp which has two VMs, a web server (11.86.53.56) and db server (11.86.53.58). each has a different public IP.

## Goal
A script to build above components from scratch, and also use puppet to provision web server and db server.

## Setup
For ruby vcloud api, you can either use [vchs ruby vloud sdk](https://github.com/vchs/ruby_vcloud_sdk) or [vcloud rest](https://github.com/astratto/vcloud-rest). The following provision is using vcloud sdk.

### Create a vapp
You can create a vapp with or without vms. Notice, the VCHS_USER should be
```ruby
VCHS_URL='https://p3v10-vcd.vchs.vmware.com:443'
VCHS_USER='liwiz@example.com'
VCHS_PASSWD='password'
VCHS_ORGANIZATION='M123456789-1111'
VDC='M123456789-1111'
VCHS_VERSION='5.6'

VAPP_NAME='test'
VM1_NAME='web_server'
VM2_NAME='db_server'
VDC_NETWORK='M123456789-1111-default-routed'
VAPP_NETWORK='vApp-router'
VAPP_TEMPLATE='ubuntu/trusty64'
VM_BASE='base'
CATALOG_NAME='Liwiz'
EDGE_GATEWAY='M123456789-1111'

@connection = VCloudClient::Connection.new VCHS_URL, VCHS_USER, VCHS_PASSWD, VCHS_ORGANIZATION, VCHS_VERSION
@connection.login
@org = @connection.get_organization(connection.get_organizations[VCHS_ORGANIZATION])
@vdc = @connection.get_vdc(org[:vdcs][VCHS_ORGANIZATION])

cat = connection.get_catalog(org[:catalogs][CATALOG_NAME])
cat_item = connection.get_catalog_item(cat[:items][VAPP_TEMPLATE])
vapp = cat_item[:items][0][:vms_hash]
compose = connection.compose_vapp_from_vm(
          org[:vdcs][VDC],
          VAPP_NAME,
          'vApp created by vApp creator',
          {## vms, could be empty, or more
             VM1_NAME => vapp[VAPP_TEMPLATE][:id]
          },
          { # a network routed with vapp gateway
              name: VAPP_NETWORK,
              gateway: '10.1.1.1',
              netmask: '255.255.255.0',
              start_address: '10.1.1.2',
              end_address: '10.1.1.254',
              fence_mode: "natRouted",
              ip_allocation_mode: "POOL",
              parent_network: vdc[:networks][VDC_NETWORK],
              enable_firewall: "false",
              nat_type: 'ipTranslation'
          }
      )
 connection.wait_task_completion(compose[:task_id])
```

### Add more vms
You can add/remove vms after vapp started.
```
vapp_params = {
    name: VAPP_NAME,
    id: vdc[:vapps][VAPP_NAME]
}

cat = connection.get_catalog(org[:catalogs][CATALOG_NAME])
cat_item = connection.get_catalog_item(cat[:items][VAPP_TEMPLATE])
vapp_template = cat_item[:items][0][:vms_hash]

vm_params = {
    vm_name: VM2_NAME,
    template_id: vapp_template[VM_BASE][:id]
}
connection.wait_task_completion connection.add_vm_to_vapp(vapp_params, vm_params, {:name => 'vApp-router', :ip_allocation_mode => "POOL"})
vm = connection.get_vm_by_name org, VCHS_ORGANIZATION, VAPP_NAME, VM2_NAME
```

### Setup DNS
Once vm is powered on, and ip translation is configured correctly, the last thing to do is to setup dns, this was done:
```bash
# change config
echo 'dns-nameservers 8.8.8.8 8.8.4.4' >> /etc/network/interfaces
# restart nic
ifdown eth0 ; ifup eth0'
```
