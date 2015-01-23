## Install scalatra
### Install JDK 7 (NOT JDK 8)
### Install conscript and giter8
```bash
$ curl https://raw.githubusercontent.com/n8han/conscript/master/setup.sh | sh
$ vi ~/.bashrc # update PATH=$PATH:~/bin
$ brew update && brew install giter8
$ cd my-scalatra-web-app
$ sbt # make sure nobody use :8080, lsof -i :8080, if any, killall -9
> container:start
> exit
```
