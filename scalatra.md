## Install scalatra
### Install JDK 7 (NOT JDK 8)
### Install conscript and giter8
```bash
$ curl https://raw.githubusercontent.com/n8han/conscript/master/setup.sh | sh
$ vi ~/.bashrc # update PATH=$PATH:~/bin
$ brew update && brew install giter8
$ cd my-scalatra-web-app
$ cat "port in container.Configuration := 8090" > build.sbt
$ sbt # make sure nobody use :8090, lsof -i :8090, if any, killall -9
> container:start
> ~ ;copy-resources;aux-compile  # monitor change and reload app
> exit
```
