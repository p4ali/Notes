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

## `grep`, `sed`,`cut`and `awk`
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

## Refs
* [www.tldp.org](http://www.tldp.org/guides.html)
  * [Here documents](http://tldp.org/LDP/abs/html/here-docs.html) 
  * [Text processing commands](http://tldp.org/LDP/abs/html/textproc.html)
* [www.unixwiz.net](http://www.unixwiz.net/techtips)
  * [An Illustrated Guide to SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) 
    * [Using ssh-agent forwarding on github](https://developer.github.com/guides/using-ssh-agent-forwarding/)
