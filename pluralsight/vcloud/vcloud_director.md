# VMware vCloud Director Organizations

## The Anatomy of a vCloud Organization
![Anatomy of vCloud Organization](./images/vcloud_organization_anatomy.jpg?raw=true)

The organization can have multiple:
* Vitural Datacenter(VDC)
* Org networks
* Catalog which composed of app template and media
* Logs
* Settings
* Users

### VDC
3 types (allocation model):
* Allocation pool
* Reservation pool (most expensive)
* Pay as you go (efficient)

### vApp
A group of vms serving a particular type of application, such as
* a typical multiptier web app, composed of a web server, app server, and database server
* a network service virtual application, contains a load balancer, name server, and domain server

vApp can have its own firewall and dhcp. 

### Org Networks
In addition to vApp can have their own networks, the organization can have high-level networks called org networks.
Org networks connect vApps together as well as to connect to external world.
3 types of org networks:
* Isolation: not connected to outside world, only used to connect behind firewall
* Direct: connected to outside world with public ip directly
* Routed: connected to outside world through firewall, and have assigned to an internal ip, and ip address translation

### Catalog
vApp can be turned into templates and put into catalog. Catalog is a storage library of vApps in templates form.
It allows us to deploy a copy or multiple copies of our vApps to vdc complete without any vms, networks. It also 
contain medias such as cd images, iso files or even floppy files. You can upload your application to media and 
mount them to the vms when needed. You can have multiple catalogs, private or publics.

### Logs
For auditing and troubleshooting. Two forms: 
* tasks are logs when you do something within vcloud director, such as power on vms
* events include more details such as login to vcloud director organization 
Tasks and event can be found in My Clound > Logs, it's good for debugging.

### Settings
Orgnization setting usually setup once and probably never touch again.
* General settings such as Email and guest.
* Policies including:
 * leases - max time vapps and vapp templates can run and be stored in vdc
 * default quota - how may vms a user can store and power
 * limits - number of resource intensive operations to prevent dos attack 
 * password polices - locking user account after a number of invalid login attempts

### Users
User can be given different roles, such as admin, catalog author, vApp author, or console access only user. User can have certain vApps, and quotas.


## Managing your vCloud Organization

## Managing Virtual Applications (vApps)
### (either) Create a new vApp
A vApp with 2 vms, and connect directly to internet. vApp contains vms.
* make sure vApps template already exists
* create a vApp
* Add virtual machines and configure virtual machines
* config networks
* complete
### (or)Deploy an vApp from Template

### Customizing vApps
change local root password, and others

### Power on/off operation
With vm share and secutity options, you can control how user access the vApp.

### Lease
* runtime lease - how long to run
* storage lease - how long storage expire

### vApp templates
Created from existing vApp. First stop it and then do it(UI). Or clone it without stop it (API). 

## Managing virtual machines

### Adding vm to vApp
* Add a vm from catalog (there maybe conflicts) or new vm (has to manually install guest OS)
* Using vmware remote console to install guest os.
* Using catalog media to install guest os (this is required if you choose create new vm from step 1, and you need vmware client integration plugin)
* install vmware tool
* customize guest os

## vApp Networking with vshield Edge
* Adding orignization network to vapp does not create a new network, it's just add existing network to the vapp.
* create a new networking can be done in vapp > networking > + button.
* customize vshield edge, goto network, select and right click configure services.
 * DHCP - vm has to be configure for dhcp, dynamically give ip to vm
 * Firewall - configure firewall rules, inbound/outbound, ICMP is for ping.
 * NAT - map external ip to internal ip; port forwarding
  * port forwarding enable to configure port based on NAT.
  * changing the port-forwarding type will erase all existing rules.
 * Static routing - allow route internal vapp to another vapp.
NAT rules comes first, then firewall rules.
Networkd trouble shooting.

Next hop in routing for static routing for network, it should point to the network external ip.

