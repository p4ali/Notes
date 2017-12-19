## Shell version

`echo $BASH_VERSION`: 4.4.12(1)-release

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

## double dash `--` for end of option
A double dash (--) is used in bash built-in commands and many other commands to signify the end of command options, after which only positional parameters are accepted.

Example use: lets say you want to grep a file for the string -v - normally -v will be considered the option to reverse the matching meaning (only show lines that do not match), but with -- you can grep for string -v like this:
```
grep -- -v file
```

## Create an arry from input
```bash
IFS=$'\n'; arr=( $(echo -e "a b\nc\nd e") ); for i in ${arr[@]} ; do echo $i ; done
# output
a b
c
d e
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

## [netcat](http://www.computerhope.com/unix/nc.htm)

read write data across network with TCP and UDP.

```bash
function wait_for_postgres
{
       # sleep wait for 100 seconds
       for I in {1..300..3} ; do
               netcat -z postgres 5432
               if [ $? -eq 0 ] ; then
                       return 0
               fi
               echo "waiting for postgres"
               sleep 1
       done
       return 1
}
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

## SSH Tunnelling
```
ssh -L80:workserver.com:80 user@workdesktop.com
```
This command creates an SSH connection to your workdesktop.com computer, but at the same time opens port 80 on your local machine. If you point your web browser at http://localhost, the connection will actually be forwarded through your SSH connection to your desktop, and sent onto the workserver.com server, port 80. This is very useful for accessing intranet-only sites from home, without connecting to a VPN first.

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

make sure the default docker config contains dns setting

```bash
# add the virutalbox host as a DNS resolver
grep -q -F 'DOCKER_OPTS="--dns 192.168.33.100"' /etc/default/docker
if [ ! $? -eq 0 ]; then
	echo 'DOCKER_OPTS="--dns 192.168.33.100"' >> /etc/default/docker
fi
```

### [`sed`](https://www.gnu.org/software/sed/manual/html_node/Addresses.html) to replace nested xml

`sed` can select lines by address range, an address range can be specified by specifying two addresses separated by a comma (,). An address range matches lines starting from where the first address matches, and continues until the second address matches (inclusively). The address can be regexp.

Given 
```xml
	<parent>
	   <groupId>x</groupId>
	   <version>0.0.7-SNAPSHOT</version>
	</parent>
```
You can use `sed` to replace the nested <version> tag as following:

```
newVersion=1.0.0
sed -i -e "/<parent>/,/<\/parent>/ s|<version>[0-9a-zA-Z.-]\{1,\}</version>|<version>$newVersion</version>|g" pom.xml
```


### Regex look backward `(?<=fileSize0)\d+`, lookforward `\d+(?= )` and greedy `?`

| mode | positve | negative|
|------:|:--------|:-------|
|lookahead| `(?=foo)` | `(?!foo)`|
|lookbehind| `(?<=foo)` | `(?<!foo)`|

```
... depotFile0 //change_failed/main/a.zip#012... action0 add#012... type0 ubinary#012... rev0 1#012... depotFile1 //change_failed/main/b.zip#012... action1 edit#012... type1 ubinary#012... rev1 1
```
Using lookback and look forward with non-greedy search
```bash
echo $INPUT | grep -Po '(?<=depotFile\d )//change_failed.*?(?=#012)'
# //change_failed/main/a.zip
# //change_failed/main/b.zip
```

Greedy `?` can be replaced by negtive matching, e.g. `<img.*?>` equal to `<img[^>]*>`

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
$ APISERVER=$(kubectl config view |grep https|cut -f 2- -d':'|tr -d ' ')
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

## [printf](http://wiki.bash-hackers.org/commands/builtin/printf)

```bash
# create file 001-101.txt
for i in {1..101}; do a=`printf "%03d" $i`; touch $a.txt; done
```

## `$(...)` and `${...}`
* `${HOME}` return the value of the variable named HOME.
* `$(...)` run whatever is inside the parentheses in a subshell and return that as the value. for example, $(ls) will get a list of files and subfolders in the current folder, since ls will write files and subfolders to standard out.
* `${...}` allows command substitution, i.e. allows the output of a command to replace the command itself and can be nested.
```bash
# print each file/folder in current folder
for i in $(ls); do echo $i; done
```

