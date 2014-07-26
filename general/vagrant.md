# Setup

## Local Host
**dnsmasq** or **dnssimple**, mainly to resolve url and route the requests to server. 
For local use, you need to config /etc/host and the /usr/local/etc/dnsmasq.conf, so that all url to specific urls will route to the specific VM.
For production use, you need register a domain, e.g, cloudything.net, so that all *.cloudything.net will route correctly.

### DNSimple
To Verify we use dnsimple name servers:
``` bash
whois cloudything.net
```

After checking the domain name server setup correctly, you can verify the it server the dns records for your domain name:
``` bash
dig -t NS cloudything.net @ns2.dnsimple.com
```

#### Terms
* **TTL**: time to live within the cache of dns
* **CNAME**: a Canonical name record, i.e., a record mapping alias to the canonical name
* **A**: an A record mapping the left to an ip Address
* **AAAA**: a record mapping the left to an ipv6 Address

## Vagrant
* verify the source are up-to-date to the git head with `git cherry @ origin/master | wc -l`
* create .vagrant.ci|development|production|staging folder if does not exist, and hard link to .vagrant
* softlink config/environment.ci|development|production|staging.yml to config/environment.yml
* `vagrant up`
** a mysql db will start with _root_ no pass

### script/sanity-check
* usage `script/sanity-check [ATLAS_HOST]  # default ip=192.168.33.10`
* call POST endpoint to create a depot
** with depot named `atlas-sanity`
** with user email from `$(git config --get user.email)`
** with user name of email account `${USEREMAIL%%@*}`
** with password `password`
```
$ mysql -uroot
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| atlas_development  |
| atlas_test         |
| mysql              |
| performance_schema |
+--------------------+

mysql> use atlas_development;
mysql> show tables;
+-----------------------------+
| Tables_in_atlas_development |
+-----------------------------+
| depot_mappings              |
| depots                      |
| schema_migrations           |
| triggers                    |
+-----------------------------+

mysql> select * from depot_mappings
| id | depot_id | mapping    | created_at       | updated_at          
atlas-sanity:1666 local-atlas-sanity "rsh:p4d -iqr /var/lib/perforce/p4d/p4d-atlas-sanity" | 2014-07-26 15:47:54 | 2014-07-26 15:47:54

mysql> select * from depots;
+----+--------------+--------------+---------------------+---------------------+
| id | name         | organization | created_at          | updated_at          |
+----+--------------+--------------+---------------------+---------------------+
|  1 | atlas-sanity | NULL         | 2014-07-26 15:47:54 | 2014-07-26 15:47:54 |
+----+--------------+--------------+---------------------+---------------------+

mysql> select * from triggers;
| id | depot_id | name     | command  | shell_script | created_at          | updated_at          |
|  1 |        1 | sanity-auth-trigger.sh | Mytrigger change-commit //... "$SCRIPT$" | exit 0 | 2014-07-26 15:47:57 | 2014-07-26 15:47:57 |


```

* call POST to create a trigger
* ** with auth trigger named `sanity-auth-trigger.sh`
* call GET to show triggers
* try to `resolveip depot`, and update the /etc/hosts if not setup correctly
** ensure following entry in /etc/host: `192.168.33.10 atlas-sanity`
* verify the above setup correctly by
``` bash
# p4 -Cutf8 -p"$DEPOT:1666" -u"$USERNAME" -P"$PASSWORD" info
p4 -Cutf8 -patlas-sanity:1666 -uali -Ppassword info
# p4 -Cutf8 -p"$DEPOT:1666" -u"$USERNAME" -P"$PASSWORD" users
p4 -Cutf8 -patlas-sanity:1666 -uali -Ppassword users
> ali <ali@atlas> (ali) accessed 2014/07/26
> perforce <perforce@atlas> (perforce) accessed 2014/07/26
# p4 -Cutf8 -p"$DEPOT:1666" -u"$USERNAME" -P"$PASSWORD" protect -o
p4 -Cutf8 -patlas-sanity:1666 -uali -Ppassword protect -o
# p4 -Cutf8 -p"$DEPOT:1666" -u"$USERNAME" -P"$PASSWORD" triggers -o
p4 -Cutf8 -patlas-sanity:1666 -uali -Ppassword triggers -o
```