## vCloud API
* Restful with XML
* used for reporting, automation(deploying and scaling, adding vms), 3ed party tools
* resources: 
 * [API Programming Guide](http://www.vmware.com/pdf/vcd_15_api_guide.pdf)
 * [Java programming guide](http://pubs.vmware.com/vcd-55/topic/com.vmware.ICbase/PDF/vcd_55_sdk_java_dg.pdf)
 * [vcloud API doc](https://developercenter.vmware.com/web/dp/doc/preview?id=115)
 * [sdk](https://developercenter.vmware.com/web/sdk/5.5.0/vcloud-api)
* debug with [rest-client](https://github.com/wiztools/rest-client)
* the url for HTTP request is GET [https://p3v10-vcd.vchs.vmware.com/api/versions](https://p3v10-vcd.vchs.vmware.com/api/versions), trust sll cert
* to get a session, POST [https://p3v10-vcd.vchs.vmware.com/api/sessions](https://p3v10-vcd.vchs.vmware.com/api/sessions)
 * you need set the header with
 ```
    Accept: application/*+xml;version=5.6
 ```
 * you will get `x-vcloud-authorization: d30f409ea1714503815a0215031b2e4c`, and you should add that to header
 * Then you can use GET or POST to navigate to the different url returned by link.
 
* Then using that loginUrl with the the header

## vCloud networking
Types:
* external networks (out of the cloud, or LANs)
* Network pools
* Organization vDC networks
 * external direct
 * external routed (with edge shield sitting inbetween)
 * internal isolated
* vApp networks - only exist when vApp powered on
  * direct
  * fenced - method to protect the identity of the network, 3 copies of same vapps with same ip, same name, edge acts like a proxy
  * routed
  * isolated
* vApp -> [org] -> external
* [vCloud Director networking for dummies](http://it20.info/2010/09/vcloud-director-networking-for-dummies/)
 

## Hybrid Cloud
Two or more distinct cloud infrastructures bound together.
3 importants tools for hybrid cloud: vCloud connnector, vcenter orchiestra, powerclient cli.

### vCloud connector
Composed of server and node. Each components(cloud) must be deployed with a node, and server will 
provide a facade to public.

vSphere client can have multiple plugin, e.g., vCloud connector plugin.
* Migrate virtual machines(workload) from vSphere center to vCloud.
* deploy vApp from catalog

Console access

## vCenter Orchestrator
plugins: e.g., for vcloud director and so on.


## Caveat
* When deploying a vm, in case get static ip from pool, you will not know the ip when 
  you create the vm, but know after creation and before power on, so you can first
  create, then find the ip, then update configuration, and then power on.
* Click the by pass ssl confirmation if you are self-signed.


## Term
* ESX - VMware ESX is VMware’s enterprise server virtualization platform. The platform is 
    available in two versions -- ESX Server and ESXi Server.
* ESXi - VMware ESXi is an operating system independent hypervisor based on the VMkernel
    operating system interfacting with agent that run atop it. VMware describes an ESXi 
    system as similar to a stateless compute node. State information can be uploaded from 
    a saved configuration file. 
* Thin Provisioning - TP is a method of optimizing the efficiency with which the available 
    space is utilized in storage area networks. This is compared to Fat Provisioning (FT), 
    where storage space is allocated beyond the current needs, and cause low utilization rate
    and energy consumption high.
* Virtualization - with an abstraction layer (hypervisor) on top of physical hardware, and
    guest os (or vms) on top of supervisor. This way, multiple guest OS can be run simultanously on
    top of one physical hardware, and also if you snapshot the guest os, it can be easily
    rerun on another hardware, given the fact that supervisor provide an standard interface (ESX).
    Server virualization is the most popular form of vitualization.
* Hypervisor - A piece of software which create the virutalization layer that makes server
    virtualization possible, and it contains the vm monitor. Two popular: VMware ESXi, Hyper-V (microsoft)
* Type 1 hypervisor - loaded directly on the hardware. e.g., Hyper-V, ESXi, KVM
* Type 2 hypervisor - loaded in an OS running on the hardware, e.g., Virtual Box, Fusion, Workstaion, Parallels.
* Type 1 and Type 2 vm can be exchanged, since they both run on top of hypervisor interface.

* Organization: An Organization is the fundamental vCloud Director grouping that contains users, the vApps that they create, and the resources the vApps use. It is a top-level container in a cloud that contains one or more Organization Virtual Data Centers (Org vDCs) and Catalog entities. It owns all the virtual resources for a cloud instance and can have many Org vDCs.
* vApp: VMware vApp is a format for packaging and managing applications. A vApp can contain multiple virtual machines.
* VM: A virtualized personal computer environment in which a guest operating system and associated application software can run. Multiple virtual machines can operate on the same managed host machine concurrently.
* Catalogs & Catalog-Items: Catalog is used in organizations for storing content. Example: base images. Each item stored in catalog is referred as catalog item.
* vDC: Virtual Data Center. These are of two kinds provider vDCs (accessible to multiple organizations), and organization vDCs (accessible only by a given organization). In fog we refer to organization vDCs.
* Networks: You can setup various internal networks and assign various internal ip ranges to them


  



