## [USE DNSMASQ INSTEAD OF /ETC/HOSTS](https://www.stevenrombauts.be/2018/01/use-dnsmasq-instead-of-etc-hosts/)

The steps to setup on macosx, so that I can map `*.example` to `127.0.0.1`:

* `brew install dnsmasq`
* update `/usr/local/etc/dnsmasq.conf`, and add line `address=/.example/127.0.0.1`
* `sudo mkdir /etc/resolver`, then `sudo vi /etc/resolver/example`, and set content to `nameserver 127.0.0.1`
* `sudo brew services start dnsmasq`

*Note:*
The file under `/etc/resolver/` is the domain name, which must match the one in `/usr/local/etc/dnsmasq.conf`.
