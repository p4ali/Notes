```
############################ on HOST
 |2.1.2| saas in ~/workspace/raymond/atlas
○ → script/vagrant-env dev up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'atlas-base'...
==> default: Matching MAC address for NAT networking...
==> default: Setting the name of the VM: atlas_default_1410325519132_27969
==> default: Fixed port collision for 22 => 2222. Now on port 2200.
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 => 2200 (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2200
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection timeout. Retrying...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Setting hostname...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /srv/atlas => /Users/ali/workspace/raymond/atlas
    default: /tmp/vagrant-puppet-1/manifests => /Users/ali/workspace/raymond/atlas/puppet/atlas/manifests
    default: /tmp/vagrant-puppet-1/modules-0 => /Users/ali/workspace/raymond/atlas/puppet
==> default: Running provisioner: puppet...
==> default: Running Puppet with virtualbox.pp...
==> default: stdin: is not a tty
==> default: Warning: Setting manifestdir is deprecated. See http://links.puppetlabs.com/env-settings-deprecations
==> default:    (at /usr/lib/ruby/vendor_ruby/puppet/settings.rb:1095:in `block in issue_deprecations')
==> default: Warning: Setting templatedir is deprecated. See http://links.puppetlabs.com/env-settings-deprecations
==> default:    (at /usr/lib/ruby/vendor_ruby/puppet/settings.rb:1095:in `block in issue_deprecations')
==> default: Notice: Compiled catalog for atlas.perforce.com in environment production in 0.90 seconds
==> default: Notice: /Stage[main]/Ant/Wget::Fetch[ant]/Exec[wget-ant]/returns: executed successfully
==> default: Notice: /Stage[main]/Perforce/File[/usr/local/bin/p4broker]/content: content changed '{md5}d485de6baf505f725cd471965b694def' to '{md5}c6b2c8d2945c46a7952a1ec4934e7547'
==> default: Notice: /Stage[main]/Perforce/Exec[fetch p4]/returns: executed successfully
==> default: Notice: /Stage[main]/Perforce/Exec[fetch p4d]/returns: executed successfully
==> default: Notice: /Stage[main]/Perforce/Exec[fetch p4java]/returns: executed successfully
==> default: Notice: /Stage[main]/Ant/Exec[unpack-ant]/returns: executed successfully
==> default: Notice: /Stage[main]/Ant/File[/usr/bin/ant]/ensure: created
==> default: Notice: /Stage[main]/Perforce/Package[unzip]/ensure: ensure changed 'purged' to 'present'
==> default: Notice: /Stage[main]/Perforce/Exec[unzip p4java]/returns: executed successfully
==> default: Notice: /Stage[main]/Perforce/Exec[symlink p4java]/returns: executed successfully
==> default: Notice: /Stage[main]/Maven::Maven/Wget::Fetch[fetch-maven]/Exec[wget-fetch-maven]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[set hostname]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Package[docker.io]/ensure: ensure changed 'purged' to 'present'
==> default: Notice: /Stage[main]/Atlas/Package[zip]/ensure: ensure changed 'purged' to 'present'
==> default: Notice: /Stage[main]/Atlas/File[passenger conf]/content: content changed '{md5}e7be87662a5e122eec597e632e815419' to '{md5}3e42188b3ca02c14ea1e6d851ebbfb60'
==> default: Notice: /Stage[main]/Atlas/File[passenger conf]/group: group changed 'root' to 'perforce'
==> default: Notice: /Stage[main]/Atlas/File[passenger conf]/mode: mode changed '0644' to '0664'
==> default: Notice: /Stage[main]/Atlas/Exec[symlink docker]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/File[atlas site config]/ensure: defined content as '{md5}bebfffdf06629cb650000972b850230c'
==> default: Notice: /Stage[main]/Atlas/File[git-fusion site config]/ensure: defined content as '{md5}e96a62d4168a56b4863dbf0214879d61'
==> default: Notice: /Stage[main]/Atlas/Service[mysql]/ensure: ensure changed 'stopped' to 'running'
==> default: Notice: /Stage[main]/Atlas/Exec[install lxc-docker]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/File[/etc/environment]/content: content changed '{md5}2d92b8808ad0bb731f0b1e77fa618634' to '{md5}c816c28a93fdb251a1d0a679860b21b6'
==> default: Notice: /Stage[main]/Atlas/Exec[docker pull cloudspace/p4d:latest]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[a2ensite atlas.conf]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/File[/srv/files]/ensure: created
==> default: Notice: /Stage[main]/Atlas/Exec[start p4broker]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Package[libapache2-mod-authnz-external]/ensure: ensure changed 'purged' to 'present'
==> default: Notice: /Stage[main]/Atlas/Exec[a2enmod authnz_external]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/File[/etc/dnsmasq.conf]/content: content changed '{md5}93ce09b5406eb1760f8a1c2577c8c8bf' to '{md5}2abfe35ec001cc94d34b918a0f5a3662'
==> default: Notice: /Stage[main]/Atlas/Exec[bundle]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Service[dnsmasq]: Triggered 'refresh' from 1 events
==> default: Notice: /Stage[main]/Atlas/Exec[apt-get update]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[a2enmod headers]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/File[uploader site config]/ensure: defined content as '{md5}d235054e5327192f7953cc39e5e77c73'
==> default: Notice: /Stage[main]/Atlas/Exec[restart passenger]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[a2ensite uploader.conf]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Cron[cleanup files]/ensure: created
==> default: Notice: /Stage[main]/Atlas/Exec[a2enmod rewrite]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[a2ensite git-fusion.conf]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Service[apache2]/ensure: ensure changed 'stopped' to 'running'
==> default: Notice: /Stage[main]/Java/Package[java]/ensure: ensure changed 'purged' to 'present'
==> default: Notice: /Stage[main]/Maven::Maven/Exec[maven-untar]/returns: executed successfully
==> default: Notice: /Stage[main]/Maven::Maven/File[/usr/bin/mvn]/ensure: created
==> default: Notice: /Stage[main]/Jruby/Jruby_bin[jgem]/File[/usr/local/bin/jgem]/ensure: defined content as '{md5}9d1729ee000d36894ffe28d0a9891bd5'
==> default: Notice: /Stage[main]/Jruby/Jruby_bin[jirb]/File[/usr/local/bin/jirb]/ensure: defined content as '{md5}73b6dceb9096dd5e9b124af4483da285'
==> default: Notice: /Stage[main]/Jruby/Jruby_bin[jruby]/File[/usr/local/bin/jruby]/ensure: defined content as '{md5}1ce494ce286aa07fcea509fa89c8b268'
==> default: Notice: /Stage[main]/Jruby/Exec[download_jruby]/returns: executed successfully
==> default: Notice: /Stage[main]/Jruby/Exec[unpack_jruby]/returns: executed successfully
==> default: Notice: /Stage[main]/Jruby/Jruby_bin[jrubyc]/File[/usr/local/bin/jrubyc]/ensure: defined content as '{md5}aa9204b329920641fbd6b9fc20df22d2'
==> default: Notice: /Stage[main]/Jruby/File[/opt/jruby]/ensure: created
==> default: Notice: /Stage[main]/Atlas/Exec[jruby-bundler install]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas/Exec[uploader-bundle]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[clean hosts]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[uploader-migrate]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[add db privileges]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[db location]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[bundle exec rake db:create db:migrate]/returns: executed successfully
==> default: Notice: /Stage[main]/Atlas::Virtualbox/Exec[api location]/returns: executed successfully
==> default: Notice: Finished catalog run in 866.00 seconds

 |2.1.2| saas in ~/workspace/raymond/atlas
○ → script/sanity-check 
Destroying existing sanity depot
Creating a project
{"depots":[]}
Creating a trigger

Container could not be found or created.
Container could not be found or created.
Container could not be found or created.
Container could not be found or created.

############################ on ATLAS 
○ → script/vagrant-env dev ssh
vagrant@atlas:/srv/atlas/docker$ sudo docker build -t cloudspace/p4d .
Sending build context to Docker daemon 79.87 kB
Sending build context to Docker daemon 
Step 0 : FROM ubuntu:trusty
Pulling repository ubuntu
826544226fdc: Download complete 
511136ea3c5a: Download complete 
b3553b91f79f: Download complete 
ca63a3899a99: Download complete 
ff01d67c9471: Download complete 
7428bd008763: Download complete 
c7c7108e0ad8: Download complete 
 ---> 826544226fdc
Step 1 : MAINTAINER Michael Orr <michael@cloudspace.com>
 ---> Using cache
 ---> 5b9ee170f1f4
Step 2 : EXPOSE 1666
 ---> Using cache
 ---> 75ef26539d22
Step 3 : RUN ln -snf /usr/bin/python3 /usr/bin/python &&     apt-get update &&     apt-get install -y curl                        tcpdump
 ---> Using cache
 ---> 278c9d01b772
Step 4 : ADD http://www.perforce.com/downloads/perforce/r14.1/bin.linux26x86_64/p4 /usr/local/bin/p4
 ---> Using cache
 ---> dac60cd9c8e0
Step 5 : ADD http://www.perforce.com/downloads/perforce/r14.1/bin.linux26x86_64/p4d /usr/local/bin/p4d
 ---> Using cache
 ---> 6d99ad1c098a
Step 6 : ADD run.sh /usr/local/bin/run.sh
 ---> a608209e7179
Removing intermediate container 3e13bc87ec6a
Step 7 : ADD idley.sh /usr/local/bin/idley.sh
 ---> d5b9f0417b94
Removing intermediate container 2ec0192c8139
Step 8 : ADD p4gf_submit_trigger.py /usr/local/git-fusion/bin/p4gf_submit_trigger.py
 ---> 78463810fb4a
Removing intermediate container e29fdc60d7a8
Step 9 : RUN chmod +x /usr/local/bin/*              /usr/local/git-fusion/bin/p4gf_submit_trigger.py
 ---> Running in d30b310c3b48
 ---> d1e9fd66c546
Removing intermediate container d30b310c3b48
Step 10 : RUN useradd -d /tmp -s /usr/sbin/nologin -u 1001 perforce
 ---> Running in 8125dbad59cd
 ---> 2c2127c0fd5b
Removing intermediate container 8125dbad59cd
Step 11 : WORKDIR /
 ---> Running in 7387bbb51bd7
 ---> 9ee717fda3bc
Removing intermediate container 7387bbb51bd7
Step 12 : ENTRYPOINT ["/usr/local/bin/run.sh"]
 ---> Running in f605b59515cc
 ---> b80900e44961
Removing intermediate container f605b59515cc
Successfully built b80900e44961

############################# back to HOST
○ → script/sanity-check 
Destroying existing sanity depot
Creating a project
{"errors":{"Error configuring Git Fusion!":"Creating /var/lib/perforce/atlas-sanity.atlas.dev/p4gf_env.conf...\nConfiguring P4 user security...\nBroker connection error: failed to connect to 0.0.0.0:49164.\nTCP connect to 0.0.0.0:49164 failed.\nconnect: 0.0.0.0:49164: Connection refused\n"}}{"depots":[{"id":1,"name":"abc.atlas.dev"},{"id":3,"name":"atlas-sanity.atlas.dev"}]}
Creating a trigger
{"triggers":[{"id":3,"name":"sanity-auth-trigger.sh","command":"Mytrigger change-commit //... \"$SCRIPT$\""}]}
Broker connection error: failed to connect to 0.0.0.0:49169.
TCP connect to 0.0.0.0:49169 failed.
connect: 0.0.0.0:49169: Connection refused
Broker connection error: failed to connect to 0.0.0.0:49170.
TCP connect to 0.0.0.0:49170 failed.
connect: 0.0.0.0:49170: Connection refused
Broker connection error: failed to connect to 0.0.0.0:49171.
TCP connect to 0.0.0.0:49171 failed.
connect: 0.0.0.0:49171: Connection refused
Broker connection error: failed to connect to 0.0.0.0:49172.
TCP connect to 0.0.0.0:49172 failed.
connect: 0.0.0.0:49172: Connection refused

```