## `{}`, `()`,`(())` `[]`, `[[]]` [expansion](http://stackoverflow.com/questions/2188199/how-to-use-double-or-single-bracket-parentheses-curly-braces)
* `{}`: brace expansions create a lists of strings which are typically iterated over in loops. 
```bash
# create two subfolders.
$ mkdir -p /srv/{foo, bar}

# truncate a variable
$ var="abcde"; echo ${var%d*} #abc

# substibute 
$ var="abcde"; echo ${var/de/12}; #abc12

# default value
$ default="hello"; unset var; echo ${var:-$default} #hello

# list 
$ for num in {000..2}; do echo "$num"; done # 000, 001, 002

# list with steps
$ echo {D..T..4} # DHLPT

```
* `[]`: also named as `test`, they are bash builtins. It is available on POSIX shells. It also used for array indices
* `[[]]``: also named as `new test`, which works only in bash/zsh and korn shell. It is more powerful. It is use for testing string snad files.
* `()`: create an array with command. `dir=(*); echo "${dir[0]}" #first array element of dir `
* `(())`: is used for ArithmethicExpression operation. ` i=0; while (( i<10 )); do ((i=i+1)); echo $i; done`

## [Shell Parameter Expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)
**${parameter:offset:length}**
```bash
$ set -- 1234567890abcdefgh # set arg1 to 12..gh
$ echo ${1:7} # 7890abcdefgh
$ echo ${1:7:2} # 78
$ echo ${1:7:-2} # 7890abcdef
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

## loop a variable
Since [Brace expansion](http://linuxcommand.org/lc3_lts0080.php) occurs before variables are expanded. This does NOT work:
```
for i in {0..$total}; do echo $i; done
```

This works:
```bash
# C-style
total=${total:-10}
for ((i=0; i<=total; i++)); do echo "$i"; done

# use eval
for i in `eval echo {0..$total}`; do echo $i; done
```

## add user and group

```bash
## query uid:gid
$ id

## create docker group and add user to that group
$ sudo groupadd docker
$ sudo usermod -aG docker me # add me to docker group
$ sudo gpasswd -a me docker # alternative command to add me to docker group
$ sudo service docker restart # restart docker service
```

## `xargs -r`

also `--no-run-if-empty`, do not run command if standard input contains only blanks. e.g., `(docker ps -a -q -f status=exited) | xargs -r docker rm -v`.

## [redirection](http://wiki.bash-hackers.org/syntax/redirection#here_strings)

## [screen](http://linoxide.com/linux-how-to/screen-remote-ssh/) and [tmux](https://tmux.github.io/)
Both should be running on server which is machine you want to mutiplex.

`screen` can run a long running process without maintaining an active shell session.
```bash
$ screen
# do long time task
$ Ctrl+a,d  # detach screen
$ Ctrl+a,c  # new screen
$ Ctrl+a,dquote # list screens

#do something in a new screen
$ screen -r # reattach to the previous screen (will let you choose if many)
```

[tmux](http://www.dayid.org/comp/tm.html) is to replace `screen`
```bash
$ tmux
$ Ctrl+b,d # detach session
$ Ctrl+b,c # new session
$ tmux ls # list tmux sessions
$ tmux attach
```


## nagios and bacula, haproxy
* nagios: for monitoring
* bacula: for backup
* haproxy: load balancing

## [JQ](https://stedolan.github.io/jq/manual/)
On mac, it can be installed `brew install jq`. Good for parsing json on shell.
```
$ echo '[{"a":1},{"b":2}]'|jq '.[]' # pretty print json array, '.' represents the input
$ echo '[{"a":1},{"b":2}]'|jq '.[0]' # pretty print first element of json array
$ echo '{"x": [{"a":1},{"b":2}]}'|jq '.x[]|select(.a==1)|.a' # select a value from array of object

## extract certain attributes from an array with xpath like syntax
curl 'https://api.github.com/repos/stedolan/jq/commits?per_page=5' | jq '.[] | {message: .commit.message, name: .commit.committer.name}'
```


## Refs
* [www.tldp.org](http://www.tldp.org/guides.html)
  * [Here documents](http://tldp.org/LDP/abs/html/here-docs.html) 
  * [Text processing commands](http://tldp.org/LDP/abs/html/textproc.html)
* [www.unixwiz.net](http://www.unixwiz.net/techtips)
  * [An Illustrated Guide to SSH Agent Forwarding](http://www.unixwiz.net/techtips/ssh-agent-forwarding.html) 
    * [Using ssh-agent forwarding on github](https://developer.github.com/guides/using-ssh-agent-forwarding/)
* [bash keyboard shortcut](http://ss64.com/bash/syntax-keyboard.html)
* [bash hacker wiki](http://wiki.bash-hackers.org/)
* [ten thing about linux commandline](https://linuxacademy.com/blog/linux/ten-things-i-wish-i-knew-earlier-about-the-linux-command-line-2/)
* [bash pitfalls](http://mywiki.wooledge.org/BashPitfalls)
