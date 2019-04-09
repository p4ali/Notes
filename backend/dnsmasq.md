## [USE DNSMASQ INSTEAD OF /ETC/HOSTS](https://www.stevenrombauts.be/2018/01/use-dnsmasq-instead-of-etc-hosts/)

The steps to setup on **macosx**, so that I can map `*.example` to `127.0.0.1`:

* `brew install dnsmasq`
* update `/usr/local/etc/dnsmasq.conf`, and add line `address=/.example/127.0.0.1`
* `sudo mkdir /etc/resolver`, then `sudo vi /etc/resolver/example`, and set content to `nameserver 127.0.0.1`
* `sudo brew services start dnsmasq`

*Note:*
The file under `/etc/resolver/` is the domain name, which must match the one in `/usr/local/etc/dnsmasq.conf`.

**On ubuntu** see [here](https://askubuntu.com/questions/157154/how-do-i-include-lines-in-resolv-conf-that-wont-get-lost-on-reboot)

* `sudo apt-get update && sudo apt-get install dnsmasq`
* `sudo /etc/init.d/dnsmasq restart`
* `sudo vi /etc/dnsmasq.conf`, and add `address=/.example/127.0.0.1`
* `sudo vi /etc/network/interfaces`, and add 
```
iface ens3 inet dhcp
    dns-nameservers 127.0.0.1
```
* `ping xyz.example`, you should get bytes back. Otherwise, you may need manually update the `/etc/resolv.conf` and add `nameserver 127.0.0.1`
