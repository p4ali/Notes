## Use argparse and mpplot

```
#!/usr/bin/env python3
import re
import sys
import argparse
import matplotlib.pyplot as plt

# create an argument parser
parser = argparse.ArgumentParser(description='Filter and count source IP addresses in a conntrack table.')
parser.add_argument('input_file', nargs='?', type=argparse.FileType('r'), default=sys.stdin,
                    help='the input file to read from (default: stdin)')
parser.add_argument('--histogram', action='store_true', help='create a histogram of the source IP addresses')
args = parser.parse_args()

# create a dictionary to store the count of each src IP
src_count = {}

# read each line
for line in args.input_file:
    # use regex to extract the src IP and dport values
    match = re.search(r'src=(\d+\.\d+\.\d+\.\d+).*?dport=(\d+)', line)
    if match:
        src_ip = match.group(1)
        dport = match.group(2)
        # check if the line has dport=18080
        if dport == '18080':
            # add the src IP to the dictionary or increment its count
            if src_ip in src_count:
                src_count[src_ip] += 1
            else:
                src_count[src_ip] = 1

# print the report
for src_ip, count in src_count.items():
    print(f'{src_ip}: {count}')

# create a histogram of the source IP addresses if requested
if args.histogram:
    plt.bar(range(len(src_count)), list(src_count.values()), align='center')
    plt.xticks(range(len(src_count)), list(src_count.keys()), rotation=90)
    plt.xlabel('Source IP Address')
    plt.ylabel('Count')
    plt.title('Histogram of Source IP Addresses')
    plt.show()
```

Above script will handle file with contents like this from conntrack table:

```
$ conntrack -L -p tcp

tcp      6 431994 ESTABLISHED src=11.16.128.0 dst=11.16.95.4 sport=45624 dport=18080 src=11.16.95.4 dst=11.16.128.0 sport=18080 dport=45624 [ASSURED] mark=0 use=1
tcp      6 431998 ESTABLISHED src=11.16.95.5 dst=11.16.126.9 sport=52462 dport=8080 src=11.16.126.9 dst=11.16.95.5 sport=8080 dport=52462 [ASSURED] mark=0 use=1
tcp      6 431983 ESTABLISHED src=11.16.116.0 dst=11.16.95.4 sport=18454 dport=18080 src=11.16.95.4 dst=11.16.116.0 sport=18080 dport=18454 [ASSURED] mark=0 use=1
tcp      6 431991 ESTABLISHED src=10.229.160.23 dst=10.73.80.143 sport=41316 dport=30000 src=11.16.118.5 dst=11.16.95.0 sport=18080 dport=19421 [ASSURED] mark=0 use=1
tcp      6 431994 ESTABLISHED src=11.16.95.4 dst=11.16.125.9 sport=46366 dport=8080 src=11.16.125.9 dst=11.16.95.4 sport=8080 dport=46366 [ASSURED] mark=0 use=1
tcp      6 431980 ESTABLISHED src=10.134.217.234 dst=10.73.80.143 sport=53268 dport=30000 src=11.16.138.5 dst=11.16.95.0 sport=18080 dport=2345 [ASSURED] mark=0 use=1
tcp      6 431986 ESTABLISHED src=11.16.95.4 dst=11.16.83.45 sport=35940 dport=9090 src=11.16.83.45 dst=11.16.95.4 sport=9090 dport=35940 [ASSURED] mark=0 use=1
tcp      6 431989 ESTABLISHED src=10.73.80.143 dst=10.73.80.63 sport=45316 dport=2379 src=10.73.80.63 dst=10.73.80.143 sport=2379 dport=45316 [ASSURED] mark=0 use=1
tcp      6 431921 ESTABLISHED src=10.134.219.227 dst=10.73.80.143 sport=46352 dport=30000 src=11.16.80.5 dst=11.16.95.0 sport=18080 dport=15344 [ASSURED] mark=0 use=1
tcp      6 431921 ESTABLISHED src=10.134.219.234 dst=10.73.80.143 sport=60104 dport=30000 src=11.16.128.5 dst=11.16.95.0 sport=18080 dport=31263 [ASSURED] mark=0 use=1
tcp      6 431999 ESTABLISHED src=10.134.219.234 dst=10.73.80.143 sport=50378 dport=30000 src=11.16.150.5 dst=11.16.95.0 sport=18080 dport=30268 [ASSURED] mark=0 use=1
tcp      6 431969 ESTABLISHED src=10.229.160.205 dst=10.73.80.143 sport=50606 dport=30000 src=11.16.143.5 dst=11.16.95.0 sport=18080 dport=63561 [ASSURED] mark=0 use=1
tcp      6 431999 ESTABLISHED src=10.134.216.18 dst=10.73.80.143 sport=51492 dport=30000 src=11.16.16.9 dst=11.16.95.0 sport=18080 dport=1329 [ASSURED] mark=0 use=1
```