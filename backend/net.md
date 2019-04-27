## Background
[Linux network namespace](https://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/)


## Conntrack and netstat

```
netstat -an | grep EST | tr -s ' ' | cut -d ' ' -f 5 | sort | uniq | cut -d ':' -f 1 | sort | uniq -c
sudo conntrack -L | grep 10.17.136.49 | wc -l
```

```
sudo conntrack -L 2>&1 | grep tcp | grep -v conntrack | tr -s ' ' | cut -d ' ' -f 4,5,6,7,8,9,10,11,12 | sed 's/src=//g' | sed 's/dst=//g' | sed 's/sport=//g' | sed 's/dport=//g' |  awk '{ { print $2,$1 } }' | sort | uniq -c | awk '$3 ~ /ESTABLISHED/ {est[$2]+=$1; ips[$2]=$2} $3 ~ /TIME_WAIT/ {wait[$2]+=$1; ips[$2]=$2} END { printf "%s\t%s\t%s\n", "IP Address", "EST", "WAIT"; for (ip in ips) {printf "%s\t%s\t%s\n", ip, est[ip], wait[ip] } }'| sort -rnk2
```

```
grep "ESTABLISHED" /tmp/conntrack_log.log |cut -d '=' -f2 |sort |uniq -c |sort -n |tr -s ' ' |cut -d ' ' -f3 |while read h; do nslookup $h |grep name ; done
```
