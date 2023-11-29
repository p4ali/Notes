## ssh into node and run command

```python
#!/usr/bin/env python3

import os

hosts = {}
with os.popen("kubectl config current-context") as context:
    cluster = context.read().strip()

if not os.path.isdir(cluster):
    os.mkdir(cluster)

with os.popen("kubectl get nodes -l node-role.kubernetes.io/istio -o go-template --template '{{range .items}}{{$address := index .status.addresses 1 \"address\"}}{{printf \"%s %s\\n\" .metadata.name $address}}{{end}}'") as nodes:
    while True:
        line = nodes.readline()
        if not line:
            break
        name, ip = line.split()
        hosts[name] = ip

def fetch_host(cluster, ip):
    print(f"Fetching {fname}")
    os.system(f"ssh -oStrictHostKeyChecking=no -l app {ip} \"sudo conntrack -L -p tcp \"> {cluster}/conntrack-{ip}.txt")

for host in hosts:
    ip = hosts[host]
    fname = f"conntrack-{cluster}-{ip}.txt"
    fetch_host(cluster, ip)
```