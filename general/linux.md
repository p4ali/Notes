## Shell pattern filtering

These POSIX shells use four different pattern filtering:
* ${var#pattern} - Removes smallest string from the left side that matches the pattern.
* ${var##pattern} - Removes the largest string from the left side that matches the pattern.
* ${var%pattern} - Removes the smallest string from the right side that matches the pattern.
* ${var%%pattern} - Removes the largest string from the right side that matches the pattern.

Here are a few examples:
```bash
foo="foo-bar-foobar"
echo ${foo#*-}   # echoes 'bar-foobar'  (Removes 'foo-' because that matches '*-')
echo ${foo##*-}  # echoes 'foobar' (Removes 'foo-bar-')
echo ${foo%-*}   # echoes 'foo-bar'
echo ${foo%%-*}  # echoes 'foo'

# if you have a folder has file 1.dot 2.dot and you wan to mv them to 1.doc and 2.doc
for i in $(ls); do mv $i ${i%%[a-z\.]*}.doc; done
```

## netstat
Print  network  connections, routing tables, interface statistics, masquerade connec‐tions, and multicast memberships.

```bash
# Show only program(p) who listening(l) tcp(t) protocol and show the numerical address instead of symbolic port, host and user name
$ netstat -ntlp # mac does not support t, and you must pass -p tcp explicitly
# Active Internet connections (only servers)
# Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
# tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      948/mysqld
# tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      831/sshd
```

## SSH agent forwarding
```bash
# most os start ssh-agent automatically, to check your local ssh-agent is running
echo $SSH_AUTH_SOCK
# start it if not
ssh-agent bash -c 'ssh-add $HOME/.ssh/myprivate_key.pem && git clone me@my.com/sample.git /my/local/git/repo'
# To verify if it work
ssh -T me@my.com
# Also make sure your public key is in server ~/.ssh/authorized_keys
# And the /etc/sshd_config must NOT have `ForwardAgent no`
```

## `grep`, `sed`,`cut`, `awk` and `tr`
Given following text:
```
... action0 add
... rev0 1
... fileSize0 673
... action1 add
... rev1 1
... fileSize1 664
... digest1 096821ABA6CB1FDA50618650B7E5F58F
```
Using following script you can extract the fileSize and add them up
```bash
echo $INPUT | grep '... fileSize' | cut -d' ' -f3 | awk '{ sum+=$1} END {print sum}'
# or with sed group back-reference
echo $INPUT | grep '... fileSize' | sed 's/... fileSize[^ ]* \(.*\)/\1/' | awk '{ sum+=$1} END {print sum}'
## 1337
```

### `tr` example
Translate characters. Squeezing all white space to a single space with `tr -s [:blank:]`
```bash
# find GlobalProtect process ids and kill them
$ ps aux|grep [G]lobalProtect
> ali             77507   0.0  0.1   770504  21528   ??  S     5:30AM   0:02.50  /Applications/GlobalProtect.app/Contents/MacOS/GlobalProtect
> root            77506   0.0  0.1  2554188  16300   ??  S     5:30AM   0:03.35 /Applications/GlobalProtect.app/Contents/Resources/PanGPS

# squeeze white spaces
$ ps aux|grep [G]lobalProtect|tr -s [:blank:]
> ali 77507 0.0 0.1 770504 21528 ?? S 5:30AM 0:02.50 /Applications/GlobalProtect.app/Contents/MacOS/GlobalProtect
> root 77506 0.0 0.1 2554188 16300 ?? S 5:30AM 0:03.35 /Applications/GlobalProtect.app/Contents/Resources/PanGPS

# loop through and kill them
$ for p in $(ps aux|grep [G]lobalProtect|tr -s [:blank:]|cut -d' ' -f2); do  kill -9 $p; done
```

### `awk` example
`$0` - the whole record (the line)
`$1` - the first field

The awk default is to perform all commands on each record, but awk also allow following actions:
`BEGIN {cmds}` - the cmds will run before the first record is read
`END {cmds}` - the cmds will run after the laste record is processed

If a variable is not defined, it has a default value.

### loop through `ls`
```
ls -l|awk '
BEGIN{print "Custom Directory Listing"}
{ttl+=$5;
print $9 "  ^" $5 " ^"$3}
END{print "Total " ttl " bytes"}'
```
The output is:

<pre>
Vagrantfile  ^1867 ^ali
Vagrantfile.bak  ^1868 ^ali
data  ^68 ^ali
puppet  ^136 ^ali
script  ^102 ^ali
Total 4041 bytes
</pre>

#### Loop every line of the input file, compute max of a field
```bash
 echo input.txt | \
 awk '{ temp = substr($0, 88, 5) + 0;
           q = substr($0, 93, 1);
           if (temp !=9999 && q ~ /[01459]/ && temp > max) max = temp }
         END { print max }'
```

## `$(...)` and `${...}`
* `${HOME}` return the value of the variable named HOME.
* `$(...)` run whatever is inside the parentheses in a subshell and return that as the value. for example, $(ls) will get a list of files and subfolders in the current folder, since ls will write files and subfolders to standard out.
* `$(...)` allows command substitution, i.e. allows the output of a command to replace the command itself and can be nested.
```bash
# print each file/folder in current folder
for i in $(ls); do echo $i; done
```

## find with timestamp 
```bash
find /srv/files/ -type f -newermt "2015-04-01 00:00:00" ! -newermt "2015-07-31 00:00:00" > /tmp/tmpclients.txt
cat /tmp/tmpclients.txt | while READ FILE ; do rm -rf "$FILE" ; done
```
## `top` and `htop`
htop is a friendly top, following key are supported:
* f5: show the process in tree graph
* f6: show sort panel

## nagios and bacula, haproxy
* nagios: for monitoring
* bacula: for backup
* haproxy: load balancing

## [JQ](https://stedolan.github.io/jq/manual/)
On mac, it can be installed `brew install jq`. Good for parsing json on shell.
```
$ echo '[{"a":1},{"b":2}]'|jq '.[]' # pretty print json array, '.' represents the input
$ echo '[{"a":1},{"b":2}]'|jq '.[0]' # pretty print first element of json array
```

## Refs
* [www.tldp.org](http://www.tldp.org/guides.html)
  * [Here documents](http://tldp.org/LDP/abs/html/here-docs.html) 
  * [Text processing commands](http://tldp.org/LDP/abs/html/textproc.html)
* [www.unixwiz.net](http://www.unixwiz.net/techtips)
  * [An Illustrated Guide to SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) 
    * [Using ssh-agent forwarding on github](https://developer.github.com/guides/using-ssh-agent-forwarding/)
* [bash keyboard shortcut](http://ss64.com/bash/syntax-keyboard.html)
