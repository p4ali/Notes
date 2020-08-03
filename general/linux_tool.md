## [cron](http://www.adminschoice.com/crontab-quick-reference)

* To add a daily routine, add a script file into `/etc/cron.daily/`

## Route
The related commands in `inetutils-traceroute dnsutils` packages.(`apt-cache search *xyz*`)
* print route: `netstat -nr` 
* trace route: `traceroute baidu.com`
* nslookup: `nslookup www.penglai.com.cn`
* dig: `dig @localhost www.penglai.com.cn` # @ set the dns to dig.
* `/etc/resolv.conf`: name server list

## [ZFS](https://www.freebsd.org/doc/handbook/zfs.html)

ZFS has three major design goals:

* Data integrity: All data includes a checksum of the data.
* Pooled storage: physical storage devices are added to a pool, and storage space is allocated from that shared pool.which can be
                  increased by adding new storage devices to the pool.
* Performance: multiple caching mechanisms provide increased performance.

## [SSH checker](https://www.sslshopper.com/ssl-checker.html#hostname=helix.perforce.com) 
* [How cert are self-signed](http://www.clintharris.net/2009/self-signed-certificates/)

## [Apache Bench for load testing](https://www.petefreitag.com/item/689.cfm)

100 tests, max 10 concurrently running.
```bash
ab -n 100 -c 10 http://www.yahoo.com/
```

## [iptables and ufw](https://www.cyberciti.biz/faq/linux-iptables-drop/)

```bash
# to block or unblock
sudo iptables -I OUTPUT -d hacker.com  -j DROP
sudo iptables -I INPUT -d hacker.com  -j ACCEPT

# drop subnet and log to syslog
sudo /sbin/iptables -i eth1 -A INPUT -s 10.0.0.0/8 -j LOG --log-prefix "IP DROP SPOOF A:"

# to list blocked ip
sudo /sbin/iptables -L -v
```

[UFW](http://notepad2.blogspot.com/2012/02/linux-block-outgoing-traffic-to.html)

<details>
  <summary>Print connection status with `/proc/net/tcp`</summary>
  <p>

```bash
#!/usr/bin/bash
#*******************************************************************************
# This script will provide minimal netstat facilities to TempGen as the TempGen
# OS does not provide this program.
#
# Author:   Jim Lambert (from Freeware)
# Date:     October 2019
# Author:   Alex Li
# Date:     August 2020
#*******************************************************************************

#*******************************************************************************
# Function that converts hex to decimal using awk.
#*******************************************************************************
awk 'function hexToDec(input, decimalValue, n, i, k, c)
{
    decimalValue = 0
    n = length(input)
    for (i = 1; i <= n; i++)
    {
        c = tolower(substr(input, i, 1))
        k = index("123456789abcdef", c)
        decimalValue = decimalValue * 16 + k
    }
    return decimalValue
}

#*******************************************************************************
# Function that gets the connection status from a string.
#*******************************************************************************
function getState(input)
{
    output=""
    switch (input) {
    case "01":
        output="ESTABLISED"
        break
    case "02":
        output="SENT"
        break
    case "03":
        output="RECV"
        break
    case "04":
        output="WAIT1"
        break
    case "05":
        output="WAIT2"
        break
    case "06":
        output="WAIT"
        break
    case "07":
        output="CLOSE"
        break
    case "08":
        output="CLOSE_WAIT"
        break
    case "09":
        output="LAST_ACK"
        break
    case "0A":
        output="LISTEN"
        break
    case "0B":
        output="CLOSING"
        break
    case "0C":
        output="NEW_SYN_RECV"
        break
    case "0D":
        output="MAX_STATES"
        break
    }
    return output
}

#*******************************************************************************
# Function that gets the IP address from a string.
#*******************************************************************************
function getIP(input, output)
{
    output=hexToDec(substr(input, index(input, ":") - 2, 2));
    for (i = 5; i > 0; i -= 2)
    {
        output = output"."hexToDec(substr(input, i, 2))
    }
    output = output":"hexToDec(substr(input, index(input, ":") + 1, 4))
    return output
}
#*******************************************************************************
# Mainline
#*******************************************************************************
NR > 1 {{if(NR==2)print "Local - Remote state";local=getIP($2);remote=getIP($3);state=getState($4)}{print local" - "remote" "state}}' /proc/net/tcp
```
</p>
</details>
