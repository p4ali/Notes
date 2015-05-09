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

### Attach disk to vms
In order to be scalable, you must have separate disks, which allow to attach to different vms.
```ruby
# using vcloud_sdk
def provision_disk(vdc, name, vm, size_mb)
  begin
    disks = vdc.find_disks_by_name name
    puts "attaching a pre-existing disk......"
    vm.attach_disk disks[0]
  rescue
    puts "creating and attaching disk......"
    vm.attach_disk(vdc.create_disk name, size_mb, vm)
  end
end
```
Then format disk:
```bash
#!/bin/bash

(echo n; echo p; echo 1; echo 1; echo; echo; echo t; echo 8e; echo w) |  sudo fdisk /dev/sdb
sudo pvcreate /dev/sdb1
sudo vgcreate example-data /dev/sdb1

#size must be 3G less than size specified in setup-vcloud-disks
sudo lvcreate -L 249.5G -n lv0 example-data
sudo mkfs.ext3 /dev/example-data/lv0

(echo n; echo p; echo 1; echo 1; echo; echo; echo t; echo 8e; echo w) |  sudo fdisk /dev/sdc
sudo pvcreate /dev/sdc1
sudo vgcreate example-tmpclients /dev/sdc1

#size must be 3G less than size specified in setup-vcloud-disks
sudo lvcreate -L 97G -n lv0 example-tmpclients
sudo mkfs.ext3 /dev/example-tmpclients/lv0


sudo sh -c "echo \"/dev/example-data/lv0 /var/lib/liwiz auto defaults 0 0\" >> /etc/fstab"
sudo mount -a
sudo mkdir /var/lib/liwiz/tmpclients
sudo sh -c "echo \"/dev/example-tmpclients/lv0 /var/lib/liwiz/tmpclients auto defaults 0 0\" >> /etc/fstab"
sudo mount -a
sudo chown -R user1:group1 /var/lib/liwiz
lsblk
```

### Setup DNS
Once vm is powered on, and ip translation is configured correctly, the last thing to do is to setup dns, this was done:
```bash
# change config
echo 'dns-nameservers 8.8.8.8 8.8.4.4' >> /etc/network/interfaces
# restart nic
ifdown eth0 ; ifup eth0'
```

## Provision dependencies
After the vapp and vms are powered on, the next step is to prvovision necessary 3rd party software with puppet. This usually include install database, install and config [nginx](https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching) and so on.

```bash
# These are facts to pass to puppet, so that you can use '$a' to refer to inside .pp file.
FACTS=" FACTER_a='fact1' \
        FACTER_b='$FACTb' "
        
# copy local puppet files to server
scp -i ~/.ssh/mycert.pem -r puppet/ root@example.com:~/puppet

ssh -i ~/.ssh/perforce.pem root@$HELIX_IP << EOF
  $FACTS puppet apply --modulepath=~/puppet/modules/ ~/puppet/modules/mymodule/manifests/dns.pp;
  puppet module install puppetlabs-apt;
  puppet module install puppetlabs-concat;
  puppet module install puppetlabs/postgresql;
  $FACTS puppet apply -v --modulepath=/etc/puppet/modules:/root/puppet/modules/ ~/puppet/modules/mymodule/manifests/default.pp
EOF
```

## Provision your app
Now it's time to use capistrano to deploy and start your rails app. This usually include:
* pull source code from your git repo
* restart start [nginx/puma](http://wagn.org/Puma_and_Nginx_production_stack) after deploy

And it usually need you run ssh-agent and using [ssh agent forwarding](https://developer.github.com/guides/using-ssh-agent-forwarding/), in case your git repo need ssh authentication.

You may also need some [basic auth](https://www.digitalocean.com/community/tutorials/how-to-set-up-http-authentication-with-nginx-on-ubuntu-12-10) for your nginx based app.

