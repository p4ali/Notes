

.Given conntrack entries
```text
tcp      6 431994 ESTABLISHED src=11.16.128.0 dst=11.16.95.4 sport=45624 dport=18080 src=11.16.95.4 dst=11.16.128.0 sport=18080 dport=45624 [ASSURED] mark=0 use=1
tcp      6 431998 ESTABLISHED src=11.16.95.5 dst=11.16.126.9 sport=52462 dport=8080 src=11.16.126.9 dst=11.16.95.5 sport=8080 dport=52462 [ASSURED] mark=0 use=1
```

.Run following command to analyze
```python
import re
import pandas as pd
import os
import matplotlib.pyplot as plt
import seaborn as sns
import mplcursors

regex = re.compile(r'^(tcp|udp)\s+\d+\s+\d+\s+(\w+)\s+src=(\d+\.\d+\.\d+\.\d+)\s+dst=(\d+\.\d+\.\d+\.\d+)\s+sport=(\d+)\s+dport=(\d+)\s+src=(\d+\.\d+\.\d+\.\d+)\s+dst=(\d+\.\d+\.\d+\.\d+)\s+sport=(\d+)\s+dport=(\d+)\s+(\[.*\])\s+mark=(\d+)\s+use=(\d+)$')
regex_fname = re.compile(r'^conntrack-(\d+\.\d+\.\d+\.\d+).txt$')

pd.set_option('display.max_columns', None)

def read_conntrack(to_rows, host_ip, fname):
    print(f"Reading {fname}")
    with open(fname) as f:
        while True:
            line = f.readline()
            if not line:
                break
            m = regex.match(line)
            # if no match, continue
            if not m:
                continue
            protocol = m.group(1)
            state = m.group(2)
            if protocol != 'tcp' or state != 'ESTABLISHED':
                continue
            src = m.group(3)
            dst = m.group(4)
            sport = int(m.group(5))
            dport = int(m.group(6))
            src2 = m.group(7)
            dst2 = m.group(8)
            sport2 = int(m.group(9))
            dport2 = int(m.group(10))
            mark = int(m.group(12))
            use = int(m.group(13))

            to_rows.append({'protocol': protocol, 'src': src, 'dst': dst, 'host': host_ip, 'sport': sport, 'dport': dport, 'src2': src2, 'dst2': dst2, 'sport2': sport2, 'dport2': dport2, 'mark': mark, 'use': use})

def load_conntracks(cluster):
    rows = []
    files = os.listdir(cluster)
    for fname in files:
        fname_match = regex_fname.match(fname)
        if not fname_match:
            continue
        host_ip = fname_match.group(1)
        read_conntrack(rows, host_ip, f"{cluster}/{fname}")
    return pd.DataFrame(data = rows, columns=['host', 'protocol', 'src', 'dst', 'sport', 'dport', 'src2', 'dst2', 'sport2', 'dport2', 'mark', 'use'])

df = load_conntracks('scus-prod-a78')
# dport is the destination port of the Istio node, the one to which the ILB connects
# dst is the Istio node to which the ILB connects
# sport2 is the port of the IGW pod
# src2 is the pod IP of the IGW pod
grouped=df[df['sport2'].eq(18080) & df['dport'].eq(30000)].groupby(['src2'])['dst'].value_counts().unstack().fillna(0)

```