# Setup

## Local Host
dnsmasq: or dnssimple, mainly to resolve url and route the requests to server. 
For local use, you need to config /etc/host and the /usr/local/etc/dnsmasq.conf, so that all url to specific urls will route to the specific VM.
For production use, you need register a domain, e.g, cloudything.net, so that all *.cloudything.net will route correctly.

### DNSimple
To Verify we use dnsimple name servers:
`whois cloudything.net`

After checking the domain name server setup correctly, you can verify the it server the dns records for your domain name:
`dig -t NS cloudything.net @ns2.dnsimple.com`

**TTL**: time to live within the cache of dns
**CNAME**: a Canonical name record, i.e., a record mapping alias to the canonical name
**A**: an A record mapping the left to an ip Address.

