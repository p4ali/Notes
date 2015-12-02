## Vagrant

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

## Reference
[http://www.erikaheidi.com/blog/a-beginners-guide-to-vagrant-and-puppet-part-3-facts-conditional](http://www.erikaheidi.com/blog/a-beginners-guide-to-vagrant-and-puppet-part-3-facts-conditional)
[https://docs.puppetlabs.com/references/3.5.latest/type.html](https://docs.puppetlabs.com/references/3.5.latest/type.html)
