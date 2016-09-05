## Vagrant

* [Create a vagrant box with ubuntu desktop gui and virtual box](http://aruizca.com/steps-to-create-a-vagrant-base-box-with-ubuntu-14-04-desktop-gui-and-virtualbox/)
* [Vagrant – Adding a second hard drive](http://everythingshouldbevirtual.com/vagrant-adding-a-second-hard-drive)

## puppet
### To pass FACTs to puppet through /etc/environment
Any environment variable prefixed with `FACTER_` will be availabe in Puppet manifiests. 
```bash
export FACTER_my_var=hello
# or
FACTER_my_var=hello puppet apply --modulepath=/etc/puppet/modules:/x/y/z/modules path_to_my.pp --debug

echo "FACTER_my_var=hello" >> /etc/environment

# Or in case heredoc:
cat > /etc/environment <<EOF
  FACTER_var1=val1
  FACTER_var2=val2
EOF
```

## vagrant
```
vagrant init bento/ubuntu-16.04; vagrant up --provider virtualbox
vagrant init centos/7; vagrant up --provider virtualbox
```

## Reference
* [http://www.erikaheidi.com/blog/a-beginners-guide-to-vagrant-and-puppet-part-3-facts-conditional](http://www.erikaheidi.com/blog/a-beginners-guide-to-vagrant-and-puppet-part-3-facts-conditional)
* [https://docs.puppetlabs.com/references/3.5.latest/type.html](https://docs.puppetlabs.com/references/3.5.latest/type.html)
* [Everything should be virtual - blog](http://everythingshouldbevirtual.com/vagrant-adding-a-second-hard-drive)
