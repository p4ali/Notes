**OpenStack** is a cloud ecosystem but requires a hypervisor for the compute platform.

## Core Services

### Identity service (Keystone)
* **Identity service**: authentication and authorization
* **Tenants** are logically separated containers within your openstack cloud.

### Image service (Glance)
* store and manage guest images
* images can be managed globally and per tenant
* Uses can be authorized to upload custome images
* Store images in Switf, cinder, or in the native file system
* Can store remotely (e.g., asws s3)

### Compute service (nova)
* platform to run guest machines
* boot instances from Glance images
* Require hypervisor support:
 * KVM
 * Xen
 * vSphere
 * Hyper-V
 * and More
* separate Nova instances per hypervisor
* nova is management platform for hypervisor

### Networking (neutron)
* 


## Term
* **Region** - logical pools of openstack services
* **Aggregates** - groups of openstack nova endpoints based on cahracteristics
* **Availibility Zones** - groups of openstack nova endpoints based on location. This is also physical regions.
